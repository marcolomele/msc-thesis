# Introduction 
219 words.

## Title
Dear friends, family, Degree Assessment Board, and thesis supervisor: good afternoon.

My name is Marco, and today I will present my MSc thesis. I would like to start with a question.

## question
Is language the most effective medium humans have invented for communicating information?

## Language and technology
I ask because language has become our prominent interface with technology. Raise your hand if you have prompted a large language model in the last 24 hours.

I believe this integration of technology and daily life will continue, and that the next step is vision, in the form of smart glasses, already on the market.

## Cross-view object correspondence
Smart glasses, combined with the cameras in your house, could give you many more eyes than you have today, and help you find your keys faster. The idea extends to any context where agents, human or robotic, collaborate.

Technically, this is cross-view object correspondence. Given a mask track on a target object in one view, the goal is to predict its mask in the other.

It encapsulates two problems. First, matching the mask across views is hard, because viewpoint changes object size and shape. Second, temporal completeness is challenging, because over time cameras face motion blur, occlusions, and scene changes.

It gets even more interesting if we assume simple cameras with no geometric calibration, meaning we can only use visual information.

# Related Works
530 words.

## Related works title
Let me walk you through the theoretical foundations for solving cross-view object correspondence.

## Visual representation
Segmenting video means encoding images as vector embeddings.

Vision Transformers patch the image and apply query-key-value attention, producing a token that summarises it. However, transformers need large labelled datasets.

Self-supervised learning replaced labels with a pretext task, like predicting a masked patch. This gave rise to the DINO family, today's state of the art for image embeddings.

## Segmentation
Embeddings power segmentation, predicting a binary mask over an object. Mask R-CNN and RITM pioneered dense and promptable masks, but stayed narrow, limited by task-specific training on small datasets.

In 2023, Meta's FAIR lab introduced Segment Anything, the first general purpose promptable segmenter, trained on over a billion masks for zero-shot inference. SAM 2 extended this to video, and in November 2025 SAM 3 added concept tracking from short text prompts.

But short prompts alone cannot bridge two viewpoints. We need richer language.

## Language Grounding
Language grounding maps expressions to the pixels they describe. CLIP first aligned images and captions in a shared space via contrastive loss, and Grounding DINO added open-vocabulary detection, localizing any textual description.

SAM 3's detector builds on this, conditioning image features on the text prompt to propose segmentation candidates. Unfortunately, it caps prompts at 32 tokens.

## Foundation Models and Agents
We overcome this limit with Vision Language Models, large foundation models that reason simultaneously over images and text. Qwen is the strongest open source family; Qwen 3 adds prolonged reasoning and strong grounding.

VLMs become even more powerful inside agent loops: reasoning, acting through tools, and conditioning on the results. SAM 3 offers such an interface, letting a VLM segment from relational descriptions and even logical riddles.

## Cross-View Object Correspondence Related Methods title
But what have others done for cross-view object correspondence?

## Official baselines
The main dataset is Ego-Exo4D: roughly 4 million annotated frames, giving more than 700K cross-view paired masks. What makes it hard is the extreme viewpoint gap, since one video follows a person's eyes and the other a camera fixed in the room.

Its authors propose two baselines: XSegTx, adapting a co-segmentation model to the target object, and XView-XMem, adapting a video tracker to propagate masks across views. Being adaptations, both score poorly.

## 2025 challenge submission
Last year's Ego-Exo4D challenge saw two strong submissions.

ObjectRelator fuses visual and language features through a Multimodal Condition Fusion block, aligning views with a self-supervised XObjAlign loss.

O-MaMa instead reframes the task as mask matching: candidates from FastSAM, pooled DINOv2 features, cross-attention fusion, and a contrastive matching head. It won using only 1% of ObjectRelator's trainable parameters.

## Latest developments
In late 2025, two new methods pushed results higher still.

V²-SAM adds three experts atop SAM 2, anchoring correspondences from DINOv3 features.

LM-EEC is the strongest to date, adapting SAM 2 with a Memory-View Mixture-of-Experts fusion module and separate ego and exo memory banks. However, it fine-tunes SAM 2's full backbone, making it an in-distribution upper bound rather than a fair comparison.

## Common Limitations
Every method we saw trains a dedicated fusion component on labelled data, which has two implications:

1. The reasoning behind each correspondence is hidden in weights; beyond attention maps, there are no interpretable intermediate steps to inspect when the output is wrong.
2. Training on a single dataset arguably limits generalisation to other datasets and other camera configurations.

# Method
Max 1120 words.

## A new paradigm?
Looking for solutions to these limitations, one of our first ideas was to check what modern foundation models can already do. Over the last five years, they emerged as the new paradigm in machine learning, showing in both language and vision that training on broad data, without task-specific fine-tuning, can exceed specialist systems.

So I asked myself and my colleagues: assuming today's foundation models have acquired a sufficient understanding of the world, will composing them at inference time transfer to novel tasks?

And since language is their main interface, can language descriptions replace abstract learned features and act as a bridge between architectures?

These two questions became the language as a bridge hypothesis, the foundation of my thesis.

## Language as a bridge 
This paradigm mirrors how humans collaborate, communicating with language and reacting to each other's information. At the same time, it addresses both limitations we just saw:

1. Language is interpretable by design, letting a researcher inspect every stage of the pipeline and understand what went wrong.
2. Foundation models save us from training, which in principle makes the pipeline dataset and camera agnostic.

## "Two eyes and one mouth" pipeline
To this end, this thesis proposes the “two eyes and one mouth” pipeline, where two cameras observe the scene from different perspectives, and a single natural language description mediates the object correspondence between them.

## Some terminology 
Before the pipeline, let me clear up some terminology that will make everything after it easier to follow.

Every cross-view object correspondence method runs over a take, on a target object, in a specific direction. A take is a single scene recorded simultaneously by a pair of cameras, producing two synchronized videos.

The first video is the input. We call it source, because alongside the video we also have the masks of the object we are interested in.

The second is where we compute the output. We call it destination, because that is where our method must produce its predictions: segmentation masks for the target object.

Each prediction is a mask on a destination frame, indicating the exact pixels corresponding to the target object. Predictions need not follow the sequence of time; in other words, they can happen offline.

Using Ego-Exo4D, we have two directions: Ego2Exo, with egocentric as source and exocentric as destination, and Exo2Ego, with the roles swapped.

## Pipeline overview
The pipeline works in four stages.

First, it takes a source-view mask track and selects the single highest-quality seed frame.

Second, a vision language model generates a view- and time-independent description of the target object in that frame.

Third, that description feeds two coordinated processes: an open-vocabulary detector scores destination frames by object-text alignment to select the most informative anchor frames, and a SAM 3 agentic loop localises and segments the object in each anchor frame, using the same description as grounding signal.

Fourth, SAM 2's video tracker completes the mask track across the remaining destination frames, propagating predictions conditioned on a memory bank of anchor and recently processed frames.

Crucially, no component is fine-tuned for the task. And because the bridge between views is a human-readable JSON description, any erroneous prediction can be localised to the stage whose output is at fault.

Let's look at each stage in detail.

## Stage 1 


# Experiments
840 words.

# Conclusions
140 words. 

This was part of a research project with prof Plizzari and two other MSc students, and is on track for submission to the Winter Conference on Applications of Computer Vision 2027 conference.