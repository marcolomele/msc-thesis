# Chapter Brief: Introduction

---

You are helping me draft the Introduction of my master's thesis.
The project knowledge files contain my style guide, scope, writing samples,
and thesis outline. Read them before drafting.

**Chapter**: Introduction
**Target length**: 3
**Arc**: Our work first. 
**Deep treatment**: Real world application to stress importance, phylosophy of foundational modern models in Machine Learning world, pipeline contributions. 
**Surface only**: NA
**Skip**: NA
**Connect back to**: NA
**Connect forward to**: NA

**Paper section to expand** (follow this order of information):
\section{Introduction}
\label{sec:intro}
Identifying an object from any perspective is a fundamental capability for
intelligent vision systems operating with multiple cameras. Applications range
from human-robot collaboration~\cite{collab_1}, such as pairing the view of an
aerial vehicle with the view of a human operator~\cite{collab_2}, to augmented
reality assistants~\cite{wearable} and multi-robot manipulation~\cite{multirobot_1,
multirobot_2}. Object correspondence, the ability to match objects across
complementary views, becomes essential to enable collaboration across multiple
agents. Despite progress in object segmentation from single pictures, cross-view
segmentation remains an open challenge.

\begin{figure}[t]
  \centering
  \includegraphics[width=0.97\linewidth]{imgs/figure-1-temp.png}
  \caption{figure summarising ego-exo correspondencce with ground truth masks,
  one canonical case and two difficult cases}
  \label{fig:cross-view-corr}
\end{figure}

Ego-Exo4D \cite{grauman2024egoexo4d} is the most comprehensive public
resource for studying this task, holding hundreds of multi-view video recordings
of human activities captured simultaneously from a first-person egocentric camera
held by the participant and one or more exocentric cameras placed at fixed
positions around the scene. We address its cross-view challenge: given a
per-frame segmentation mask track for an object in one view, predict the mask
track for the same object in the other view. This task is particularly demanding
because the two views focus on different aspects of a scene. The egocentric
perspective captures object detail and manual interactions, but lacks scene
context, suffers from motion blur, and presents frequent occlusion from hands.
Conversely, the exocentric perspective provides a global view of the environment
and the person's body, but compromises on pixel-level information of each object.
As a result, between the two views, the same object differs drastically in scale,
angle, position in the frame, and color due to different lighting conditions.
Moreover, cameras lack geometric calibration information, which renders
triangulation-based approaches unfeasible. Finally, the large variety in recorded
scenes, camera optics, and imaging conditions translates into a relatively wide
domain shift range.

\begin{figure*}
    \centering
    \includegraphics[width=\linewidth]{imgs/pipeline-diagram.png}
    \caption{Pipeline temp figure}
    \label{fig:pipeline}
\end{figure*}

Current state-of-the-art methods~\cite{hu2025lmeec, o-mama, pan2025v2sam}
achieve competitive results by learning cross-view feature correspondences via
trained modules. However, these approaches carry two crucial limitations. First,
they rely on training pipelines that tie the model to the distribution of
available cross-view data, preventing them from working on novel camera
configurations or scenarios without retraining. Second, they lack
interpretability: any study of why a mask was produced for a particular object in
a particular frame is limited to attention maps, which hinders adoption in
safety-critical applications~\cite{xai-sensors}.

These limitations reflect a broader paradigm in computer vision, in which
problems are addressed by designing architectures and collecting supervision for
each specific task. The emergence of large language models has demonstrated that
models trained on broad data and used without task-specific fine-tuning can match
or exceed specialist systems \cite{wu2023exploringtradeoffsunifiedlarge,
kim2023languagemodelssolvecomputer, gruver2024largelanguagemodelszeroshot}. Maes
et al. \cite{maes2026leworldmodelstableendtoendjointembedding, assran2025vjepa2selfsupervisedvideo} argue that a similar transition is forthcoming in
vision, driven by models that acquire a general understanding of the world through video rather than specialized pattern recognition. Our work is motivated by this hypothesis: if foundation models for vision are general enough, carefully composing them at inference time should transfer to novel tasks without retraining.

To this end, we propose a fully training-free pipeline in which
natural language serves as the bridge between viewpoints, illustrated in
Figure~\ref{fig:pipeline}. No component is fine-tuned for the task, the bridge
between views is a human-readable description, and any erroneous prediction can
be localised to the specific block whose output is at fault.

Our pipeline achieves $37.7$ (\textit{Ego2Exo}) and $40.6$ (\textit{Exo2Ego})
IoU on the test v2 split, representing a relative gain of $9\%$ and $62\%$
against the official challenge baselines respectively. These results fall within
a few percentage points of O-MaMa~\cite{o-mama}, the winner of the 2025
Ego-Exo4D Correspondences Challenge. In this paper, we first describe the full
pipeline and benchmark our approach against published methods. We then examine
the remaining gap with a detailed ablation study, providing insights into the
effectiveness of foundation models when exposed to a task none were designed for.

---

## Post-Draft Checklist

- [ ] Voice matches style guide (read the Red Flags section)
- [ ] Structural flow matches the paper section's order of information
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Every figure is referenced in text before it appears
- [ ] Forward and backward connections are present
- [ ] No concept is explained that scope.md says to skip
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Closing paragraph pivots correctly to what comes next
- [ ] Nothing sounds like it was written by an AI
