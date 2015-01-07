---
layout: post
title: Honours Thesis
summary: Abstract of my honours thesis.
---

The following is the abstract for my honours thesis, __One-Shot Gesture Recognition as a Page Turning Device for Digital Music Stands__, submitted for a _Bachelor of Computer Science (Honours)_ at Swinburne University of Technology in 2014.

# Abstract #

Digital music stands are an effective method of transporting large quantities of music scores, and as such are increasing in popularity amongst professional musicians.
However, the page turning methods currently used generally require a foot pedal or similar device, which is not feasible for some instruments.
The work in this thesis explores the use of one-shot gesture recognition as a page turning mechanism for digital music stands.
A curated database of test gestures was created, where movements were consciously performed at fast, medium and slow speeds under high, medium and low lighting conditions.
Numerous preprocessing techniques were identified and applied to this data and the impact of each on the distance between equivalent movements was recorded and analysed.

It was found that the use of motion history and motion energy images as movement templates is an effective approach to identifying equivalent actions using input from a webcam, as is the use of Hu Moments for feature extraction.
Reducing the size of input using an overhead cutoff, and lowering levels of noise through a median filter was found to improve results, though not in all cases.
Multiple distance measures were applied, and it was found that although Hu Moments are measured on vastly different scales, aggregate measures such as the Euclidian and Hamming distances can successfully distinguish equivalent and non-equivalent gestures.

Further work should focus on improving these results, expanding the size of and complexity of test data, and investigating the use of multiple training examples.

