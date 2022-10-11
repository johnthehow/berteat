# BERTET: BERT Explainability Toolkit
## glassbox_bert.py
### Illustration of class variable groups
![glassbox_bert grouping](https://github.com/johnthehow/bertet/blob/master/figure1.png)
### Features
1. The glassbox_bert class returns an instance which encloses both data and model aftering being feed with a sentence
2. Unlike Huggingface BertModel instance, with glassbox_bert you can easily obtain all intermediate representations you can ever imagine, not just Attention and Hidden states.
3. The components in glassbox_bert are highly aligned with the concepts of Transformer encoder as in Vaswani et al. 2017.
4. All components (as class variables) are divied into six groups, namely:
4.1 Symbolic: with class variable prefix ***sym_***
4.2 Pre-Embedding: with class variable prefix ***preemb_***
4.3 Multi-Head Self-Attention: with class variable prefix ***selfattn_***
4.4 Add & Norm 1: with class variable prefix ***addnorm1_***
4.5 Feedforward: with class variable prefix ***ffnn_***
4.6 Add & Norm 2: with class variable prefix ***addnorm2_***
5. The results generated by BertModel Pipeline are stored with class variables with prefix ***pipeline_***
6. The results generated by step-wise manual procedure are called with class variables with prefix ***manual_***

### Note
1. In query, key and value weight matrices, every 64 rows correspond to a head
2. In query, key and value bias matrices, every 64 items correspond to a head
3. Matrix multiplications are done with XW.T, not other way round
3. The activation function of FFNN is gelu
4. The Softmax in Multi-Head Self-Attention is parameterized with axis=-1
