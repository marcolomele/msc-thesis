# Introduction 
280 words.

## Title
What is the strongest compression medium humans have known for the last 1000 years? It's language.

My name is Marco, and today, I will present my MSc thesis. 

This work was developed in colaboration with prof Chiara Plizzari and the help of two other MSc students. I'm proud to say that it is on track to be submitted to WACV 2027. 

## Cross-view object correspondence
I believe in a future of increased integration between our daily lives and technology. One of these technologies will be vision, in the form smart glasses, a technology already available to consumers. 

Have you ever found yourself in a situation where you are looking for your keys at home? 

Smart glasses could help solve that. Combined with the cameras in your house, you could have many eyes. This finds applications in several fields where agents, whether humans or robots, collaborate together. 

Technically, this task is called object recognition. We will focus on a specific form of this task called cross-view object correspondence. 

Cross-view object correspondence aims at matching objects across synchonized video recordings of the same scene. Given a video from one perspective with a mask track on a target object, the goal is to predict the mask of the same object in the other perspective. 

We will work with Ego-Exo4D dataset, the larget public resource for this cross view object correspondence. 

The task is particularly challenging because it encapsulates two problems in one. The first is matching the mask of an object across the two videos, challenging due to viewpoint changes across ego and exocentric, which affect object size and shape. The second is temporal completeness in the video, diffiuclt due to motion blur, temporary occlusions, and scene changes. 

We impose an additional restriction which complicates: we can only use visual information, since no geometric calibration between cameras is available. 


# Related Works
420 words.

## Related works title
To address cross-view object correspondence, we first need some theoretical foundations. In the next minutes, I will walk you through the related works and highlight their relevance to our method. 

## Visual representation
In order to segment objects in videos, which are just sequences of images, we need to encode them numerically. In machine learning terms, this translates to mapping images to vector embeddings.

For this we use Vision Transformers. They divide the image into patches, then use query-key-value attention to create an embedding for each patch that aggregates information across patches and produces a classification token that summarises the information contained in the image. However, transformers require relatievly large amounts of labelled data to be trained, which is expensive.

To this end, researches have developed self-supervised learning, which replaces labels with a pretext task, such as predicting a masked patch of the image. This approach allows models to autonomously learn how to distinguish relevant objects from noisy background, and has lead to the DINO family, which is the current state of the art for creating general purpose image embeddings.

## Segmentation
Image embeddings empower several machine learning tasks. We focus on segmentation, which involves predicting a binary mask over the image that indicates which pixels belong to a
specific object. 

Early methods like Mask R-CNN focused on predicting dense per-pixel predictions, and some methods like RITM, also allowed for promptable segmentation with points and bounding boxes. However, they were all trained on relatvively small datasets and were desiged for specific settings, making their applicability narrow and limited. 

In 2023, Meta's FAIR lab introduced the Segment Anything Model as the first general purpose, promptable foundational model for image segmentaion. They they SAM on a dataset of 1 billion images and achieve zero-shot inference on unseen prompts and images. SAM 2 followed with an extention that allowed promptable segmentation on video. In november 2025, the latest iteration SAM 3 was released, which enabled advanced segmentation that tracked instances of concepts on videos via short textual prompts.

Simple prompts like short text and points are not powerful enough for object segmentation across two videos. We need a more expressive bridge. We combine two directions. 

## Language Grounding

The first is language grounding, which aims at mapping natural language expressions to the image pixels that show what the expressions names. 

CLIP is the first effort of this kind, producing via contrastive loss a shared vector representation in which an image and a captions describing it land close together. Later, Grounding DINO enriches this field by introducing open-vocabulary detection, the ability to localise an object in an image from any textual description. 

SAM 3 lands in this area as well. The most interesting contribution is its detector, which uses several modules to procude image representations informed by the text that then are used to propose segmentation candidates. However, the authors limit the textual prompts to 32 tokens with the architecture. 

## Foundation Models and Agents
We overcome this limit Vision Language Models, the last piece of the puzzle of our pipeline. These are general purpose large foundations models that can reason over images and text simultaneously. 

## Cross-View Object Correspondence Related Methods
So now that we have theoretical foundations, let's have a look at what other people have done to solve cross-view object correspondence.



# Method
1120 words.

# Experiments
840 words.

# Conclusions
140 words. 