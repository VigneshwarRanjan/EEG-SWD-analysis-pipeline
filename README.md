# EEG-SWD-analysis-pipeline

EEG SWD Analysis Pipeline
A four-script Python pipeline for detecting, classifying, verifying, and measuring
Spike-Wave Discharges (SWDs) in rodent EEG recordings.
 
Pipeline Overview
Raw EEG (.txt)
     |
     v
[1] spike_detection.py          --> <name>_spike_times.xlsx
     |
     v
[2] analyse_spikes.py           --> <name>_spike_times_bursts.xlsx
                                    <name>_spike_times_individual_spikes.xlsx
                                    <name>_spike_times_with_intervals.xlsx
     |
     | (Move each block1 file + its outputs into a subject folder)
     |
     v
[4] manual_swd_verifier_GUI.py  --> <prefix>_cleaned_true_bursts.xlsx
                                    <prefix>_batch_verification_results.xlsx
                                    <prefix>_false_positive_burst_ids.xlsx
     |
     v
[5] swd_analyser.py             --> <session>_double_verified_results.xlsx
                                    <session>_double_verified_false_positives.xlsx

 
Folder Structure
Step 1 & 2 — Run from the shared folder containing all block1 files
recordings/
├── 273_I80TM_PHTD1 block1.txt
├── 274_I80TM_PHTD2 block1.txt
├── spike_detection.py
└── analyse_spikes.py

Step 3 — Move each recording into its own subject folder
273_I80TM_PHTD1/
├── 273_I80TM_PHTD1 block1.txt
├── 273_I80TM_PHTD1 spike_times.xlsx
├── 273_I80TM_PHTD1 spike_times_bursts.xlsx
├── 273_I80TM_PHTD1 spike_times_individual_spikes.xlsx
├── 273_I80TM_PHTD1 spike_times_with_intervals.xlsx
├── manual_swd_verifier_GUI.py
└── swd_analyser.py

 
Scripts
1. spike_detection.py
Detects downward spike events in raw EEG using a rolling Z-score method.
•	Input: *block1*.txt files in the current directory (tab-delimited, no header)
•	Output: <name>_spike_times.xlsx
•	Key parameters: SAMPLING_FREQUENCY, WINDOW_DURATION, THRESHOLD
2. analyse_spikes.py
Groups spike timestamps into bursts using inter-spike interval (ISI) criteria.
•	Input: *spike_times*.xlsx files in the current directory
•	Output: _bursts.xlsx, _individual_spikes.xlsx, _with_intervals.xlsx
•	Key parameters: MAX_ISI, MIN_BURST_DURATION, MIN_MEAN_ISI, MAX_MEAN_ISI
4. manual_swd_verifier_GUI.py
PyQt6 GUI for interactive batch verification -- mark false positives and duplicates.
•	Input: *block1*.txt (raw signal) + *bursts.xlsx (burst list) -- auto-discovered
•	Output: Cleaned burst list, verification results, FP/duplicate ID lists
•	Controls: Left-click = False Positive | Right-click = Duplicate
5. swd_analyser.py
PyQt6 GUI for measuring SWD duration and spike count on verified bursts.
•	Input: Raw .txt signal + burst .xlsx + optional previous results .xlsx
•	Output: _double_verified_results.xlsx, _double_verified_false_positives.xlsx
•	Modes: Duration measurement (click start/end) | Spike counting (click threshold)
 
Installation
pip install pyqt6 matplotlib numpy pandas scipy tqdm openpyxl neo

 
Usage
Step 1 -- Spike detection
Place spike_detection.py in the folder with your block1 files and run:
python spike_detection.py

Step 2 -- Burst classification
Place analyse_spikes.py in the same folder and run:
python analyse_spikes.py

Step 3 -- Organise by subject
Move each recording and its output files into a named subject folder
(e.g. 273_I80TM_PHTD1/), then copy scripts 4 and 5 into that folder.
Step 4 -- Manual verification (GUI)
Run from inside the subject folder:
python manual_swd_verifier_GUI.py

Mark false positives (left-click) and duplicates (right-click). Session is
auto-saved and resumable.
Step 5 -- Duration & spike measurement (GUI)
Run from inside the subject folder:
python swd_analyser.py

Load the raw signal, cleaned burst list, and (optionally) a previous results
file to resume. Measure SWD duration and spike count for each burst.
 
Output File Reference
File	Produced by	Contents
<name>_spike_times.xlsx	Script 1	Timestamps of all detected spikes
<name>_spike_times_bursts.xlsx	Script 2	Burst events with timing and ISI stats
<name>_spike_times_individual_spikes.xlsx	Script 2	Spikes not belonging to any burst
<name>_spike_times_with_intervals.xlsx	Script 2	Full spike list with ISI column
<prefix>_cleaned_true_bursts.xlsx	Script 4	Verified true-positive bursts only
<prefix>_batch_verification_results.xlsx	Script 4	All bursts labelled TRUE/FALSE/DUPLICATE
<prefix>_false_positive_burst_ids.xlsx	Script 4	Rejected burst IDs
<session>_double_verified_results.xlsx	Script 5	Measured durations and spike counts
<session>_double_verified_false_positives.xlsx	Script 5	FP list from second-pass verification

 
.gitignore
This repo excludes raw EEG data and generated output files to keep the
repository clean and avoid accidentally sharing animal data:
*.txt
*.abf
*_spike_times.xlsx
*_bursts.xlsx
*_individual_spikes.xlsx
*_with_intervals.xlsx
*_verification_results.xlsx
*_verified_results.xlsx
*_false_positives.xlsx
*_progress.json

 
License
MIT License -- free to use and modify with attribution.
