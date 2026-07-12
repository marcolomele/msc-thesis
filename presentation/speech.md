# Introduction 
218 words.

## Title
Dear friends, family, Degree Assessment Board, and thesis supervisor; good afteroon. 

My name is Marco, and today I will present my MSc thesis. 

For this presentation, I would like you to think about this question: 

## question
is language the most effective medium that humans have invented for communicating information?

## Language and technology
I ask you this because today language has become the prominent interface with technology. Raise your hand if you have prompted a large language model in the last 24 hours. 

I believe that this integration with technology and daily life will continue and the next aspect will be vision, in the form of smart glasses, already on the market.

## Cross-view object correspondence
This is because smart glasses combined with the cameras in your house could give you many eyes than the have you have currently, and help you find your keys at home faster. This ideas extends to any context where many agents, whether human or robotic, collaborate.

Technically, this task is called cross-view object correspondence. Given a mask track on a target object in one view, the goal is to predict its mask in the other.

The task encapsulates two problems. First, matching the object's mask across views is hard because viewpoint changes object size and shape. Second, temporal completeness, challenging because over time cameras have motion blur, occlusions, and scene changes.

The problem becomes even more interesting if we assume simple cameras that have no geometric calibration, meaning that we can only use visual information.

# Related Works
440 words.

## Related works title
Let me walk you thorugh the theoretical foundations to solve cross view object correspondence.

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
But what have others done for cross-view object correspondence?

## Official baselines
To study this task, researchers use Ego-Exo4D. It is the main dataset for studying ego-exo videos understanding, because it contains about 4 million annotated frames, resulting in more than 700K cross-view paired masks. 

The authors also propose two baselines: XSegTx, adapting a co-segmentation model to the target object, and XView-XMem, adapting a video tracker to propagate masks across views. Since these are adaptation, both score low.

## 2025 challenge submission
Last year Ego-Exo4D's challenge saw two strong submissions. 

The first is ObjectRelator, which fuses visual and language features through a Multimodal Condition Fusion block, aligning views with a self-supervised XObjAlign loss. 

The second is O-MaMa, which reframes the task as mask matching: candidates from FastSAM, pooled DINOv2 features, cross-attention fusion, and a contrastive matching head. O-MaMa won using only 1% of ObjectRelator's trainable parameters.

## Latest developments
In late 2025, two new methods were proposed that achieve even higher results.

The first is V²-SAM, which adds three experts atop SAM 2, anchoring correspondences from DINOv3 features.

The second is LM-EEC, the strongest up to date. It adapts SAM 2 with a Memory-View Mixture-of-Experts fusion module and separate ego and exo memory banks, reaching state of the art. However, it also fine-tunes SAM 2's full backbone, making it an in-distrubtion Upper bound rather than a fair comparison to the previous methods.

## Common Limitations
So far, every method we saw trains a dedicated fusion component on labelled data. This has two implicatios:

1. The reasoning behind each mask correspondence is hidden in weights and complex architectures; there are no interpretable intermediate steps to inspect for wrong outputs other than attention maps. 
2. Arguably, training on a single datasets limits the generalisability to other datasets, as well as other camera configurations. 

# Method
1120 words.

## A new paradigm?
When I started researching solutions to these limitations, One of the first ideas was to check the capabilities of modern foundation models. Over the last five years, foundation models emerged as the new paradigm in machine learning. In both language and vision, Foundation models have demonstrated that training on broad data and without task-specific fine-tuning can exceed specialist systems. 

I asked myself and my colleagues: assuming today's foundation have acquired a sufficiently understanding of the world, will composing them at inference time transfer to tasks none was designed for? 

And since language is the main interface for these foundation models, can language descriptions serve as a bridge between machine learning architectures that replaces complex learned features? 

These two questions became the foundations for the language as a bridge hypothesis, the foundation of my thesis work. 

## Language as a bridge 
The language as a bridge paradigm mirrors how humans collaborate, communicating with language and reacting to other's information. At the same time, it solves both limitations from previous models in cross-view object correspondence:

1. language is interpretable by design, which allows a researcher to inspect every stage of the pipeline and understand what went wrong.
2. Using foundation models saves us from having to train which in principle makes our pipeline dataset and camera agnostic.

## "Two eyes and one mouth" pipeline
To this end, this thesis proposes the “two eyes and one mouth” pipeline, where two cameras observe the scene from complementary perspectives, and a single natural language description mediates the object correspondence between them.



# Experiments
840 words.

# Conclusions
140 words. 

This was part of a research project with prof Plizzari and two other MSc students, and is on track for submission to the Winter Conference on Applications of Computer Vision 2027 conference.