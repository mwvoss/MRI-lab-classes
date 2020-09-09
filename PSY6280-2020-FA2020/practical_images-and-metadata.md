The purpose of this lab is to understand the basics of how images are stored and get hands-on practice interacting with images and the meta-data in their headers through the command line. 

**By the end of this practical you should be able to:** <br/>
* [ ] understand the difference between image-voxel and scanner-world coordinates  <br/>
* [ ] recognize nifti files and access their header data with `fslinfo` and `fslhd` <br/>
* [ ] use header data to describe anatomical image resolution: field of view, matrix size, slice thickness, voxel dimensions <br/>
* [ ] use header data to identify image orientation  <br/>
<br/>


**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>


**Download our data for today**: <br/>
*  Open the terminal
*  Change directories from your home directory to `fmriLab` with the command: `cd fmriLab`
*  To download data, copy/paste to the terminal `wget -O ds005.tar.gz https://www.dropbox.com/s/a3xsd5svookh6f9/ds005.tar.gz?dl=0`
*  To unpack the download copy/paste `tar -xvf ds005.tar.gz`
*  Move yourself into the folder with data by typing `cd ds005`
<br/>


<br/>

**Introduction to two coordinate spaces for our MR images:** <br/>
* **Image-voxel space**
  * Think of this as image storage space with voxels as our 3-dimenstional pixels that are our spatial unit of measurement for intensity values in the image. The coordinate origin of this space is (i,j,k)=0,0,0, positioned in the corner of the image as the initial index for storage of data in a matrix, where all values are positive and refer to the location of the mid-point of each whole voxel that will be given an image intensity value.
* **Scanner-world space**
  * Think of this as our destination space for our measurements of the brain, where we have mapped the image to the object as positioned in the physical world of the scanner using common scientific units of distance such as millimeters (mm). The coordinate origin of this space is (x,y,z)=0,0,0, positioned near the center of the image bounding box for subject-space and at a specific anatomical landmark near the center of the brain for standard-space (e.g., anterior commissure), so there will be both negative and positive values in this coordinate space.
<br/>
<br/>

**The table below summarizes common coordinate spaces in MRI/fMRI, with anatomical labels and common terms for referencing image orientation:**
    ![image-coordinate-spaces](images/images-and-metadata_coordinate-spaces-table.png)
<br/>
<br/>

**The figure below visualizes voxel and world space in the LPI/RAS+ orientation that NIfTI images are reconstructed to:** <br/>
    ![image-coordinate-spaces](images/images-and-metadata_coordinate-spaces.png)
<br/>


**Now let's look at how we access this information within an image header**: <br/>
* Go back to the terminal, and move yourself to where the T1 image is in our lab data set. 
  * `cd ~/fmriLab/ds005/sub-01/anat`
  * see what is here by typing `ls`
* Use the command-line tool `fslhd` to see the meta-data for the T1 image
  * see usage by typing `fslhd` in the terminal
* Make a text file of the header contents by redirecting the output of the command to a text file
  * Copy/paste into the terminal: `fslhd sub-01_anat_sub-01_T1w.nii.gz > sub-01_anat_sub-01_T1w_header-annotated.txt`
  * Open the text file with gedit: `gedit sub-01_anat_sub-01_T1w_header-annotated.txt`
* In breakout groups
  * Use the NIfTI reference links from our readings for today to comment in annotations for the following lines in the header:
    * Lines 4 through 8
    * Lines 17 through 21
    * Lines 45 through 62
    * Pick someone in your group who's willing to share your group's answers 
  * Draw the image space as a cube with labels for the following information:
    * Size of the matrix for a slice (hint it's square)
    * Slice thickness and orientation
    * Voxel size
    * Pick someone in your group who's willing to share their drawing


**Use what you've learned to inspect the header for the fMR image**: <br/>
* Go back to the terminal, and move yourself to where the functional image is in our lab data set. 
  * `cd ~/fmriLab/ds005/sub-01/func`
  * In breakout groups
    * Compare and contrast the image data for lines we've annotated above
      * How does the slice orientation differ?
      * How many volumes are in the functional image?
      * How does the voxel size differ?
      * What is the time of repetition (TR) for each volume?
      * How does the image orientation differ?
  * Draw the image space as a cube with labels for the following information:
      * Size of the matrix for a slice (hint it's square)
      * Slice thickness and orientation
      * Voxel size
      * Pick someone in your group who's willing to share their drawing

**Lab homework data**: <br/>
*  Open the terminal
*  Change directories from your home directory to `fmriLab` with the command: `cd fmriLab`
*  Download new data by copying/pasting to the terminal `wget -O ds003030.tar.gz https://www.dropbox.com/s/phxqxtclprz5gef/ds003030.tar.gz?dl=0`
*  To unpack the download copy/paste `tar -xvf ds003030.tar.gz`
*  Move yourself into the folder with data by typing `cd ds003030`
<br/>


