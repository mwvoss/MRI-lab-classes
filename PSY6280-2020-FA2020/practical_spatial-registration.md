*Spatial registration**
</br>
The goal of this lab is to learn the process of co-registration between two image modalities such as function to structure, and spatial normalization of lower-resolution images (such as funcitonal MRIs). We will continue to use skills of checking alignment of image registration with fsleyes and contour maps.
</br>

**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [flirt](http://web.mit.edu/fsl_v5.0.10/fsl/doc/wiki/FLIRT(2f)UserGuide.html) tool co-register your T2* bold image to your high-resolution T1 image <br/>
* [ ] concatenate transformation matrices to perform spatial normalization on the T2* bold image <br/> 
* [ ] use FSLeyes and contour overlays to check the alignment of your T2* bold image to the MNI standard brain <br/> 

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>

**Lab data** <br>
We will continue working with the T1 and bold image from `sub-01` in `ds003030` during class time.

**Step 1: Check prerequisite anat derivatives** <br>
* Our base `bids directory` is `~/fmriLab/ds003030`
    * `cd ~/fmriLab/ds003030/derivatives/anat/sub-01`
    * This lab assumes you have 
        * a skull-stripped T1 image from the last lab: `sub-01_T1w_brain.nii.gz`
        * an affine transformation for aligning the T1 to MNI: `sub-01_T1w_brain_MNIaff.mat`
</br>

**Step 2: Prepare a new derivatives directory for the functional bold data**
* At the terminal, type: `mkdir ~/fmriLab/ds003030/derivatives/func/sub-01/registration`
    * move yourself to this new directory with `cd`
    * copy our bold image here for ease to work with: `cp ~/fmriLab/ds003030/sub-01/func/sub-01_func_sub-01_task-FlickeringCheckerBoard_run-1_bold.nii.gz sub-01_task-bold.nii.gz`
    * co-registration aligns two 3D volumes, so we need to "splice" out a bold volume that is representative of the series - we'll compare two common options which are the first and middle volumes
        * a command-line tool to "splice": `fslroi` 
        * example usage to splice out the first volume: `fslroi sub-01_task-bold.nii.gz example_func_vol-first.nii.gz 0 1`
            * Reads: "Reads: take input image, give back image after extracting volumes starting at vol 0 with length 1"
        * use this example to create an image named `example_func_vol-mid.nii.gz` that is the middle volume of the series
    * use `bet` to strip the skull from the functional images
        * example for first volume: `bet example_func_vol-first.nii.gz example_func_vol-first_brain.nii.gz`
* By the end of this step, within the registration directory, you should have these two output images prepared for the next step:
    * `example_func_vol-first_brain.nii.gz`
    * `example_func_vol-mid_brain.nii.gz`
</br>

**Step 3: Co-registration of the bold image to the same subject's T1 image**

* We will use flirt with the following options, and let's talk about why

```
flirt -in example_func_vol-first_brain.nii.gz \
-ref ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain.nii.gz \
-out example_func_vol-first_toT1_6dof \
-omat example_func_vol-first_toT1_6dof.mat \
-cost corratio \
-dof 6 \
-searchrx -90 90 -searchry -90 90 -searchrz -90 90 \
-interp trilinear
```

Put this command in a script with the following steps:
* gedit run_registration.sh
* with this file open, copy in the text below, and hit `Save`

```
#!/bin/bash

flirt -in example_func_vol-first_brain.nii.gz \
-ref ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain.nii.gz \
-out example_func_vol-first_toT1_6dof \
-omat example_func_vol-first_toT1_6dof.mat \
-cost corratio \
-dof 6 \
-searchrx -90 90 -searchry -90 90 -searchrz -90 90 \
-interp trilinear
```

* run the script in the terminal by typing: `bash run_registration.sh`
* registration outputs:
    * check alignment with fsleyes: `fsleyes ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain.nii.gz example_func_vol-first_toT1_6dof.nii.gz`
    * use `fslhd` to see if image orientation for T1 and bold are correctly matched










