<p style="text-align: center;"> <h1> PEGASUS: Pre-training with Extracted Gap-sentences forAbstractive Summarization </h1></p>

</br>

## Summary :</br>
The authors propose a new self-supervised pre-training objective for abstractive summarization: Gap Sentence Generation (GSG); Important sentences are removed/masked from an input document and are generated together as one output sequence from the remaining sentences/ document .
They demonstrate that PEGASUS achieves state-of-the-art per-formance on all 12 downstream data sets and a surprising performance on low-resource summarization, which surpasses previous state-of-the-art results on 6 datasets with only 1000 examples
</br>
</br>

## Brief about Text Summarization :</br> 
Summarization aims at generating accurate and concise summaries from input document(s) 
Types of summarization in NLP
- _Extractive summarization_ which merely copies informative fragments from the input  
- _Abstractive summarization_ may generate novel words. A good abstractive summary covers principal information in the input and is linguistically fluent.
</br>
</br>

## **PEGASUS** : </br>
Important sentences are removed/masked from an input document and are generated together as one output sequence from the remaining sentences/ document . The authors use ROUGE1-F1 between the sentence and the rest of the document as the proxy for importance and masking. 
The authors hypothesize that this objective is suitable for abstractive summarization as it closely resembles the down-stream task. Using GSG to pre-train a Transformer encoder-decoder on a large corpora of documents (Web and news articles) results in Pre-training with Extracted Gap-sentences for Astractive Summarization Sequence-to-sequence models, or PEGASUS.</br>Read more about rogue score here : https://rxnlp.com/how-rouge-works-for-evaluation-of-summarization-tasks/#.Xvuhbl_iuUk
</br>
</br>

## Method :</br>
Select and mask whole sentences from documents, and concatenate the gap-sentences into a pseudo-summary. The corresponding position of each selected gap sentence is replaced by a mask token [MASK1] to inform the model.
<div align = "center">
<img align = "center" src = "image/pegasus_model.PNG" height = 400>
</div>
</br>

They tried 3 primary strategies for selecting m gap sentences from a document D = {xi}n containing n words
- **Random** → Uniformly get m words randomly
- **Lead** → First m words
- **Principle** →
_By independently scoring each sentence_ (**Ind**)</br>
Select top-m scored by importance assigned by ROGUE F1 score</br>
Si = rogue( Xi , D / {Xi} )</br>
_Selecting sentences sequentially (Seq)_ by greedily maximizing the ROUGE1-F1 between selected sentences S∪{xi}, and remaining sentences,D / (S∪{xi})</br>
S U {Xi} (history + current} and D / S U {Xi} (remaining document given seq)</br>
Si = rogue(S U {Xi}, D / S U {Xi})</br></br>
<div align = "center">
<img align = "center" src = "image/pegasus_algo.PNG" height = 200>
</div>
</br>

They also considered n-grams as a set (**Uniq**) instead of double-counting identical n-grams as in the original implementation (Orig), resulting in four variants of the principal sentence selection strategy



**Masked Language Model** : Similar to that of BERTs. 
Select 15% tokens in the input text; the selected tokens are :
80% of time replaced by a mask token [MASK2]
10% of time replaced by a random token
10% of time unchanged

Apply MLM to train the Transformer encoder as the sole pre-training objective or along with GSG
## Ablations :
Ablation experiments were performed on a  reduced-size model PEGASUS SMALL to make choices for large model (final) PEGASUS LARGE.

**GSG** :   
Results suggested choosing principal sentences worked best for downstream summarization tasks,and they chose Ind-Orig for the PEGASUS LARGE.

**GSR** : 
Gap Sentence Ratio, refers to the number of selected gap sentences to the total number of sentences in the document, 
A low GSR makes the pre-training less chal-lenging and computationally efficient. 
  high  GSR  loses  contextual information necessary to guide the generation
An effective GSR of 30% was chosen for PEGASUS LARGE

**MLM** : 
Model pre-trained with MLM alone performed significantly worse 
MLM & Ind-Orig had similar performance as Random
Hence they choose not to include MLM in PEGASUS LARGE

## Main Contributions :
- A new self-supervised pre-training objective for abstractive summarization, GSG and studied strategies for selecting those sentences.
- Evaluate the proposed pre-training objective on a broad range of downstream summarization tasks, with careful ablations to choose the best model settings, with the state-of-the-art on all 12 downstream datasets considered
- Showed how good abstractive summarization performance  can  be  achieved  across  broad  domains  with very little supervision by fine-tuning the PEGASUS
- Conducted human evaluation studies to validate experimental design and demonstrate human-level summarization performance


