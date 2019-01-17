---
title: 'A different perspective to Pulse Shape Discrimination: Unsupervised technique in waveform analysis'
date: 2019-01-01
permalink: /posts/2019/alphabeta/
tags:
  - Research
  - Unsupervised Learning
  - Waveform Classification
---
Event Selection based on Pulse Shape
====
In my research work as a graduate student, one particular task I encounter as an experimental physicist is event selection. It is a procedure where we pick the signal we desired to study from raw experimental data that is often filled with multitude of unwanted background. By anticipating how would the desired signal show up in our experiment(ie:- a certain pulse shape of the waveform detected), we picked waveforms that closely resembled to the signal desired to study. We can anticipates the signal pulse shape through prior knowledge such as the energy of a particular radioactive decay or the decay constant of the signal in the detector system. 

In our experiment collaboration, the signal is obtain by comparing to a standard template waveform of the desired signal. This template waveform is used to compare each and every events(pulses/waveform) in the experimental dataset through standard $\chi^2$ curve fitting. The lower the $\chi^2$ value is, the more probable the event detected is the signal we desired. 

For example, to do an event selection of single beta decay events. To make the template waveform, we selects events that is slightly higher than the 2.615keV in an extremely narrow region, ie:- 2.665keV to 2.675keV. There a few other reconstructed parameters that was used to make this event selection such as the ratio of area under pulse between different portion of waveform. This template waveform is obtained by averaging the waveform shape of all the narrowly survive events where we can say with a high probability that this template waveform is the shape of a single beta decay event.

As $\chi^2$ involved comparing every single point of the waveform to the template, the computed $\chi^2$ value includes the fluctuations within the pulse(noise) which result in a higher than expected $\chi^2$ value. Although the noise are part of the signal itself, but if a sudden sharp fluctuation due to external noise(from electrical noise/etc.), the $\chi^2$ value would be outside our desired range and considered as a background. Although we considered these sharp fluctuations as background, the final value we are concerned is the energy of the events where usually we can obtain the actual energy value where such sharp fluctuation are ignored in the reconstruction of the energy.

![Sharp Fluctiation][sharp]

As we compare every event to a standard template, that means for every known signal, we have to painstakingly recreate the template waveform for every signal. This procedure has to be repeated if there is a minor change to detector system as a simple temperature can affect th quality of the waveform detected. There should be a much simpler and efficient way to do event selection which give better certainty for event selection.

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

To ensure every node in the VAE is properly trained, I made additional training data by flipping the waveform backwards, randomly shift the pulse randomly and generate pure noise (np.flip/np.roll/np.shuffle). At the end I have about ~200,000 waveforms that can be used to train the VEA. In a batch of 200 waveform, the VAE was train with 20 epoch so that VAE reaches a steady state. It is not necessary that the VAE to reproduce the waveform perfectly but sufficiently captures the features of the waveforms in its latent variable.

![vae]

[sharp]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/noise.png
[vis]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/sketchblog.png
[train]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/train.png
[vae]: https://raw.githubusercontent.com/hareyakana/hareyakana.github.io/master/images/vae.png

