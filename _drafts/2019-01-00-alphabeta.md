---
title: 'A different perspective to Pulse Shape Discrimination: Unsupervised technique in waveform analysis'
date: 2019-01-01
permalink: /posts/2019/alphabeta/
tags:
  - Research
  - Unsupervised Learning
  - Event Selection
---
Event Selection based on Pulse Shape
====
In my research work as a graduate student, one particular task I would encounter as an experimental physicist is event selection. It is a procedure where we pick the signal we desired to study from raw experimental data that is often filled with multitude of unwanted background. By anticipating how would the desired signal show up in our experiment(ie:- a certain pulse shape of the waveform detected), we picked waveforms that closely resembled to the ideal signal waveform desired to study. In reality, it is difficult to know exactly how the signal would appear in the experiment but with some prior knowledge of some properties such as it energy and the pattern of how its distributes its energy. We can anticipates how the signal pulse shape appears in the experiment. Often we also used known sources to characterise the experiment itself.

In our experiment collaboration, the signal is obtain by comparing to a standard template waveform of the desired signal. These template waveform is used to compare each and every events(pulses/waveform) in the experimental dataset through the usual $\chi^2$ curve fitting. The lower the $\chi^2$ value is, the more likely the event is to the signal. 

For example, to do an event selection of single beta decay events. To make the template waveform, we make this template waveform from a well known beta event events 2.615keV Tl208 peak. By select energy of events in a narrow region, ie:- 2.665keV to 2.675keV in combination with other parameters that describes the general shape of the waveform, we obtain the averaged waveform where it is used as the template waveform. One such parameter used is the ratio between the area under curve of the early and later portion of the pulse/waveform (ie:- the ratio of area under the curve before and after xx millisecond).

![beta]
An example of the stadard template, the vertical line indicates the cut-off used to calculate the ratio of the area beneath the waveform.

Signal noise
![Sharp Fluctuation][sharp]

As we compare every event to a standard template, that means for every known signal, we have to painstakingly recreate the template waveform for every type of signal. This procedure has to be repeated if there is a minor change to detector system as a simple temperature can affect the way of the waveform detected. However, the signal is not always necessary consistent for the same kind of signal. Noises within the data can distort the interpreted information and it is heavily dependent by us as physcist to identify all possible senarios beforehand. Are machine learning tools can be incorperated into 

Unsupervised Learning as Anomaly detection
====

Within physics, machine learning has gain quite the popularity due to versatility of neural network can be applied in multitude of different ways. In some cases, improving the result compared to other know techniques/algorithms. With the wreath of data available in physics, it is no surprise how beneficial ML technique to physicist where ML techniques are mostly driven by the vast availability of data.

A particular algorithm in ML, t-Distributed Stochastic Neighbor Embedding (t-SNE) is an extremely useful technique where dimensionality of data can be reduced and the dataset can be visually represented in 2D/3D plots. This can be useful to observe the number of subset that present in the dataset. This can prove useful in physics experiments especially in experiments where physicist are hunting signal is unknown to mankind, ie:- dark matter. As we lack the full knowledge of how dark matter interacts normal matter of our detectors, we can only best predict dark matter signal by getting rid of known signals or predict the signal based on theoretical assumption of how dark matter might produce the signal. t-SNE might be a useful technique to visualise the number of subset from experimental dataset.

A particular type of neural network, auto-encoder where it is used to reproduce an inputted image where information of the inputted image is condensed into size that is much smaller than the input image but able to reproduce the inputted image from the condensed size of information. Albeit not a 100%  reproduction of the input image, it is capable to reproduce an image that closely resemble the input iamge hereby creating an artificial image. How does an auto-encoder condense the large input information into smaller sized information? Are neural auto-encoder capable to learn by itself mathematically of condensing large information? This part of an auto-encoder is similar to how experimental physicist reconstructs variable from raw experimental data. The raw experimental data akin to image captured by camera where the reconstructed variable are the information in the image such as position of face in an image. 

Here I am going to explore this aspect of a Variational Auto-encoder(VAE) where the latent variable learned by VAE from the pulse shape is then subsequently visualise via t-SNE on a 2-dimensional plot. I will also attempt to compare how well this method performs to conventional $\chi^2$ method.

![vis]

Training the VAE
====
The goal of the VAE is to reproduce the waveform of a single events. Therefore the only information that will be fed into the VAE is the waveform only. To train the VEA, I used actual waveforms from raw uncut dataset from experiment.(~50,000 waveform)

![train]

To ensure every node in the VAE is properly trained, I made additional training data by flipping the waveform backwards and randomly shift the pulse randomly and generate pure noise (np.flip/np.roll). At the end I have about ~200,000 waveforms that can be used to train the VEA. It is not necessary that the VAE has to reproduce the waveform perfectly but sufficiently captures the features of the waveforms in its latent variable. The reproduce waveform captures the primary features of the waveform is sufficient for such task.

![vae] 

Separating 2 different events
====
Using 2 dataset that was used to create the template waveform, a blind test was carried out. The 2 dataset are combined and shuffled while retaining the label for checking(only waveform information was used, the labels are hidden away-blinded). Initial plot of the t-SNE show promising result that the VAE were able to identify unique features of respective group of waveforms. Although the clustering was not perfect as there are some overlap. These overlap are of no indication of misidentification but rather the uncertainties exist in the dataset itself.

![alphabeta]

Adding 3rd unique type of events
====
Here I added a different type of dataset into the mix where the difference of this additional dataset is larger than the difference of the previous 2. Here I am going test if the clustering this time will lump the 2 previous group together as they have smaller difference in shape compared to this additional events.

Here we can see a distinct 3 different clusters which corresponds to the 3 different dataset

Applying to Actual dataset
====
This is a dataset where events happens nears the region of interest based on the reconstructed energy parameter from the waveforms. This energy parameter is primarily based on the height and area under the pulse as these two attributes is proportional to the energy release per events and the amount of scintillating light.

Here we see a much more messy clustering where there is no single dominant cluster can be singled individually. This reflects that there is not a clearcut separation can be made of the dataset. However if we are to solely separate the dataset through the pulse shape only. This will provide an alternative method to analyse the dataset.

Calculating the half life limit of $0\nu\beta\beta$ of Ca$_{48}$
====

Improving VAE with GAN
====
Produced a similiar result to standalone VAE. However, during training, the loss stll slowly decreasing which an indication the GAN is still learning. The waveform quality reproduced by VAE also show considerable improvement.


Aspects to consider:-
* Analogous to physicist in creating a discriminating parameter to separates different events. Are VAE capable of deriving its own parameter to achieve the same thing?
* If VAE can derives discriminating parameters by itself, how can we interpret it effectively?

[beta]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/beta.png

[sharp]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/noise.png
[vis]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/sketchblog.png
[train]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/train.png
[vae]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/vae.png
[alphabeta]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/2sam.png
[2zoom]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/2samfo.png
