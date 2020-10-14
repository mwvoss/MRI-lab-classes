**Spatiotemporal denoising with independent component analysis**
</br>
The purpose of this lab is to get hands-on experience interpreting the output of spatial independent component analysis as run in FSL's MELODIC program, with emphasis on labeling components to denoise fMRI data. The experience of manual labeling will help help you become familiar with the various sources of noise in fMRI data even after standard preprocessing, and will help you appreciate when and how to apply ICA approaches that are a blend of manual and automatic component classification (e.g., FSL's FIX) or fully automated with tuning to specific types of noise (e.g., ICA-AROMA for motion). 

</br>


**By the end of this practical you should be able to:** <br/>
* [ ] interpret the output of FSL's [MELODIC](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/MCFLIRT) tool for a single-subject ICA analysis
* [ ] open MELODIC-estimated components within fsleyes's melodic scene viewer
* [ ] understand the process of manually labeling components as signal or noise using a conservative "innocent until proven guilty" criteria such as outlined in [Griffanti et al., 2017](https://github.com/mwvoss/MRI-lab-classes/tree/master/PSY6280-2020-FA2020/pdfs/Griffanti-2017-ICA.pdf)
* [ ] understand the implications of spatial smoothing when applying single-subject ICA for denoising


**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>


**Lab data** <br>
We will continue working with the data from `sub-01` in `ds003030` during class time. Because melodic is a blind source separation process, the order of the componentns can be slightly different each time it is run on the same dataset. For our group work it will be easier to all have the same output, so you'll download melodic output and we'll focus on interpretation.


**Step 1: Prepare a new derivatives directory for motion correction**
* At the terminal, move yourself to the `func` derivatives directory: `cd ~/fmriLab/ds003030/derivatives/func/sub-01/`
    * Download melodic derivatives folder: `wget -O melodic.tar.gz https://www.dropbox.com/s/1r3iw2wyswlr8ls/melodic.tar.gz?dl=0`
    * Unpack: `tar -xvf melodic.tar.gz`
    * Remove uneeded tar zip: `rm melodic.tar.gz`
    * You now have melodic deriviatives for `sub-01`, which includes output of single-subject ICA on their flickering checkerboard bold data both with (`melodic.ica`) and without spatial smoothing (`melodic_no-smooth.ica`).
 </br>


 **Step 2: Open melodic-derived components in fsleyes** 
    * We'll start with the spatially smoothed data (FWHM was set to 5 mm)
