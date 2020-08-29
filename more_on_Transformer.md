From: 

- https://www.youtube.com/playlist?list=PLoROMvodv4rOhcuXMZkNm7j3fVwBBY42z
- http://web.stanford.edu/class/cs224n/slides/

# Attention

<img src="image/attention_1.png" width="700"/>
<img src="image/attention_2.png" width="700"/>
<img src="image/attention_3.png" width="700"/>

In below, we pass the concatenated vector through a feedforward neural network (one trained jointly with the model).
The output of the feedforward neural networks indicates the output word of this time step.

<img src="image/attention_4.png" width="700"/>
<img src="image/attention_5.png" width="700"/>
<img src="image/attention_6.png" width="700"/>
<img src="image/attention_7.png" width="700"/>

"If attention gives us access to any state … maybe we don’t need the RNN?"

# Transformer

From:

- http://jalammar.github.io/illustrated-transformer/

<img src="image/The_transformer_encoder_decoder_stack.png" width="700"/>
<img src="image/encoder_with_tensors.png" width="700"/>

Say the following sentence is an input sentence we want to translate:

”The animal didn't cross the street because it was too tired”

What does “it” in this sentence refer to? The street or to the animal?

As the model processes each word (each position in the input sequence), self-attention allows it to look at other positions in the input sequence for clues lead to a better encoding for this word. When the model is processing the word “it”, self-attention allows it to associate “it” with “animal”.

<img src="image/transformer_self-attention_visualization.png" width="700"/>

### How to calculate self-attention using vectors

The **first step** in calculating self-attention is to create three vectors from each of the encoder’s input vectors (in this case, the embedding of each word). So for each word, we create a Query vector, a Key vector, and a Value vector. These vectors are created by multiplying the embedding by three matrices that we trained during the training process. E.g. Multiplying x1 by the WQ weight matrix produces q1, the "query" vector associated with that word.

Notice that these new vectors are smaller in dimension than the embedding vector. Their dimensionality is 64, while the embedding and encoder input/output vectors have dimensionality of 512. They don’t HAVE to be smaller, this is an architecture choice to make the computation of multiheaded attention (mostly) constant.

<img src="image/transformer_self_attention_vectors.png" width="700"/>

The **second step** in calculating self-attention is to calculate scores. The scores determine how much focus to place on other parts of the input sentence as we encode a word at a certain position.

The score is calculated by taking the dot product of the query vector with the key vector of the respective word we’re scoring. So if we’re processing the self-attention for the word in position #1, the first score would be the dot product of q1 and k1. The second score would be the dot product of q1 and k2.

<img src="image/self-attention-output.png" width="700"/>

The **third and forth steps** are to divide the scores by 8 (the square root of the dimension of the key vectors used in the paper – 64. This leads to having more stable gradients. There could be other possible values here, but this is the default), then pass the result through a softmax operation. Softmax normalizes the scores so they’re all positive and add up to 1.

This softmax score determines how much each word will be expressed at this position. Clearly the word at this position will have the highest softmax score, but sometimes it’s useful to attend to another word that is relevant to the current word.

The **fifth step** is to multiply each value vector by the softmax score (in preparation to sum them up). The intuition here is to keep intact the values of the word(s) we want to focus on, and drown-out irrelevant words (by multiplying them by tiny numbers like 0.001, for example).

The **sixth step** is to sum up the weighted value vectors. This produces the output of the self-attention layer at this position (for the first word).

That concludes the self-attention calculation. The resulting vector is one we can send along to the feed-forward neural network. In the actual implementation, however, this calculation is done in matrix form for faster processing. 
