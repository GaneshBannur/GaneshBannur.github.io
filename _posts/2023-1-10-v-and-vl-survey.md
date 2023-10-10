---
layout: post
title: "Survey of vision and vision-language tasks for robots"
date: 2023-1-10
---

This is a survey of techniques and datasets for some vision and vision-language tasks. The survey was done on tasks which could be useful to a robot. A variety of tasks are explored:
* ***Extraction of relevant information from the surrounding scene***  
    This is done through the robot's vision pipeline. Different extraction methods are applied on the robot's video stream to extract different types of information from the scene. Object detection, event detection, human action detection and text detection and recognition are some of the key extraction methods applied. Text detection and recognition techniques are explored in more detail [here]({% post_url 2023-1-10-v-and-vl-survey %}).  
    Retrieving extracted information based on natural language inputs provided by humans is also very important for a robot. Some techniques to retrieve objects in the scene based on their description are also explored.   
* ***Navigation***  
    Navigation is a fundamental task for a robot. Recent techniques have explored navigation as a vision-language task in which the robot must navigate based on natural language commands and its video stream.
* ***Gaining visual knowledge***  
    In order to perform generic tasks such as zero-shot navigation in a new environment or visual-question answering based on the current scene, a robot must have large scale visual knowledge. A robot can either query an external knowledge base or accumulate knowledge in the parameters of a deep learning model. Recent work has explored the latter since end-to-end models with large scale visual knowledge perform better at visual tasks. Some papers proposing frameworks for large scale visual knowledge acquisition are explored.  
* ***Miscellaneous***  
    Other interesting papers on datasets, custom architectures for video tasks, pretraining and finding specific moments in video are also surveyed.   

Papers proposing techniques for these categories are listed. The key ideas put forth by papers in each category are summarized.

<div style="text-align:center">
    <a href="/assets/v-vl-survey/Vision-Query-Literature-Survey.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/v-vl-survey/Vision-Query-Literature-Survey.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>

Here is a survey of datasets available for the tasks mentioned above.

<div style="text-align:center">
    <a href="/assets/v-vl-survey/Vision-Query-Datasets.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/v-vl-survey/Vision-Query-Datasets.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>

# Predicting the future
Humans are able to predict the future. Well, at least in the short term. We are able to judge facial expressions and predict what a person may do, observe movements and predict where a person is headed and in general predict future actions based on familiar visual cues received till now. Predicting the intent or next action of a human based on current and past video frames is crucial for robots to be proactive rather than reactive when interacting with humans. Predicting the short term (or even better - long term) goals of a person will let a robot take preemptive action. Current work focuses on video-based prediction as it is easy to acquire a video feed from a robot. [**1**] justifies this by showing that video-based prediction is not inferior to using 3D kinematics, which is able to capture every dimension of human motion.

Here is a survey of techniques that have been used for predicting human actions and intents

<div style="text-align:center">
    <a href="/assets/v-vl-survey/Vision-Based-Intent.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/v-vl-survey/Vision-Based-Intent.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>

And here are some datasets that exist for this task

<div style="text-align:center">
    <a href="/assets/v-vl-survey/Vision-Based-Intent-Datasets.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/v-vl-survey/Vision-Based-Intent-Datasets.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>

# References
[1] Zunino, Andrea, et al. "What will i do next? The intention from motion experiment." Proceedings of the IEEE conference on computer vision and pattern recognition workshops. 2017.