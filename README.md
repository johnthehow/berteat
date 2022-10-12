# BERTET: BERT Explainability Toolkit
## glassbox_bert.py
### Illustration of class variable groups
![glassbox_bert grouping](https://github.com/johnthehow/bertet/blob/master/figure1.png)
### Features
1. The glassbox_bert class returns an instance which encloses both data and model aftering being feed with a sentence
2. Unlike Huggingface BertModel instance, with glassbox_bert you can easily obtain all intermediate representations (44 in total) you can ever imagine, not just Attention and Hidden states.
3. The components in glassbox_bert are highly aligned with the concepts of Transformer encoder as in Vaswani et al. 2017.
4. All components (as class variables) are divied into six groups, namely:
	1. Symbolic: with class variable prefix ***sym_***:
	1. Pre-Embedding: with class variable prefix ***preemb_***:
	1. Multi-Head Self-Attention: with class variable prefix ***selfattn_***:
	1. Add & Norm 1: with class variable prefix ***addnorm1_***:
	1. Feedforward: with class variable prefix ***ffnn_***:
	1. Add & Norm 2: with class variable prefix ***addnorm2_***:
5. The results generated by BertModel Pipeline are stored with class variables with prefix ***pipeline_***:
6. The results generated by step-wise manual procedure are called with class variables with prefix ***manual_***:

### Example Usage
```python
from bertet import glassbox_bert
instance = glassbox_bert.glassbox_bert('an example sentence here')

```

### Note
1. In query, key and value weight matrices, every 64 rows correspond to a head
2. In query, key and value bias matrices, every 64 items correspond to a head
3. Matrix multiplications are done with XW.T, not other way round
3. The activation function of FFNN is gelu
4. The Softmax in Multi-Head Self-Attention is parameterized with axis=-1

### A complete list of class variables of glassbox_bert
* ****model****: a huggingface BertModel instance
* **tokenizer**: a huggingface BertTokenizer instance
\ 

* **pipeline_res**: pipeline result of BertModel
* **pipeline_attns**: pipeline attention of BertModel
* **pipeline_hiddens**: pipeline hidden states of BertModel
\ 

* **sym_sent**: plain text input sentence
* **sym_token_ids**: Bert vocabulary token ids (integer)
* **sym_tokens**: Word-Piece tokens by BertTokenizer (including [CLS] and [SEP])
* **sym_seq_len**: the length of word-piece-tokenized sentence (including [CLS] and [SEP])
\ 

* **preemb_vocab_emb**: pre-trained static word embeddings (30522×768)
* **preemb_word_emb**: static word embeddings from preemb_vocab_emb
* **preemb_pos_emb**: position embeddings
* **preemb_seg_emb**: segmentation embeddings
* **preemb_sum_emb**: preemb_word_emb+preemb_pos_emb+preemb_seg_emb
* **preemb_norm_sum_emb**: LayerNorm(preemb_sum_emb)
\ 

* **selfattn_query_weight**: Query weight matrix 12×12×64×768
* **selfattn_key_weight**: Key weight matrix 12×12×64×768
* **selfattn_value_weight**: Value weight matrix 12×12×64×768
* **selfattn_query_bias**: Query bias vector 12×12×64
* **selfattn_key_bias**: Key bias vector 12×12×64
* **selfattn_value_bias**: Value bias vector 12×12×64
* **selfattn_query_hidden**: Query hidden states matrix 12×12×6×64
* **selfattn_key_hidden**: Key hidden states matrix 12×12×n×64
* **selfattn_value_hidden**: Value hidden states matrix 12×12×n×64
* **selfattn_qkt_hidden**: QK.T 12×12×n×n
* **selfattn_qkt_scale_hidden**: QK.T/sqrt(dk) 12×12×n×n
* **selfattn_qkt_scale_soft_hidden**: Attention:= Softmax(dim=-1)(selfattn_qkt_scale_hidden) 12×12×n×n
* **selfattn_attention**: ==selfattn_qkt_scale_soft_hidden 12×12×n×n
* **selfattn_contvec**: Attention×selfattn_value_hidden 12×12×n×64
* **selfattn_contvec_concat**: Concatenated selfattn_contvec 12×n×768
\ 

* **addnorm1_dense_weight**: Dense weight matrix of Add & Norm 1 layer 12×768×768
* **addnorm1_dense_bias**: Dense bias vector of Add & Norm 1 layer 12×768
* **addnorm1_dense_hidden**: Dense hidden states of Add & Norm 1 layer 12×n×768
* **addnorm1_add_hidden**: Residual connection: preemb_norm_sum_emb + addnorm1_dense_hidden 12×n×768
* **addnorm1_norm_hidden**: LayerNorm(addnorm1_add_hidden) 12×n×768
\ 

* **ffnn_dense_weight**: Dense weight matrix of Feed-forward layer 12×768×3072
* **ffnn_dense_bias**: Dense weight vector of Feed-forward layer 12×3072
* **ffnn_dense_hidden**: Dense hidden states of Feed-forward layer 12×n×3072
* **ffnn_dense_act**: ffnn_dense_hidden after gelu activation 12×n×3072
\ 

* **addnorm2_dense_weight**: Dense weight matrix of Add & Norm 2 layer 12×3072×768
* **addnorm2_dense_bias**: Dense bias vector of Add & Norm 2 layer 12×768
* **addnorm2_dense_hidden**: Dense hidden states of Add & Norm 2 layer 12×n×768
* **addnorm2_add_hidden**: Residual connection: addnorm1_norm_hidden + addnorm2_dense_hidden 12×n×768
* **addnorm2_norm_hidden**: LayerNorm(addnorm2_add_hidden) 12×n×768
\ 

* **manual_hiddens**: [preemb_norm_sum_emb, addnorm2_norm_hidden]


