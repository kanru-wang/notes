From:

- https://stackoverflow.com/questions/53628622/loss-function-its-inputs-for-binary-classification-pytorch
- https://nathanbrixius.wordpress.com/2016/06/04/functions-i-have-known-logit-and-sigmoid/

- torch.nn.CrossEntropyLoss includes a Softmax activation function.
- torch.nn.BCEWithLogitsLoss includes a Sigmoid activation function.
- torch.nn.BCELoss needs a Sigmoid activation function but doesn't have one, so need to pass through a Sigmoid before BCELoss.