# Introduction 
219 words.

## Title
Dear friends, family, Degree Assessment Board, and thesis supervisor: good afternoon.

My name is Marco, and today I will present my MSc thesis. I would like to start with a question.

## question
Is language the most effective medium humans have invented for communicating information?

I ask because language has become our prominent interface with technology. Raise your hand if you have prompted a large language model in the last 24 hours.

## Language and technology
I believe this integration of technology and daily life will continue, and that the next step is vision in the form of smart glasses, which are already on the consumer market.

## Augemnted vision
Smart glasses, combined with the cameras in your house, could give you augmented vision, and help you find your lost keys in the house faster. The idea extends to any context where agents, human or robotic, collaborate.

## The task: Cross-view object correspondence
Technically, this is cross-view object correspondence. Given a mask track on a target object in one video, the goal is to predict its mask in a second video of the same scene. The quality of the predictions is evaluated using the standard metric for mask prediction: intersection over union.   

## The dataset: Ego-Exo4D
The main dataset for cross-view object correspondence is Ego-Exo4D. This dataset provides roughly 4 million annotated frames, giving more than 700K cross-view paired masks across 7 scenarios like cooking and playing basketball. 

Each pair of vides combines an egocentric head-mounted camera with one exocentric camera fixed in the room. That pairing produces the strongest viewpoint change of any cross-view benchmark: the same object can fill the frame from one eye level and shrink to a handful of pixels from the other.

```Slide description. Title is 'The dataset: Ego-Exo4D'; the content is divided into three parts: (1) a slim horizontal rectangle at the top with 3 main statistics about the dataset (4 million annotated frames, 700K cross-view paired mask tracks, 7 scenarios); (2) a vertical rectangle on the left with written on the top 'Egocentric' with an example of an egocentric frame (still need to add); (3) a vertical rectangle on the right of (2) with written on top 'Exocentric' with an example of an exocentric frame (still need to add) below. Make actually two slides with element (1) appearing on first slide and elements (2) and (3) appearing on the second slide.```

## The challenges
This exacerbates the two problems baked into cross-view correspondence itself. 

First, matching the mask across views is hard, because viewpoint changes object size and shape. 

Second, temporal completeness is challenging, because over time cameras face motion blur, occlusions, and scene changes.

Finally, it assumes simple cameras with no geometric calibration, meaning that the correspondence can rely only on the visual information.

```Slide description. Title is 'The challenges'; the content is divided into three vertical columns, each having a title preeceded by a number and 1 picture below them: (1) viewpoint change; (2)temporal completeness; (3) only visual information.```

## Previous methods
Since Ego-Exo4d was released two years ago, several works have been published attemping to solve it.

Every method so far creates feature representations for masks and then train ad-hoc fusion component on Ego-Exo4D's labelled data. 

This produces two limitations common to the entire field:

1. The reasoning behind each correspondence is hidden in weights; beyond attention maps, there are no interpretable intermediate steps to inspect when the output is wrong.
2. Training on a single dataset arguably limits generalisation to other datasets and other camera configurations.

## A new paradigm?
When looking for solutions to these limitations, one idea was to check what modern foundation models could already do. 

Over the last five years, foundation models emerged as the new paradigm in machine learning, showing in both language and vision that training architectures very large in number of parameters on very large datasets on some pre-text tasks can exceed specialist systems.

So I asked myself and my colleagues: assuming today's foundation models have acquired a sufficient understanding of the world, will composing them at inference time transfer to a novel tasks?

And since language is their main interface, can language descriptions replace abstract learned features and act as a bridge between architectures?

These two questions became the language as a bridge hypothesis, the foundation of my thesis.

## Language as a bridge 
This paradigm mirrors how humans collaborate, communicating with language and reacting to each other's information. 

At the same time, it addresses both limitations we just saw:

1. Language is interpretable by design, letting a researcher inspect every stage of the pipeline and understand what went wrong for an errouneous prediciton. 
2. Foundation models save us from training, which in principle makes the pipeline dataset and camera agnostic.

## "Two eyes and one mouth" pipeline
To this end, this thesis proposes the “two eyes and one mouth” pipeline, where two cameras observe the scene from different perspectives, and a single natural language description mediates the object correspondence between them.

## Some terminology 
Before the pipeline, let me clear up some terminology that will make everything after it easier to follow.

Every cross-view object correspondence method runs over a take, on a target object, in a specific direction. A take is a single scene recorded simultaneously by a pair of cameras, producing two synchronized videos.

The first video is the input. We call it source, because alongside the video we also have the masks of the object we are interested in.

The second is where we compute the output. We call it destination, because that is where our method must produce its predictions: segmentation masks for the target object.

Each prediction is a mask on a destination frame, indicating the exact pixels corresponding to the target object. Predictions don't have to follow the sequence of time; in other words, they can happen offline.

Using Ego-Exo4D, we have two directions: Ego2Exo, with egocentric as source and exocentric as destination, and Exo2Ego, with the roles swapped.

## Pipeline overview
The pipeline works in four stages.

First, we select the frame from the source video where the target object is most visible. 

Then, we generate a textual description of the object. 

Third, we guess the frames in the destination video where the object is most visible and run its segmentation, creating anchor masks.

Finally, we propagate the anchor masks to the remaining frames of the destination video. 

Crucially, no component is fine-tuned for the task. And because the bridge between views is a human-readable JSON description, any erroneous prediction can be localised to the stage whose output is at fault.

## Assumptions
Four assumptions carry this design. First, language captures enough detail to separate the target from clutter. Second, the foundation models we compose interpret that language consistently, so the same phrase points to the same concept across their backbones. Third, a handful of reliable anchors in the destination view is enough for a tracker to complete the rest. Fourth, because the bridge is a written description rather than a live signal, prediction can happen offline.

Let's look at each stage in detail, connecting to the related works powering the pipeline. 

## Stage 1
In stage one the pipeline selects the source frame. For every annotated frame tau, we compute a visibility score, s of tau, from two cues read off the ground-truth mask. First, a of tau, the normalised mask area, the fraction of pixels the object occupies. Second, c of tau, its centrality, one minus the normalised distance from the mask centroid to the frame centre.

Area weighs 0.99 and centrality 0.01, because a large object gives the vision language model far more to describe than a small but perfectly centred one. The top frames form the seed set S, and we set this set of size one.

This design rests on a simple principle: the more of the frame the object fills, the more accurate the generated description. 

## Stage 2
Stage two writes the description. We leverage Qwen 3.5 to read the seed frame and return a JSON object W, the object description.

The description must satisfy two properties: view independence, so that it holds from the radically different destination viewpoint, and time independence, so that it holds at every destination frame, not just the seed's moment. We enforce both by asking only for intrinsic attributes, namely colour, canonical identity, material, and structural parts, which change neither with the camera nor with time.

We pass two images: the frame with the mask overlay, which tells the VLM which object to describe, and the raw frame, which preserves its true appearance as well as scene context. 

## Vision Language Models
At the heart of stage two is Qwen 3.5, a Vision Language Model. These are large foundation models trained to reason jointly over images and text by letting visual and textual tokens cross-attent to each other. 

Qwen 3.5 uses a Vision Transformer as image encoder. That's why we pass the frame in the source video where the tagret object is the largest. Because it means that the object will cover more patch tokens, which in run will claim a larger weight of the resulting image representation, and therefore transfer more signal for the VLM to generate a more accurate description. 

## Stage 3
With the description in hand, we move to stage 3. Here, the pipeline consecutively answers two questions: where in the destination video is the target best seen, and then can it be segmented.

## Stage 3.1
For where, the pipeline uses Grounding DINO to run detection on every destination frame with the description W as the query. 

Grounding DINO is state of the art language grounding model, achieving zero-shop open-vocabulary mapping of expressions to the pixels they describe by outputting bounding boxes. 

Along with the bounding box, it outputs a confidence score, which represents ...

We use this the confidence score as a proxy for quality, and store the top three to form the anchor set A.

This re-selection matters, because the destination frame synchronised with the seed is rarely the best place to segment. The correlation between the two views' visibility scores is only 0.14.

Next, we move to segmenting the target object. 

## Stage 3.2
Each anchor is segmented independently by a SAM 3 agent loop, orchestrated by Qwen 3.5.

The agent first simplifies the description into a short noun phrase, because contrastive alignment pools a sentence into one embedding, so a paragraph of attributes dilutes the match. 

SAM 3 then segments every instance matching that phrase, and the agent inspects each candidate mask against the original description, accepting or rejecting it. 

If all are rejected, the loop restarts with a different phrase. 

The accepted masks are our anchor masks on A.

## Segmentation
SAM is not a friend doing the segmentation by hand, but rather stands for Segment Anything Model, the current foundation model for segmentation. 

Its first version was introduced in 2023, training on a billion masks. The key cotnribution was zero-shot segmentation starting from visual prompts such as point and bounding boxes. 

SAM 2 extended this to video via a specialised tracker with memory, and SAM 3, in 2025, added segmentation from text prompts. 

## Stage 4
Stage four completes the track in time. 

SAM 2's video tracker propagates the anchor masks across the remaining destination frames, conditioning each prediction on a memory bank that holds all the anchors and the recently processed frames. 

This matters, particularly in egocentric video, where motion blur and occlusion often make the previous frame a poor reference, so attention reaches back to a distant anchor instead.

We propagate bidirectionally, forward from the earliest anchor and backward from the latest, keeping for each frame the prediction from the pass arriving from its nearest anchor.

# Experiments
851 words.

## Results
How does this approach fair?

We begin by looking at previously published methods. As a reminder, they all work with abstract feature representations and train specific fusion modules on Ego-Exo4D. 

## Official Baselines
Along with the dataset, Ego-Exo4D's authors propose two baselines: XSegTx and XView-XMem. Since both are adaptations of single-view segmentation, they score relatively poorly.

## 2025 challenge submission
Next, we have last year's Ego-Exo4D challenge, which saw two strong submissions: ObjectRelator and O-Mama. The latter won the challenge. 

## Latest developments
In late 2025, two new methods pushed results higher still: V²-SAM, which  adds three experts atop a shared SAM 2 mask decoder, and LM-EEC, which adapts SAM 2 with a two-branch mixture of experts and a separate memory bank for each video view. However, LM-EEC fine-tunes SAM 2's full backbone, making it an in-distribution upper bound rather than a fair comparison.

## 2 eyes and one mouth pipeline
On Intersection over Union we reach 37.7 Ego2Exo and 40.6 Exo2Ego, surpassing the official baselines and the ObjectRelator, and trailing O-MaMa by about ten percent. The gap widens against the most recent works.

The results are moderate, but are a first evidence that language as a bridge works. 

Foundation models never trained together on this task are able to recover significant quality against the trained models, and the remaining gap is explained by training and fine-tuning on Ego-Exo4D.

## Qualitative results
Let's have a look at some successesfull cross-view object segmentations of our pipeline.

As you can see, the masks are very high quality, driven by the capabilities of SAM 3.  

So how can the gap to the state of the art be so large?

## Gap analysis
To find the source of this discrepancy, I ran an head-to-head analysis with LM-EEC on the results. 

Plotting the distribution of our predictions against the state of the art, the issue become evident: on roughly a third of all cases, our pipeline scores zero IoU. 

I call this the dead mass, because it drags down the results of our pipeline.

```Slide design notes: title in the center as usual per other slides; gap-analysis-distribution.png in the middle centered; below centered the following text: "Mean IoU Exo2Ego: 40.5 Our Pipeline (Our Pipeline in plt classic default blue) vs 64.4 LM-EEC (LM-EEC in plt classic default orange)."```

## Removing the dead mass
In fact, when we remove the dead mass, our mean IoU rises significantly, with an average score of 61.7 in Exo2Ego. 

This is LM-EEC's terrain, the absolute state of the art that did a massive fine-tuning of SAM 2 and representing our in-distribution upper bound. 

This means that there are specific failure points, and the pipeline is not uniformly weak.

```Slide design notes: title in the center as usual per other slides; gap-analysis-distribution-removed.png in the middle centered; below centered the following text: "Mean IoU Exo2Ego after removing cases in the dead mass: 61.7 Our Pipeline (Our Pipeline in plt classic default blue) vs 64.4 LM-EEC (LM-EEC in plt classic default orange)."```

## Source of the dead mass
I continued the gap analysis with resect to LM-EEC by first setting a threhold for successful prediction at 50 IoU. 

Then, I assigned cases to 4 groups: namely with they were successful in both our pipeline and the state of the art, only our, only LM-EEC, and neither. 

Finally, I looked into variables that best separated the groups: object size, scenario, and anchor quality. 

The defining factor behind the gap became evident: the quality of the mask immediately after the bridge.

This exposes the main weakness of the pipeline. Everything hinges on the bridge. If we land on the wrong object, that wrong mask propagates faithfully across the entire take.

```Slide design notes: title in the center as usual per other slides; ch4/gap-analysis-groups.png in the middle centered; below centered the following text: Anchor mask quality is the determing factor; incorrect bridges propagate error to entire take."```

## Naming issue
So where is the dead mass born? To find out, I did a waterfall analysis, checking the intermediate results for all runs and charging every dead case to the first stage of the pipeline that breaks.

The main contribuent causing poor anchor masks is the VLM naming a different object than the target.

This issue accounted for 59% of the dead mass in Ego2Exo, and 81% in Exo2Ego. 

The following question thus is: why does the VLM mislabel an object? 

```Slide design notes: two columns; the first has examples of naming failures (image to do); the second has the text "Source of bad anchor masks in 59% of the cases in Ego2Exo, and 81% in Exo2Ego". Original slide with table moved to supplementary material.```

## Perception issue
The answer is the size of the source mask. On the dead cases, the selected source mask is three to six times smaller than on the successful cases. In Exo2Ego, half the failures show the object at under 0.1% of the frame.

Interestingly, rather then hallucinating, the VLM names a real larger neighbour. 

```Slide design note: two colums and one wide horizontal rectangle at the bottom; the first has a table created from this paragraph in the thesis (". On the dead cases the source mask handed to the model is three to six times smaller than on the successful ones (median 1.43% against 4.05% of the frame in Ego2Exo, and 0.098% against 0.564% in Exo2Ego), and in Exo2Ego half of the failures show the object at under 0.1% of the frame.") note that "Median source mask size" should be the title of the table and "Source mask 3--6$\times$ smaller on dead cases." is irrelevant since it is already seen in the table and "In \textit{Exo2Ego}, half the failures fall under 0.1\% of the frame." is an extra note below the table; the second has the foveated tokenisation example (/Users/marcolomele/Documents/Repos/msc-thesis/presentation/imgs/ch4/foveated-tokenization.png); the wide horizontal rectangle below has the examples of failures (still todo).```

## Ablations
After discovreing where the pipeline breaks and why, I wanted to understand which component of the pipeline deserved the most attention for future improvements. To this end, I ran ablations. 

First, I defined a naive version of the pipeline: describe every source frame and run SAM 3 one shot per destination frame. This scored 10.6 IoU. 

Then, I added frame selection, lifting IoU to 16.8.

Swapping in SAM 3 agent on every frame raised the score to 35.5, the biggest boost. Howeber, it also costed 21 seconds per frame, or 59 hours on 10% of the validation set. 

Propagation solves the timing constraing at 0.82 seconds per frame, and Grounding DINO anchor selection further raise IoU at 38.1.

That is 260% over the baseline at a third of the time.

The conclusion from this ladder is clear: selection and propagation are already at their ceiling, while description and segmentation are where future efforts should go.

```Slide design note: table right in the center. Table caption says "Ego2Exo on 10% of the validation set."```

## Recent developments
In fact, ever since submitting my thesis, there have been more efforts. With the help of Prof. Plizzari, Filippo Dario Paolucci, and Giovanni Mantovani, two MSc students, our pipeline received several updates. 

Currently, it sits at 41.5 on Ego2Exo and 48.1 on Exo2Ego, a few percentage points shy than the comparable state of the art. 

Additionally, it also achieves state of the art on another benchmark. 

```Slide design note: two nice tables in the center with one line caption at the bottom each. The first table has the pipeline at the thesis end of may checkpint, the pipeline today checkpoint, and v2-sam (the comparable model); its the caption says "Latest pipeline performance on the Ego-Exo4D test set."; the second table holds the results from the other benchmark (see prompt); its caption says "SOTA permance on HANDAL-X benchmark.".```

# Considerations.
So what did we learn from all these experiments?

## Design Level
First, a consideration from the design itself. Running foundation models at inference needs real hardware: we ran everything on an NVIDIA H200 with more than 80GB of VRAM, which won't be accessible to everyone.

## Assumption Level
Second, some of our assumptions still need work.

On language detail, the evidence isn't conclusive. The most common failure on dead cases was the VLM description, but the cause runs deeper: not enough signal from the target's visual features reaches the VLM, so it simply cannot see the object well enough. I suspect the root problem is grid tokenization, and hypothesize that an approach like foveated tokenization could dramatically improve performance.

On reliable anchors, we found the pipeline's main weakness. A handful of correct segmentations does complete the track, but a single wrong anchor propagates its error across the whole take.

The other two assumptinons held. For Consistent interpretation, the pipeline recovered significant quality against published work. For offline prediction, it held by design; in fact, our pipeline could be extend to live scenarios, since once the bridge is built, only SAM 2's video propagation of stage 4 needs to run.

# Conclusion

## Summary
In this thesis presentation, I presented the Two eyes and one mouth pipeline to address cross view object correspondence.

We built its 4 stages, and found that it achieves results comparable to last year with quality raising to the state of the art when ignoring dead cases.

We also investigated the source of dead mass, finding VLM description accuracy to be the current bottle neck. Given the interpretable nature of the pipeline, I ran several tests, far more than I have been able to discuss here today. 

More broadly, my thesis work directly tested to the language as a bridge hypothesis: can natual language connect foundational architectures and replace visual features? and can all of this done without training?

```Slide design notes: title "summary", enumerate list with keywords from the speech.```

## Can language act as a bridge between architectures?
Yes, partially. 

On the one hand, we faced some limitations, in part coming from the design of the pipeline, in part due to our starting assumptions. 

On the other hand, we have demonstrated that foundation models together were able to recover significant quality on a novel task none were trained on. 

## A trending question
A trending question emerging this year in Machine Learning is: if intelligent agents already compute on rich abstract embeddings, why should they communicate in natural language at all? 

A shared mathematical space could be more efficient due to no decoding step, consume less energy, and possibly produce more accurate results since embeddings focus on true signal, not noise of connectors and filler words.

## Question
But it raises human questions too. 

If agents reason in a space we cannot read, do we become supervisors once removed, or are we pushed out entirely? 

I'm not saying this is bad, like humans now don't build cars or don't make astrophysics sumulations by hand anymore. 

Worth asking which tasks we want automated, and which we simply enjoy doing for the sake of doing. 

## end
Thank you for listening!

# Extentions

## VLM input
We settled on this pairing by ablation: raw frame plus mask overlay reached 41.2 IoU, adding a crop on top changed nothing at 41.1, but a tight crop alone collapsed to 18.0, since a small object's identity lives in its surroundings, not just its pixels.

## Diagnosis tests
To strengthen the diagnosis, I tested three interventions.

First, I assisted the VLM by providing a scene vocabulary at the description generation stage. This gave a mere 1.1 point improvement on a small set, but the change became negligible on the validation set. Next, I brightening the frames to make small objects more visibile. However, this lead to a worsening of 3.7 points. 

Finally, I cropped the anchor frame around the bounding box proposed Grounding DINO, hoping that this would help the VLM guiding SAM 3 discern the small object from the neightbour. However, this costed 4.4 points.

To get a better understanding of the decrease in quality, I took the last test, all frames, and stratified by frame. Curiously, the hint rescues frames we were already failing, but corrupts the ones that were already positively before, and that larger population drives the mean negative. 

As a final check, I tried cropping the source frame on the target object. However, a tight crop severely lower IoU, from 41.1 to 18.0, because the identity of a small object is carried by its surroundings.

This isolates the real constraint: the signal of the target object in the embedding is obfuscated by the other dominant signals in the frame. 

Cropping was the blunt version of this fix. A sharper one, foveated tokenisation, is on our list, and it is exactly the Stage 1 assumption catching up with us.