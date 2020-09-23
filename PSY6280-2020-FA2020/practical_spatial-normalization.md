**Spatial normalization**
</br>
The goal of this lab is to learn the basic process of spatial normalization for at T1 anatomical image, and practice checking alignment between images in fsleyes.
</br>

**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [flirt](http://web.mit.edu/fsl_v5.0.10/fsl/doc/wiki/FLIRT(2f)UserGuide.html) tool to perform affine registration on a T1 image <br/>
* [ ] understand how to view and interpret the resulting affine transformation matrix <br/> 
* [ ] use FSLeyes and contour overlays to check the alignment of your T1 image to the MNI standard brain <br/> 
* [ ] for your lab homework you will complete running FSL's [fnirt](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FNIRT) on the command-line and compare your results to affine registration
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
    * what orientation is the standard?  use the `i` button underneath the `File` menu to see metadata in the image header
</br>


**Step 3: Understand FLIRT options with the GUI** <br>
* Open FSL at the commandline: `fsl &`
* Click on the `FLIRT linear registration` button
* Our initial options:
    * Mode: keep default as 2-stage registration
    * Reference image: use file browswer to pick the MNI standard of choice
    * Model/DOF: look at options, relate to lecture, what are the transformations allowed in the 12 parameter model?
    * Input image: use file-browser to specify our skull-stripped T1 image `sub-01_T1w_brain.nii.gz`
    * Output: name as follows in the same directory `sub-01_T1w_brain_MNIaff.nii.gz`
    * Advanced options:
        * Search: check options, what do we want? 
        * Cost function: `correlation ratio` is preferred for inter-modal and `normalised correlation` is preferred for intra-modal. 
        * Interpolation: trilinear is good option for affine image registration
    * Press `Go`
    </br>

* You should now see everything we specified in the terminal. Often when learning a new tool or trying different options, it can be helpful to use the GUI and then document your iterations with the code reference. Then when you have a good solution for your data, you can use scripting to automate the process for speed and reproducibility. For example, below is how you would run the steps we selected on the command-line. The backward slash allows you to continue the command in a new line, which can help with seeing what options are specified for each argument. </br>

    ```
    flirt -in ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain.nii.gz \
    -ref $FSLDIR/data/standard/MNI152_T1_1mm_brain \
    -out ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain_MNIaff.nii.gz \
    -omat ~/fmriLab/ds003030/derivatives/anat/sub-01/sub-01_T1w_brain_MNIaff.mat \
    -bins 256 \
    -cost normcorr \
    -searchrx -180 180 -searchry -180 180 -searchrz -180 180 \
    -dof 12  \
    -interp trilinear
    ```

</br>

* What does `omat` refer to?
    * This is the affine transformation matrix summarizing the transform from native to standard space that was just completed.
    * View in the terminal by typing: `cat sub-01_T1w_brain_MNIaff.mat`
    * This is your `recipe` for moving in reverse from MNI to T1 native space, or for applying this same transform to another image. This affine transform can also form as the starting point for non-linear registration with FNIRT.</br>


**Step 4: Check output in FSLeyes** <br>
* Open our registered T1 with our MNI template:
    * `fsleyes $FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz sub-01_T1w_brain_MNIaff.nii.gz`
    * Start at the origin for world coordinates: 0,0,0
        * Turn cross-hairs on
        * Use opacity control to flicker between images and check alignment
        * Move through landmarks of CSF/white matter/gray matter edges from center out to gyri in each lobe. 
        * How is alignment overall? Problem areas?
    * FSL's `slicesdir` program to create a png file of axial slices showing your spatially normalized T1 image with the MNI template as a red contour overlay:
`slicesdir -p $FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz sub-01_T1w_brain_MNIaff.nii.gz`
        * View the .png file of the overlay in the `slicesdir` directory
        * View a list of overlays in the `index.html` folder

* What if you think it could be better?
    * [FLIRT FAQs](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FLIRT/FAQ)


**Step 5: Refine with non-linear registration using FNIRT** <br>
* After an initial affine registration, better local alignment can often be achieved with non-linear registration.
* Below shows how to run `FNIRT` on the command-line:

```
fnirt --iout=sub-01_T1w_MNI_head --in=sub-01_T1w \
--aff=sub-01_T1w_brain_MNIaff.mat \
--cout=sub-01_T1w_MNI_warp --iout=sub-01_T1w_MNInonlin \
--jout=sub-01_T1w_MNI_jac \
--ref=MNI152_T1_1mm.nii.gz --refmask=MNI152_T1_1mm_brain_mask_dil.nii.gz \
--warpres=10,10,10
```

* FNIRT can take some time, when it is finished: 
    * check your alignment: `fsleyes $FSLDIR/data/standard/MNI152_T1_1mm.nii.gz sub-01_T1w_MNInonlin.nii.gz` 


* You can now apply your non-linear warp to your skull-stripped T1 with `applywarp`:
```
applywarp -i sub-01_T1w_brain.nii.gz \
-r $FSLDIR/data/standard/MNI152_T1_1mm_brain \
-o sub-01_T1w_MNInonlin.nii.gz \
-w sub-01_T1w_MNI_warp.nii.gz
```

* Check your alignment and compare to affine: `fsleyes $FSLDIR/data/standard/MNI152_T1_1mm.nii.gz sub-01_T1w_brain_MNIaff.nii.gz sub-01_T1w_MNInonlin.nii.gz` 

* Add your new non-linear result to your list of overlays comparing alignment with FSL's `slicesdir` program:
`slicesdir -p $FSLDIR/data/standard/MNI152_T1_1mm_brain.nii.gz sub-01_T1w_brain_MNIaff.nii.gz sub-01_T1w_MNInonlin.nii.gz`

</br>
You should now see an overlay for both the affine and non-linear registrations: 

![image-slicesdir](images/normalization_slicesdir_output.png)

