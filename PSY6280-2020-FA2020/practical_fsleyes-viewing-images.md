**By the end of this practical you should be able to:** <br/>
* [ ] use the terminal to navigate to where your MRI data for this course will be <br/>
* [ ] open MR images with fsleyes <br/>
* [ ] distinguish between structural and functional images in contrast, resolution, and dimensions <br/>
* [ ] use fsleyes to identify the coordinates and intensity at the cursor location, and toggle images on/off <br/>
* [ ] view the timeseries of a functional image
<br/>

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>


**First prep step**, make a folder for holding data for our labs: <br/>
*  Open your terminal by clicking on the icon showing a little black screen
*  Type `pwd`. Where are you in the computer filesystem?
*  Type `ls`. What other files are here? 
*  To make a new folder using the terminal type `mkdir fmriLab`
  *  Did it work? Type `ls` 
  *  How did that work? Type `man mkdir`
*  Go into the new folder you made by typing `cd fmriLab`
<br/>

**Second prep step**, download some images: <br/>
*  Copy/paste `wget -O lab01_images.tar.gz https://osf.io/bprq5/download/`
*  Use commands introduced above to see what was downloaded
*  To unpack the download copy/paste `tar -xvf lab01_images.tar.gz`
*  Move yourself into the folder with data by typing `cd osfshare`
<br/>

**Open fsleyes**: <br/>
*  Open fsleyes by typing `fsleyes &`
  *  The `&` at the end tells the terminal to run this program in the background, meanwhile you still have access to the terminal to run other commands
<br/>

**Open an image with fsleyes**: <br/>
*  Click on `File` -> `Add from file`
*  Open the file named `sub-97-T1w_defaced.nii.gz`
  * "defaced" means the face has been removed from the image, why do you think we did that?
<br/>

**Understanding the T1 image**: <br/>
* Guided observations: review these questions with your group
  * Place your cursor at different places in the image and see how it affects coordinates shown here to identify the x, y, and z-axis of the image:</br>
    ![image-location-coordinate](images/Introduction-to-FSLeyes_coordinates.png). 
    * How many dimensions are there in the image?
  * In addition to the coordinate location of your cursor, what additional information is being shown here?
    ![image-location-coordinate-intensity](images/Introduction-to-FSLeyes_coordinates-and-intensity.png)
  * Image intensity
    * Order gray matter, white matter, and CSF according to their brightness on the image.
    * Use the menu at the top-left of your screen to view a histogram of the image intensities: View -> Histogram
      * This is a histogram of all the values in the image. Does its shape surprise you?
    * Now open the image `sub-97-T1w_brain.nii.gz` and view its histogram in comparison 
      * How does the shape correspond to gray matter, white matter, and CSF?
<br/>

**Terms for talking about anatomy in an image**: <br/>
*  Hover over these icons and click them on/off to see how they affect the cross-sectional views of the brain that you can see: <br/>
![slice-view-tool](images/Introduction-to-FSLeyes_slice-view-tool.png). 
  * These are sagittal, coronal, and axial views respectively. 
*  What do the letters on the four sides of each view shown below mean: R/L, A/P, S/I <br/>
![slice-view-orthogonal](images/Introduction-to-FSLeyes_slice-view-orthogonal.png)
<br/>


**Add a functional image on top of the structural**: <br/>
* Use steps learned from above to add a new image
  * Add the file named `sub-97_task-flanker_bold.nii.gz`
  * Place cursor in the brain somewhere and then toggle on/off the functional image using this button: 
![toggle-eye-fsleyes](images/Introduction-to-FSLeyes_toggle-eye-fsleyes.png)
* Guided observations: review these questions with your group
  * Order gray matter, white matter, and CSF according to their brightness on the image. How does the order compare to the T1 image?
  * Does the functional image have more or less anatomical detail than the structural T1 image?
  * Use the menu at the top-left of your screen to view the BOLD timeseries for a voxel: View -> Time Series
    * Compare time series data inside and outside the brain
    * Change plotting mode to compare magnitude of signal change in different locations. How does the percent signal change compare in gray matter to within the eyes and outside the brain?
  * How many dimensions are there in the functional image?
* Example with timeseries viewer on:
  ![fsleyes-timeseries](images/Introduction-to-FSLeyes_time-series.png)