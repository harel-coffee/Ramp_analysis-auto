
# How to rerun data for the Tennant et al., 2022 Ramp cell manuscript

The manuscript uses data from 4 cohorts of animals that can be found here :

''

Individual recordings from each mouse, session and cohort are spike sorted using the following pipeline :

'https://github.com/MattNolanLab/in_vivo_ephys_openephys' 

cohort 1-4 : analysed using the vr_fix_bugs branch
cohort 7 : analysed using harrys branch

Once sorted, data for each cohort is concatenated together, giving one dataframe for each cohort/mouse. The following script is used to do this:

Once you have a dataframe with all the sessions from one cohort, you are ready to run the python post sorting analysis pipeline. 


## Steps to run RampAnalysis (Python pipeline)

There are three main stages to running the python post sorting pipeline. 

1. Post Sorting data from the virtual reality
2. Post Sorting data from the open field 
3. Concatenating the two together

The third part of this assumes for each mouse and day you have a recording from the virtual reality AND a recording from the open arena and requires that you have sorted your virtual reality and matching open field recordings together (i.e. from Harry's pipeline). This ensures clusters are matched from the virtual reality and open field and have the same cluster_id number across the two dataframes.

## Additional data needed

Additional data needs to be loaded for the python post sorting. Additional data can be found in 'Data/...'

Ramp score :
The ramp manuscript utilises a 'ramp score' generated by Teris Tam. Ramp scores for each recording session (for each cohort etc) can be found in 'Data/ramp_peak_analysis.csv'. 

Brain region : 
The labelling as to brain region for each recording session (for each cohort etc) can be found in 'Data/tetrode_locations.cvs'

Graduation day :
The day each mouse graduates to probe trials can be found at 'Data/Criteria_days.csv'.


## Post Sorting data from the virtual reality

1. Copy the path to your data frame (the concatenated one for a whole cohort) in the process_allmice_dir function in ‘LoadDataFrames.py’
2. Go to the main function in ‘Control_PostSorting_Analysis.py’. The following functions should be uncommented : 
- remove_false_positives
- curate_data
- make_neuron_number
- add_mouse_to_frame
- load_crtieria_data_into_frame
- load_brain_region_data_into_frame
- load_Teris_ramp_score_data_into_frame
- Run_main_figure_analysis   
- run_supple_figure_analysis
- drop_columns_from_frame

3. In run_main_figure_analysis  the following functions should be uncommented: 
- split_data_by_reward
- extract_time_binned_firing_rate
- generate_acceleration_rewarded_trials
  
4. In run_supple_figure_analysis the following functions should be uncommented:
- split_time_data_by_trial_outcome
- extract_time_binned_firing_rate_runthru_allspeeds
- extract_time_binned_firing_rate_try_allspeeds
- extract_time_binned_firing_rate_rewarded_allspeeds
- split_and_save_speed_data
- extract_time_binned_speed_by_outcome
- calc_histo_speed

5. The last line of the main function contains a path for the dataframe to be saved too, change this to your desired output and dataframe name (lets call it dataframe1). 
 
## Post sorting open field recordings

6. Copy the path to your data frame in the process_allmice_dir_of function in ‘LoadDataFrames.py’. Note : this is the open field dataframe (i.e. of recordings from the open field), sorted WITH the VR
7. Go to the main function in ‘Control_PostSorting_Analysis_of.py’. The following functions should be uncommented : 
- calculate_spike_width
- generate_spike_isi

8. The last line of the main function contains a path for the dataframe to be saved too, change this to your desired output and dataframe name. Note : choose a DIFFERENT name to the output of the VR analysis, let's call it dataframe2.  

## Match open field and virtual reality recordings

9. Go to ‘Match_Session_and_Cluster.py’ and in process_allmice_of copy the path to your data frame output from the ‘Control_PostSorting_Analysis_of.py’ i.e. dataframe2. 
10. In process_allmice_VR copy the path to your data frame output from the ‘Control_PostSorting_Analysis.py’ i.e. dataframe1. 
11. Navigate to the main function in ‘Match_Session_and_Cluster.py’. Make sure the output path of the dataframe has a distinct name. 
12. Run the main function in ‘Match_Session_and_Cluster.py’.

Now you have ran the python postsorting pipeline, you are ready to run your datasets in R. Please switch to 

## Adding new analyses

New analysis code should be added in a way that uses the data frames. If analyses require access to raw data, then a processing step should be used to add the required data to the data frames. Results of subsequent analyses should be added into these data frames as new columns. For instance, if we implement calculating the speed score of cells, this should be a new column in the data frame that contains information on clusters.

## How to contribute
Please submit an issue to discuss.