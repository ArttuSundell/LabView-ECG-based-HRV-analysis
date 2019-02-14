# LabView-ECG-based-HRV-analysis
A National Instruments' SensorDAQ and LabView based system for HRV-analysis
Cuntributors: Arttu Sundell, Wille Tuovinen and Esko Saloranta

The main device used to create our project was National Instruments Sensor-DAQ and an EKG bundle, made by Vernier, EKG-BTA. 
These two were then programmed to work in a desired way with Labview programming software. Later on into the project, 
when a need arose to get the HRV measurements into a more readable form, Microsoft Excel was used to analyze the data. 
These all devices and programs were used in or through a PC.

The basis of the program is LabView 2014 Vernier physiological measurement example VI, called LQ EKG.vi. 
The VI reads ECG-signal from the SensorDAQ (or file) according to given parameters (sample rate and length of experiment). 
Only the parameter controls, reset, read stop, the real time waveform graph and a result waveform graph for comparison were 
preserved from the original VI. The original parts of the VI can be seen in the lower left corner of the block diagram and 
lower right hand side of the event structure (Waveform graph). The raw ECG-signal, 
that is outputted in array form is then taken to a signal processing chain formed by a classical waveform filter 
and a wavelet denoise filter.
First the signal is input into the a Biosignal Filtering vi -object (included in the Labview Biomedical Toolkit) 
to remove baseline wandering. The filter object was set to classical array filter mode and specified to operate 
as a Kaiser Window highpass filter cutting of frequencies lower than 0,5 Hz   [1; 2] . The sampling rate for the filter 
is wired as an input from the original sample rate specifications of the experiment. The filtered signal is output to a graph 
for adjusting purposes and further wired to a Wavelet Denoise Express vi-objects input to remove high frequency interference. 
The denoise -object was configured according to a LabView ECG signal processing tutorial and a project report by Bhyri et. al . 
Using undecimated wavelet transform (UWT) style, 8 level Daubechies(db06) transform settings and soft threshold , minimax, 
single level rescaling level thresholding settings is claimed to be the best fit for the purpose and to produce the best results, 
and therefore complied in our project too. The denoised output signal is output to a graph for adjusting purposes and amplified by
simply multiplying the signal by three (defined experimentally).

After preprocessing the  R-peaks are identified and specified using a WA Multiscale Peak Detection VI -object 
(Advanced Signal Processing Toolkit). The object is used to detect peak values that are higher than a set threshold, 
which can be adjusted by the controller added. The object outputs the number of the peaks detected, the peak location 
(sample index) and a graph of the analyzed waveform and the peaks detected. The number of peaks is divided by the duration 
and then multiplied by 60 to get the mean heart rate as bpm (Graph n5). The time location of the peaks is defined 
by multiplying the sample location index by the inverted sample rate. Then the data is listed into two arrays, 
both having n-1 embryos (where n is the number of peaks). In order to ignore the interval from the start of the experiment
to the first peak, the first array is listed from the second peak location [1] to the second last last, and the other is listed
from the third peak location [2] to the last. Finally the two arrays are subtracted to get the peak to peak intervals, 
which are multiplied by 1000 in order to get milliseconds. The intervals are presented as a previous R-R interval over 
time location graph using Build XY Graph vi, and written to an excel file using Write To Measurement File vi.

REFERENCES
Lakhwani, Rinky; Ayub, Shahanaz; Saini, J.P. 2013. Design and Comparison of Digital Filters for Removal of Baseline Wandering from ECG Signal. Conference paper: 2013 5th International Conference and Computational Intelligence and Communication Networks. Luettu 18.11.2018. 

National Instruments 2017. LabVIEW for ECG Signal Processing. A National Instruments tutorial. Available online <http://www.ni.com/tutorial/6349/en/#reviews>.Viewed online 01.12.2018

Bhyri, Channappa; V, Kalpana; Hamde, S.T.; Waghmare L.M. 2009. Estimation of ECG features using LabVIEW. TECHNIA – International Journal of Computing Science and Co mmunication Technologies, 2009 (1;2) Available online <https://pdfs.semanticscholar.org/ca9a/678185f02807ae1b7d9a0bf89247a130d949.pdf>. Viewed 7.12.2018.



