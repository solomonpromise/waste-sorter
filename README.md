# ♻️ Waste Sorter

Take or upload a photo of one item, and an AI model suggests the recycling bin:
cardboard, glass, metal, paper, plastic or general trash.

**Live app:** https://solomonpromise.github.io/waste-sorter/

The model runs entirely in the browser with [ONNX Runtime Web](https://onnxruntime.ai/), so photos never leave
the device and no server is needed.

## The model

A ResNet18 convolutional neural network, pretrained on ImageNet (torchvision), with its last block fine-tuned in
Lab 2 of our Deep Learning course on the [TrashNet](https://github.com/garythung/trashnet) dataset (Gary Thung and
Mindy Yang, MIT licence) plus a small set of real-world web photos. Exported to ONNX (`model.onnx`, with the larger
weights stored in 16 bits).

### Is it waste at all?

A six-bin classifier always picks a bin, even for a photo of a person. So the model has a second output,
`waste_score`: a logistic-regression gate on the network's general ImageNet features (the original layer 4, run on
the shared frozen layers 1-3), trained on our waste photos against 1,000 everyday [COCO](https://cocodataset.org)
photos with open licences. Its threshold keeps 95% of the waste validation photos. On held-out photos it flagged
97.5% of non-waste COCO photos and let through 99.7% of TrashNet and 95.5% of real-world waste photos. Below the
threshold the app says "Not a waste item?" instead of naming a bin. On 300 close-up photos of single objects
(Imagenette) it flagged 77%; objects made of metal or plastic (a French horn, a cassette player) got through about
half the time.

The example photos in `examples/` are from TrashNet (MIT licence).

## Limitations

Trained on a few thousand photos: it works best on one item against a plain background, it confuses glass and
plastic most often, and it can be wrong. When it is less than 60% sure, it says so. The waste check is not perfect:
some non-waste objects slip through, especially metal and plastic ones, and some real waste photographed like a product shot is
flagged. Check your local recycling rules.
