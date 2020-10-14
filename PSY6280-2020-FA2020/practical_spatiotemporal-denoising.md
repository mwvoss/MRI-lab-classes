**Spatiotemporal denoising with independent component analysis**
</br>
The purpose of this lab is to get hands-on experience interpreting the output of spatial independent component analysis as run in FSL's MELODIC program, with emphasis on labeling components to denoise fMRI data. The experience of manual labeling will help help you become familiar with the various sources of noise in fMRI data even after standard preprocessing, and will help you appreciate when and how to apply ICA approaches that are a blend of manual and automatic component classification (e.g., FSL's FIX) or fully automated with tuning to specific types of noise (e.g., ICA-AROMA for motion). 

</br>


**By the end of this practical you should be able to:** <br/>
* [ ] interpret the output of FSL's [MELODIC](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/MCFLIRT) tool for a single-subject ICA analysis
* [ ] open MELODIC-estimated components within fsleyes's melodic scene viewer
* [ ] understand the process of manually labeling components as signal or noise using a conservative "innocent until proven guilty" criteria such as outlined in [Griffanti et al., 2017]((pdfs/Griffanti-2017-ICA.pdf))
* [ ] understand the implications of spatial smoothing when applying single-subject ICA for denoising


**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>


**Lab data** <br>
We will continue working with the data from `sub-01` in `ds003030` during class time.


**Step 1: Prepare a new derivatives directory for motion correction**
* At the terminal, move yourself to the `func` derivatives directory: `cd ~/fmriLab/ds003030/derivatives/func/sub-01/`
    * 
 </br>