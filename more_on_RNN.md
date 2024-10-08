From: 

- https://www.youtube.com/playlist?list=PLoROMvodv4rOhcuXMZkNm7j3fVwBBY42z
- http://web.stanford.edu/class/cs224n/slides/

- Language Model is about predicting the next word. RNN is a way to build a Language Model.
- The core idea of RNN is that the same weight matrix is used in all steps.
- Compute loss J(θ) for a batch of sentenses, compute gradient and update weights. Repeat...

<img src="image/train_rnn_1.png" width="700"/>
<img src="image/train_rnn_2.png" width="700"/>
<img src="image/evaluate_language_models.png" width="700"/>
<img src="image/vanishing_gradient_1.png" width="700"/>
<img src="image/vanishing_gradient_2.png" width="700"/>
<img src="image/vanishing_gradient_3.png" width="700"/>
<img src="image/exploding_gradient_1.png" width="700"/>
<img src="image/exploding_gradient_2.png" width="700"/>
<img src="image/bidirectional_rnn_1.png" width="700"/>
<img src="image/bidirectional_rnn_2.png" width="700"/>

In below, notice that in training (unlike in testing) the predicted word is not fed into the next step; the ground truth word is fed into the next step.

<img src="image/seq2seq_1.png" width="700"/>
<img src="image/seq2seq_2.png" width="700"/>