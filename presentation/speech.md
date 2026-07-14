# Introduction 
219 words.

## Title
Dear friends, family, Degree Assessment Board, and thesis supervisor: good afternoon.

My name is Marco, and today I will present my MSc thesis. I would like to start with a question.

## question
Is language the most effective medium humans have invented for communicating information?

## Language and technology
I ask because language has become our prominent interface with technology. Raise your hand if you have prompted a large language model in the last 24 hours.

I believe this integration of technology and daily life will continue, and that the next step is vision in the form of smart glasses, which are already on the consumer market.

## Cross-view object correspondence
Smart glasses, combined with the cameras in your house, could give you many more eyes than you have today, and help you find your keys faster. The idea extends to any context where agents, human or robotic, collaborate.

Technically, this is cross-view object correspondence. Given a mask track on a target object in one video, the goal is to predict its mask in a second video of the same scene.

The task's proving ground is Ego-Exo4D, pairing a head-mounted camera with one fixed in the room. That pairing produces the strongest viewpoint change of any cross-view benchmark: the same object can fill the frame from one eye level and shrink to a handful of pixels from the other.

This exacerbates the two problems baked into cross-view correspondence itself. First, matching the mask across views is hard, because viewpoint changes object size and shape. Second, temporal completeness is challenging, because over time cameras face motion blur, occlusions, and scene changes.

It gets even more interesting if we assume simple cameras with no geometric calibration, meaning we can only use visual information.

# Related Works
530 words.

## Related works title
Let me walk you through the theoretical foundations for solving cross-view object correspondence.

## Visual representation
Segmenting video starts with encoding images as vector embeddings. Vision Transformers patch and attend over the image to produce a summarising token, and self-supervised pretext tasks free this from labelled data, giving rise to the DINO family, today's state of the art.

## Segmentation
Embeddings power segmentation, predicting a binary mask over an object. Early work, Mask R-CNN and RITM, stayed narrow, trained on small task-specific datasets, until Segment Anything, in 2023, trained on a billion masks for zero-shot promptable segmentation. SAM 2 extended this to video, and SAM 3, in 2025, added concept tracking from short text prompts.

But short prompts alone cannot bridge two viewpoints. We need richer language.

## Language Grounding
Language grounding maps expressions to the pixels they describe. CLIP first aligned images and captions in a shared space via contrastive loss, and Grounding DINO added open-vocabulary detection, localizing any textual description.

SAM 3's detector builds on this, conditioning image features on the text prompt to propose segmentation candidates. Unfortunately, it caps prompts at 32 tokens.

## Foundation Models and Agents
We overcome this limit with Vision Language Models, large foundation models that reason jointly over images and text. Qwen is the strongest open source family; Qwen 3 adds prolonged reasoning and stronger grounding.

VLMs become even more powerful inside agent loops: reasoning, acting through tools, and conditioning on the results. SAM 3 offers such an interface, letting a VLM segment from relational descriptions and even logical riddles.

## Cross-View Object Correspondence Related Methods title
But what have others done for cross-view object correspondence?

## Official baselines
Ego-Exo4D provides roughly 4 million annotated frames, giving more than 700K cross-view paired masks.

Its authors propose two baselines: XSegTx, conditioning a co-segmentation model on the query mask, and XView-XMem, treating the second view as the next tracked frame. Being adaptations, both score poorly.

## 2025 challenge submission
Last year's Ego-Exo4D challenge saw two strong submissions.

ObjectRelator fuses visual and language features through cross-attention, conditioning visual mask features on a language category name, backed by a self-supervised loss that pulls matching ego and exo embeddings together. Language here is auxiliary; the correspondence itself lives in the trained visual embedding space.

O-MaMa instead reframes the task as mask matching. FastSAM proposes candidate masks in the destination view, an encoder pools DINOv2 features over each into a fixed descriptor, cross-attention lets the source and destination descriptors inform each other, and a contrastive head pulls the true pair together while pushing away hard negatives, masks spatially adjacent to the target. It won using only 1% of ObjectRelator's trainable parameters.

## Latest developments
In late 2025, two new methods pushed results higher still.

V²-SAM adds three experts atop a shared SAM 2 mask decoder. One matches DINOv3 patch features across views by cosine similarity, turning confident matches into a geometric prompt. A second reconstructs the target's shape from geometric priors through two small mapping networks, yielding an appearance-guided prompt. A third fuses both, and a consistency check at inference picks whichever prompt survives being applied both ways.

LM-EEC is the strongest to date, adapting SAM 2 with a two-branch mixture of experts, one reweighing channels via pooling and small MLPs, the other reweighing space via convolutions, plus separate ego and exo memory banks. However, it fine-tunes SAM 2's full backbone, making it an in-distribution upper bound rather than a fair comparison.

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

First, source video and mask track in, highest-quality seed frame out.

Second, seed frame in, object description out, via the vision language model.

Third, description in, anchor frames and anchor masks out, via an open-vocabulary detector and a SAM 3 agentic loop.

Fourth, anchor masks in, completed destination track out, via SAM 2's video tracker.

Crucially, no component is fine-tuned for the task. And because the bridge between views is a human-readable JSON description, any erroneous prediction can be localised to the stage whose output is at fault.

Four assumptions carry this design. First, language captures enough detail to separate the target from clutter. Second, the foundation models we compose interpret that language consistently, so the same phrase points to the same concept across their backbones. Third, a handful of reliable anchors in the destination view is enough for a tracker to complete the rest. Fourth, because the bridge is a written description rather than a live signal, prediction can happen offline.

Let's look at each stage in detail.

## Stage 1
Stage one selects the source frame. For every annotated frame tau, we compute a visibility score, s of tau, from two cues read off the ground-truth mask. First, a of tau, the normalised mask area, the fraction of pixels the object occupies. Second, c of tau, its centrality, one minus the normalised distance from the mask centroid to the frame centre.

Area weighs 0.99 and centrality 0.01, because a large object gives the vision language model far more to describe than a small but perfectly centred one. The top K frames form the seed set S, and we set K to one.

This rests on an assumption we'll come back to: the more of the frame the object fills, the better the model can describe it.

## Stage 2
Stage two writes the description. We use Qwen 3.5 to reads the seed frame and return a JSON object W, the object description.

It must satisfy two properties: view independence, so that it holds from the radically different destination viewpoint, and time independence, so that it holds at every destination frame, not just the seed's moment. We enforce both by asking only for intrinsic attributes, namely colour, canonical identity, material, and structural parts, which change neither with the camera nor with time.

We pass two images: the frame with the mask overlaid in red, which tells the VLM which object to describe, and the raw frame, which preserves its true appearance as well as scene context. 

We settled on this pairing by ablation: raw frame plus mask overlay reached 41.2 IoU, adding a crop on top changed nothing at 41.1, but a tight crop alone collapsed to 18.0, since a small object's identity lives in its surroundings, not just its pixels.

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
How does this approach fair?

## Quantitative results
Let's start with the quantitative results. On Intersection over Union we reach 37.7 Ego2Exo and 40.6 Exo2Ego, surpassing both official baselines and the ObjectRelator submission, and trailing O-MaMa by about ten percent. The gap widens against the most recent works.

Location error is lower than the baseline, so the pipeline recovers the correct object region but places it with coarser spatial precision than our competitors.

The results are moderate, but are a first evidence that language as a bridge works. That's because foundation models never trained together on this task recover significant quality, and the remaining gap is explained by direct training on Ego-Exo4D.

## Qualitative results
Looking at our successes, the masks themselves are of high quality. 

So how can the gap to the state of the art be so large?

## Dead mass
The main symptom is what I call dead mass.

Plotting the distribution of our predictions against the state of the art, roughly a third of all cases, 32% Ego2Exo and 31% Exo2Ego, score zero IoU. Remove the dead mass, and our mean IoU rises by about eighteen points, up to LM-EEC's terrain, with an average score of 56.9 and 61.7. 

This means that there are specific failure points, and the pipeline is not uniformly weak.

I then looked grouped results into 4 categories, compared directly with LM-EEC, and searched the variables in the data that could the gap. 

First, it appeared that scenarios with small objects were challenging. But then, object size showed medium objects to be the dominant weakness wrt LM-EEC. 

The aspect that best separated the buckets was anchor IoU.

This exposes the main weakness of the pipeline. Everything hinges on the bridge. If we land on the wrong object, that wrong mask propagates faithfully across the entire take.

## Naming issue
So where is the dead mass born? To find out, I checked the intermediate results for all runs, and charge every dead case to the first stage of the pipeline that breaks.

The finding was clear. The second stage of description generation is the single largest cause: 59% of the dead mass in Ego2Exo, and 81% in Exo2Ego. Anchor selection in stage three is next, and the two together explain 74% and 92% of all dead cases. 

For comparison, Grounding DINO boxing the wrong object, and the agent segmenting the wrong thing inside a good box are minor problems.

Crucially, zero cases from the dead mass group originated at propagation.

## Perception issue
The following question thus is: why does the VLM mislabel an object? 

The answer is the size of the source mask. On the dead cases, the selected source mask is three to six times smaller than on the successful cases. In Exo2Ego, half the failures show the object at under 0.1% of the frame.

Interestingly, rather then hallucinating, the VLM names a real larger neighbour. It honestly describes the container, the tool, or the surface the small target object is resting on. In comparison, true vocabulary near misses are negligible. 

That diagnosis rules out the cheap explanations. It is not a weak prompt, and it is not a poor vocabulary. It is perception. The model cannot see a small enough object well enough to name it.

## Diagnosis tests
To strengthen the diagnosis, I tested three interventions.

First, I assisted the VLM by providing a scene vocabulary at the description generation stage. This gave a mere 1.1 point improvement on a small set, but the change became negligible on the validation set. Next, I brightening the frames to make small objects more visibile. However, this lead to a worsening of 3.7 points. 

Finally, I cropped the anchor frame around the bounding box proposed Grounding DINO, hoping that this would help the VLM guiding SAM 3 discern the small object from the neightbour. However, this costed 4.4 points.

To get a better understanding of the decrease in quality, I took the last test, all frames, and stratified by frame. Curiously, the hint rescues frames we were already failing, but corrupts the ones that were already positively before, and that larger population drives the mean negative. 

As a final check, I tried cropping the source frame on the target object. However, a tight crop severely lower IoU, from 41.1 to 18.0, because the identity of a small object is carried by its surroundings.

This isolates the real constraint: the signal of the target object in the embedding is obfuscated by the other dominant signals in the frame. 

Cropping was the blunt version of this fix. A sharper one, foveated tokenisation, is on our list, and it is exactly the Stage 1 assumption catching up with us.

## Ablations
After discovreing where the pipeline breaks and why, the I wanted to understand which component of the pipeline deserved the most attention for future improvements. To this end, I ran ablations. 

First, I looked at what each block actually contributes. The naive version of the pipeline is: describe every source frame and run SAM 3 one shot per destination frame. This scored 10.6 IoU. Replacing our description with the ground-truth object name lifted it to 17.0, which again isolated naming as the bottleneck. Nonetheless, most of the oracle advantage was captured back with frame selection startegy.

Swapping in she SAM 3 agent on every frame raised the score to 35.5, but costed 21 seconds per frame, or 59 hours for the validation run. Propagation solves the timing constraing at 0.82 seconds per frame, and Grounding DINO anchors further raise IoU at 38.1.

That is 260% over the baseline at a third of the time.

Two softer limits are worth naming here too: the bridge itself rests on just a handful of frames, and language carries an irreducible ambiguity no amount of engineering removes.

The conclusion from this ladder is clear: selection and propagation are already at their ceiling, while description and segmentation are where future efforts should go.

# Conclusions
140 words. 

## Recent developments
In fact, ever since submitting my thesis, there have been more efforts. My work was born from a research project with prof Plizzari and two other MSc students. Over the last weeks, we further developed the pipeline. 

The key innovations have been adding an VLM call to judge resulting segmentations and leveraging SAM'3 negative exemplars to discern better target object from neighbours. Together, these changes brough our results to 43.7 on Ego2Exo and 48.1 on Exo2Ego, a few points shy than the comparable state of the art. 

Motivated by the results, we are on track for submission to the Winter Conference on Applications of Computer Vision 2027 conference.

## What about language?
But does all of this answer our language as a bridge hypothesis?

Yes, partially. Language bridges foundation models without training, especially useful when the foundation models have different embedding backbones. 

A trending question this year is emerging: if intelligent agents already compute on rich abstract embeddings, why should they communicate in natural language at all? A shared mathematical space could be more efficient due to no decoding step, consume less energy, and possibly produce more accurate results since embeddings focus on true signal, not noise of connectors and filler words.

But it raises a human question too. If agents reason in a space we cannot read, do we become supervisors once removed, or are we pushed out entirely? 

Not saying bad, like humans now don't build cars eanymore. 

Worth asking which tasks we want automated, and which we simply enjoy doing for the sake of doing. 

Thank you!