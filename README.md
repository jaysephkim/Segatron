# Segatron: Segment-Aware Transformer for Language Modeling and Understanding

## Overview
Transformers are powerful for sequence modeling! Nearly all state-of-the-art language models and pre-trained language models are based on the Transformer architecture. 
However, the algorithm distinguishes sequential tokens only with the token position index. 
This article hypothesizes that better contextual representations can be generated from the Transformer with richer positional information. To verify this, the paper proposes a segment-aware Transformer (Segatron), by replacing the original token position encoding with a combined position encoding of paragraph, sentence, and token. 

## Approach/Model
- This algorithm was applied first by replacing the vanilla Transformer index in Transformer XL, and then to a pre-trained language model, BERT. 
- The new method would encode a paragraph index in a document, sentence index in a paragraph, and a token index in a sentence. 


### Segatron XL
- Transformer-XL is a memory-augmented Transformer with relative position encoding, thus allowing long range dependencies. 
- We replace the vanilla Transformer index in Transformer-XL with Segatron. 
- This proposed method introduces paragraph and sentence segmentation to the relative position encoding. 
- The new embeddings Ri,j are defined as:

<img width="396" alt="Screen Shot 2022-03-26 at 2 21 07 PM" src="https://user-images.githubusercontent.com/69778066/160254104-9c23d442-7466-4f46-9399-1b805229617b.png">

- Each are the relative position embedding of token, sentence, and paragraph.
- To equip the recurrence memory mechanism of Transformer-XL with the segment-aware relative position encoding, the paragraph position, the sentence position, and the token position indexes of the previous segment should also be cached together with the hidden states. 
- Then, the relative position can be calculated by subtracting the cached position indexes from the current position indexes.

<img width="369" alt="Screen Shot 2022-03-26 at 2 42 55 PM" src="https://user-images.githubusercontent.com/69778066/160254773-c96e75a6-371e-4f9c-9254-4309a42a0e3d.png">


### Pre-trained Segatron(SegaBERT)
- Pretrain a language model with the proposed Segatron.
- BERT is a good baseline model, as it requires less computational resources.
- SegaBERT pre-trained with Wiki-books dataset and 1M training steps
- Input X of SegaBERT is a sequence of tokens, which can be one or more sentences or paragraphs.
- 


<img width="330" alt="Screen Shot 2022-03-26 at 2 44 01 PM" src="https://user-images.githubusercontent.com/69778066/160254806-b54df98b-326f-42e1-809e-d0a8f2b95625.png">


## Performance
#### Dataset
- WikiText-103: preserves both punctuations and paragraph line breakers.

#### Main Results
- Experimental results show that BERT pre-trained with Segatron (SegaBERT) can outperform BERT with vanilla Transformer on various NLP tasks

<img width="367" alt="Screen Shot 2022-03-26 at 4 59 17 PM" src="https://user-images.githubusercontent.com/69778066/160258399-13378e67-6c62-40b3-93e1-0cb3e5b12bc2.png">

Here we see that the PPL of Transformer-XL decreases from 24.35 to 24.07/22.51 after adding paragraph/sentence position encoding, and further decreases to 22.47 by encod- ing paragraph and sentence positions simultaneously. The results show that both the paragraph position and sentence po- sition can help the Transformer to model language. Sentence position encoding contributes more than paragraph position encoding in our experiments.


<img width="492" alt="Screen Shot 2022-03-26 at 5 01 44 PM" src="https://user-images.githubusercontent.com/69778066/160258462-fbc57ec1-4252-463e-b378-f22f2b078724.png">
<img width="421" alt="Screen Shot 2022-03-26 at 5 02 43 PM" src="https://user-images.githubusercontent.com/69778066/160258474-d2455ab4-0efb-4f68-b5bc-89760cc4a64c.png">

## Discussion Topic 1
Why is this segment-aware method able to consistently outperform the original transformer methods?

## Discussion Topic 2
What could be a drawback of this application?

## Discussion Topic 3
What situations would Segatron be prefferent and what situations what it not be preffered? 


## Other Resources
https://arxiv.org/abs/2004.14996

https://www.semanticscholar.org/paper/Transformer-XL%3A-Attentive-Language-Models-beyond-a-Dai-Yang/c4744a7c2bb298e4a52289a1e085c71cc3d37bc6

https://www.semanticscholar.org/paper/ALBERT%3A-A-Lite-BERT-for-Self-supervised-Learning-of-Lan-Chen/7a064df1aeada7e69e5173f7d4c8606f4470365b
