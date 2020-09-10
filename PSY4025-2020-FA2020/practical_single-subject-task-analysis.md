**By the end of this practical you should be able to:** <br/>
* [ ] create and understand a model of task-related BOLD signal in the FEAT GUI <br/>
* [ ] understand how to test for activation differences between conditions <br/>
* [ ] locate and view the activation maps in fsleyes <br/>
<br/>

**Access FastX** through the remote login: <br>
https://fastx.divms.uiowa.edu:3443/  <br/>
<br/>

**Create model of task-related BOLD signal in FEAT:** <br/>
*  In terminal, move yourself to the derivatives folder by typing `cd ~/fmriLab/data/bids/derivatives/`
*  Type `fsl` and click on `FEAT FMRI analysis`
*  Now instead of `Preprocessing` select `Statistics` in the drop-down menu at the top, like so: 
![feat-set-statistics-only](images/single-subject_feat-set-statistics-only.png)

*  Then select `Input is a FEAT directory` and navigate to `sub-001`'s `flanker.feat` directory: 
![feat-select-featdir](images/single-subject_feat-select-featdir.png)

*  Move to the `Stats` tab, first turn on `Add additional confound EVs` and then select the text file that is shown below: <br/>
![feat-confound-is-outliers-textfile](images/single-subject_feat-confound-is-outliers-textfile.png)

*  Next, click on `Full model setup`. We will walk through set up of this as a class, including a closer look at the structure of the input text files. However, for reference, tabs for each explanatory variable (EV) should look like what is shown below: <br/>

EV1: <br/>
![feat-full-model-setup-ev1](images/single-subject_feat-full-model-setup-ev1.png) <br/>

EV2: <br/>
![feat-full-model-setup-ev2](images/single-subject_feat-full-model-setup-ev2.png) <br/>

EV3: <br/>
![feat-full-model-setup-ev3](images/single-subject_feat-full-model-setup-ev3.png) <br/>

EV4: <br/>
![feat-full-model-setup-ev4](images/single-subject_feat-full-model-setup-ev4.png) <br/>

*  After setting up the EVs, stay in the `Full model setup` window and go to the `Contrasts & F-tests` tab. Set up the contrasts as below, and we will discuss what this means. Click `Done` when setup complete. <br/>
![feat-contrast-settings](images/single-subject_feat-contrast-settings.png)

* After completing the `Full model setup` you will get your model! Let's walk through it to understand the figure. <br/>
![feat-model-with-temporal-derivative](images/single-subject_feat-model-with-temporal-derivative.png)

* The last tab in the FEAT GUI is `Post-stats`. We will leave the defaults on for now. The settings here are most relevant when we get to group-level analyses.

* Click `Go`. When it's finished, the results will appear within your `flanker.feat` directory. Your html report will then include output of brain activation maps as previewed below. We will walk through the contents of the report in class. <br/>
![feat-html-report-header-afterstats](images/single-subject_feat-html-report-header-afterstats.png)

* Using `fsleyes` will allow us to view the results more interactively:
    * Use the html report to locate the directory where the activation maps are on your computer
    * Use the terminal to move yourself there: `cd ~/fmriLab/data/bids/derivatives/sub-001/func/flanker.feat`
    * Open fsleyes through the terminal with settings for viewing FEAT output: `fsleyes -ad filtered_func_data.nii.gz stats/zstat1 stats/zstat2`
    * You should see a display like below in `fsleyes`. Clicking on the buttons with arrows in the column labeled `Z Max location` will move your cursor to the location of that peak in brain activation. With this interactive table open, you can also view your activation maps using the `Lightbox` view we previewed when learning about `fsleyes`. Give it a try!
</br>

![feat-in-fsleyes](images/single-subject_feat-in-fsleyes.png)

</br>
* Once you've completed all the steps above for `sub-001`, repeat for `sub-002`. Before next class, you should have statistical maps for the same contrasts, in the **same** order, for both subjects.


