# De Bivort Lab Git Tutorial

## Install

*Linux* 

From terminal:
```
sudo apt get install git-all
```

*Mac*

From terminal, run the following command and follow the prompt.
```
git --version
```

*Windows*

Follow the link to <a src="https://git-scm.com/downloads">download git</a> and run the setup wizard. Select to install Git Bash as well when prompted. Launch Git Bash after installation. This will serve as your terminal.

## Initialize a repo

Navigate to where you would like to start your new repo via the terminal and create a new directory, preferrably one to match the name of your new repo.

*Unix/Linux
```
md git_tutorial
cd git_tutorial
```
*Windows*
```
mkdir git_tutorial
cd git_tutorial
```

Now initalize an empty repository via:

```
git init
```

Populate the empty repo with two new files:
1. A new piece of code to define some function
2. A readme Markdown file to describe our repo's purpose (fill it in later)

```
touch myfunc.m
touch readme.md
```

Now lets open our code file with Vim to familiarize ourselves with the very basics of the Vim text editor.

```
vi myfunc.m
```

And edit the file to define a new function.

```Matlab
function out = myfunc(arg)

out = arg + 2;
```

To save our edits and exit Vim, we can instruct it to *Write* and *Quit* by pressing *ESC* and run

```
:wq
```

## Tracking and committing changes

So far our repo is not tracking any of our work. To instruct it to track changes to our files, we have to first mark them to be tracked. This is called *staging* commits, which essentially means that the changes we have made will be queued for saving to the repo next time we commit. We can inspect which files are tracked/modified via:

```
git status
```

