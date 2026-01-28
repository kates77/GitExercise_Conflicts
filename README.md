# GitExercise_Conflicts


## Purpose
To teach you to deal with git merge conflicts in 
- Jupyter Notebooks
- on Datahub using graphical notebook based tools

The instructions here are specific to the COGS 108 Datahub setup. 

Note that this is different than your previous exercises... they were on GitHub (not on Datahub) and there were no graphical diff tools in use



## Problem overview

This repo contains two branches: main and ex1. There is a single line text conflict in one cell between the two branches, as well as other line differences that do not conflict.

Below is a graph of the commit situation in the repo right now... each * indicates a commit and the lines show the evolution of the repo over time

```
----*----*[main]
     \
      \----*[ex1]
```

We will do two seperate git operations 
- [Part 1] add a new change to branch main, thus introducing a second difference with ex1 branch
- [Part 2] then we will merge the changes from branch ex1 into main

Thus after Part 1 the new graph will look like

```
----*----*----*[main]
     \
      \----*[ex1]
```

And after Part 2 the new graph will look like

```
----*----*----*----*[main]
     \             /
      \----*------/
```

## Setup this stuff first
1. Fork this repo by pressing the Fork button at the top of this webpage
     - When forking **UNCHECK** the default option to "Copy the main branch only". You want all the branches!
     -  Forking this repo made a copy of this repo on YOUR GitHub.  You have direct write access to the forked copy, but not the original repo
1. Let's get a local copy of your forked repo...
     - login to Datahub
     - if you have not already created SSH keys for datahub, do so now and place them on your GitHub account per https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account  <br> **NOTE:** these keys are different than the ones you made on your laptop. Each computer you use with GitHub needs its own keys!
     - Open a terminal window on Datahub.
     - Type: `cd ~/private` which will put you inside a subdirectory of your datahub file system.
     - Type: `git clone git@github.com:your_username/GitExercise_Conflicts.git` <br> but replace  `your_username` with your actual GitHub username
     - Your new local copy of the repository will appear in the file browser inside the folder `~/private/GitExercise_Conflicts` Double click around the file browser until you find the folder and open it

## Part I - Make a local change on main
1.  Open `Project.ipynb`
     -  Just for fun (you don't have to do this substep, but its a good teaching point)
          -  run both cells of the notebook to see the plot
          -  Unfortunately running the notebook changes the metadata stored inside it.  The counter for how many times a cell was run, the binary data from the plot.  This is stuff we do NOT want to version control... its not important and it clogs up the system because notebook metadata for plots can be very big.
          -  Save the notebook, open a terminal window in this directory and type `git diff`, now you will see the insane line differences generated just by running the notebook
          -  Before we do any git operations it is a best practice to click on the pulldown menu item `Edit -> Clear Outputs of All Cells`.  Do this now, and press save so we don't have to put this BS into the git history
1. Lets add a change we do want:
     - the last line of the notebook currently says `ax.grid()` which (although it works) is bad Python form...
     - change it to the more readable and more Pythonic `ax.grid('on')` 
     - save the file when you are done!
1. Add / commit the changes using the GUI (git extension for Jupyterlab)
     - Yes you could do all this in the terminal window, but I'm showing you the GUI way here.  If you're a command line hero, feel free to do this task there :)
     - On the far left you will see a vertical stack of icons in a grey panel.  Click on the one that's a diamond with some lines inside it, that's the git panel.
     -  At the bottom of the git panel you can see there are three categories of files that could be the subject of git operations <br>
         "Untracked" for new files you create that are not yet in the repo  <br>
         "Changed" for files already in the repo, but you changed something  <br>
         "Staged" which tracks all the changes you've asked to be grouped together into a commit  <br>
     -  Look at the "Changed" section, `Project.ipynb` is there... hover your mouse over it.  You will see icons for opening the file, for making a diff summary of the changes, for discarding the changes, and one for staging the changes for a commit (a + symbol).
     -  Stage the changes!
     -  Now that `Project.ipynb` is in the "Staged" category you can scroll down to the bottom of the git panel and commit the changes.  Add a summary (<50 characters), e.g.  "improved code readability".  There is also a space for longer description if necessary.  Press the "Commit" button, and you're done with Part I!

## Part II - Merge ex1 into main
1. Get a local copy of the ex1 branch using the GUI. <br> When you cloned the repository you only got a copy of the default branch "main"... there were other branches but they are still only on the remote repo up on GitHub.  Remote branches are marked with the keyword "origin". So "main" is a local branch, "origin/main" is the main branch on GitHub.
     - On the git panel there's a place at the top where it says "Current Branch: main"; click on that menu item to see a list of all the branches available.
     - Click on "origin/ex1", this makes a local copy called "ex1"  and sets it up to track the remote branch "origin/ex1".
     - Now that we have local copies of both main and ex1 we can merge the branches
1. Merge ex1 into main
     -  OK here's the task: we want to merge the change in branch ex1 (a change of title on the graph) into main, keeping the ex1 title erasing the old title on main.  But we want to keep our `ax.grid('on')` change which is only on main and not ex1. 
     -  Here's how to do it... click on "main" to make sure we are currently on the main branch.  We want to be on the branch that is the destination of the merge.
     -  Hover over the local "ex1" branch and a button icon that looks like 3 dots with connecting lines will appear.. when you hover over the button it says "merge this branch into the current one"... go ahead and click it!
     -  You will get an error message "Failed to merge ex1 into main".  And now there is a new category "Conflicted" just above "Staged"!  Our file `Project.ipynb` is in that heading as expected!  Hover over the file, and click on the icon to "diff this file" that appears.
     -  You will now see the diff panel... where there are conflicts you have from left to right three possiblities: <br>
         "Current" (what was in main before the merge) <br>
         "Common Ancestor" (what the file looked like when ex1 split off from main) <br>
         "Incoming" (what was in ex1 before the merge) <br>

