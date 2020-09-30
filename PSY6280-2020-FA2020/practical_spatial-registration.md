**Spatial registration**
</br>
The goal of this lab is to learn the process of co-registration between two image modalities such as function to structure, and spatial normalization of lower-resolution images (such as funcitonal MRIs). We will continue to use skills of checking alignment of image registration with fsleyes and contour maps.
</br>

**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [flirt](http://web.mit.edu/fsl_v5.0.10/fsl/doc/wiki/FLIRT(2f)UserGuide.html) tool co-register your T2* bold image to your high-resolution T1 image <br/>
* [ ] use FSL's tool [epi_reg](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FLIRT/UserGuide#epi_reg) tool to run boundary-based registration [bbr](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FLIRT_BBR) to compare with `flirt` alone
* [ ] concatenate transformation matrices to perform spatial normalization on the T2* bold image <br/> 
* [ ] use FSLeyes and contour overlays to check the alignment of co-registration and normalization results <br/> 

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>

**Lab data** <br>
We will continue working with the T1 and bold image from `sub-01` in `ds003030` during class time.

**Step 1: Check prerequisite anat derivatives** <br>
* Our base `bids directory` is `~/fmriLab/ds003030`
    * Our derivatives directory is `~/fmriLab/ds003030/derivatives/`
    * Within ~bids/derivatives/anat/sub-01, this lab assumes you have 
        * a skull-stripped T1 image from the last lab: `sub-01_T1w_brain.nii.gz`
        * an affine transformation for aligning the T1 to MNI: `sub-01_T1w_brain_MNIaff.mat`
</br>

**Step 2: Prepare a new derivatives directory for the functional bold data**
* At the terminal, type: `mkdir -p ~/fmriLab/ds003030/derivatives/func/sub-01/registration`
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

We will use flirt with the following options, and let's talk about why

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
* in the terminal type: `gedit run_registration.sh`
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
* **look at** registration outputs:
    * check alignment with fsleyes: `fsleyes ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain.nii.gz example_func_vol-first_toT1_6dof.nii.gz`
    * use `fslhd` to see if image orientation for T1 and bold are correctly matched

</br>

For comparison, run flirt result with flirt+bbr using FSL's `epi_reg` tool:
```
epi_reg --epi=example_func_vol-first_brain.nii.gz \
--t1=../../../../sub-01/anat/sub-01_anat_sub-01_T1w.nii.gz \
--t1brain=../../../anat/sub-01/sub-01_T1w_brain.nii.gz \
--wmseg=../../../anat/sub-01/sub-01_T1w_brain_seg_2.nii.gz \
--out=example_func_vol-first_toT1_bbr.nii.gz
```

* compare results in fsleyes:
`fsleyes ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain.nii.gz example_func_vol-first_toT1_6dof.nii.gz example_func_vol-first_toT1_bbr.nii.gz example_func_vol-first_toT1_bbr_fast_wmedge.nii.gz -cm red`

* compare results with paired contour overlay images: 
`slicesdir -o example_func_vol-first_toT1_6dof.nii.gz example_func_vol-first_toT1_bbr_fast_wmedge.nii.gz  example_func_vol-first_toT1_bbr.nii.gz example_func_vol-first_toT1_bbr_fast_wmedge.nii.gz`

</br>


**Step 4: Spatial normalization of the bold image to the MNI template**

We now have the transformation "recipe" for bold->T1w->MNI. However, we don't want to apply these sequentially on the images themselves because once the bold image is transformed we lose some precision in the image compared to the original. So we'll combine these transforms mathematically via concatenation with FSL's `convertwarp` tool.</br>
</br>

**Run concatenation step on both transform recipes:**
* Flirt corratio with 6 dof
```
# flirt corratio with 6 dof
convertwarp --ref=$FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz \
--premat=example_func_vol-first_toT1_6dof.mat \
--warp1=../../../anat/sub-01/sub-01_T1w_MNI_warp \
--out=example_func_vol-first_toMNI_warp.nii.gz
```


* Flirt corratio + bbr
```
# flirt corratio + bbr
convertwarp --ref=$FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz \
--premat=example_func_vol-first_toT1_bbr.mat \
--warp1=../../../anat/sub-01/sub-01_T1w_MNI_warp \
--out=example_func_vol-first_toMNI-bbr_warp.nii.gz
```

**Apply concatenated transforms for both recipes:**
* Flirt corratio with 6 dof
```
# flirt corratio
applywarp --ref=$FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz \
--in=example_func_vol-first_brain.nii.gz \
--out=example_func_vol-first_toMNI.nii.gz \
--warp=example_func_vol-first_toMNI_warp.nii.gz
```

* Flirt corratio + bbr
```
# flirt corratio + bbr
applywarp --ref=$FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz \
--in=example_func_vol-first_brain.nii.gz \
--out=example_func_vol-first_toMNI-bbr.nii.gz \
--warp=example_func_vol-first_toMNI-bbr_warp.nii.gz
```

* Make a contour overlay of our result for a static image:
```
slicesdir -o \
example_func_vol-first_toT1_6dof.nii.gz example_func_vol-first_toT1_bbr_fast_wmedge.nii.gz \ example_func_vol-first_toT1_bbr.nii.gz example_func_vol-first_toT1_bbr_fast_wmedge.nii.gz \
example_func_vol-first_toMNI.nii.gz $FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz \
example_func_vol-first_toMNI-bbr.nii.gz $FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz 
```


**Lab Homework** 

* Write a script named `run_registration_vol-mid.sh` that completes the same series of steps for the middle bold volume. Your output should include the addition of a new contour overlay image to your slicesdir index.html, which summarizes the co-registration of both your first and middle bold volumes.





