# U-Net

From: https://towardsdatascience.com/understanding-semantic-segmentation-with-unet-6be4f42d4b47

- By down sampling, the model better understands “WHAT” is present in the image, but it loses the information of “WHERE” it is present.
- In case of segmentation we need both “WHAT” as well as “WHERE” information.
- Hence there is a need to up sample the image, i.e. convert a low resolution image to a high resolution image to recover the “WHERE” information.
- Encoder part (left side of the U-Net) is necessary for finding “WHAT” is in the image; decoder part (right side of the U-Net) is necessary for the “WHERE” information.

From: https://www.kaggle.com/julian3833/2-understanding-and-plotting-rle-bounding-boxes

- Understanding RLE-encoded bounding boxes
