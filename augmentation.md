- Visualize the effect here: https://albumentations-demo.herokuapp.com/

- From: https://albumentations.readthedocs.io/en/latest/probabilities.html

        All pre / post processing transforms: Compose, PadIfNeeded, CenterCrop, RandomCrop, Crop, RandomCropNearBBox, RandomSizedCrop, RandomResizedCrop, RandomSizedBBoxSafeCrop, CropNonEmptyMaskIfExists, Lambda, Normalize, ToFloat, FromFloat, ToTensor, LongestMaxSize have default probability values equal to 1. All others are equal to 0.5

```
albumentations.Compose([
    albumentations.RandomResizedCrop(image_size, image_size, scale=(0.9, 1), p=1), 
    albumentations.HorizontalFlip(p=0.5),
    albumentations.ShiftScaleRotate(p=0.5),
    albumentations.HueSaturationValue(hue_shift_limit=10, sat_shift_limit=10, val_shift_limit=10, p=0.7),
    albumentations.RandomBrightnessContrast(brightness_limit=(-0.2,0.2), contrast_limit=(-0.2, 0.2), p=0.7),
    albumentations.CLAHE(clip_limit=(1,4), p=0.5),
    albumentations.OneOf([
        albumentations.OpticalDistortion(distort_limit=1.0),
        albumentations.GridDistortion(num_steps=5, distort_limit=1.),
        albumentations.ElasticTransform(alpha=3),
    ], p=0.2),
    albumentations.OneOf([
        albumentations.GaussNoise(var_limit=[10, 50]),
        albumentations.GaussianBlur(),
        albumentations.MotionBlur(),
        albumentations.MedianBlur(),
    ], p=0.2),
    albumentations.Resize(image_size, image_size),
    albumentations.OneOf([
        JpegCompression(),
        Downscale(scale_min=0.1, scale_max=0.15),
    ], p=0.2),
    IAAPiecewiseAffine(p=0.2),
    IAASharpen(p=0.2),
    albumentations.Cutout(max_h_size=int(image_size * 0.1), max_w_size=int(image_size * 0.1), num_holes=5, p=0.5),
    albumentations.Normalize(),
])
```
