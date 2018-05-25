![alt text](https://startupbeat.com/wp-content/uploads/2015/12/springboard-logo-secondary-RGB.jpg "Logo Title Text 1")
# Capstone Project 2 - Milestone Report
### Prepared by Ben Olsen
## What Is The Problem You Want To Solve?
In what may have been a previously unintended effect of healthcare insurance is that the administration of certain medical treatment is indirectly driven by what health insurance companies deem to be “reimbursable.” While whether or not this relationship between healthcare providers and insurance companies is ultimately beneficial to healthcare is open to debate, the reality of the situation is that no procedure is going to be considered “appropriate” if it cannot be practically and accurately documented. This is an issue that has held entire professions from breaking into the healthcare industry because insurance will not cover treatments if their outcomes cannot be convincingly documented. In terms of measuring skeletal muscular actions, usage of EMG data has thus far been popularly used as a diagnostic tool thanks to few difficulties in collecting the data, but has not been considered for documentation purposes mostly due to the noisiness of the data in EMGs. However, new advances in the area of signal decomposition may make it possible to establish a population baseline for EMG data associated with certain musculoskeletal motions, opening the possibility of using EMGs to document therapeutic progression of muscle rehabilitation. This would make documentation much easier to collect, and potentially make the healthcare industry more accessible to new therapies.
## Who Is Your Client And Why Do They Care About This Problem?
Currently those who stand to gain from the ability to simply connect a few electrodes and quickly know the muscular functionality of a client is physical and occupational therapists, as documentation eats up a large portion of their time and effort. Additionally, other less widely accepted forms of therapy could prove their value to insurance companies by utilizing easy diagnostic tools, thus allowing new modalities of treatment that may have tradeoffs with current modalities. More importantly, medical diagnostic companies can gain from introducing EMG-based diagnostic devices that are cheaper to make, easier to use, and completely non-invasive, while making reliable documentation more accessible to practitioners. Examples of such companies include:

* GE Healthcare
* Siemens Healthcare
* Philips Healthcare
* Shimadzu Corporation

## What Data Is Necessary? 
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

### Data Wrangling Steps
#### 1. Concatenate Files
Each file is a `.mat` file, one for each subject from whom data was collected. Each file contains a dictionary so that the data is organize subject -> grip motion -> measurements. Each measurement entry is 3000 samples, and there is a total of 30 entries per grip. To follow best practices, each signal must be encoded as a seperate observation, along with categorical variables to identify subject, grip, channel number, grip_channel, and sex of each observation. For lack of a better way to format the data, each array of 3000 readings will be oriented as rows and each reading with be a column. 
#### 2. Correcting the Centrality of the Signals
Each signal should be oriented around the x-axis, but due to error EMG signals are sometimes collected off-axis. By subtracting `np.mean()` from each array, the signals can be recentered around the x-axis.
#### 3. Calculating the Signal Envelope
A common EMG decomposition technique is to use a lowpass filter to filter out extremes and capture the approxiamte activation of the signal over time. This was done using `scipy.signal.butter()` and `scipy.signal.filfilt()` to apply a 4th order butterworth filter with a lowpass cutoff of 10 hz (the butterworth filter requires that the nyquist frequency be calculated. this is equal to the lowpass frequency divided by the sampling frequency).  
#### 4. Extracting Windowed Amplitudes
A simpile way to reduce number of features while still capturing the shape of the signal is the extract the mean amplitude value over a adjacent windows of the data. In this situation I've selected the average value to represent the maximum activation, although the max value may be considered. By extracting the mean signal value from every 50 signals, I've reduced the number of variables from 3000 to 60 while still maintaining its shape. 
#### 5. Extracting Overlapping Windowed Amplitudes
Like before, but this time with overlapping windows instead of adjacent windows. This results in a much smoother line. Using the average of the windows gives a result even smoother than the lowpass filter with reduction in necessary samples by 50x. For consistency, I've sampled windows of 345 with an overlap of 300 so as to maintain 60 samples. 

## What Other Data Could Be Useful?
Finding similar data to this data set has been difficult to say in the least, as most data sets dealing with EMG signals and hand guestures include many many more channels for each group and utilize different electrode placement. There is a possibilty that these data sets could be decomposed into smaller more similar components, but in the event that someone wished to reproduce the experiment that produced the data set being worked with here, it would certainly not be difficult to do so.

## Initial Findings
### Exploratory Data Analysis
According to the data analysis, it appeared that the the signal envelope will be the most useful to establish a baseline due to the fact that it captures the most variation with a very smooth signal. This sensitivity is important to capture a large amount of variation for a baseline to which other samples can be be compared acurately, but does represent a large feature set (3000) which might be computationly heavy in future calculations. The mean amplitude over adjacent windows is a close representation of the signal envelope if the latter proves to be to large to handle computationally efficiently, with a x50 reduction in sample size (3000 to 60). Finally, it would appear that the mean amplitude over overlapping windows produces the smoothest and least varied line, making it a possibilty for comparing new signals to a baseline calculated by the signal envelope/adjacent windows.
### Inferential Statistics
While it would appear that the average signal envelope for each grip across the population is distinct, the average signals per subject within those grip groups varied quite heavily. This seems to be due to variables that were intentionally unaccounted for in the data collection (grip force, speed of contraction, etc). While this analysis seems to show that the average grip signal envelope captures that grip group well and a 95% confidence interval for the signals is for the most part distinct from the other  grip groups, the variation between subjects within that group is statistically significant and would make me wary to assume that others subjects would fall into that description rather than vary even more wildly than the ones we have observed in this data set. 
## Next Steps
Keeping the objective of calculating a baseline for this population of samples requires that I state a testable hypothesis concerning the particular data set. The hypothesis that I should aim to test then is that this collection of samples is a representitive sample of what we might expect to see in other subjects, and thus a baseline computed from this sample would be both practical for analysis and useful for application. In order for this hypothesis to be accepted, there a two criteria that must be met. 
1. The signals grouped by grip must be statistically different.
    * This is important because it would indicate that statistically speaking, each grip is fundamentally different from the other grips according to this population
2. The signals for subjects within a grip grouping must be statistically similar.
    * This is important because it would indicate that we can expect other subjects to produce signals similar to the ones we observe in this data set.
