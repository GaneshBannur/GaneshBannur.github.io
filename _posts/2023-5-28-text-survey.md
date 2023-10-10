---
layout: post
title: "Survey of text detection and recognition"
date: 2023-5-28
---

This post is a brief survey of the techniques in text detection and recognition.

Extracting text from images, called text spotting, is arguably the most important technique of extracting information from images. This is because text is the most dense representation of information present in images. Real-world scenes are rich in text and identifying the text present in a scene can vastly improve the understanding of the scene.  

Traditionally text spotting has been divided into text detection and text recognition, both performed by separate methods. This is true even of recent deep learning methods. There are dedicated text detection and recognition models. Recently there has been an increased effort to train end-to-end text spotting models, which are single models that recognize the text in an image in one shot. These methods are expected to have better results due to information sharing between the tasks of detection and recognition. While these end-to-end methods are improving they are still behind pipelined methods which break the task down into detection and recognition. This is evidenced by [**1**], in which they state

> It’s widely believed that end-to-end models enjoy shared feature extraction which leads to better accuracy. However, the results of our competition say otherwise.  

In that spirit the rest of this post gives surveys and comparisons of text detection and recognition techniques. However, keep in mind that only papers with official code implementations were considered since I was surveying techniques to integrate them into a pipeline.

<br>

# Detection
I ultimately chose to use the unified detector since it produced the best results empirically. However the boundary transformer method is the best on paper and may be better in other scenarios. Here is a comparison of techniques on the same reference image along with the results from the corresponding paper.

<div style="text-align:center">
    <a href="/assets/text-survey/Text-Detection.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/text-survey/Text-Detection.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>

# Recognition
Here is a survey of text recognition techniques. The result tables for some highlight techniques are given.
<div style="text-align:center">
    <a href="/assets/text-survey/text-recognition.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/text-survey/text-recognition.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>

# References
[1] Long, Shangbang, et al. "ICDAR 2023 Competition on Hierarchical Text Detection and Recognition." arXiv preprint arXiv:2305.09750 (2023).