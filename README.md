# ♻️ Waste Sorter

Take or upload a photo of one item, and an AI model suggests the recycling bin:
cardboard, glass, metal, paper, plastic or general trash.

**Live app:** https://solomonpromise.github.io/waste-sorter/

The model runs entirely in the browser with [ONNX Runtime Web](https://onnxruntime.ai/), so photos never leave
the device and no server is needed.

## The model

A ResNet18 convolutional neural network, pretrained on ImageNet (torchvision), with its last block fine-tuned in
Lab 2 of our Deep Learning course on the [TrashNet](https://github.com/garythung/trashnet) dataset (Gary Thung and
Mindy Yang, MIT licence) plus a small set of real-world web photos. Exported to ONNX (`model.onnx`).

The example photos in `examples/` are from TrashNet (MIT licence).

## Limitations

Trained on a few thousand photos: it works best on one item against a plain background, it confuses glass and
plastic most often, and it can be wrong. When it is less than 60% sure, it says so. Check your local recycling rules.
