### Inception Network

- From https://towardsdatascience.com/a-simple-guide-to-the-versions-of-the-inception-network-7fc52b863202
- Use different filter sizes so that objects of all sizes are more easily captured
- Use more types of filters of different sizes (having a wide network) than other architectures (having a deep network), so more information can be retained
- Use rectangular filters (one 1xn followed by one nx1, instead of one nxn square filter), which actually saves computation cost
- 1x1 filters to reduce channels, so (1) can increase speed (bottleneck), (2) used to match numbers of channels (depth sizes), which is required for Inception-ResNet
- Auxiliary classifiers are branched out, and weighted loss can be calculated, so the middle part of the network will not suffer from vanishing gradient
