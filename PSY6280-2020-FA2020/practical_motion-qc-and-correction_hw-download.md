
**Download links and prep for Lab 6**: <br/>

*  Open the terminal
*  Change directories from your home directory to `fmriLab` with the command: `cd ~/fmriLab/ds003030`
*  Download new subjects:
    * Steps to download `sub-02`
        * `wget -O sub-02.tar.gz https://www.dropbox.com/s/5q3zntfbmtsah3t/sub-02.tar.gz?dl=0`
        * `tar -xvf sub-02.tar.gz`
        * `rm sub-02.tar.gz`


    * Repeat to download `sub-03`
        * `wget -O sub-03.tar.gz https://www.dropbox.com/s/wpemy4fixqpt7ru/sub-03.tar.gz?dl=0`
        * `tar -xvf sub-03.tar.gz`
        * `rm sub-03.tar.gz`

* Prepare derivative directories for motion
    * `mkdir -p ~/fmriLab/ds003030/derivatives/func/sub-02/motion`
        * copy our bold image here for ease to work with: `cp ~/fmriLab/ds003030/sub-02/func/sub-02_func_sub-02_task-FlickeringCheckerBoard_run-1_bold.nii.gz ~/fmriLab/ds003030/derivatives/func/sub-02/motion/sub-02_task-bold.nii.gz`
    * `mkdir -p ~/fmriLab/ds003030/derivatives/func/sub-03/motion`
        * copy our bold image here for ease to work with: `cp ~/fmriLab/ds003030/sub-03/func/sub-03_func_sub-03_task-FlickeringCheckerBoard_run-1_bold.nii.gz ~/fmriLab/ds003030/derivatives/func/sub-03/motion/sub-03_task-bold.nii.gz`

* Download mriqc reports for each subject's funcitonal data
    * [sub-02](https://www.dropbox.com/s/pi21o5p9xcbxko5/sub-02_MRIQC_%20individual%20bold%20report.pdf?dl=0)
    * [sub-03](https://www.dropbox.com/s/mgnlvdci6mx51qj/sub-03_MRIQC_%20individual%20bold%20report.pdf?dl=0)

