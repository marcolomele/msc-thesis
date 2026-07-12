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
Max 1050 words.

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
Stage one selects the source frame. For every annotated frame tau, we compute a visibility score, s of tau, from two cues read off the ground-truth mask. First, a of tau, the normalised mask area, the fraction of pixels the object occupies. Second, c of tau, its centrality, one minus the normalised distance from the mask centroid to the frame centre.

Area weighs 0.99 and centrality 0.01, because a large object gives the vision language model far more to describe than a small but perfectly centred one. The top K frames form the seed set S, and we set K to one.

## Stage 2
Stage two writes the description. We use Qwen 3.5 to reads the seed frame and return a JSON object W, the object description.

It must satisfy two properties: view independence, so that it holds from the radically different destination viewpoint, and time independence, so that it holds at every destination frame, not just the seed's moment. We enforce both by asking only for intrinsic attributes, namely colour, canonical identity, material, and structural parts, which change neither with the camera nor with time.

We pass two images: the frame with the mask overlaid in red, which tells the VLM which object to describe, and the raw frame, which preserves its true appearance as well as scene context. 

## Stage 3.1
Stage three splits into where to look, and what is there.

For where, Grounding DINO runs open-vocabulary detection on every destination frame with the description w as the query. A frame scores highly when the detector localises the described object with high confidence, and the top three form the anchor set A.

The bridge stays linguistic here, with no geometry and no calibration. And this re-selection matters, because the destination frame synchronised with the seed is rarely the best place to segment: the correlation between the two views' visibility is only 0.14.

## Stage 3.2
For what is there, each anchor is segmented independently by a SAM 3 agent loop, orchestrated by the same VLM.

The agent first simplifies the description into a short noun phrase, because contrastive alignment pools a sentence into one embedding, so a paragraph of attributes dilutes the match. SAM 3 then segments every instance matching that phrase, and the agent inspects each candidate mask against the original description, accepting or rejecting it. If all are rejected, the loop restarts with a different phrase. The accepted masks are our anchor masks on A.

## Stage 4
Stage four completes the track in time. SAM 2's video tracker propagates the anchor masks across the remaining destination frames, conditioning each prediction on a memory bank that holds all the anchors and the recently processed frames. This matterls, particularly in egocentric video, where motion blur and occlusion often make the previous frame a poor reference, so attention reaches back to a distant anchor instead.

We propagate bidirectionally, forward from the earliest anchor and backward from the latest, keeping for each frame the prediction from the pass arriving from its nearest anchor.

# Experiments
851 words.

## Evaluating the pipeline
Is this approach any good?

## Quantitative results
Let's start with the quantitative results. On Intersection over Union we reach 37.7 Ego2Exo and 40.6 Exo2Ego, surpassing both official baselines and the ObjectRelator submission, and trailing O-MaMa by about ten percent. The gap widens against the most recent works.

Location error is lower than the baseline, so we recover the correct object region but place it with coarser spatial precision than our competitors.

The results are moderate, but foundation models never trained together on this task recover significant quality, with the remaining gap explained by direct supervision on Ego-Exo4D. That is our first evidence that language as a bridge works.

## Qualitative results
Looking at our successes, the masks themselves are of high quality. So how can the gap to the state of the art be so large?

## Dead mass
In my opinion, most of the responsibility lies with what I call dead mass.

Plotting the distribution of our predictions against the state of the art, roughly a third of all cases, 32% Ego2Exo and 31% Exo2Ego, score essentially zero IoU. The cases that survive score 56.9 and 61.7, already level with the state of the art. So the deficit is not uniform sloppiness. Remove the dead mass, and our mean IoU rises by about eighteen points, up to LM-EEC.

I then looked for the variable that explained it. Scenario and object size hinted at small, fine-grained objects, but the one that truly separated the buckets was anchor IoU.

And that exposes the main weakness of the pipeline. Everything hinges on the bridge. If we land on the wrong object, that wrong mask propagates faithfully across the entire take.

## Naming issue
So where is the dead mass born? To find out, we walk the pipeline in order and charge every dead case to the first stage that breaks.

The answer is stark. Stage two, the naming itself, is the single largest cause by a wide margin: 59% of the dead mass in Ego2Exo, and 81% in Exo2Ego. Anchor selection in stage three is next, and the two together explain 74% and 92% of all dead cases. Grounding DINO boxing the wrong object, and the agent segmenting the wrong thing inside a good box, are minor by comparison.

And propagation? Zero, by construction. The loss is born in cross-view transfer, not in tracking, exactly the hard sub-problem we flagged at the start.

## Perception issue
So why does the VLM mislabel? On the dead cases, the source mask we hand it is three to six times smaller than on the successful ones, and in Exo2Ego half the failures show the object at under 0.1% of the frame.

And when it misnames, it names a real, larger neighbour. In 38% of Ego2Exo failures, that neighbour is itself annotated in the same take. It never hallucinates an absent object. It honestly describes the container, the tool, or the surface the tiny target is resting on, and true vocabulary near misses are a small minority.

That diagnosis rules out the cheap explanations. It is not a weak prompt, and it is not a poor vocabulary. It is perception. The model cannot see a small enough object well enough to name it.

## Diagnosis tests
We tested this diagnosis with three interventions, and I present them as method, not as misses.

Conditioning the description on a scene vocabulary gave 1.1 points, a gain that did not survive the larger subset. Cropping the destination around the Grounding DINO box cost 4.4 points, and brightening the frame cost 3.7.

The crop is the informative one. Stratifying by frame, the hint rescues frames we were already failing, but corrupts the many we already handled, and that larger population drives the mean negative. Cropping the source is worse still: a tight crop halves IoU, from 41.1 to 18.0, because the identity of a small object is carried by its surroundings.

So the negatives kill the cheap fixes and isolate the real constraint. The fix must use the VLM better, not pre-process the image.

## Ablations
So we know where the pipeline breaks and why. The question now is where to spend our next effort, and for that we need to see what each block actually contributes. We add the stages one at a time.

The naive baseline, describing every source frame and running SAM 3 once per destination frame, scores 10.6. Replacing our description with the ground-truth object name lifts it to 17.0, which again isolates naming as the bottleneck. Our frame selection reaches 16.8, matching that oracle with no privileged information.

The SAM 3 agent on every frame scores 35.5, but at 21 seconds per frame, 59 hours for the run. Propagation matches it, 37.7, at 0.82 seconds per frame, 25 times faster, and Grounding DINO anchors close at 38.1.

That is 260% over the baseline at a third of the time, so the split was the right call. But read the same table as a map of headroom, and it tells us where to go next: selection and propagation are already at their ceiling, while description is the one block that an oracle beats. That is where the remaining points are, and where the future work goes.

# Conclusions
140 words. 

This was part of a research project with prof Plizzari and two other MSc students, and is on track for submission to the Winter Conference on Applications of Computer Vision 2027 conference.