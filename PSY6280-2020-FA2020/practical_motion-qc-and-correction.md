**Motion detection and correction**
</br>
The purpose of this lab is to practice correcting for volume-to-volume motion in a functional MR image, and to learn about common correction parameters and how to describe them with numbers and figures.

</br>


**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [mcflirt](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/MCFLIRT) tool to run motion correction 
* [ ] use FSL's [fsl_motion_outliers](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FSLMotionOutliers) tool describe motion in funcitonal MRI data <br/>
* [ ] interpret the output of FSL's motion detection and correction tools 
* [ ] interpret the output of [mriqc](https://mriqc.readthedocs.io/en/stable/) for assessing motion


**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>


**Lab data** <br>
We will continue working with the data from `sub-01` in `ds003030` during class time.


**Step 1: Prepare a new derivatives directory for motion correction**
* At the terminal, type: `mkdir -p ~/fmriLab/ds003030/derivatives/func/sub-01/motion`
    * move yourself to this new directory with `cd`
    * copy our bold image here for ease to work with: `cp ~/fmriLab/ds003030/sub-01/func/sub-01_func_sub-01_task-FlickeringCheckerBoard_run-1_bold.nii.gz sub-01_task-bold.nii.gz`
 

**Step 2: Run motion correction**

Using the code below, make a bash script called `run_mcflirt.sh` to run motion correction, and run it to produce motion correction output.
    * suggest to use VS Code editor
    * to make a new file or edit an existing file: `code run_mcflirt.sh`

```
#!/bin/bash

invol="sub-01_task-bold"

echo "Determining middle volume in functional series"

halfPoint=$(fslhd "${invol}.nii.gz" | grep "^dim4" | awk '{print int($2/2)}')
echo "Middle volume is $halfPoint"


# check if .mat directoy output exists and if so delete it
if [ -d ${invol}*.mat ]
	then 
		echo "cleaning up existing .mat file output" 
		rm -R ${invol}*.mat
	else
		echo "no existing .mat file detected (OK)"
fi
	

echo "Running MCFLIRT motion correction"

# Run mcflirt
mcflirt -in ${invol}.nii.gz \
-out ${invol}_proc-mcf \
-cost normcorr \
-refvol ${halfPoint} \
-plots \
-stats \
-rmsrel -rmsabs -spline_final


echo "Making plots of rotations, translations, and displacements"

# Plot rotations (radians)
fsl_tsplot -i ${invol}_proc-mcf.par \
-t 'MCFLIRT estimated rotations (radians)' \
-u 1 --start=1 --finish=3 -a x,y,z -w 640 -h 144 -o mot_rot.png

# Plot translations (mm)
fsl_tsplot -i ${invol}_proc-mcf.par \
-t 'MCFLIRT estimated translations (mm)' \
-u 1 --start=4 --finish=6 -a x,y,z -w 640 -h 144 -o mot_trans.png

# Plot volume displacement. Abs=relative to reference vol. Rel=relative to vol n-1.
fsl_tsplot -i ${invol}_proc-mcf_abs.rms,${invol}_proc-mcf_rel.rms \
-t 'MCFLIRT estimated mean displacement (mm)}' \
-u 1 -w 640 -h 144 -a absolute,relative -o mot_disp.png

echo "MCLFIRT script done and plots saved"


# Compute and plot frame-wise displacement as computed by Power et al., 2012
fsl_motion_outliers -i ${invol}.nii.gz \
-o ${invol}_mot_confounds-fd \
--fd -s ${invol}_mot_fd-ts.txt -p ${invol}_mot_fd-plot 
# use awk to compute mean of single column
fd_mean=$(awk '{ total += $1; count++ } END { print total/count }' ${invol}_mot_fd-ts.txt)


# Compute and plot frame-wise displacement as computed by Jenkinson et al., 2002
fsl_motion_outliers -i ${invol}.nii.gz \
-o ${invol}_mot_confounds-fdrms \
--fdrms -s ${invol}_mot_fdrms-ts.txt -p ${invol}_mot_fdrms-plot 
# use awk to compute mean of single column
fdrms_mean=$(awk '{ total += $1; count++ } END { print total/count }' ${invol}_mot_fdrms-ts.txt)


# Compute and plot frame-wise displacement as computed by Power et al., 2012
fsl_motion_outliers -i ${invol}.nii.gz \
-o ${invol}_mot_confounds-dvars \
--dvars -s ${invol}_mot_dvars-ts.txt -p ${invol}_mot_dvars-plot 
# use awk to compute mean of single column
dvars_mean=$(awk '{ total += $1; count++ } END { print total/count }' ${invol}_mot_dvars-ts.txt)


# Report motion descriptives to terminal
abs_mean=`cat sub-01_task-bold_proc-mcf_abs_mean.rms`
echo "Mean absolute head displacement was ${abs_mean}"

rel_mean=`cat sub-01_task-bold_proc-mcf_rel_mean.rms`
echo "Mean relative head displacement (Jenkinson et al., 2002) was ${rel_mean}"
echo "This should match mean fdrms (mm) as ${fdrms_mean}"

echo "Mean FD (Power et al., 2012) was ${fd_mean}"

echo "Mean DVARS (Power et al, 2012) was ${dvars_mean}"
```

Interpretation of relative motion displacement estimates: </br>

**Jenkinson et al., 2002**
* Differentiating head realignment parameters across frames yields a six dimensional timeseries that represents instantaneous head motion. 
* Rotational displacements are converted from degrees to millimeters by calculating displacement on the surface of a sphere of radius 80 mm.
* Takes into account the range of voxel displacements over the sphere by computing the root-mean-square (with mean across all intracerebral voxels) of translation (mm) of the voxels before sphere model

**Power et al., 2012**
* Rotational displacements are converted from degrees to millimeters by calculating displacement on the surface of a sphere of radius 50 mm.
* Assumes that all voxels undergo equivalent displacements along the sphere in response to a given rotation
* These tend to be about twice as large as the Jenkinson estimates







**Lab Homework** 

* Download another subject with their mriqc plot
    * interpret carpet plot and motion profile
* Describe the difference between a volume displacement metric and a signal intensity metric
    * Use a metaphor to describe how each captures distinct aspects of motion in the data
* Look at five papers in your field and evaluate the following questions about how they delt with motion:
    * Was any data discarded due to motion, and if so, based on what criteria?
    * Was the tool used for motion correction specified?







