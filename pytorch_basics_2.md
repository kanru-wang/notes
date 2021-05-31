From: https://towardsdatascience.com/pytorch-layer-dimensions-what-sizes-should-they-be-and-why-4265a41e01fd

### How to read tensor sizes in PyTorch

The first dimension is always batch size (N).

Example: The tensor dimensions PyTorch likes.

    """Example tensor size outputs, how PyTorch reads them, and where you encounter them in the wild. 
    Note: the values below are only examples. Focus on the rank of the tensor (how many dimensions it has)."""

    >>> torch.Size([32])
    # 1d: [batch_size] 
    # use for target labels or predictions.
    
    >>> torch.Size([12, 256])
    # 2d: [batch_size, num_features (aka: C * H * W)]
    # use for nn.Linear() input.
    
    >>> torch.Size([10, 1, 2048])
    # 3d: [batch_size, channels, num_features (aka: H * W)]
    # when used as nn.Conv1d() input.
    # (but [seq_len, batch_size, num_features]
    # if feeding an RNN).
    
    >>> torch.Size([16, 3, 28, 28])
    # 4d: [batch_size, channels, height, width]
    # use for nn.Conv2d() input.
    
    >>>  torch.Size([32, 1, 5, 15, 15])
    # 5D: [batch_size, channels, depth, height, width]
    # use for nn.Conv3d() input.

To load a grey scale, 28 x 28 pixel image into a Conv2d network layer, see the example above. 
Since it wants a 4d tensor, and you already have a 2d tensor with height and width, just add batch_size, and channels (see rule of thumb for channels below) to pad out the extra dimensions, like so: [1, 1, 28, 28].

### Initializing a torch.nn.Conv2d layer


    """
    Class 
    torch.nn.Conv2d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True, padding_mode='zeros')
    Parameters
    in_channels (int) - Number of channels in the input image
    out_channels (int) - Number of channels produced by the convolution
    """

Rule of thumb for “in_channels” on your first Conv2d layer:

- If your image is black and white, it is 1 channel.
- If your image is color, it is 3 channels (RGB).
- If there is an alpha (transparency) channel, it has 4 channels.

This means for your first Conv2d layer, even if your image size is something enormous like 1080px by 1080px, your in_channels will typically be either 1 or 3.

out_channels (int) is the number of channels produced by the convolution. The out_channels is how deep you want your network to be.

For example,

    def __init__(self):
        super(Net, self).__init__()
        # 1 input image channel, 6 output channels, 3x3 square convolution
        # kernel
        self.conv1 = nn.Conv2d(1, 6, 3)
        self.conv2 = nn.Conv2d(6, 16, 3)
        # an affine operation: y = Wx + b
        self.fc1 = nn.Linear(16 * 6 * 6, 120)  # 6*6 from image dimension
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

### Fully connected (torch.nn.Linear) layers


    """
    Class

    torch.nn.Linear(in_features, out_features, bias=True)
    Parameters
    in_features – size of each input sample
    out_features – size of each output sample
    """

Notice that “in_features” and “in_channels” are completely different.

    # Asks for in_channels, out_channels, kernel_size, etc
    self.conv1 = nn.Conv2d(1, 20, 3)
    # Asks for in_features, out_features
    self.fc1 = nn.Linear(2048, 10)

If you want to pass in a 28 x 28 image into a linear layer:

- The 28 x 28 pixel image cannot be input as a [28, 28] tensor. This is because nn.Linear will read it as 28 batches of 28-feature-length vectors. Since it expects an input of [batch_size, num_features], need to transpose it using view().
- The batch size passes unchanged through all the layers. No matter how the data changes as it passes through a network, the first dimension will end up being the batch_size.

        Use view() to change a tensor’s dimensions, so that it can be fed into a linear layer
        image = image.view(batch_size, -1)
        “-1” tells Pytorch to figure out other numbers. The tensor will now feed properly into any linear layer.

To initialize the very first argument of your linear layer, pass it the number of features of your input data. For 28 x 28, our new view tensor is of size [1, 784] (1 * 28 * 28):

        batch_size = 1

        # Simulate a 28 x 28 pixel, grayscale "image"
        input = torch.randn(1, 28, 28)

        # Use view() to get [batch_size, num_features].
        # -1 calculates the missing value given the other dim.
        input = input.view(batch_size, -1) # torch.Size([1, 784])

        # Intialize the linear layer.
        fc = torch.nn.Linear(784, 10)

        # Pass in the simulated image to the layer.
        output = fc(input)

        print(output.shape)
        >>> torch.Size([1, 10])

Remember this — if you’re ever transitioning from a convolutional layer output to a linear layer input, you must resize it from 4d to 2d using view, as described with image example above.
So, a conv output of [32, 21, 50, 50] should be “flattened” to become a [32, 21 * 50 * 50] tensor. And the in_features of the linear layer should also be set to [21 * 50 * 50].
The second argument of a linear layer, if you’re passing it on to more layers, is called H for hidden layer. You just kind of play positional ping-pong with H and make it the last of the previous and the first of the next, like this:
"""The in-between dimensions are the hidden layer dimensions, you just pass in the last of the previous as the first of the next."""
fc1 = torch.nn.Linear(784, 100) # 100 is last.
fc2 = torch.nn.Linear(100, 50) # 100 is first, 50 is last.
fc3 = torch.nn.Linear(50, 20) # 50 is first, 20 is last.
fc4 = torch.nn.Linear(20, 10) # 20 is first. 
"""This is the same pattern for convolutional layers as well, only it's channels, and not features that get passed along."""
The very last output, aka your output layer depends on your model and your loss function. If you have 10 classes like in MNIST, and you’re doing a classification problem, you want all of your network architecture to eventually consolidate into those final 10 units so that you can determine which of those 10 classes your input is predicting.
The last layer is dependent on what you want to infer from your data. The operations you can do to get the answer you need is a topic for another article, because there is a lot to cover. But for now you should have all the basics covered.
