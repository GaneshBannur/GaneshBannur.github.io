---
layout: post
title: "Conversational Agents: A survey of techniques, frameworks and datasets"
date: 2022-11-1
---

This article is a survey of conversational agents that I did when I was working to build one myself.  
<br>
# Architecture
The general architecture of a conversational agent is given in *Figure 1*. Traditional techniques follow this architectural division closely and implement specialized modules for each component of the agent. However it is becoming increasingly common to combine one or more components in favour of more end-to-end conversational agents. This is especially true when using large deep learning models as they are able to implement more than one component, often with better results due to information sharing between the tasks of individual components.

![](/assets/ca/architecture.png)
<h4 align="center">
<i>Figure 1: Architecture of a Conversational Agent</i>
</h4>
<br>
# State-of-the-art techniques
Here's a survey of state-of-the-art techniques for building entire conversational agents or one or more individual modules shown in *Figure 1*. All papers are linked in the table and also at the end. A few other interesting papers, papers on human-in-the-loop techniques and studies on building conversational agents are also linked at the end.

<div style="text-align:center">
    <a href="/assets/ca/Literature-Search.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/ca/Literature-Search.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>
<h4>State-of-the-art techniques for each module</h4>
Here is a grouping of papers based on which architectural component their techniques apply to. The key points of each paper are summarized.

<div style="text-align:center">
    <a href="/assets/ca/Papers-By-CA-Module.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/ca/Papers-By-CA-Module.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>
# Frameworks
Frameworks are an important starting point for the development of a conversational agent as they can provide a standard data format, pretrained models, dialog system pipelines, etc. I ultimately chose Rasa to implement my conversational agent but there are many frameworks out there suited for a wide variety of tasks. Here's a survey of the most popular platforms and frameworks available.

<div style="text-align:center">
    <a href="/assets/ca/Platforms-And-Tools.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/ca/Platforms-And-Tools.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>
# Datasets
Datasets are necessary for training custom models to use for language understanding/dialog management or for training end-to-end conversational agents. Good quality datasets are very important for dialog applications since the agent needs to produce good quality text to hold a satisfying conversation. Here is a survey of the most popular datasets available for training conversational agents.

<div style="text-align:center">
    <a href="/assets/ca/Datasets.pdf"><center><i>Download the document or open it in a new tab</i></center></a>
    <object data="/assets/ca/Datasets.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>
# Additional notes on select papers

<div style="text-align:center">
    <a href="/assets/ca/Paper-Notes.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/ca/Paper-Notes.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>
<br>
# Further Reading
<i>Speech and Language Processing</i> by <i>Dan Jurafsky</i> and <i>James H. Martin</i>  
Chapter 15: <i>Chatbots & Dialogue Systems</i>  
[https://web.stanford.edu/~jurafsky/slp3/15.pdf]()

<i>Conversational Agents: Theory and Applications</i> by <i>Mattias Wahde</i> and <i>Marco Virgolin</i>
[https://browse.arxiv.org/pdf/2202.03164.pdf]()  
Here's an extremely short summary of the key points they present.

<div style="text-align:center">
    <a href="/assets/ca/Conversational-Agents-Theory-and-Applications-Summary.pdf"><i>Download the document or open it in a new tab</i></a>
    <object data="/assets/ca/Conversational-Agents-Theory-and-Applications-Summary.pdf" type='application/pdf' width="100%" height="1000"></object>
</div>