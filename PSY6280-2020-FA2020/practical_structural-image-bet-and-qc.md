**Structural image (T1w) preprocessing and quality checks**
</br>
Now that we have a feel of the data structure of structural and functional brain images, the next step is assessing the quality of the data in the image. The goals of a T1 image are to differentiate the brain from non-brain tissues, and to differentiate between at least 3 tissue types within the brain including gray matter (GM), white matter (WM), and cerebrospinal fluid (CSF). In this vein, steps for checking image quality include: (1) general visual inspection, (2) test  how well brain and tissue types can be differentiated by our software, both qualitatively (visual inspection) and quantitatively (ratios).
</br>

**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [bet](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/BET/UserGuide) tool to remove the brain from a T1 image <br/>
* [ ] use FSL's [fast](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FAST) tool to segment the brain into 3 classes: GM, WM, CSF
* [ ] use FSLeyes to check the output of `bet` and `fast` <br/> 
* [ ] use FSL's `fslstats` tool to compute ratios summarizing image quality
<br/>

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>

**Overview of lab exercise** <br>
We will work with the T1 image from sub-01 in ds003030. To start consistent habits for working with data in the bids structure, we'll store our output in a derivatives folder called `anat`. We will then use the commandline tool `fslstats` to do math with the images to create variables that quantify how well different tissues are differentiated.

**Step 1: Set up derivatives** <br>
We can make new directories with sub directories using `mkdir` with some additional options:
* Our base `bids directory` is `~/fmriLab/ds003030`. Change directories to move yourself there in the terminal, and make our derivatives directories:
    * `cd ~/fmriLab/ds003030`
    * `mkdir -p derivatives/anat/sub-01`
    * You should now have a `derivatives` directory, with a folder inside of it named `anat` which contains `sub-01`
</br>

**Step 2: Run BET to separate brain from non-brain** <br>
We will run commands from within the subject derivatives directory and use relative file path addresses to refer to files in our base bids directory.
* In the terminal, move yourself to `sub-01`'s anat derivatives folder:
    * `cd ~/fmriLab/ds003030/derivatives/anat/sub-01`
*  We will use FSL's `bet` tool (**b**rain **e**xtraction **t**ool) to strip away skull
    * Type `bet` into the terminal prompt to see its usage
        * Our `<input>` image is the reconstructed T1w image in our base `bids directory`
        * We will name our `<output>` image the same as our input, except add `_brain` to the filename
        * The `Robust` brain center and neck clean-up almost always helps
        * `bet` almost always underestimates brain vs. skull in the cortex, so I suggest starting with the fractional intensity parameter set at slightly below the default of .5, such as `-f .45`
        * It is also very helpful to generate a binary mask of the `*_brain` image
        * Below then is the syntax for a typical good starting point with `bet`:
            * `fsleyes ../../../sub-01/anat/sub-01_anat_sub-01_T1w.nii.gz sub-01_T1w_brain.nii.gz -R -f .45 -m`
        * View the results in `fsleyes` by opening the input and output images together:
            * `fsleyes ../../../sub-01/anat/sub-01_anat_sub-01_T1w.nii.gz sub-01_T1w_brain.nii.gz sub-01_T1w_brain_mask.nii.gz`
        * How would you describe what the `mask` is in words?
</br>

**Step 3: Run FAST to separate tissue types within the skull-stripped image** <br>
Now that we have a T1 image with only the brain, we can segment the image further into tissue types. Now our input image will be the `*_brain` image.
 * If not there currently, move again to `sub-01`'s derivative `anat` directory: `cd ~/fmriLab/ds003030/derivatives/anat/sub-01`
    * Type `fast` into the terminal prompt to see its usage
        * Run fast with this syntax: `fast -t 1 -n 3 -g -b -B -v sub-01_T1w_brain.nii.gz`
        * While it's running we'll decode the settings we just specified
* Check `fast` output and understand what we have
    * In `fsleyes` open `sub-01_T1w_brain.nii.gz` and overlay all the images that end with `*_seg_*.nii.gz`
        * How do the values 0, 1, and 2 map to different tissue types?
        * Hint: in `fsleyes` assign each `seg` image a different color to see them in a different color on top of the brain image
    * Now add the images that end with `*_pve_*.nii.gz`
        * PVE stands for partial volume estimate: each voxel is assigned a value ranging from 0-1 that represents the proportion of that class's tissue present in the voxel
        * With the two images overlayed, can you tell what `pve` value translates to a `1` in the `seg` binary mask for each tissue type?

</br>

**Step 4: Use FAST output to compute image quality ratios**
In order to compute quantitative ratios summarizing T1 image quality, we will need to extract quantitative estimates of the mean (signal) and compare to an estimate of noise (variability of signal or variability of background noise). To demonstrate the basics of the process we'll work just with the gray matter and white matter tissue signal for the lab. </br>

* `fslstats` is a command-line tool to extract descriptive statistics for image values
    * Type `fslstats` into the terminal prompt to see its usage
    * Use the usage to translate this syntax into what it's doing in words: `fslstats sub-01_T1w_brain.nii.gz -k sub-01_T1w_brain_seg_1.nii.gz -M`
    * Use options within `fslstats` to compute:
        * Mean and standard deviation of gray matter (GM) signal intensity
        * Mean and standard deviation of white matter (WM) signal intensity
        * In whatever program you like, compute the absolute value of the difference of GM and WM intensities, and divide by the sum of their standard deviations. Share your answers and compare steps if you get something different.

* What aspect of the image are we missing with our CNR estimate above? 
    * Hint, see mriqc's image quality metrics summarized [here](https://mriqc.readthedocs.io/en/latest/iqms/t1w.html#mriqc.qc.anatomical.cnr)














