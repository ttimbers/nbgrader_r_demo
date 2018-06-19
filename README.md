## Working example of nbgrader from the command line using R and `testthat`:

1. Clone/download this repository and navigate into its root directory in the terminal. *Note - I used `nbgrader quickstart course_id` to generate course template and nbgrader config file.*

2. Added this to `nbgrader_config.py`
```
c.Exchange.root = "/tmp/exchange"

## The delimiter marking the beginning of a solution
c.ClearSolutions.begin_solution_delimeter = 'BEGIN SOLUTION'

## The code snippet that will replace code solutions
c.ClearSolutions.code_stub = {
    "r": "# your code here\nfail() # No Answer - remove if you provide an answer",
    "python": "# your code here\nraise NotImplementedError",
    "javascript": "// your code here\nthrow new Error();"
}

## The delimiter marking the end of a solution
c.ClearSolutions.end_solution_delimeter = 'END SOLUTION'
```

*note - thanks to https://github.com/sagemathinc/cocalc/wiki/nbgrader for suggestions on how to modify the config file to work with R!*

3. specify the “exchange” directory to be `/tmp/exchange`
```
# remove existing directory, so we can start fresh for demo purposes
rm -rf /tmp/exchange

# create the exchange directory, with write permissions for everyone
mkdir /tmp/exchange
chmod ugo+rw /tmp/exchange
```

4. Create the assignment with solutions and tests in a Jupyter notebook. It is important to ensure you have the assignment toolbar and choose the correct cell types from the drop-down menu. You can access this via View > Cell Toolbar > Create Assignment. Then assign points for the question in the autograder test cells.

5. Create the student version of the assignments:
```
nbgrader assign ps1
```

*note - in this example it is assumed we would use some mechanism (not nbgrader) to then distribute copies of this assignment and then collect them into the `submitted` directory after the students worked on them. Here for this demo, I manually create the `submitted` directory and the student directories inside this (which correspond to the students in the config file):*

```
mkdir submitted
mkdir submitted/hacker
cp -r release/ps1 submitted/hacker/ps1
mkdir submitted/bitdiddle
cp -r release/ps1 submitted/bitdiddle/ps1
```

*I then manually edited the submitted notebooks with sample work a student might hand in.*

7. Run the autograder:
```
nbgrader autograde ps1
```

8. Export the database to a csv:
```
nbgrader export
```
