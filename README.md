# EEG-SWD-analysis-pipeline

Raw EEG (.txt)
     |
     v
 spike_detection.py          --> <name>_spike_times.xlsx
     |
     v
 analyse_spikes.py           --> <name>_spike_times_bursts.xlsx
                                    <name>_spike_times_individual_spikes.xlsx
                                    <name>_spike_times_with_intervals.xlsx
     |
     | (Move each block1 file + its outputs into a subject folder)
     |
     v
 manual_swd_verifier_GUI.py  --> <prefix>_cleaned_true_bursts.xlsx
                                    <prefix>_batch_verification_results.xlsx
                                    <prefix>_false_positive_burst_ids.xlsx
     |
     v
 swd_analyser.py             --> <session>_double_verified_results.xlsx
                                    <session>_double_verified_false_positives.xlsx
