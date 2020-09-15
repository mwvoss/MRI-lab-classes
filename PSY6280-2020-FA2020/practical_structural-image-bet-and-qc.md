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

**

