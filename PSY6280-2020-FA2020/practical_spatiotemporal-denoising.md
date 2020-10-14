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


**Step 2: Open the melodic web report for overview of results** 
    * Within the file browser, navigate to the `melodic.ica` directory and open the `report.html` file with a web browser
    * Break-out questions
        * `Pre-Stats` page: is relative motion more similar to the FD Jenkinson or FD Power metric?
        * `Registration` page: look through overlays to evaluate alignment of functional data to MNI. Any concerns?
        * `Log` page: here's where everything that was run is documented, starting with `Initialisation` you can see the initial input was the raw nifti bold image without preprocessing. What file is the input to melodic processing?
        * Finally, the main results are shown in the `ICA` page: take a look with your group and discuss questions from your exploration. We will come back and review understanding as a group.
</br>


 **Step 3: Open melodic-derived components in fsleyes** 
    * Now let's see how to view the component output interactively in fsleyes to support manual labeling of components as signal or noise
    * In the terminal, move yourself to the `melodic.ica` directory: `cd ~/fmriLab/ds003030/derivatives/func/sub-01/melodic/melodic.ica`
    * Open fsleyes with the melodic scene setting: `fsleyes --scene melodic -ad filtered_func_data.ica filtered_func_data.ica/melodic_IC.nii.gz`
    * You should see something like below: <br>
![melodicview](images/denoising_melodicView.png)
