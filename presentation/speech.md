# Introduction

## Title
Dear friends, family, Degree Commission, and thesis supervisor: good afternoon.

My name is Marco, and today I will present my MSc thesis.

## Vision as the next interface
Language has become our primary interface with technology, and today each of us texts a large language model for many different tasks. I believe this integration of technology and daily life will continue, and that the next step is vision, in the form of smart glasses, which are already on the consumer market.

## Augmented vision
Smart glasses, combined with the cameras in your house, could give you augmented vision, and help you find your lost keys in the house faster. The idea extends to any context where agents, human or robotic, collaborate.

## The task and dataset
Technically, this is cross-view object correspondence. Given a mask track on a target object in one video, the goal is to predict its mask in a second video of the same scene, evaluated using intersection over union.

The main dataset is Ego-Exo4D, which pairs an egocentric head-mounted camera with a fixed exocentric camera, giving roughly 4 million annotated frames across 7 scenarios like cooking and playing basketball.

## Challenges
This camera pair highlights three challenges: strong changes in perspective, temporal completeness, and reliance on video only.

## Previous methods
Methods for cross view correspondence have already been publihed.

## Common Limitations
However, they all face the same limitations.

One, the produce not interpretable predicitons, because their architectures are built on abstract visual embeddings.

Second, they all train fusion modules on Ego-Exo4D data.

## Language as bridge
To solve these limitations, this thesis introduces the language as a bridge paradigm: using language to connect foundation models and mediate information across two cameras.

This paradigm mirrors how humans collaborate, communicating with language and reacting to each other's information. 

## Limitations solved
At the same time, it addresses both limitations we just saw:

1. Language is interpretable by design, letting a researcher inspect every stage of the pipeline.
2. Foundation models save us from training, which in principle makes the pipeline dataset and camera agnostic.

## "Two eyes and one mouth" pipeline
This thesis proposes the “two eyes and one mouth” pipeline, where two cameras observe the scene from different perspectives, and a single natural language description mediates the object correspondence between them.

## Some terminology 
Every cross-view object correspondence method runs over a take, on a target object, in a specific direction.

A take is a single scene recorded simultaneously by a pair of cameras, producing two synchronized videos.

The input is the first video and the masks of the objects we are interested in. We call this source.

The output is the the predicted segmentation masks on the frames of the second video. We call it destination. 

Using Ego-Exo4D, we have two directions: Ego2Exo, with egocentric as source and exocentric as destination, and Exo2Ego, with the roles swapped.

## Pipeline overview
The pipeline works in four stages.

First, we select the frame from the source video where the target object is most visible. 

Then, we generate a textual description of the object with a Vision Language Model. 

Third, we guess the frames in the destination video where the object is most visible and run its segmentation, creating anchor masks.

Finally, we propagate the anchor masks to the remaining frames of the destination video. 

## Assumptions
Four assumptions carry this design.

First, language captures enough detail to separate the target from clutter. 

Second, the foundation models we compose interpret that language consistently, so the same phrase points to the same concept across their backbones. 

Third, a handful of reliable anchors in the destination view is enough for a tracker to complete the rest. 

Fourth, because the bridge is a written description rather than a live signal, prediction can happen offline.

Let's look at each stage in detail, connecting to the related works powering the pipeline. 

## Stage 1
In stage one the pipeline selects the source frame. 

For every annotated frame, we compute a visibility score of the target object using the input mask track.

The most visible frame is that where the mask area is the largest and where the object is most centered. 

This design rests on a simple principle: the more of the frame the object fills, the more accurate the generated description. 

## Stage 2
Stage two writes the description with a vision language model. We leverage Qwen 3.5 and pass to it the frame with the mask overlay and the raw frame, which tells the VLM which object to describe and preserves scene context. 

The output is a JSON object description satisfying two properties: view and time independence. 

Thus, we ask Qwen to describe only intrinsic attributes, namely colour, canonical identity, material, and structural parts, which change neither with the camera perspective, nor with the time of the video.

## Vision Language Models
Qwen 3.5 uses a Vision Transformer as image encoder. 

That's why we pass the frame in the source video where the target object is the largest: 

the object covers more patch tokens, which in turn claim a larger weight of the resulting image embedding, transferring more signal for the VLM to generate a more accurate description. 

## Stage 3.1
With the description in hand, stage 3 uses Grounding DINO, to run detection on every destination frame with the description W as the query.

Grounding DINO also outputs a score representing its confidence in the content of the detection matching the text query. 

We use this as a proxy and keep the top three highest-confidence boxes as the anchor set A.

This re-selection matters, because the destination frame synchronised with the seed is rarely the best place to segment. 

## Examples
As you can see in the examples, we may face issues of objects being out of frame, size differences, and occlusions. 

## Stage 3.2
Each anchor is segmented independently by a Segment Anything Model (SAM) 3 agent loop, orchestrated by Qwen 3.5.

The agent first simplifies the description into a short noun phrase, maching the input SAM 3 has been trained for. 

Then, SAM 3 then every instance matching that phrase. 

Finally, the agent inspects each candidate mask against the original description, accepting or rejecting it. 

The accepted masks are our anchor masks set.

## Segmentation
SAM is the state of the art segmentation model. It takes as input visual prompts like points and bounding boxes. 

SAM 2 extends this to video with a tracker, and SAM 3 enables segmentation from text prompts, which stage 3 uses to turn the description into anchor masks. 

## Stage 4
Stage four completes the track in time. 

It leverages SAM 2's video tracker to propagate anchor masks across the remaining destination frames.

It uses a memory bank that conditions with attention to all anchor frames and the 6 most recently processed frames. This makes it very robust against viewpoint change. 

## Example
The result is a sequence of frames with a predicted mask track. 

# Experiments
851 words.

## Results
How does this approach fair?

We begin by looking at previously published methods, which all work with abstract feature representations and train specific fusion modules on Ego-Exo4D. 

## Official Baselines
Along with the dataset, Ego-Exo4D's authors propose two baselines, XSegTx and XView-XMem, which score relatively poorly since they are adaptations of single-view models. 

## 2025 challenge submission
Next, last year's Ego-Exo4D challenge saw two strong submissions, ObjectRelator and O-MaMa, with the latter winning.

## Latest developments
In late 2025, two new methods pushed results higher still: V²-SAM and LM-EEC. 

However, LM-EEC fine-tunes SAM 2's full backbone, making it an in-distribution upper bound rather than a fair comparison.

## 2 eyes and one mouth pipeline
On Intersection over Union we reach 37.7 Ego2Exo and 40.6 Exo2Ego, surpassing the official baselines and the ObjectRelator, and trailing O-MaMa by about ten percent.

The results are moderate, but are a first evidence that language as a bridge works. 

Foundation models never trained together on this task are able to recover significant quality against the trained models, and the remaining gap is likely explained by training and fine-tuning on Ego-Exo4D.

## Qualitative results
Let's have a look at some successesfull cross-view object segmentations of our pipeline.

As you can see, the masks are very high quality, driven by the capabilities of SAM 3.  

So how can the gap to the state of the art be so large?

## Gap analysis
To find the source of this discrepancy, I ran an head-to-head analysis with LM-EEC on the results. 

Plotting the distribution of our predictions against the state of the art, the issue become evident: on roughly a third of all cases, our pipeline scores zero IoU. 

I call this the dead mass, because it drags down the results of our pipeline.

## Removing the dead mass
In fact, when we remove the dead mass, our mean IoU rises significantly, with an average score of 61.7 in Exo2Ego. 

This is LM-EEC's terrain, the absolute state of the art that did a massive fine-tuning of SAM 2 and representing our in-distribution upper bound. 

This means that there are specific failure points, and the pipeline is not uniformly weak.

## Source of the dead mass
I continued the gap analysis against LM-EEC by setting a 50 IoU success threshold and creating 4 groups.

Then I looked for the variables that best separated them.

The defining factor explaining the gap is evident: the quality of the mask immediately after the bridge. 

For the cases where our pipeline produces a low quality achor mask, the gap to the state of the art grows the largest.

## Naming issue
It turns out that the main contribuent causing poor anchor masks is the VLM naming a different object than the target.

This issue accounted for 59% of the dead mass in Ego2Exo, and 81% in Exo2Ego. 

Interestingly, rather then hallucinating, the VLM names a real larger neighbour. 

The following question thus is: why does the VLM mislabel an object? 

## Perception issue
The answer is the size of the source mask. 

On the dead cases, the selected source mask is three to six times smaller than on the successful cases. In Exo2Ego, half the failures show the object at under 0.1% of the frame.

## Ablations
To find which component deserves the most attention for future improvements, I ran ablations starting from a naive baseline: describe every source frame and run SAM 3 one shot per destination frame, scoring 10.6 IoU.

The final configuration increases prediction quality by 260% over that baseline, and runs faster.

The ablations point to description generation and segmentation as the components where future efforts should go.

## Recent developments
In fact, ever since submitting my thesis, there have been more efforts. With the help of Filippo Dario Paolucci, and Giovanni Mantovani, two MSc students, this pipeline received several updates. 

Currently, it sits at 41.5 on Ego2Exo and 48.1 on Exo2Ego, a few percentage points shy than the comparable state of the art. 

## SOTA Benchmark
Additionally, it also achieves state of the art on HANDAL-X, another cross-view benchmark. 

# Conclusion
In conclusion, what did we learn from these experiments? can language be used to connect foundation models and mediate information across two cameras? Yes, partially.

On the one hand, two limitations remain. 

At the design level, running foundation models at inference needs real hardware: we ran everything on an NVIDIA H200 with more than 80GB of VRAM, which won't be accessible to everyone. 

At the assumption level, language detail and reliable anchors still need work: as we saw in the naming issue, the VLM's description fails when grid tokenization starves it of signal on small objects, and a single wrong anchor propagates its error across the whole take.

On the other hand, our two remaining assumptions, consistent interpretation and offline prediction, held: language connected foundation models that were never trained together, recovering significant quality on a novel task none of them had seen, without any additional training.

But is language truly necessary?

## A closing thought
A development in the field pushes intelligent agents to reason over rich abstract embeddings rather than natural language.

But such embeddings are impossible to interpret unless decoded, which raises a human question: will we become supervisors, or be excluded from certain tasks entirely?

I don't think this is intrinsically bad: we moved from building engines by hand to supervising chains of robots building cars, and airplane pilots now supervise autopilot, intervening only on exceptions. 

## question
So let me leave you with a better question: in a world where intelligent agents can do anything, which tasks do you want to be automated, and which instead do you want to keep for yourself, just because you enjoy doing them?

## end
Thank you for listening!