# Models

## Huggingface Transformers

[Installation](https://huggingface.co/docs/transformers/installation)

## Transformer

## BERT

- Bidirectional Encoder Representations from Transformers
- Transformer encoder: bidirectional transformer
- Bidirectioal: each word can see itself indirectly based on left and right context
- self-attention without mask
- training:
  - mask words
  - next sentence prediction

Input/Output:
- single sentence/pair of sentences
- token Embeddings(WordPiece) + Segment Embeddings + Position Embeddings
- Softmax output layer for token-level tasks
- [CLS] for classification
- SQuAD v1.1: introduce start and end vector during fine-tuning
- SQuAD v2.0: start and end at [CLS]
  

### mBert

- Multilingual sequence-to-sequence denoising auto-encoder
- encoder only
- training:
  - mask words
  - next sentence prediction
- 104 languages
- Zero shot
  - massively multi-lingual MT (Johnson et al.,2017; Gu et al., 2019)
  - distillation through pivoting (Chen et al., 2017)
- Unsupervised Machine Translation
  - Back-Translation
  - Language Transfer
  - Combined

## GPT-2

A language model with sufficient capacity will begin to learn to infer and perform the tasks demostrated in natural language sequences in order to better predict them, regardless of their method of procurement.

- WebText
- Transformer based
- OpenAI GPT with a few modifications
- unsupervised
- multitask
- masked self-attention

Input/Output(GPT):
- input: text + position embedding
- predict vector with decoder layers and sample a word with top-k sampling
- use decoder to predict next token in language modeling
- start, delimiter($) and end tokens

## Language modeling

predict the next token based on previous tokens

## T5: Text-To-Text Transfer Transformer

[huggingface t5](https://huggingface.co/docs/transformers/model_doc/t5)

- Architecture -> Encoder-Decoder
  - Encoder-Decoder ✅
    - Encoder: fully-visible attention mask
    - Decoder: causal masking pattern
  - Language model (decoder-only)
  - Prefix LM (decoder-only)
- Unsupervised objectives
  - High-level approaches
    - BERT-style ✅
    - Language modeling
    - Deshuffling
  - Corruption strategies
    - Replace spans ✅
    - Mask
    - Drop
  - Corruption rate
    - 10%
    - 15% ✅
    - 25%
    - 50%
  - Corrupted span length
    - i.i.d ✅
    - 2
    - 3
    - 5
    - 10
- pre-trained on C4
- allows the use of exactly the same training objective (teacher-forced maximum- likelihood) for every task
- computational effort

Input/Output:
- sequence-to-sequence
- task-specific prefix in original input sequence
- sequence of input tokens are mapped to a sequence of output embedding
- output is fed into a dense layer with a softmax output, whose weights are shared with the input embedding matrix
- text embedding + position embedding

### mT5: multilingual pre-trained text-to-text transformer

[huggingface mt5](https://huggingface.co/docs/transformers/model_doc/mt5)

- pre-trained on mC4
- 101 languages
- pre-trained unsupervisedly -> no real advantage to using a task prefix during single-task fine-tunning

Zero-Shot Generation

- Domain Preserving Training
- Illegal Predictions
  - Normalization: most mT5-XXL illegal predictions are resolved by normalization
  - Grammatical adjustment
  - Accidental translation
    - don't use English only for fine-tuning
    - reduce the language sampling parameter $\alpha$ (e.g. to 0.1)

Q: fine-tune on an English generative task, then it works for all other lanugages?

A: not 100% reliable

## XLM

- BERT-based (encoder)
  - Casual Language Modeling (CLM)
  - Masked Language Modeling (MLM)
  - Translation Language Modeling (TLM)
- cross-lingual pre-training objectives
- reduce perplexity
- 100 languages

### XLM-R

- Sentence piece model
- longer training time
- larger model
- outperforms monolingual BERT by making use of multilingual training
- used CommonCrawl data instead of Wikipedia
- trained with a cross-lingual masked language modeling objective
- analysis the trade-offs and limitations of multilingual language models
- 100 languages

## BART

- combined BERT and GPT
- mask and reconstruct sentences
- an additional encoder for machine translation before encoder

### mBART

- sequence to sequence
- denoising full texts in multiple languages
- trained with a combination of span masking and sentence shuffling objectives
- 25 languages (subset of XLM-R), 50 languages in mBART-50
- denoising full texts in multiple languages

## MARGE

- document-level sequence-to-sequence
- encoder-decoder
- Encoder: retrieve related texts in many languages
- Decoder: reconstruct the original text from retrieved texts
- reconstruct a document in one language by retrieving documents in other languages
- performs well even merely pre-training
- 26 languages

## RemBert (Rebalanced multilingual BERT)

- re-evaluate the standard practice of sharing weights between input and output embeddings
- decoupled embeddings provide increased modeling flexibility, allowing us to significantly improve the efficiency of parameter allocation in the input embedding of multilingual models
- reallocate the input embedding parameters
- allocate additional capacity to the output embedding provides benefits to the model that persist through the fine-tuning stage even though the output embedding is discarded after pre-training
- larger output embeddings prevent the model's last layers from overspecializaing to the pre-training task and encourage Transformer representations to be more general and more transferable to other tasks and languages

## Evaluation

### XTREME: A Massively Multilingual Multi-task Benchmark for Evaluating Cross-lingual Generalization
