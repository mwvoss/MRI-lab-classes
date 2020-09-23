**Spatial normalization**
</br>
The goal of this lab is to learn the basic process of spatial normalization for at T1 anatomical image, and practice checking alignment between images in fsleyes.
</br>

**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [flirt](http://web.mit.edu/fsl_v5.0.10/fsl/doc/wiki/FLIRT(2f)UserGuide.html) tool to perform affine registration on a T1 image <br/>
* [ ] understand how to view and interpret the resulting affine transformation matrix <br/> 
* [ ] use FSLeyes to check the alignment of your T1 image to the MNI standard brain <br/> 
* [ ] for your homework you will learn how to run FSL's [fnirt] on the command-line and compare your results to affine registration
<br/>

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>

**Lab data** <br>
We will continue working with the T1 image from `sub-01` in `ds003030` during class time.

**Step 1: Check prerequisite anat derivatives** <br>
* Our base `bids directory` is `~/fmriLab/ds003030`
    * `cd ~/fmriLab/ds003030/derivatives/anat/sub-01`
    * This lab assumes you have a skull-stripped T1 image from the last lab: `sub-01_T1w_brain.nii.gz`
    * Linear registration with `flirt` takes the skull-stripped image as input
</br>


**Step 2: Use FSLeyes to open MNI standard images bundled with FSL and think about best reference image for your T1** <br>
* Use `fslhd` to remind yourself of the T1's spatial resolution and image orientation (LPI or RPI)
* Open FSL at the commandline: `fsl &`
* Click on the `FSLeyes` button
* Menu options: File -> Add standard
* Compare the `MNI152_T1_2mm.nii.gz` to `MNI152_T1_1mm.nii.gz`
    * which reference should we use for our T1 image?
    * what orientation is the standard?  use the "i" button underneath the `File` menu to see metadata in the image header
</br>


**Step 3: Understand FLIRT options with the GUI** <br>
* Open FSL at the commandline: `fsl &`
* Click on the `FLIRT linear registration` button
* Our initial options:
    * Mode: keep default as 2-stage registration
    * Reference image: we have a 1mm standard image
</br>



