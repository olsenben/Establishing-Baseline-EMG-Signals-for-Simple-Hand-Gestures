# Steps to cleaning the(MBTI) Myers-Briggs Personality Type Dataset
This dataset includes a large number of people's MBTI type and content written by them.

## Data Set Context
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
### Acknowledgements

For the database 1), 
â€¢ C. Sapsanis, G. Georgoulas, A. Tzes, D. Lymberopoulos, â€œImproving EMG based classification of basic hand movements using EMDâ€ in 35th Annual International Conference of the IEEE Engineering in Medicine and Biology Society 13 (EMBC 13), July 3-7, pp. 5754 - 5757, 2013. 
For the database 2): 
â€¢ C. Sapsanis. 'Recognition of basic hand movements using electromyography'. 2013.

## Steps
### 1. Concatenate Files
Each file is a `.mat` file, one for each subject from whom data was collected. Each file contains a dictionary so that the data is organize subject -> grip motion -> measurements. Each measurement entry is 3000 samples, and there is a total of 30 entries per grip. To follow best practices, each signal must be encoded as a seperate observation, along with categorical variables to identify subject, grip, channel number, grip_channel, and sex of each observation. For lack of a better way to format the data, each array of 3000 readings will be oriented as rows and each reading with be a column. 
### 2. Correcting the Centrality of the Signals
Each signal should be oriented around the x-axis, but due to error EMG signals are sometimes collected off-axis. By subtracting `np.mean()` from each array, the signals can be recentered around the x-axis.
### 3. Calculating the Signal Envelope
A common EMG decomposition technique is to use a lowpass filter to filter out extremes and capture the approxiamte activation of the signal over time. This was done using `scipy.signal.butter()` and `scipy.signal.filfilt()` to apply a 4th order butterworth filter with a lowpass cutoff of 10 hz (the butterworth filter requires that the nyquist frequency be calculated. this is equal to the lowpass frequency divided by the sampling frequency).  
### 4. Extracting Windowed Amplitudes
A simpile way to reduce number of features while still capturing the shape of the signal is the extract the mean amplitude value over a adjacent windows of the data. In this situation I've selected the average value to represent the maximum activation, although the max value may be considered. By extracting the mean signal value from every 50 signals, I've reduced the number of variables from 3000 to 60 while still maintaining its shape. 
### 5. Extracting Overlapping Windowed Amplitudes
Like before, but this time with overlapping windows instead of adjacent windows. This results in a much smoother line. Using the average of the windows gives a result even smoother than the lowpass filter with reduction in necessary samples by 50x. For consistency, I've sampled windows of 345 with an overlap of 300 so as to maintain 60 samples. 
