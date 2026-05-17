# Writing Samples
Examples of the writing style and voice from different samples for each section type. Paragraphs are not consecutive. Ignore content; the aim is to depict and extract the style fingerprint.

## Sample 1 – Technical Report
### Introduction
Real estate is one of the few markets that can truly impact the economy of a country. Therefore, policy makers, businesses, and consumers look to understand its dynamics and predict its movements. A key metric that stakeholders track is sale prices. Houses have certain features, all of which have an impact on the price. Morevoer, as a city evolves, different areas emerge, with some being more premium and expensive than others. The aim of this research is to identify which house features have the most variable effect on the price across Melbourne's council areas. We hypothesize that the importance of house features is heterogeneous across space, meaning that features have varying value across Melbourne's geography.

### Data
Next, we examine correlations among the variables in the dataset. The heatmap (see Figure 1 in the Appendix) highlights notable relationships: \texttt{Rooms}, \texttt{Bedroom2}, \texttt{Bathroom}, and \texttt{Car} exhibit positive correlations, suggesting that properties with more rooms tend to have additional bedrooms, bathrooms, and car spaces, which aligns with expectations for larger properties. \texttt{Landsize} and \texttt{Building Area} also display a moderate positive correlation, indicating that larger lands typically have bigger buildings. \texttt{Price} correlates moderately with \texttt{Building Area} and \texttt{Rooms}, suggesting these factors significantly influence property prices. In contrast, \texttt{Year Built} shows weak correlations, likely due to variations in design, features, and external factors like location and renovations.

### Method
To address this limitation and satisfy our research goal, we employ the following approach: ...

This approach requires some assumptions. Namely, (i) that slope random effects across different features are uncorrelated, (ii) that month-specific intercept random effects don't interact with slope random effects, (iii) that fixed effects estimates $\beta$ remain stable regardless of which feature has random slopes, (iv) that CAR prior spatial relationships between councils are feature-independent, and (v) that errors are homoscedastic between the models. 

### Results
The largest standard deviation is observed for \texttt{Distance} (0.3484), indicating significant spatial variability in its impact on house prices, followed by \texttt{Bedroom2} (0.2532), reflecting differing preferences for additional bedrooms across council areas. In contrast, \texttt{Building Area} has the lowest standard deviation (0.0489), suggesting a uniform influence on property values.

### Conclusion
Before concluding, we would like to identify future research opportunities with regards to spatial heterogeneity. On the one hand, more nuanced models could be explored, for instance with multiple slope random effects or a learnable precision matrix in the CAR component. On the other hand, the dimensionality of the dataset could be expanded, including additional layers of hierarchy and additional factors affecting house prices.  


## Sample 2 – Technical Report
### Abstract
The 11\textsuperscript{th} Revision of the \emph{International Classification of Diseases} (ICD-11) serves as the global diagnostic standard established by the World Health Organization (WHO). However, \textbf{ICD-11} contains \textbf{limited and sparse descriptions} that challenge effective computational use. Current proprietary biomedical language models, such as Google's Med-PaLM 2, demonstrate the capability to enhance medical textual representation but remain broadly inaccessible. We leverage \textbf{Llama3-OpenBioLLM-70B} to systematically enrich ICD-11 descriptions, significantly improving their completeness and semantic density.

### Introduction
To address this limitation, we leverage Llama3-OpenBioLLM-70B, an open-source biomedical language model known for its robust performance, to systematically enrich ICD-11 descriptions. \cite{Ura2025} In this paper, we first explore existing research in embedding ICD codes and automated medical classification (Section 2). We then detail our method for data extraction, description generation, and validation procedures (Section 3), to emphasize the linguistic, medical, and hierarchical aspects of the newly generated texts. Subsequently, we outline our experimental setup for evaluating various embedding methods, including traditional (TF-IDF, FastText) and contextual transformer models (PubMedBERT, BioBERT, and GatorTron).

### Method
\subsection{Experimental Setup}
Our embedding generation pipeline consists of several steps. First, we append structured ICD-11 metadata to the generated descriptions, including diagnostic synonyms, related conditions, and hierarchical relationships. Then, we apply text preprocessing using the NLTK package, including lowercase conversion, special character removal, and whitespace normalization. Next, we set up our models. We specify TF-IDF with a vocabulary of 3000 features and apply SVD reduction to match BERT's hidden layer size. We configure FastText with a skip-gram model and a window size of five. For transformer models, we leverage their pre-trained weights. All models process texts in batches, with proper attention masking for transformers. We generate embeddings using mean pooling over the last hidden states, maintaining consistent 768-dimensional vectors across all approaches for fair comparison.

### Results
We also measure how often two models predict the same top disease for a symptom. On average, TF-IDF’s predictions match those of other models in only 19.4\% of symptoms, indicating its less aligned behaviour. In contrast, contextual and medical-trained models like PubMedBERT, BioBERT, and GatorTron produce more consistent results, agreeing on the top disease for about 35\% of symptoms.

### Conclusion
This limitation points to three directions for future work: (i) integrate graph neural networks (GNNs) trained with self-supervised objectives (e.g., link prediction) to capture hierarchical and cross-disease relationships better; (ii) utilize cutting-edge foundation models such as GPT-4.1, Claude Sonnet 4, or Grok 3 to enhance embedding quality; and (iii) explore scaling laws by varying embedding dimensions, model capacity, and input length to optimization effectiveness. \cite{ScalingLaws}

Together, these improvements aim to make ICD-11 embeddings more representative of biological mechanism and disease interactions and adaptable to questions posed in everyday language, ultimately supporting many real-world applications across multiple layers of society.


## Sample 3 – Technical Report
### Abstract
Understanding how to relate objects across egocentric and exocentric viewpoints is a fundamental challenge in multi-camera perception. In this work, we build upon \mbox{O-MaMa}, the state-of-the-art method that reformulates cross-view segmentation as an object mask matching problem, leveraging FastSAM for mask proposals and DINOv2 features for semantic alignment. 

Overall, our analyses expose important trade-offs between efficiency, finetuning stability, and robustness, offering insights toward developing general cross-view perception systems.

### Introduction
\textbf{Egocentric and exocentric visual perspectives} provide complementary information about human activities and their surrounding environment. Egocentric (first-person) cameras capture fine-grained details of hand-object interactions, but they suffer from self-occlusion and a restricted field of view. Conversely, exocentric (third-person) cameras provide a global understanding of a scene, but miss subtle interaction details due to distance, object scale variability, and limited resolution. Bridging these two perspectives is crucial for applications in augmented reality, robotics, and multi-camera perception systems.

### Related Works
Motivated by its relevance, part of our analysis will inspect the tradeoff between feature representation quality and computational cost by running O-MaMa on two additional backbones: DINOv3~\cite{dino3} and ResNet50-DINOv1. 

### Data
We restrict our study to the Health scenario, specifically the "Covid-19 Rapid Antigen Test" task, as it provides 20+ hours of video of consistent scenes with a relatively low variety in object form and high quality annotations on object relations across cameras. 

### Method
In this section, we formalize the Exo→Ego correspondence task, summarize the O-MaMa architecture, and detail the methodological explorations ran during our study: feature pre-extraction, finetuning analysis, and feature extractor backbone substitution. Each modification is designed to investigate O-MaMa's capabilities. 

### Results
We inspect the worsening of performance induced by finetuning by plotting masks on individual frames. Looking at situations of high divergence in IoU between predictions of baseline weights and predictions of finetuned weighs (see Appendix \hyperref[fig:delta]{Fig. 7}) we uncover two emergent behaviors: "over-specification" and "easy distraction". 

Over-specification refers to O-MaMa's tendency to reduce the area of the predicted masks by focusing on smaller portions of objects. This is analogous to tuning FastSAM such that it segments details rather than follow object contours. Easy distraction refers to O-MaMa exhibiting a distraction mechanism, segmenting the object that is at the center of the frame under manipulation, rather than sticking to the actual object it should predict. See Appendix Fig. \ref{fig:-emergent-phen} for detailed examples. 

We provide three conjectures that explain such results.

### Conclusion
Finally, O-MaMa exhibits robustness over unseen data, but struggles significantly with moderate to heavy occlusion. This emphasizes how the current mask-matching pipeline relies on visible cues in both exo and ego point of view. They ignore information about missing object parts, are unable to leverage scene geometry, and fail to reason around nuanced interactions among objects. In contrast, we humans are able to infer object positions even when complete occlusion appear, because of our ability to track space and time conjunctively, while only relying on the ego point of view. 

These observations suggest several promising directions for future research: 

## Sample 4 – BSc Thesis
### Introduction
\chapter{Introduction}
Network systems are ubiquitous in the real world, with applications spanning business, science, and beyond. At the heart of biological systems, communication grids, e-commerce websites, and social media platforms lies a common structure: a network of interacting entities.

\vspace{5mm} 
To model networks, we use graphs, a high-level mathematical structure that represents entities with nodes and relationships with edges. Graphs are versatile and possess many components that makes them easy to adapt to any network. However, as the size of the network increases, it becomes increasingly inefficient to use the original formulation with nodes and edges.

\vspace{5mm} 
Graph representation learning deals with converting large-scale graphs into numerical vectors in a latent space known as embeddings. These vectors comprise abstract features that capture the original information contained in the graph. One advantage of this formulation is that it enables Machine Learning tasks on graphs. 
Over the last decade, Graph Neural Networks (GNNs) have emerged as the leading paradigm for building effective embeddings that enable powerful inference tasks such as node classification or link prediction.

### Theory
\section{Representation Learning}
\subsection{Motivation}
In traditional machine learning, representations are constructed through feature engineering, transforming raw data into more expressive features towards the target variable. In our previous example, a simple feature engineering operation could be computing the ratio between temperature and pressure rather than using the two measures separately. 

\vspace{5mm} 
However, this process is labour-intensive, relies on domain expertise, and is subject to selection bias. Over the last decade, research has focused on developing algorithms that depend less on feature engineering. The goal is to design algorithms that learn features autonomously in latent spaces. 

### Method
subsection{Convolutions on Graphs}
Inspired by CNNs, Kipf et al. (2016) introduced Graph Convolutional Networks (GCN). \cite{Kipf2017} The fundamental idea remains unchanged; a kernel is slid over the graph, and the embeddings are formed as a weighted sum of the embeddings of the neighbours, but now the size of the kernel varies according to each node's 1-hop neighbourhood.

\vspace{5mm}
The propagation rule of GCN at the $\ell+1$-th layer is:
\[ H^{(\ell+1)}=\sigma(\tilde{D}^{-\frac{1}{2}}\tilde{A}\tilde{D}^{-\frac{1}{2}}H^{(\ell)}W^{(\ell)}) 
= \sigma(\hat{A}H^{(\ell)}W^{(\ell)})\]

Here, $\tilde{A}=A+I$, with $I \in \mathbb{R}^{N\times N}$ being the identity matrix. It is added to the adjacency matrix to incorporate self-loops, making a node a neighbour of itself, and thus incorporating its own features during the updating of the representations. $\tilde{D}$ is a diagonal matrix called the degree matrix, with $\tilde{D}_{ii}=\sum_{j}\tilde{A}_{ij}$ representing the number of connections of node $i$ plus 1. 

The first part inside $\sigma$ is the $AGGREGATE$ function. It is a sum of all the node representations from the previous layer $\ell-1$, weighted by $W^{(\ell)}$ and scaled by entries from the adjacency and degree matrices. The second part is the $COMBINE$ function. It says that the aggregated representations of the neighbours are combined via summation to the weighted and scaled representation of node $i$ from the previous layer. Figure \ref{fig:GCN-how} depicts the propagation rule for GCN. 

### Experiment
\subsubsection{Problem Setup}
The global agriculture trade problem, introduced by L. Jiang et al. (2023), is the experiment in the research field that most closely resembles our commodity trade problem. \cite{Jiang2023} Both involve predicting the volumes of commodities traded among countries given past information about trade and other variables such as production and demand. Accurate trade forecasts benefit many parties, from government agencies that can design better policies to companies that can make more precise production plans

\vspace{5mm}
Trade occurs in a global and interconnected market that presents both structural and temporal dynamics. The structure of the market is defined by the relationships that countries share, determined by trade agreements, geographical distances, and geopolitical conditions. Activity in the market is dictated by the forces of supply and demand, which are influenced by many variables such as production and available stocks. Most of these variables fluctuate during the year, roughly following seasonal trends, which injects a temporal dimension into the problem.

\vspace{5mm}
Agriculture trade can be modelled as a CTDG $\mathcal{G}=(G_0, \mathcal{O})$ with countries as nodes and trades as edges. Each observation $o_t=((i, j), t, \omega_t)\in \mathcal{O}$ indicates that a directed edge with weight $\omega_t \in \mathbb{R}$ between source node $i$ and destination node $j$ occurred at time $t$. The weight indicates the volume that has been traded between country $i$ and country $j$. Moreover, $\mathcal{G}$ has a time varying node set $\mathcal{V}_t$ and edge set $\mathcal{E}_t$. 

### Conclusion
\chapter{Conclusions}
\subsubsection{On the Commodity Trade Problem}
The results from the previous chapter suggest that the TGN model is a step in the right direction for solving the commodity trade problem. However, they also highlight a clear research gap, as one baseline forecasting method outperformed the GNNs in all but one test condition. These results suggest that current Dynamic Graph Neural Network models may not yet be capable of solving temporal edge regression.

\vspace{5mm}
New training techniques may be necessary. The authors of \cite{Jiang2023} found that sampling negative edges in different proportions significantly affects test performance. Additionally, future research could explore edge-based embedding methods, such as Edge2Vec, which maps edges into structure-preserving embeddings. \cite{Changping2020}

Finally, there is the broader question of training complex models on intricate relational data using loss, a one-dimensional channel of information that tells the model how it needs to learn. While, this approach yielded models with human-level performance on tasks with images and text, which are standard-sized inputs, graphs lack this structure. The beauty of advanced fields like Dynamic Graph Neural Networks lies in their ability to make us reconsider the most basic foundations of our knowledge.

## Sample 5 – Oral presentation script 1
Hello, today I will present the foundations of diffusion models, which stand at the basis modern image generation systems.
Image generation finds many useful applications.
--
For instance, this Christmas I generated a new haircut, and the result was realistic enough that it convinced me to do it.
But what is an image?
--
It is a point in a high dimensional space, defined as tensor of 3 arrays of two dimensions, one for each colour channel.
The image space is quite large; considering a full HD image, the number of pixel value configurations is of the order 10^13.
However, most points in this space are random noise, and real images cluster with other images that have pixels of similar values and structure, creating sort of islands. 
To better study the image space, we endow it with a probability measure induced by the frequencies of pixel values observed in a dataset. This gives a probability landscape, where regions of high likelihood correspond to islands of real images, and regions of low likelihood correspond meaningless noise. 
--
Hence, the problem statement. Given an image dataset, our goal is to model the true data distribution with some model ￼ and then generate new images by sampling from it. 
Ideally, we look for a model that is both flexible and tractable. However, this is a common challenge in classical statistical modelling, because flexibility warrants complex functions, which in turn bring intractability issues. 
--
In 2015, a group of Stanford and Berkley researchers propose Diffusion Probabilistic Models, a novel solution that transforms generative modelling into series of regression tasks linked by a Markov chain.
With this approach they achieve extreme flexibility, exact sampling, and high tractability.
The main components of DPMs are the forward and backward trajectories.
--
In the forward trajectory, our goal is to convert the complex data distribution into a simple (0, identity) Gaussian distribution. This is achieved by repeated multiplication of a Markov diffusion kernel, which is explicitly parametrized by another Gaussian distribution with a noise scheduler parameter beta. 
Thus, we get the forward diffusion as a discrete stochastic process ￼. It describes an iterative injection of noise, moving from low entropy set up, the original data distribution, to a high entropy set up, the ending Gaussian noise. 
--
Visually, we cement two ideas. 
The first is that of the forward trajectory is the information destruction direction, i.e. moving from a real image to random noise.
The second is that the object of interest is a time-evolving probability distribution, obtained by running multiple trajectories.
--
In the backward trajectory, our goal is to convert the simple gaussian distribution into the original nuanced data distribution. 
We work with the reverse stochastic process ￼, which describes an iterative noise removal procedure. 
The Markov diffusion kernel is again parametrized by a Gaussian distribution, but crucially this time we leave the mean and variance free to be learned by two multi-layer perceptrons. 
Visually, we move from a random starting point near 0 towards a high probability region, reversing time steps from ￼ to 0. 
This is done both during training and sampling and thus can be thought of as the information generation direction. 
--
To train a DPM, we resort to the evidence lower bound, as the direct computation of the log likelihood ￼ is intractable. 
The ELBO is derived by applying the variational trick, taking expectations under the forward process, and interpreting DPM as a Markovian Hierarchical VAE, which I discuss in the Appendix.
The important point here that the derived objective function is composed of three elements.
The first accounts for reconstruction of the starting data distribution, the second looks at the similarity of the ending distribution among the two trajectories, and the third quantifies the difference between the transitions for all diffusion steps. The latter element makes the bulk of the estimation, and it is called denoising matching. 
--
Visually, we can see why. 
At its core, a DPM learns to minimize the difference between the forward ground-truth denoising step, obtained as the posterior of the forward diffusion kernel ￼, and the model’s approximate denoising step ￼. 
Because we defined the forward trajectory explicitly, we can derive the expression of the ground-truth denoising analytically. In contrast, the reverse diffusion kernel expression is unknown, so we estimate it via the neural networks. 
That is, DPM learns to match as closely as possible the statistical properties of the green arrows with the statistical properties of the pink arrows. 
 
--
The authors provide two important qualities of DPMs.
The first is multiplicity. It says that DPMs allow for composable modelling with positive bounded functions without the need to retrain.
The second is the entropy bound. It acts a diagnostic tool to identify when reconstructing the image is too difficult. 
Intuitively, we aim at a noise injection schedule that is not too fast to be unable to reconstruct the original image and not too slow to avoid the process being computationally expensive. 
--
Looking at the results, DPM succeeds on various toy problems, but achieves mediocre results on image generation. 
This is due to several weaknesses, mainly a hard optimization objective and unstable dynamics when predicting together the mean and variance of the reverse trajectory. 
--
In 2020, a group of researchers from Berkely address these limitations with their paper Denoising Diffusion Probabilistic Models.
They propose clever parametrizations that allow them to ground DPMs in score matching and Langevin dynamics theory, which simplifies the objective function, and generate images of quality equal to the state of the art at their time.
--
Here is the architecture. 
First, they set a linearly increasing noise scheduler, which makes noise injection small at the beginning, to slowly destroy image details, and large at the end, to quickly navigate random noise regions.
Next, they reparametrize the intermediate diffusion points ￼ as a weighted sum of the starting data point ￼ and noise perturbations ￼ injected by the forward noising process. With this representation, the incremental noise additions get collapsed into a single total noise value.
Finally, they propose a configuration of the reverse diffusion kernel, divided into two.
One, set the variance to time dependent constants. This immediately simplifies training to matching the mean of the forward posterior with the mean of the reverse diffusion kernel.
Second, they derive a new expression for the estimate of the mean. This shifts the task of the neural network to predicting the noise present in ￼ relative to ￼. 
This noise is the exact same ￼ of the reparametrization trick. 
Note that the actual neural network is based on U-Net with parameters shared across time.

 
--
The authors also introduces additional tricks to further improve result quality.
First, they scale data to ensure that the reverse process neural network receives consistent inputs.
Second, they set the last term of the reverse process to an independent discrete decoder. Akin to quantization, this ensures that mass is assigned to discrete values and makes the ELBO a lossless signal for training. 
Finally, they simplify the loss by removing schedule-related terms. 
This reveals the updated form of the problem: learning a diffusion model is equivalent to predicting the value of the added noise to an image at each time step. 
Interestingly, there exists another formulation of this objective, which connects us to score matching. 
--
The score is commonly used in energy-based models as a workaround to the problem of intractability. 
It is calculated as the derivative with respect to the data of the log of the probability distribution. 
Geometrically, the score represents the direction that most increases the likelihood. 
Using Tweedie’s formula, we rewrite the ELBO and obtain a new objective, which I also cover in the appendix. However, it depends on true score, which is unknown.
To solve this, we use score matching, an integration by parts trick that makes the loss function depended only on the neural network learning the score and our ability to sample from the real distribution. 
This highlights the connection: to predict the direction of the added error is equivalent to predicting the direction to update the data point to increase its log likelihood. In fact, they are the opposite of one another.
--
Let’s consolidate the point visually. 
The score function defines a vector field pointing towards the local increase in likelihood. 
As the data distribution evolves throughout the forward process, the vector field follows. 
Hence, the score acts as a compass telling how to navigate the data space at each diffusion time step. 
This analogy is also useful to understand the second connection.
--
First, note that the stochastic processes of the forward and backward trajectories admit continuous time representations.
Formally, we can characterize them with Brownian motion, which gives us a forward stochastic differential equation and a corresponding reverse SDE. 
Finally, under a certain parametrization of the SDEs that uses the score, we can retrieve a specific form known as Langevin dynamics, a well-studied approach in physics involving models that evolve part deterministic and part stochastic. 

 
--
The connection opens many interpretations of Diffusion models, as different parametrization of the SDEs yield different generation dynamics.
Fundamentally, the score and the Langevin-like properties of the reverse trajectory are the tools that allow DPMs to navigate the image space and generate visual information from random noise. 
--
The changes introduced in DDPM prove to be successful. 
The authors train and generate samples from multiple datasets and achieve results matching the quality of other SOTA models, sometimes even outperforming them. 
Ablation studies confirm that the approach predict-the-error dramatically outperforms the original predict-the-mean on the simplified ELBO. Additionally, they validate that keeping the variances to time-dependent constants further improves sample quality.
--
The authors also recognize how Diffusion models act as lossy compressors.
The compression plots make it clear: in the low-rate region, distortion drops very quickly. This shows that most of the bits, the information budget of a Diffusion Model, are being allocated to imperceptible distortions, invisible to the naked eye.
At generation, behavior is also consistent, with large scale image features appearing first and details appearing last.
The quality of over time plots support this, with an interesting phenomenon: there appears to be a phase transition behavior at the middle of the reverse diffusion. This was confirmed later in 2024 by Dynamical regimes of Diffusion Models, a work of professor Mezard from our faculty. 
The final discovery of DDPM is more of a quirk: interpolation is possible among pairs of images by first interpolating manually and then, removing erroneous artifacts with denoising. 
This produces transitions among pictures for different mixing levels which resembles a continuity-like property of the image space.
--
In conclusion, DPM proposed a novel framework, and DDPM validated it: image generation is equivalent to learning trajectories that navigate the data space.
After DDPM’s state of the art results many developments followed. These were aimed at adding guidance to the navigation, increasing its speed, on a different space, with a different backbone architecture, and applied to other domains like text and audio. 
Studying diffusion models has been thought-provoking, mostly because at first I found them un-intuitive. They don’t resemble how we humans understand and generate images.
Think about transformers: they learn to generate natural language by encoding word co-occurrence via self-attention, which is akin to how we can discern links between words in a sentence.
In contrast, diffusion models learn to generate images distributions implicitly by learning to navigate the data space. 
The results can be indistinguishable from reality, yet the mechanism still feels non-semantic.
This made me think: will machine learning keep getting better at tasks that demand human intelligence – and yet grow more distant from how we understand the world? If so, what does that say about human intelligence?
--
Thank you for listening!
 
## Sample 6 – Oral presentation script 2
Slide 1
Explore if this statement is true: Independent Samples Are Not Always Islands. 
2017 paper titled “Bayesian prediction with multi-samples information”, by professor Prunster, professor Lijoi, and others. 

Slide 2
We find ourselves in the context of prediction: learning from the past about the future
The famous italian probabilist De Finettiin in 1931 in his work Probabilismo, states that science cannot limit iself to just describing accomplished facts; it must forsee. 
Later on in his career, De Finetti also provided a connection to probability theory: probability doesn’t exit in nature but instead is the human expression of knowledge about an unknown event. 
Subjectivity, central to Bayesian school of thought, finds ground in other fields. For instance neuroscience, with Friston’s Free Energy Principle, which models the human brain as a inference machine that aims at minimising sensory input by updating prior beliefs about the world.
So we established that the goal is to use the past to infer the future. However, traditional probability theory imposes i.i.d. observations, which is overly restrictire and counter human intuition of past actions influencing future outcomes. 

Slide 3
Now consider the following complication: what if we are dealing with multi-sample data? 
To be more specific, what if data comes from  different experiments? We represent this new data as a sequence of sequences.
where experiments are indepdent in the sense of different physical conditions. For example, different grographical areas or different categories. Nonetheless, we are assuming that the exeperiments measure fundamentally same objects, using the same units.

Slide 4
To this end, we consider Partial Exchangeability, an extention of Exchangeability. 
A sequence of observations is exchangeable if the distributional properties of the sample are preserved under permutation of the order in which data is observed.
A sequence of sequences is partial exchangeable if the probability law of each sample is permutation invariant within the sample.
Analogously, De Finetti’s representation theorem, in more excplicit form, now defines the probability of a partition, i.e. different realisation in the different samples, as mixture of i.i.d. laws. 
Looking at the Bayesina Hierarchical view, we see a clearer picture of what is happening. The set of observations from differed  experiments conditional on a vector of RPMs are i.i.d. w.r.t to the mixture of the RPMs. 
Additionally, the vector of RPMs is sampled from  a prior distribution over the space of probability measures which encodes how the samples are related across each other. 
Note conditional iid implies that our igonrance of  induces dependence and uncertainty in the data.
Partial Exchangeability lands perfectly for bayesian multi-sample prediction problems with discrete realisations. 

Slide 5
Which the literature dubbed by as species prediction.
Consider each independent sample  as separate jungles or restaurants, where each observation  has discrete realisations , like animal species or dish served. 
Next, suppose we draw one observation for sample . 
3 scenarios emerge naturally:
·       New observation has a value that has already been seen in the sample, dubbed Old
·       New observation has a value that has not been seen in the sample, but that has been seen in other samples, labelled with LocNew
·       New observation has value never been seen across all samples, dubbed as GlobNew
Our goal is the following: predict the values of the new draws in each sample, given the previous observations. We need some formalisms. 

Slide 6
We start by taking De Finetti’s representation theorem condition it on the observed data, obtaining the posterior probability distribution over the partitions. 
Specifically, the posterior structure is observed inside , which now denotes an updated dependece structure following the data.
Next, following the paper, we restrict without loss of generality our attention to 2 samples, which allows us to define clearly four groups of interest, each having associated a quanity of interest. 
In particular:
·        defines the collection of unique values that the observations can take across samples, with  regulating whether a sample is considered or not in the collection. 
·        counts where the new draws fall, with  determining the sample and  the number of new draws being considered.
In essence, we are defining collections of values that observations can take, then observe the new draws and count in which groups they land. Consider this visualisation.

Slide 7
In stage 1, we define the collection of values.  represents …,  represents …, and  represents….
In stage 2, we observe new values and count in which group they land. Focusing on the blue sample, you see: most values will land within the blue region, the Old case. Additionally, some blue points fall insidei the green sample, the LocNew, and other more migh fall outside of both samples’ collections, a GlobNew discovery.
How can we predict these amounts? Using theorem 1, the main contribution by Professor Pruensner and the others. 

Slide 8
Under partial exchangeability, the expected value of the counts where the new draws fall into , is equal to  , the quantity of new draws in experiment , times the one step probability.
Lets consider interesting implications and leave the proof for the appendix.
First, the expected count of observations falling in any collection is linear in , which means that the proportion of values that will belong to each area of interest is constant to additional sampling. 
In other words, the probability law governing future observations does not depend on ordering of data within each group.
Second, this generalises the binomial experiment to a general meaning of dependence and dimensionality across samples. In fact, this result holds true regardless of , the prior over the RPM vector space. 
We can now move to the practical side of things.

Slide 9
Here’s the plan. First, we identify the prior model  on the space of vector RPMs. Then, we devise a Gibbs samples to approximate the posterior, as we saw in the formal set up. Finally, we run simulations with real world datasets and investifage the results. 

Slide 10
Following the Bayesian non-parametric framework, we look to place a prior  over the space of probability measures whose support is large enough to approximate any data generation distribution. 
The starting prior is the Dirichlet process (DP), which features a prior base measure assigning probability to all measures and  a confidence parameter over the base measure. 
The DP features a fairly a simple predictive distribution which also highlights its main drawback: that the probability of sampling a Global New value, the first term, depends only on the total sample size , and ignores the freqency structure within each sample. 
In species problems, whther we expect new species depends heavily on the clusters that already formed in the sample, not just the sample size. If the sample contains many rare types, DP wouldlead to a systematic underestimation of unseen species.

Slide 11
To solve this problem, the paper considers a generalisation of the DP known as Pitman Yor Process. It introduces a new parameter  which controls how fast the number of clusters grows.
The predictive structure, obtained as before, is a modified version of the predictive of the DP. The key differences are that there is an explicit control over the probability of choosing a new value, first term, vs an existing cluster, second term, with  representing the total number of clusters from  observations. 
A key property of the PYP is that the expected number of clusters satisfies  , a power law growth, which better models heavy-tailed species abundance distributions and yields more realistic predictions of unseen species.

Slide 12
Now that we have a proper prior, we need to extend it to our multi-sample species prediction problem. 
The key intuition is that we want to model the probability of observing new values both within each sample and across samples. We achieve this by stacking two PY processes on top of each other. 
At the lowest level, we have the individual observation .
Each  is a draw from its sample-specific random measure  , which assigns different species weights and captures sample-level idiosyncrasies.
Each  itself depends on a common top-level model  via a PY prior with common parameters (), ensuring that all samples share the same structural reinforcement and heterogeneity mechanisms.
 is the top level RPM, acting as an hyperprior and governing how species are assigned across all samples. 
Finally,  which governs how dishes are selected from the universe of possible dishes. 
Looking at the parameters, we see two levels. Local parameters tweak local dynamics, like richer-get richer, and the global parameters determine global behaviour, like species sharing vs discovery. 
However, there is an important missing detail: we have hierarchy in the model, but not in the observations. Without it, global and local measurements collapse, and model fails to learn differences across samples. 

Slide 13
To this end, we introduce latent variables called tables, and interpret the model with the Chinese Restaurant Franchise metaphor. 
We have a chain of restaurants that serve dishes from a global common menu. There are some differences between restaurants, just like McDonalds has a global menu and local specialties depending on the country. Each customer of each restaurant is assigned a table,  , and each table consumes a single dish. This means that each restaurant  will have  customers eating dish  divided into  tables. 
Notice that we have a one-to-one correspondence between what each customer eats and at what table they sit at. This structure is not redundant informaiton; rather, it creates the crucial structure that enables us to disentangle local relationships from global relationships. Think of dishes as global species, and tables as local species.  
Consider the visualisation.

Slide 14
Going top down along the hierarchy, we have the base measure , which determines which dishes exist. We consider this as diffuse, meaning uniform across all possible discrete realisations. Then, there is the global non-parametric prior , which assignes dishes to individual restaurants. Finally, we have , the local law, which re-weights dishes within each restaurant, adapting to idyosyncracies of each sample.
As you can see, there are many dishes a the global menu level, and restaurants focus mostly on a subset, with some dishes being shared.

Slide 15
The last step in our experimental pipeline is devising a Gibbs Sampler that is able to sample from the posterior distribution and generate new data. 
High level, the sampler focuses on a key iteration.
Step a is related to the existing data. For each observed customer, we temporarely remove it, and sample its latent table from the full conditional probability disributions. 
Step b entails the new data, where for each prediction we sample both the table and the dish allocation using one-step normalised HPYP probabilities involving ratios of pEPPFs. More detail in the appendix. 
This approach is restaurant-specific, but achieving ties between samples via the hyperprior parameters. More details are in the appendix. 
S
lide 16
Having defined the HPYP as prior model over the space of RPMs, and designed a Gibbs sapler, we can move to the practical side of things and run simulations on real world datasets. 
Our mission is clear: we want to validate empirically the structural property of theorem 1 using 3 datasets different from the original paper; demonstrate the advantage of using a hierachical model over independent models, known as the borrowing of strength effect; and finally, explore model failure, where this structural dependence can actually worsen the results. 

Slide 17
To make results comparable, we follow a standard methodology across all experiments. 
For each experiment: explore the data, check linearity, and compute diagnostics. 

Slide 18
The first dataset contains US baby names across 4 states. Each state is an independent sample, and the names are the discrete realisations. 

Slide 19
We check linearity by plotting the toatal discovery count (local new + global new) for each sample, as we find it checks out. 
Interestingly, smaller states with less unique names, NY and FL, exhibit higher discovery when using the dependent model, becaue dependence boost use of names in other states.

Slide 20
Borrowing of strength effect is evident by the decrease in High Posterior Density (HPD) ratios on average and across values of m. 
Another diagnostic is the global-new share measures the ratio between global new discoveries and local new discoveries. 
Decreasing means that there is borrowing and true global novelty becomes rarer; constant means FL has unique name distribution such that balance between novelty and borrowing.

Slide 21
The second dataset contains Soil Types by 4 wilderness areas. Each area is an independent experiment, and the soil types are the discrete realisations. 
This second case has lower sharing, with individual samples exhibiting high concentration on few soil types. 

Slide 22
Linearity checks out again.
However, we immediately see larger HPD widths in the dependent model case. Moreoer, area 4 deviates from the other areas, remaining with a relatively higher probability of discovery. 

Slide 23
Looking at the HPD ratio, we see why; when drawing few samples, small m, the hierarchy hasn’t yet “learned” the idiosyncrasies of each area. 
What happens is that more similar groups dominate early on and populate  the common measure, which mis-represents the idiosyncracies of non-dominant samples. This bias in the beginning which causes an increase in variance (uncertainty), later managed for large  when model has seen all menu. 

Slide 24
The third dataset contains nouns by 2 article categories: space and religion. Each category is an independent experiment, and the nouns are the discrete realisations. In the appendix, I consider an extention with 6 categories. 
This last case has the lowest sharing, and even the shared nouns have low frequency within each category. 

Slide 25
Negative borrowing of strength due to low sharing, called negative pooling, becomes appearent.
The probability of new discovery is lower in the independent model; the HPYP forces positive correlation amonve two data generative processes of which support has negligible overlap, resulting in spurious transfers. 

Slide 26
Evidence of negative pooling appears also in the HPDs: they increase in both samples, and the HPD ratio is greater than 1 and moves erratically as m increases, showing that the hierarchy is unable to learn and seaprate the two samples. In the appendix I explain this in more details. 
Solutions include: a more diffuse on top level base measure, pushing  large or  small, or simply prefer independent models when the HPD widths increase under dependent model. 

Slide 27
In conclusion, we achieve our goals.
First, we demonstaret that -step expectations are linear with slopes given by the one-step discovery probabilities.
Second, the HPYP enables principled information sharing with borrowing of strength effects controllable by the local and global parameter sharing. 
Finally, we discovered a unique requirement for the HPYP to work in multi-sample species problems: there must be non-negligibel overlap between in the observed values and their frequencies between the two experiments. 
If the condition holds, then even when samples are physically independent, distinct states, different wilderness area, separate article category), information sharing is possible and benefits prediction tasks.
Therefore, independent samples are not always islands. 

## Sample 7 – Related Works 
### BSc Thesis extract
\subsection{Inference Tasks}
There are three categories of inference tasks on graphs, each applicable at different levels. First, \textbf{classification}; given a set of embeddings, predict the class of nodes, of edges, or of the entire graph. Second, \textbf{regression}; given a set of embeddings, predict one or multiple attributes of the nodes or of the edges. Third, \textbf{link prediction}; given a set of embeddings, predict the existence of a link between pairs of nodes. 

\vspace{5mm}
Another distinction is between transductive and inductive learning. The former involves tasks where the predictions are made on entities that the model already saw during training. The latter relates to tasks where the predictions are made on new entities, which reveals the ability of the model to generalise. \cite{Wu2023}

\vspace{5mm}
$(\bigstar)$ Because we appended trade volumes on the edges, we can formulate the commodity trade problem as an edge regression task: predict the edge weights given the structure of the graph, country-specific attributes on the nodes, and pair-specific features on the edges. Additionally, we are in a transductive setting, as countries in the graph remain unchanged between training and testing.  

\subsection{Early Approaches}
Early advancements in graph embedding research are based on dimensionality reduction techniques, which map data points from a high-dimensional space into lower-dimensional embeddings. Their primary objective is to learn representations that, upon being projected back to the original space, closely approximate the original points. Early approaches thus focused on the first requirement of effective graph embedding. 

\vspace{5mm}
An example is Isomap by J. Tenenbaum et al. (2000), a non-linear dimensionality reduction technique that involves modelling a dataset as a graph and calculating shortest paths between point neighbourhoods. \cite{Tenenbaum2000} 

\subsection{Modern Approaches}
Modern approaches incorporate more information about the structure the graph. Additionally, they target downstream inference tasks, satisfying the second requirement of effective graph embedding. \cite{Wu2023}

\vspace{5mm} DeepWalk by B. Perozzi et al. (2014) is the first popular approach. It defines the neighbourhood of a node by leveraging random walks and iteratively updating embeddings. The core idea is that nodes appearing frequently in each other's paths are more likely to be in the same neighbourhood. Thus, nodes which have similar neighbourhoods will acquire similar representations. \cite{Perozzi2014} 

\vspace{5mm}
A. Grover et al. (2016) extended this concept with Node2Vec, a second-order random walk. In particular, they bias transition probabilities between nodes at various shortest path distances to strike a balance between breadth-first and depth-first search. Then, Node2Vec iteratively updates embeddings by maximising the likelihood of seeing a neighbourhood for a node conditioned on the node's representation. \cite{Grover2016}

\vspace{5mm}
Modern approaches also incorporate additional kinds of information that introduce further proximity measures allowing the representation of the nodes can be learned more comprehensively. This information involves side content from nodes and edges, such as types and attributes, or even the error found during supervised training, which establishes a connection between the representation of the nodes and the target task.

\subsection{Deep Learning Approaches}
Over the last 10 years, deep learning has emerged as the leading paradigm in machine learning, with superior performance in many domains such as computer vision and natural language processing. Y. LeCun et al. (2015) define deep learning as a set of representation learning methods obtained by combining multiple non-linear modules that each transform the representation at one level into a representation at a higher, slightly more abstract level. \cite{LeCun2015}

\vspace{5mm}
However, deep learning methods are inapplicable to graph data. First, graphs are highly irregular structures and have no spatial locality, meaning that nodes can be arranged in any arbitrary shape and without affecting the information they together contain. This makes it difficult to generalise mathematical operations. 

\vspace{5mm}
Furthermore, the type and properties of graphs depend on the specific problem that is being modelled. Two graphs can be isomorphic, meaning that they have the same structure but different names and properties of the nodes. This further amplifies the issue of generalising operations. In contrast, any image or text can be converted into a standardised matrix or array of numbers.

\vspace{5mm}
Additionally, large-scale graphs can have millions of nodes and billions of edges. This poses a significant constraint on the complexity of the models, which ideally have to scale linearly with respect to graph size. Finally, graphs can hold side information, so they require models flexible enough to incorporate this knowledge. 

\vspace{5mm}
The most effective solution to these challenges is Graph Neural Networks (GNNs). In February 2020, a team of researchers from MIT used GNNs to discover halicin, a novel antibiotic capable of killing the world's worst disease-causing bacteria. \cite{Stokes2020} This achievement highlights just one of the many potential applications driving research in GNNs, which we will explore in detail in the next chapter.

### Technical Report
\section{Related Works}

Previous efforts in automating ICD coding primarily focus on improving disease representation and retrieval. Feng et al. (2024) demonstrate that straightforward methods like TF-IDF and cosine similarity still effectively match medical narratives to ICD-11 codes, but their approach lacks contextual awareness, limiting semantic precision \cite{feng2024tfidf}. 

Lee et al.'s ICD2Vec (2023) introduces vector embeddings for ICD codes to capture disease similarity and comorbidity patterns. Their work provides the foundation for our work, as it is tailored for ICD-10 and does not handle natural language queries \cite{lee2023icd2vec}. Chen and Xu (2020) integrate WordNet to enrich ICD-11 descriptions, enhancing semantic alignment without addressing the original ICD texts' inherent sparsity. \cite{chen2020wordnet} Conversely, Klotzman (2024) finds embedding-based methods superior to large language models (LLMs) like GPT-4 in automatic ICD coding. \cite{klotzman2024embed}

Recent transformer-based approaches further highlight the potential of contextual embeddings. PLM-ICD (Wang et al., 2023) successfully applies pretrained transformer models for clinical text coding, though focused on ICD-9 rather than ICD-11, and primarily on long-form documents rather than interactive queries. \cite{wang2023plmicd} DiLBERT provides specialized embeddings for disease-related tasks, outperforming general-domain embeddings in semantic medical tasks. \cite{DiLBERT}

Finally, the ICDBigBird model combines Graph Convolutional Networks (GCN) and transformers to manage lengthy clinical texts, achieving robust ICD assignment performance but at the cost of significant computational overhead, unsuitable for real-time interactions. \cite{ICDBigBird2022}

Overall, existing research underscores the importance of specialized biomedical embeddings but does not comprehensively address the limitations of ICD-11's description sparsity or provide optimized solutions for real-time natural language querying.

## Sample 8 – Discussion
### BSc Thesis extract
A3T-GCN and the decoder are trained end-to-end with backpropagation and loss metric $||Y-\hat{Y}||_2^2$, where $Y$ are the true values of traffic for the next $T$ time steps. The authors compare the results to Auto-regressive Integrated Moving Average (ARIMA), GCN, and GRU, which capture either the temporal or the spatial dynamics of the graph. A3T-GCN scores the highest in accuracy and lowest in Root Mean Squared Error (RMSE), proving its effectiveness in generating expressive representations. 

TGN and the decoder are trained end-to-end with loss $||Y-\hat{Y}||_2^2$. Interestingly, training with negative sampling proved to be the most effective technique. Negative sampling involves randomly selecting one negative edge for each positive edge during training. \cite{Jiang2023} Negative edges are those that do not exist in the original graph and have a weight of 0. Also, note that the authors do not add additional node and edge features. 

\vspace{5mm}
Jiang et al. compare TGN's performance against several forecasting techniques, GCN, and two other DGNNs (JODIE and DyRep). TGN consistently outperformed the DGNNs, surpassed the GCN thanks to the added temporal dimension, and achieved the second-lowest Mean Squared Error (MSE) in all but one instance, where it was the best. These findings validate TGN as an effective DGNN architecture. Strikingly, Persistence Average, a simple technique, almost always the best model. This highlights how TGN may not be sophisticated enough to perform well on temporal edge regression.

### Technical report

\section{Results}
Next, we run the experiments and discuss the results. In section 5.1, we validate our implementation of O-MaMa's original architecture with on-the-fly feature computation, and then attempt finetune. From section 5.2 onward, we use pre-extracted features, and abbreviate O-MaMa configurations by the feature encoder, since that is the main changing block across the experiments, with the rest of the architecture kept fixed. 

\subsection{Reproducing O-MaMa Baseline}
We begin by taking the original architecture of O-MaMa with the pre-trained weights provided by the authors and running inference on our health subset. We obtain a IoU of 0.53, 0.09 points above the reported values in the original work. This confirms the correctness of both our data extraction-pairing pipeline and model implementation. Additionally, it validates our assumption: the Health scenario has fairly predictable dynamics relative to other scenarios. 

Next, we attempt finetuning of O-MaMa's weights (excluding DINOv2, which is kept frozen) over the randomly selected subset of data. Due to computational limitations, we can run training loops for only 20 hours, which amounts to 3 complete training passes and 2 complete validation passes. Nonetheless, we encounter a striking result: finetuning leads to a decrease in IoU \hyperref[tab:omama-baseline-extra]{(Tab 1)}.

\begin{table}[!htbp]
\centering
\begin{tabular}{lcc}
\toprule
Model & IoU $\uparrow$ & IoU Std $\downarrow$ \\
\midrule
O-MaMa baseline   & \textbf{0.533} & \textbf{0.408} \\
O-MaMa finetuned & 0.526 & 0.414 \\
\bottomrule
\end{tabular}
\caption{Comparison of O-MaMa baseline and finetuned variants with on-the-fly feature extraction.}
\label{tab:omama-baseline-extra}
\end{table}

\subsection{Feature Pre-Extraction}
Pre-extraction prevents repetitive calls to DINOv2 during training. On-the-fly extraction takes \(\sim 2.5\) seconds per iteration, whereas pre-extracted tensors reduce processing to \(\sim 0.24\) seconds \hyperref[fig:timing-main]{(Fig 3)}. We also observe a dramatic decrease in time spent during the backward propagation of the loss, a topic we leave for future works. For a more detailed timing profile see Appendix \hyperref[tab:dinov2-timing-comparison]{Tab 7}.
\\

\subsection{Finetuning Analysis}
First, we finetune O-MaMa's baseline architecture using the pre-computed features and we compare it with the finetuned on-the-fly counterpart. The pre-extraction results in a notable decrease in the model performance post finetune \hyperref[tab:precompute-appendix]{(Tab 2)}. Anyhow, we are comparing a model finetuned over 10 epochs vs a model trained over 3 epochs.

\begin{table}[htbp]
\centering
\begin{tabular}{lcc}
\toprule
Model & IoU $\uparrow$ & IoU Std $\downarrow$ \\
\midrule
DINOv2 finetuned (precomputed) & 0.4108 & \textbf{0.4044} \\
DINOv2 finetuned (on-the-fly)   & \textbf{0.5263} & 0.4145 \\
\bottomrule
\end{tabular}
\caption{\textbf{Comparison of precomputed vs.\ on-the-fly feature extraction.} Feature pre-extraction decreases model performance.}
\label{tab:precompute-appendix}
\end{table}

To check the finetune effects on the model performance, we compare O-MaMa with baseline vs. finetuned weights using the DINOv2 pre-extrated features; finetuning significantly decreases IoU \hyperref[tab:backbone-main]{(Tab 3)}.

\begin{table}[htbp]
\centering
\begin{tabular}{lcc}
\toprule
Model & IoU $\uparrow$ & IoU Std $\downarrow$ \\
\midrule
DINOv2 baseline & \textbf{0.5262} & \textbf{0.4036}\\
DINOv2 finetuned & 0.4108 & 0.4044 \\
\bottomrule
\end{tabular}
\caption{\textbf{Comparison of baseline vs. finetuned weights for DINOv2 backbone}. Finetuning decreases model performance.}
\label{tab:backbone-main}
\end{table}

A deeper analysis of the training logs reveals a high degree of overfitting to the training data, with little improvement on the validation \hyperref[fig:backbone-grid]{(Fig. 4)}.

We then analyze the effect of changing the feature extractor backbone on the model performance. From \hyperref[tab:backbone-main-v3-res]{Tab 4}, it can be seen that both alternative models, after model finetuning, perform poorly compared to DINOv2. In particular, DINOv3 presents an especially low IoU parameter, contradicting our prior expectations. The most striking metric is the performance of both feature extractors when the model uses O-MaMa's baseline weights, both of them falling short of 8\%. Training-wise, the logs reveal a similar overfitting picture as DINOv2 (Appendix \hyperref[fig:backbone-grid-v3-res]{Fig. 5}).
For additional metrics on models performance see Appendix \hyperref[fig:all-iou]{Fig. 6}, \hyperref[fig:dinov2-all]{Fig. 8}, \hyperref[fig:dinov3-all]{Fig. 9} and \hyperref[fig:resnet-all]{Fig. 10}.


\begin{table}[htbp]
\centering
\begin{tabular}{lcc}
\toprule
Model & IoU $\uparrow$ & IoU Std $\downarrow$ \\
\midrule
DINOv3 baseline & 0.0791 & 0.2405 \\
DINOv3 finetuned & 0.2897 & 0.3819 \\
\cmidrule(lr){1-3}
ResNet50+DINOv1 baseline & 0.0757 & \textbf{0.2315} \\
ResNet50+DINOv1 finetuned & \textbf{0.3647} & 0.4140 \\
\bottomrule
\end{tabular}
\caption{\textbf{Comparison of baseline vs. finetuned weights for DINOv3 and ResNet50 backbones.} Clear under performance of baseline weights models.}
\label{tab:backbone-main-v3-res}
\end{table}


Three criticalities emerge from our analysis: strong overfitting to the training set, no improvement (and often degradation) in validation IoU, and critical under performance of the model when using baseline weights and pre-extracted features from DINOv3 and ResNet50-DINOv1. The first two issues are addressed in the next section. The latter is likely a consequence of our methodology: projection layers are trained together during the finetuning of O-MaMa's parameters. When switching back to the baseline, interactions are decoupled, leading to low IoU. 

\subsection{Investigating Finetuning Failure}
We inspect the worsening of performance induced by finetuning by plotting masks on individual frames. Looking at situations of high divergence in IoU between predictions of baseline weights and predictions of finetuned weighs (see Appendix \hyperref[fig:delta]{Fig. 7}) we uncover two emergent behaviors: "over-specification" and "easy distraction". 

Over-specification refers to O-MaMa's tendency to reduce the area of the predicted masks by focusing on smaller portions of objects. This is analogous to tuning FastSAM such that it segments details rather than follow object contours. Easy distraction refers to O-MaMa exhibiting a distraction mechanism, segmenting the object that is at the center of the frame under manipulation, rather than sticking to the actual object it should predict. See Appendix Fig. \ref{fig:-emergent-phen} for detailed examples. 

We provide three conjectures that explain such results. First, our starting assumption of the data distribution and object-scene variety in the subset of the Health scenario dataset failed. The set over which we finetuned O-MaMa was too restrictive, meaning that at inference, the model encountered unseen objects. We confirm this thesis by looking at object label overlap across train and test annotations, which we find to be minimal. 

Second, the contrastive loss function corrupted the feature representation by over-emphasizing the minimal object variety of the train set. This lead to space collapse and spurious learning that took the form of the described emergent phenomena. 

Finally, we observed catastrophic forgetting. Borrowed from the Natural Language Processing field, it refers to the degradation of previously learned capabilities when a model is fine-tuned on a new task. O-MaMa's baseline weights, initially trained across multiple scenarios, were fine-tuned on a subset of the Health scenario. The domain shift between the full training distribution and a idiosyncratic Health subset led to performance degradation of the original task.

\subsection{Robustness to Unseen Data}
We test inference of the top two model configurations on our hand-made dataset. We find that finetuned models outperform models with pretrained weights \hyperref[tab:dinov2-finetune-extra]{(Tab 5)}. However, absolute IoUs remain comparable to previous findings, and qualitative results reveal frequent failures under heavy occlusion. See Appendix Fig.~\ref{fig:custom-occlusion} for examples. The difference in performance, that counters the other findings of our report, is likely explained by a coincidental resemblance between our dataset and the finetuning dataset. 