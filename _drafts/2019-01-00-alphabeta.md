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

As we compare every event to a standard template, that means for every known signal, we have to painstakingly recreate the template waveform for every type of signal. This procedure has to be repeated if there is a minor change to detector system as a simple temperature can affect the way of the waveform detected. However, the signal is not always necessary consistent for the same kind of signal. Noises within the data can distort the interpreted information and it is heavily dependent by us as physcist to identify all possible senarios beforehand. This labourous task is proven to be effective in the data analysis process in our physics experiment.

Unsupervised Learning as Anomaly detection
====

Within physics, machine learning has gain quite the popularity due to versatility of neural network can be applied in multitude of different ways. In some cases, improving the result compared to other know techniques/algorithms. With the wreath of data available in physics, it is no surprise how beneficial ML technique to physicist where ML techniques are mostly driven by the vast availability and size of data.

A particular algorithm in ML, t-Distributed Stochastic Neighbor Embedding (t-SNE) is an extremely useful technique where dimensionality of data can be reduced and the dataset can be visually represented in 2D/3D plots. This can be useful to observe the number of subset that present in the dataset. This can prove useful in physics experiments especially in experiments where physicist are hunting signal is unknown to mankind, ie:- dark matter. As we lack the full knowledge of how dark matter interacts normal matter of our detectors, we can only best predict dark matter signal by getting rid of known signals or predict the signal based on theoretical assumption of how dark matter might produce the signal. t-SNE might be a useful technique to visualise the number of subset from experimental dataset.

A particular type of neural network, auto-encoder where it is used to reproduce an inputted image where information of the inputted image is condensed into size that is much smaller than the input image but able to reproduce the inputted image from the condensed size of information. Albeit not a 100%  reproduction of the input image, it is capable to reproduce an image that closely resemble the input iamge hereby creating an artificial image. How does an auto-encoder condense the large input information into smaller sized information? Are neural auto-encoder capable to learn by itself mathematically of condensing large information? This part of an auto-encoder is similar to how experimental physicist reconstructs variable from raw experimental data. The raw experimental data akin to image captured by camera where the reconstructed variable are the information in the image such as position of face in an image. 

Here I am going to explore this aspect of a Variational Auto-encoder(VAE) where the latent variable learned by VAE from the pulse shape is then subsequently visualise via t-SNE on a 2-dimensional plot. I will also attempt to compare how well this method performs to conventional $\chi^2$ method.

![vis]

Training the VAE
====
The goal of the VAE is to reproduce the waveform of a single events. Therefore the only information that will be fed into the VAE is the waveform only. To train the VEA, I used actual waveforms from raw uncut dataset from experiment.(~50,000 waveform)

![train]

To ensure every node in the VAE is properly trained, I made additional training data by flipping the waveform backwards and randomly shift the pulse randomly and generate pure noise (np.flip/np.roll). At the end I have about ~200,000 waveforms that can be used to train the VEA. The goal was of the training is to reproduce the input waveform as close as possible but not necessary a perfect clone of it. A general reproduction of generic shape of the waveform would be sufficient. The goal here is that the VAE is able to encapsulate the primary features of waveform in its latent variable.

![vae] 

Separating 2 different events
====

Using 2 dataset that was used to create the template waveforms, I used them and tested them on the trained VAE. The 2 dataset are combined and shuffled while retaining the label for checking. The difference between the template dataset was shown as below:-

![alphabeta]

The main difference between the two dataset is in the early portion of the waveform.

![2zoom]

This shuffled data set is then feeded into the Encoder of the VAE to obtain the latent variable that represent the waveform of the dataset. This "transform" latent variable dataset is pprocess through a t-SNE algorithm where each event is represented on a 2 dimensional plot.

![tsne]

Here we can clearly see that there is 2 distinct cluster on the t-SNE plot which does show that this dataset does indeed have 2 different type of waveform. To verify the that the 2d data points on the t-SNE plot does indeed correspond to the 2 waveform, I labelled each point according to the waveform template dataset that is was derived from.

![tsneori]

The result does reflect the 2 distinct group of data. the overlapping of some point is as expected due to the similarities in the waveform shape. The conventional method, $\chi^2$ fitting also produces similar distribution. The $\chi^2$ parameter that was originally derived to separate these 2 group of data, also does show some overlapping. 

A $\chi^2$ fitting parameter that considered the whole information of the waveform.

![psd1]

A $\chi^2$ fitting parameter that only considered the early part of the waveform. This is the parameter that was used in our physics analysis to distingusih these 2 type of waveforms.

![psd7]

The tsne plot using the cut off point in the $\chi^2$ parameter distribution

![tsnepsd]

Adding 3rd type of events into the mix
====
To verify that the result of the latent variable of the VAE shown through t-SNE is not a fluke. I added an additional different type of dataset into the previous 2 dataset. This 3rd dataset have a larger difference with the previous 2 where this 3rd dataset is a sharp repidly decaying pulse. The shape itself is distinctively different.

The plot clearly shows this 3rd dataset, "LS" is clearly different from the previous 2 "beta" and "alpha".

![3gr]

The dataset are shuffled while retaining the label it comes from for checking purposes. The shuffled dataset is process through the encoder of the VAE to obtain the latent representation and visualised through t-SNE. The same procedure as distinguish the 2 group dataset.

![3tsne]

The plot below shows the origin of the dataset it comes from.

![3tsneori]

The plots below shows if the labelling of the data done based on the distribution of a $\chi^2$ fitting parameter.

![3dist]

![3tsnepsd]

Applying to Actual dataset
====
To see how well this perform in an actual physics experiment dataset, I apply the same VAE and visualise the latent variable with the same method as mentioned above. Primarily goal here is to see whether clustering of data can be formed where the VAE learns the dataset features based solely on the waveform information only. The t-SNE plot is as shown

![roi]

At first glance, we can see many clusters of data forming but as t-SNE is only a visualise the data in 2dimensional space, we cannot conclude about the formation of the clusters seen. 

Comparing to curve fitting method
====
Here I labelled each data point according the distribution of the $\chi^2$ parameter.

![roipsd]

![roipsd1]

![roipsd7]

In the t-SNE map, we can see there is that latent variable learnt by the VAE is is able to group shape that are similar. It is possible to use the latent variable of the VAE for event selection. This open up the opportunity to fully categorize the physics data and possible for the use in anomally detection to search for rare "out of the group" type of data. 

Comparing latent variables to conventional parameters
====
High energy events
----

Event positions
----

Event time lags**
----
The time difference between two event(comparing to the event before)

Energy corrections**
----



Can we draw parallel of this method to the conventional raw->reconstruction->signal???
====

====

training VAE with GAN
====
The VAE that was trained from scratch using GAN manage to perform similarly to training a standalone VAE.


Aspects to consider:-
* Analogous to physicist in creating a discriminating parameter to separates different events. Are VAE capable of deriving its own parameter to achieve the same thing?
* If VAE can derives discriminating parameters by itself, how can we interpret it effectively?

[beta]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/beta.png
[sharp]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/noise.png
[vis]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/sketchblog.png
[train]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/train.png
[vae]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/vae.png
[tsne]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/tsne.png
[tsneori]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/tsneori.png
[tsnepsd]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/tsnepsd.png
[alphabeta]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/2sam.png
[2zoom]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/2samfo.png
[psd1]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/psd1.png
[psd7]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/psd7.png
[3gr]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/3gr.png
[3tsne]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/3tsne.png
[3tsneori]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/3tsneori.png
[3tsnepsd]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/3tsnepsd.png
[3dist]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/3distribution.png
[roi]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/tsneroi.png
[roipsd]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/tsneroipsd.png
[roipsd1]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/roipsd1.png
[roipsd7]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/roipsd7.png

