---
layout: post
title: Machine Learning - MRI Analysis Project (Part I)  
date: 2017-05-07 16:22
description: Machine Learning, Projects, ETHZ
---
<div class="img_row" style = "height: 275px;">
	<img class="col three" src="{{ site.baseurl }}/img/MachineLearningHeader.jpg" alt="" title="MachineLearningHeader"/>
</div>
<div class="col three caption"> 
</div>

<i>
*This is an introductory post for those that don’t have yet any significant experience in machine learning. It is divided into two parts, in the first one, the one you are reading right now, I introduce you to a practical project I’ve worked on at ETH. Next, I use this illustrative example to explain two important elements of the machine learning pipeline, namely preprocessing and feature extraction. In the second part of the post that I will share with you later, I will catch up on this and give you an explanation of the remaining steps of the pipeline (model selection and validation). I will then wrap up with some useful takeaways from the project, that you can eventually use in your own work and concise summary.*</i>
<br/>

<i>
*If you happen to be more seasoned in the field of machine learning, don’t worry, I will address follow-up posts to you that go more into depth with things I just touched upon here. Till then, stay tuned.* </i>
<hr>
<br/>

You’ve heard about it many times already. It is supposedly influencing how we cast our votes, putting people off their jobs and even making sure that the pineapples you buy in your supermarket are always fresh. Machine learning is a hot topic and you are increasingly likely to come across it not only in your work, but in your day to day life in general.
<div class="img_row">
	<img class = "col three" src="{{ site.baseurl }}/img/MachineLearningTrend.PNG" alt="" title="MachineLearningTrend" style='height: 100%; width: 100%; object-fit: contain'/>
</div>
<div class="col three caption"> Fig. 1 Popularity of 'Machine Learning' as by Google Trends </div>


### Warm Up
<br/>
I opted for the course for several reasons, one prominent being the belief that project work can very well complement an otherwise rather theoretical course. This proved to be true and I feel I have learned a lot, also thanks to the amount of time invested in it. The project itself was divided into three parts, each of which was to be submitted at the end of following month. The project was set up as a competition and carried out in teams of three. There was in total around 130 teams taking part and competing against each other.

We were given great freedom in how to approach the problem as there was little to no guidance from the part of the instructors. One had to pick up information sort of ‘by-the-way’ during lectures or find it online. This indeed is quite an inefficient way how to get stuff done, especially if one is new in the field, but from the point of learning new concepts and acquiring useful skills, I felt that it did pretty good job. We were also allowed to implement the algorithms in a language of our choice. The most frequent choices being R, Python and Matlab. Our team opted for Python, as we all felt that getting better in it is something we will build on later. It is versatile, fun to work with, and many people, find it more than on-par with Matlab.


### Data and Assignement
<br/>
When one decides to tackle some problem with machine learning, arguably the first thing he must assure is access to data. The more of it he can get the better. Why? Well, imagine trying to complete a crossword puzzle that’s already half full versus a one that has only few words in it. In the former case, you can use the existing letters to support your thinking, whereas in the latter case you often need to do quite a lot of guesswork. Similarly, the more data you have as an input for your algorithm, the more confident will your final predictions be.
In the case of our project, we were provided with roughly 400 3D MRI scans of human brain. Each image is a three dimensional, 176×208×176  array of pixel values. The value of each pixel ranges from 0 to 4096 (ie. 2<sup>12</sup>, corresponding to 12-bit images which are common in medical diagnostics). A slice through typical 3D image can be viewed here:
<div class="img_row">
	<img class="col three" src="{{ site.baseurl }}/img/mri_brain_scan_example.png" alt="" title="MRIexample" style='height: 100%; width: 100%'/>
</div>
<div class="col three caption"> Fig. 2: Three orthogonal cross sections from a typical image from our dataset </div>
<br/>
Our task in the project was divided into three parts: 1) predict person’s age, 2) predict health status and 3) predict age, health status and gender simultaneously. Together with the image data, we were provided with correct labels for 2/3 of the images. This basically means that somebody was noting the person’s age, gender and health status when taking the scan and that we were dealing with <a href="https://en.wikipedia.org/wiki/Supervised_learning">supervised learning</a> problem. Our task then was to make prediction on remaining 1/3 of images. 

### Preprocessing
<br/>
As a first step in practically any image analysis task, one needs to assure that the images can be easily compared against each other, that they don’t include any artifacts from the imaging process and that they contain as much useful information as possible while leaving out what we are not interested in. We call this procedure preprocessing. In the case of MRI scans, the frequent steps in this process include: 
-	brain extraction (removal of non-brain tissue),
-	bias-field correction (compensation intensity gradients in the image that are due to magnetic field non-uniformities during scanning),
-	resizing and
-	voxel alignment across individual scans (voxel is a pixel in 3D space)

In our project, the available data has already been preconditioned with these approaches, thus saving us quite a lot of work that normally needs to be invested in this phase.



### Feature Extraction
<br/>
With good-quality input data one may proceed to arguably the most important part of a machine learning pipeline – the feature extraction. The nature of the process lies in transforming the data from a very high-dimensional space to a space with lower dimensions, while retaining the discriminative information and throwing out what will not help you in decision making. 

To relate this to the task at hand, recall that initially each image represents array of more than 6 million (176×208×176) values while, at the end, you are supposed to give it a label that consists of a single number (either age or 1/0 for healthy/sick or 1/0 for male/female). The big question here is: how does one go from the former to the latter? Intuitively, not all the voxel values will play the same role in prediction. Some regions of brain will be more telling when it comes to predicting age, other when determining health status and so on. The objective in feature extraction process is to find those pixels. There are many tactics how to approach this problem. In a crude division - one can either rely on purely statistical methods, or to include so called ‘expert knowledge’. 

You can better understand this with an example from the project our team was working on. Imagine having to decide whether the brain scan you are looking at belongs to a person struck by a mental illness or not. If you happen to have some knowledge on brain physiology, you may expect such disease to show up by altering the appearance of <a href="https://en.wikipedia.org/wiki/Hippocampus">hippocampus</a>, thus you limit your features to come only from that region. (This is what the most successful teams in this part of the project did.)

On the other hand, if you have little to no knowledge of where the “signal” should come from. then you will likely make use of some general method to disclose structure in your data. In the case of our project, we chose, arguably rather simplistic, method of <a href="https://en.wikipedia.org/wiki/Histogram">histograms</a>. One clear disadvantage of histograms you could immediately think of is that they don’t preserve spatial relationships in the data (i.e. when histogram counts pixel values, it doesn’t care about where that pixel comes from). In order to partly circumvent this, we divided the large 3D scan into 27 (3×3×3) smaller cubes and only subsequently computed histograms in each of those. In this way, we retain some, although arguably very crude, information on the distribution of pixel values in space. This is illustrated in the following image:
<div class="img_row" style = "height: 100%; min-height: 100% !important;">
	<img class="col three" src="{{ site.baseurl }}/img/MLhistograms.png" alt="" title="MLhistograms"/>
</div>
<div class="col three caption"> Fig. 3: Visualization of Multi-histogram approach (Author: Viktor Wegmayr) </div>
<br/>
Note, that as we used 35 bins per histogram (dividing the span from 0 to 4096 into 35 equidistantly spaced intervals and counting intensity value occurrences in those), we reduced dimensionality of our problem more than 6 500× by this simple procedure.
 
Further quasi-necessary step to be done either in the preprocessing or feature extraction stage is data conditioning. This is done mainly to improve numerical stability during subsequent steps. The common approach is to scale the data to unit variance and remove their mean, so that they are centered around zero.

<div class="img_row" >
	<img class="col three" src="{{ site.baseurl }}/img/prepro1.jpeg" alt="" title="Preprocessing" style='height: 100%; width: 100%; object-fit: contain'/>
</div>
<div class="col three caption"> Fig. 4: It is common practice to zero-center data and normalize it to unit variance (<a href = "http://cs231n.github.io/neural-networks-2/">Image source</a>) </div>
<br/>

### Intermezzo
<br/>
This has been the first of the total of two parts of introductory post on machine learning. If you enjoyed reading it and want to find out about other crucial elements of the pipeline including model selection and validation, stay tuned for the second part that is to come in just a few weeks!



<br/>
*Note:
The title image is courtesy of <a href="http://brainposts.blogspot.ch/2010/11/brain-mri-white-matter-intensities.html" target="blank">Brain Posts blog</a>.*
