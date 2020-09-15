**Structural image (T1w) preprocessing and quality checks**
</br>
Now that we have a feel of the data structure of structural and functional brain images, the next step is assessing the quality of the data in the image. The goals of a T1 image are to differentiate the brain from non-brain tissues, and to differentiate between at least 3 tissue types within the brain including gray matter (GM), white matter (WM), and cerebrospinal fluid (CSF). In this vein, steps for checking image quality include: (1) general visual inspection, (2) test  how well brain and tissue types can be differentiated by our software, both qualitatively (visual inspection) and quantitatively (ratios).
</br>

**By the end of this practical you should be able to:** <br/>
* [ ] use FSL's [bet](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/BET/UserGuide) tool to remove the brain from a T1 image <br/>
* [ ] use FSL's [fast](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FAST) tool to segment the brain into 3 classes: GM, WM, CSF
* [ ] use FSLeyes to check the output of `bet` and `fast` <br/> 
* [ ] use FSL's fslstats tool to compute contrast to noise ratio (CNR) for a T1 image
<br/>

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>

**Overview of lab exercise** <br>
We will work with the T1 image from sub-01 in ds003030. To start consistent habits for working with data in the bids structure, we'll store our output in derivative folders, each named after the preprocessing step being run (`bet` and `fast`). Within the `fast` directory, we will then use the commandline tool `fslstats` to do math with the images to create variables that quantify how well different tissues are differentiated.

**Step 1: Set up derivatives** <br>
We can make new directories with sub directories using `mkdir` with some additional options:
* Our base `bids directory` is `~/fmriLab/ds003030`. Change directories to move yourself there in the terminal, and make our derivatives directories:
    * `cd ~/fmriLab/ds003030`
    * `mkdir -p derivatives/bet/sub-01`
    * `mkdir -p derivatives/fast/sub-01`
    * You should now have a `derivatives` directory, with two folders inside of it named `bet` and `fast`, which each have a directory for `sub-01`
</br>

**Step 2: Run BET to separate brain from non-brain** <br>
We will run commands from within the subject derivatives directory and use relative file path addresses to refer to files in our base bids directory.
* In the terminal, move yourself to sub-01's bet derivatives folder:
    * `cd ~/fmriLab/ds003030/derivatives/bet/sub-01`
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
</br>

**Step 3: Run FAST to separate tissue types within the skull-stripped image** <br>
Now that we have a T1 image with only the brain, we can segment the image further into tissue types. Now our input image will be the `*_brain` image and our output image will be within the `fast` derivatives directory. Similar to what we did with `bet`, we'll run `fast` within the output directory.
* 








