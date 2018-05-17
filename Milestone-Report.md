# Data Science Immersive: Capstone Project 1 Proposal
### Prepared By Benjamin Olsen

## What Is The Problem You Want To Solve? 
Documentation is a pillar of healthcare and drives medical advancement, encourages growth as a community, and most importantly provides insurance companies with enough proof for them to reimburse medical treatments. In what may have been a previously unintended effect of healthcare insurance is that the administration of certain medical treatment is indirectly driven by what health insurance companies deem to be “reimbursable.” While whether or not this relationship between healthcare providers and insurance companies is ultimately beneficial to healthcare is open to debate, the reality of the situation is that no procedure is going to be considered “appropriate” if it cannot be practically and accurately documented. This is an issue that has held entire professions from breaking into the healthcare industry because insurance will not cover treatments if their outcomes cannot be convincingly documented. In terms of measuring skeletal muscular actions, usage of EMG data has thus far been popularly used as a diagnostic tool thanks to few difficulties in collecting the data, but has not been considered for documentation purposes mostly due to the noisiness of the data in EMGs. However, new advances in the area of signal decomposition may make it possible to establish a population baseline for EMG data associated with certain musculoskeletal motions, opening the possibility of using EMGs to document therapeutic progression of muscle rehabilitation. This would make documentation much easier to collect, and potentially make the healthcare industry more accessible to new therapies.

## Who Is Your Client And Why Do They Care About This Problem?
Currently those who stand to gain from the ability to simply connect a few electrodes and quickly know the muscular functionality of a client is physical and occupational therapists, as documentation eats up a large portion of their time and effort. Additionally, other less widely accepted forms of therapy could prove their value to insurance companies by utilizing easy diagnostic tools, thus allowing new modalities of treatment that may have tradeoffs with current modalities. More importantly, medical diagnostic companies can gain from introducing EMG-based diagnostic devices that are cheaper to make, easier to use, and completely non-invasive, while making reliable documentation more accessible to practitioners. Examples of such companies include:
* GE Healthcare
* Siemens Healthcare
* Philips Healthcare
* Shimadzu Corporation

## What Data Are You Going To Use For This? How Will You Acquire This Data?
The data in question comes from: https://archive.ics.uci.edu/ml/datasets/sEMG+for+Basic+Hand+movements
### Instrumentation: 
The data were collected at a sampling rate of 500 Hz, using as a programming kernel the National Instruments (NI) Labview. The signals were band-pass filtered using a Butterworth Band Pass filter with low and high cutoff at 15 Hz and 500Hz respectively and a notch filter at 50Hz to eliminate line interference artifacts. 
The hardware that was used was an NI analog/digital conversion card NI USB- 009, mounted on a PC. The signal was taken from two Differential EMG Sensors and the signals were transmitted to a 2-channel EMG system by Delsys Bagnolia Handheld EMG Systems. 
### Protocol: 
The experiments consisted of freely and repeatedly grasping of different items, which were essential to conduct the hand movements. The speed and force were intentionally left to the subjectâ€™s will. There were two forearm surface EMG electrodes Flexor Capri Ulnaris and Extensor Capri Radialis, Longus and Brevis) held in place by elastic bands and the reference electrode in the middle, in order to gather information about the muscle activation. 
The subjects were asked to perform repeatedly the following six movements, which can be considered as daily hand grasps: 
* Spherical: for holding spherical tools 
* Tip: for holding small tools 
* Palmar: for grasping with palm facing the object 
* Lateral: for holding thin, flat objects 
* Cylindrical: for holding cylindrical tools 
* Hook: for supporting a heavy load 
### Two different databases are included: 
* 5 healthy subjects (two males and three females) of the same age approximately (20 to 22-year-old) conducted the six grasps for 30 times each. The measured time is 6 sec. There is a mat file available for every subject. 
* 1 healthy subject (male, 22-year-old) conducted the six grasps for 100 times each for 3 consecutive days. The measured time is 5 sec. There is a mat file available for every day.

## In Brief, Outline Your Approach To Solving This Problem.
### Process signals.
* Determine optimal dataframe orientation
* Perform signal decomposition
  * Amplitude across adjacent windows
  * Amplitude across overlapping windows
  * Activation Envelop
* Engineer categorical variables for subjects, channel, and grasp.
* Remove resting phase from signals. 
### Compare subjects for similarity and distinctness 
* Compute an average signal for each subject for each grasp with a confidence interval.
* Compare similar motions between subjects to look for population baseline.
* Compare signals between motions for the same subjects, then compare across subjects
### Possibility: Extract features for use with linear SVM classification
* Possible Features to extract
  * Amplitude
    * Adjacent vs overlapping windows
  * Activation measured via envelope
  * iEMG extraction
  * Zero-crossing
  * Slope Sign Changes
  * waveform length
  * Willison amplitude
  * Variance
  * Skewness
  * kurtosis
  * IMF extraction (EMD decomposition algorithm)
### Test transformations
* Testing for distinguishability, not generalizability since there are not a large number of observations. 
### Make final observations
* Was the data collected adequate for distinguishing multiclass hand gestures?
  * Number of channels
* Do the results generalize across populations?

## What Are Your Deliverables? 
-Original raw dataset .
-A wrangled dataset for each decomposition of signals and categorical variables.
  -Corrected EMG signals.
  -Extracted Amplitude over adjacent windows.
  -Extracted Amplitude over overlapping windows.
  -Extracted EMG signal Envelope.
-Codebase for data wranngling, exploratory analysis, and statistical inference. 
-Textual analysis of the data, and discussion of the results. 
-Slide deck summarizing findings.
