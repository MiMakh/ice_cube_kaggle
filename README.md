# IceCube - Neutrinos in Deep Ice Kaggle Notebooks
This repository contains my notebooks and solutions for IceCube competition on Kaggle.
The notebooks are:
  1. EDA and first look
  
    * First look at the dataset and preliminary analysis
  2. Constucting features
  
    * Features engineering using Polars
  
  3. EDA and first look
  
    * First look at the dataset and preliminary analysis
  4. 2 dots model
  
    * Making a baseline model by connecting 2 dots
  5. Classification of the events
  
    * Scientific papers overview and my approach to the classification of the events

  5. Pytorch model
  
    * Final NN model using pytorch


# Intro

*Disclaimer: I am not an expert of any sort in astrophysics. These are just findings from the papers that were recommended to read and my personal observations. I apologies for any errors / mistakes in this notebook.*


In this notebook we do a quick overview of classification problem, literature overview and suggest some solutions for classification.


**What are the different events?**

Per year, [IceCube reads out roughly](https://storage.googleapis.com/kaggle-forum-message-attachments/1958559/18618/kaggle_webinar_small.pdf):
   1. 10^10 events caused by atmospheric muons
   2. 10^9 events caused by noise
   3. 100 000 events from atmospheric neutrinos
   4. A handful of very high energy events likely to be of astrophysical origin
   
   
   
The above is the split of events in the real life, however, in our Kaggle competition we have synthetic data and from what we know from the [presentation](https://storage.googleapis.com/kaggle-forum-message-attachments/1958559/18618/kaggle_webinar_small.pdf): **every event has a neutrino in them (either atmospheric or of astrophysical origin)**, some events have noise + atmospheric muons and thus are hard to reconstruct.

## We can classify the events as following:

1. Events that are easy to reconstruct (no dominating noise, atmospheric muons)
2. Events that are hard to reconstruct (a lot of noise and/or atmospheric muons)



## What do these events look like? Showers/Cascades and Tracks

Neutrino interactions in IceCube have two primary topologies:

* **Showers (or Cascades)**: When a high-energy neutrino collides with a nucleus, it produces a cascade of particles, including charged particles such as electrons and muons, as well as gamma rays and other types of radiation. These particles then continue to collide with other nuclei in the atmosphere, producing more particles and radiation in a cascading effect.
* **Secondary muon tracks**  are created when neutrinos crash into other particles. This impact causes a big explosion of even smaller particles called hadrons. In this explosion, some of the particles are muons that leave behind a special track that we can detect.

Approximately 80% of the observed events would appear as [showers](https://arxiv.org/pdf/1311.5238.pdf).


## Example of neutrino-induced particles shower

Thanks to [edguy99](https://www.kaggle.com/edguy99) for the construction of these animations. ([forum discussion](https://www.kaggle.com/competitions/icecube-neutrinos-in-deep-ice/discussion/388858))

![neutrinourl](https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F67794%2F7aae4ef747964f42d82a331d8d64040a%2Fevent_333339210.gif?generation=1676956714333381&alt=media)


In here we see an example of 'cascade-like' behavior - the neutrino 'explodes' inside the detector and deposits its energy in there.

Here is an illustration from a [paper](https://arxiv.org/pdf/1311.4767.pdf)


## Example of neutrino-induced muon-track


![track](https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F67794%2F0a0ddfd0fdf99900db06e390ad97c547%2Fevent_509612375.gif?generation=1676956257096545&alt=media)

Here is an illustration from [paper](https://arxiv.org/pdf/1311.4767.pdf) with captions that reads that muon has started in the detector and escaped through one of the facets.



# What are hard to reconstruct events?

We know that each event has a neutrino in it, however, in some cases these neutrinos are hard to detect. We want to concentrate specifically on classifying the atmospheric muons.


Animation of hard to detect neutrinos - you can see the line of neutrinos which is kind of hard to make sense given the lightened up detectors.

![hard](https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F67794%2F9f74c0fcb517044e9cb0314b262e8e88%2Fevent_2325.gif?generation=1677796378563907&alt=media)





**What do we know about atmospheric muons?**:
   * The primary mechanism for separating the cosmic ray muons from the neu156 trino muons is reconstructing the muon track and determining whether the muon was traveling downwards into the Earth or upwards out of the Earth. Because neutrinos can penetrate through the Earth but cosmic ray muons cannot, it follows that a muon traveling out of the Earth must have been generated by a neutrino. Thus, by selecting only the muons that are reconstructed as up-going,the cosmic ray muons can, in principle, be removed from the data. Because the number of cosmic ray muons overwhelms the number of neutrino muons, high accuracy is critical for preventing erroneous reconstruction of cosmic ray muons as neutrino-induced
   * Atmospheric muons can in principle be excluded by simple geometrical considerations. To exclude the contribution from downward-going atmospheric muons, it is sufficient to identify upward-going events, which can only be produced by neutrinos.
       * This distinction requires a reliable determination of the elevation angle, because downward-going atmospheric muons outnumber upward-going atmospheric neutrinos by 5 to 6 orders of magnitude for typical neutrino telescope installation depths. [Source](https://arxiv.org/abs/1105.4116)
       
       
# Literature Overview Summary

## Classification:
* In our view it is worth to classify at least two kind of events:

1. Events that are easy to reconstruct (no dominating noise, atmospheric muons)
2. Events that are hard to reconstruct (a lot of noise and/or atmospheric muons)


Given the paper overview we would suggest to do geometrical containment classification - it both should be time efficient and easy to implement, thus:

1. **Events that are easy to reconstruct**:
    1. **Geometry**: Originated within the IceCube (cut off the boarders as per veto regions in the picture above)
    2. **Direction** Account for up-going movement (GridFit or some kind of rough approximation of angular movement could be used)
2. **Events that are hard to reconstruct**:
    1. Anything else
    
 
 
It is worth noting that energy criterion could be used to identify higher energy events that occur from neutrino coming from the space



## References:
1. [Evidence for High-Energy Extraterrestrial Neutrinos at the IceCube Detector](https://arxiv.org/pdf/1311.5238.pdf)
2. [An algorithm for the reconstruction of neutrino-induced showers in the ANTARES neutrino telescope](https://arxiv.org/abs/1708.03649)
3. [Neutrinos from the Milky Way](https://www.nikhef.nl/pub/services/biblio/theses_pdf/thesis_EL_Visser.pdf)
