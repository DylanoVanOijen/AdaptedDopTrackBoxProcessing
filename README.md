When running the code of this repository on the DopTrackBox prototype's Fitlet3 machine, this repository is expected to be named *adapted_processing*, and placed in the *DTB_Software* folder.
The code consists of several dedicated parts:

1. Main directory scripts:
The scripts in the main directory are mainly used for the primary analysis of satellite recordings. The analysis chain is split into several pieces with each having typically three scripts:
1. A python script to perform the analysis part for one specific recording
2. A scheduler python script that keeps track (using a log file, found under the *DTB_Recordings/locally_processed/* folder) of which files have been processed and runs all remaining file consecutively.
3. A shell script that simply activates a python environment and runs the scheduler script to start processing.

**Analysis steps**:
Processing:
Single file script: process_single_pass.py
Scheduler script: schedule_processing.py
Shell execution script: run_spectograms.sh
Step explaination: Performs the FFT on the raw IQ samples, makes a selection of the 
As this step is relatively CPU intensive and takes some time, it is recommended to run this automatically using the following crontab job to process all remaining passes twice per day:
```#00 5,22 * * * /home/doptrackbox/DTB_Software/adapted_processing/run_spectograms.sh >> /home/doptrackbox/DTB_Recordings/monitoring/processing_cron_log.txt 2>&1```

Curvefitting:
Single file script: curvefit.py
Scheduler script: schedule_curvefitting.py
Shell execution script: run_curvefitting.sh
Step explaination

Plotting:
Single file script: make_pass_plots.py
Scheduler script: schedule_plotting.py
Shell execution script: run_plotting.sh
Step explaination
