# Introduction 
218 words.

## Title
What is the strongest compression medium humans have known for the last 1000 years? It's language.

My name is Marco. Today I present my MSc thesis, developed with prof Chiara Plizzari and two other MSc students, and on track for submission to the WACV 2027 conference.

## Cross-view object correspondence
I believe in a future of integration between daily life and technology. One of these being vision, in the form of smart glasses, already on the market.

Have you ever found yourself looking for your keys at home? Smart glasses, combined with the cameras in your house, could give you many eyes. This concept extendes to any context where agents, whether human or robotic, collaborate.

Technically, this is object recognition; we focus on cross-view object correspondence, matching objects across synchronized recordings of the same scene. Given a mask track on a target object in one view, the goal is to predict its mask in the other, using Ego-Exo4D, the largest public resource for this task.

The task encapsulates two problems. First, matching the object's mask across views, hard because viewpoint changes between ego and exocentric footage affect object size and shape. Second, temporal completeness, hard due to motion blur, occlusions, and scene changes.

We impose one further restriction: only visual information, since no geometric calibration between cameras is available.


# Related Works
440 words.

## Related works title
Before our pipeline, we need theoretical foundations. Let me walk quickly through the related works.

## Visual representation
Segmenting video means encoding images as vector embeddings. 

Vision Transformers patch the image and apply query-key-value attention, producing a token summarizing it. However, transformers need large labelled datasets. 

Self-supervised learning replaced labels with a pretext task, like predicting a masked patch. This gave rise to the DINO family, today's state of the art for image embeddings.

## Segmentation
Embeddings power segmentation, predicting a binary mask over an object. Mask R-CNN and RITM pioneered dense and promptable masks, but remained narrow due to task specific training and small datasets. 

In 2023, Meta's FAIR lab introduced Segment Anything, the first general purpose promptable segmenter, trained on over a billion masks for zero-shot inference. SAM 2 extended this to video, and in November 2025 SAM 3 added concept tracking from short text prompts. 

But short prompts alone can't bridge two viewpoints. We need detailed language descriptions.

## Language Grounding
Language grounding maps expressions to the pixels they describe. 

CLIP first aligned images and captions in a shared space via contrastive loss. Grounding DINO added open-vocabulary detection, localizing any textual description. 

SAM 3's detector builds on this, conditioning image features on the text prompt to propose segmentation candidates. Unfortunately, it but caps prompts at 32 tokens.

## Foundation Models and Agents
We overcome this limit with Vision Language Models. These are general purpose large foundation models that can reason simultaneously over images and text.

Qwen is the strongest open source family; Qwen 3 adds prolonged reasoning and strong grounding. 

VLMs become even more powerful inside agent designs: loops for reasoning, acting through tools, and conditioning on the results. SAM 3 offers such an interface, letting a VLM segment from relational descriptions and even logical riddles.

## Cross-View Object Correspondence Related Methods title
With the building blocks set, what have others done for cross-view correspondence?

## Official baselines
Ego-Exo4D proposes two baselines: XSegTx, adapting a co-segmentation model to the target object, and XView-XMem, adapting a video tracker to propagate masks across views. Since these are adaptation, both score low.

## 2025 challenge submission
Last year's challenge saw two strong submissions. 

ObjectRelator fuses visual and language features through a Multimodal Condition Fusion block, aligning views with a self-supervised XObjAlign loss. 

O-MaMa reframes the task as mask matching: candidates from FastSAM, pooled DINOv2 features, cross-attention fusion, and a contrastive matching head. O-MaMa won using only 1% of ObjectRelator's trainable parameters.

## Latest developments
Two later methods surpass the challenge. 

V²-SAM adds three experts atop SAM 2, anchoring correspondences from DINOv3 features alongside structural and fusion experts. 

LM-EEC, the strongest, adapts SAM 2 with a Memory-View Mixture-of-Experts fusion module and separate ego and exo memory banks, reaching state of the art. 

It fine-tunes SAM 2's full backbone, so it isn't directly comparable to the others.

## Common Limitations
Every method trains a dedicated fusion component on labelled data. This has two implicatios:

1. Each mask match is hidden in weights and complex architectures; there are no interpretable intermediate steps to inspect for wrong outputs. 
2. Arguably, training on a single datasets limits the generalisability to other datasets and other camera configurations. 

# Method
Our method addresses both these implications and is 1. training-free and in principle 2.camera and dataset agnostic. 

1120 words.

# Experiments
840 words.

# Conclusions
140 words. 