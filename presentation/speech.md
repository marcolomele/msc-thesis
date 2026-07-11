# Introduction 
280 words.

## Title
What is the strongest compression medium humans have known for the last 1000 years? It's language.

My name is Marco, and today, I will present my MSc thesis. 

This work was developed in colaboration with prof Chiara Plizzari and the help of two other MSc students. I'm proud to say that it is on track to be submitted to WACV 2027. 

## Cross-view object correspondence
I believe in a future of increased integration between our daily lives and technology. One of these technologies will be vision, in the form smart glasses, a technology already available to consumers. 

Have you ever found yourself in a situation where you are looking for your keys at home? 

Smart glasses could help solve that. Combined with the cameras in your house, you could have many synthetic eyes.  

Technically, this task is called object recognition. We will focus on a specific form of this task called cross-view object correspondence. 

Cross-view object correspondence aims at matching objects across synchonized video recordings of the same scene. Given a video from one perspective with a mask track on a target object, the goal is to predict the mask of the object in the other perspective. 

We will work with Ego-Exo4D dataset, the larget public resource for this cross view object correspondence. 

The task is particularly challenging because it encapsulates two problems in one. The first is matching the mask of an object across the two videos, challenging due to viewpoint changes across ego and exocentric, which affect object size and shape. The second is temporal completeness in the video, diffiuclt due to motion blur, temporary occlusions, and scene changes. 

Additional restriction: use only visual information; no geometric calibration between cameras is available. 


# Related Works
420 words.


## Related works title
To address cross-view object correspondence, we first need some theoretical foundations. In the next minutes, I will walk you through the related works and highlight their relevance to our method. 

## Visual representation
In order to segment objects across videos we need to segment objects in individual frames, which effectively are images, and in order to segment objects in images we need to encode them numerically. In machine learning terms, this translates to mapping images to vector embeddings.

For this we use Vision Transformers. They divide the image into patches, then use query-key-value attention to create an embedding for each patch that aggregates information across patches and produces a classification token that summarises the information contained in the image. However, transformers require relatievly large amounts of labelled data to be trained, which is expensive.

To this end, researches have developed self-supervised learning, which replaces labels with a pretext task, such as predicting a masked patch of the image. This approach introduces inductive bias that lets the model learn autonomously how to distinguish relevant objects from noisy background, and has given current state of the art image embedders DINO family.

## Segmentation
Image embeddings empwer several machine learning tasks. We focus on segmentation, which involves predicting a binary mask over the image that indicates which pixels belong to a
specific object. 

Early methods like Mask R-CNN focused on ... . However, these models segment without any guidance, which renders them useless given that we want to segment sepcifically in one camera vide conditioned on the information from the other view. 

The Segment Anything Model solves this gap by introducing prompatable segmentation. 

## Language Grounding

## Foundation Models and Agents
The last piece of the puzzle ... 

# Method
1120 words.

# Experiments
840 words.

# Conclusions
140 words. 