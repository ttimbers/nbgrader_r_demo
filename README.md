# Working example of nbgrader from the command line using R and `testthat`:

### Dependencies:
(click on links for download and installation instructions)
- [Python & Jupyter](https://www.anaconda.com/download/)
- [nbgrader](http://nbgrader.readthedocs.io/en/stable/user_guide/installation.html)
- [nbgrader Formgrader extension](http://nbgrader.readthedocs.io/en/stable/user_guide/installation.html#installing-and-activating-extensions)
- [R](https://cran.r-project.org/) & R packages 
    - [IR kernel](https://irkernel.github.io/installation/)
    - [testthat](https://github.com/r-lib/testthat#installation)
    - [digest](https://github.com/eddelbuettel/digest) (should be installed as a dependency of IR kernel...)
    - [stringr](https://github.com/tidyverse/stringr)


## The Demo (how I created it and ran it):
1. I cloned/download this repository and navigate into its root directory in the terminal. *Note - I used `nbgrader quickstart course_id` to generate course template and nbgrader config file.*

2. I edited [`nbgrader_config.py`](https://github.com/ttimbers/nbgrader_r_demo/blob/master/nbgrader_config.py) to specify the “exchange” directory to be `/tmp/exchange`, to enable R to be used with nbgrader, edited the name of the assignment, and commented out `c.IncludeHeaderFooter.header = "source/header.ipynb"` (so that I don't need to have a separate header notebook).

*note - thanks to https://github.com/sagemathinc/cocalc/wiki/nbgrader for suggestions on how to modify the config file to work with R!*

3. On my computer, I ran the following at the command line to "refresh" the “exchange” directory:
```
# remove existing directory, so we can start fresh for demo purposes
rm -rf /tmp/exchange

# create the exchange directory, with write permissions for everyone
mkdir /tmp/exchange
chmod ugo+rw /tmp/exchange
```

4. I created the assignment (`source/lab1/lab1_r_notebook.ipynb`) with solutions and tests in a Jupyter notebook. It is important to ensure you have the assignment toolbar and choose the correct cell types from the drop-down menu. You can access this via View > Cell Toolbar > Create Assignment. Then assign points for the question in the autograder test cells.

5. I created the student version of the assignments:
```
nbgrader assign --force lab1
```

*note - in this example it is assumed we would use some mechanism (not nbgrader) to then distribute copies of this assignment and then collect them into the `submitted` directory after the students worked on them. Here for this demo, I manually create the `submitted` directory and the student directories inside this (which correspond to the students in the config file):*

```
mkdir submitted
mkdir submitted/hacker
cp -r release/lab1 submitted/hacker/lab1
mkdir submitted/bitdiddle
cp -r release/lab1 submitted/bitdiddle/lab1
```

*I then manually edited the submitted notebooks with sample work a student might hand in.*

6. I then ran the autograder:
```
nbgrader autograde lab1
```
*note - to do manual grading you need to click on the "Formgrader" tab from the Jupyter Home page and then click on manual grading.*

7. I then created the feedback form for students:
```
nbgrader feedback lab1
```

8. I finally exported the database to a `.csv`:
```
nbgrader export
```

*note - using SQLite you can get more info from `gradebook.db` than you can get from the exported `.csv`*
