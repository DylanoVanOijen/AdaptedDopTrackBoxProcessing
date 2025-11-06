When running the code of this repository on the DopTrackBox prototype's Fitlet3 machine, this repository is expected to be named *adapted_processing*, and placed in the *DTB_Software* folder.
The code consists of several dedicated parts:

### 1. Main directory scripts:
The scripts in the main directory are mainly used for the primary analysis of satellite recordings. The analysis chain is split into several pieces with each having typically three scripts:
1. A python script to perform the analysis part for one specific recording
1. A scheduler python script that keeps track (using a log file, found under the *DTB_Recordings/locally_processed/* folder) of which files have been processed and runs all remaining file consecutively.
1. A shell script that simply activates a python environment and runs the scheduler script to start processing.

**Analysis steps**:\
Processing:\
Single file script: *process_single_pass.py*\
Scheduler script: *schedule_processing.py*\
Shell execution *script: run_spectograms.sh*\
Step explaination: Performs the FFT on the raw IQ samples, makes a selection of the 
As this step is relatively CPU intensive and takes some time, it is recommended to run this automatically using the following crontab job to process all remaining passes twice per day:
```#00 5,22 * * * /home/doptrackbox/DTB_Software/adapted_processing/run_spectograms.sh >> /home/doptrackbox/DTB_Recordings/monitoring/processing_cron_log.txt 2>&1```

Curvefitting:\
Single file script: *curvefit.py*\
Scheduler script: *schedule_curvefitting.py*\
Shell execution *script: run_curvefitting.sh*\
Step explaination: This step takes the frequency points classified as signal, and fits a hyperbolic tangent to the data. Then a further selection takes place until the signal is (hopefully) extrcated as well as possible.

Plotting:\
Single file script: *make_pass_plots.py*\
Scheduler script: *schedule_plotting.py*\
Shell execution *script: run_plotting.sh*\
Step explaination: This script produces plots of the noise and SNR (based on the maximum power in the bin) as function of time, as well of a plot of the SNR of the extracted signal over time.


### 2. DopTrackComparison Folder:
The code in this repository follows the convention described above, but then when the goal of analyzing and comparing DopTrack data. 
The *doptrack_comparison.py* and *doptrack_comparison_v2.py* scripts (together with their respective scheduling and shell execution scripts) are used to perform time and frequency bias estimations of the DopTrackBox signal to the DopTrack signal. The difference between the normal and "v2" is that the v2 only takes account the DTB datapoints 150 Hz around the DT datapoints. 

The *doptrack_comparison_own_processing.py* makes SNR comparisons of passes that have been recorded by both DopTrack and DopTrackBox, in which the DopTrack files have also been processed using the adapted processing scripts (run_doptrack_recordings.sh -> run_doptrack_curvefit.sh).

The three *get_extracted_biases_x.py* files extract the fitted biases (and SNR differences) for a selected set of satellite passes, in different configurations.

*doptrack_bias_upperlimit.py* is used to gather the maximum expected DopTrack time biases for a selected number of passes in the script, using the YML data of DopTrack passes on the webdrive.

### 3. Component Verifications Folder:
This folder contains individual scripts to make noise and SNR plot comparisons between different hardware configurations, software settings or other factors that could have an impact, as presented in my thesis.
