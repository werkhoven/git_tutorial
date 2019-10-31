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

## User credentials

Before starting we need to tell Git who we are by configuring some global variables. Define your name and email via the following:

```
git config --global user.name "John Doe"
git config --global user.email johndoe@gmail.com
```

## Initialize a repo

Navigate to where you would like to start your new repo via the terminal and create a new directory, preferrably one to match the name of your new repo.

*Unix/Linux*
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

Press *i* to insert into the editor and edit the file to define a new function.

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
As we can see, so far git is not tracking any of our new files.
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        myfunc.m
        readme.md
```

Track the new files.

```
git add myfunc.m readme.md
```

Checking the status, we now see that our files are staged for commit.

```
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   myfunc.m
        new file:   readme.md
```

Now that our modified files are staged, we can commit the changes and append a message so we can have some idea of what changes we made during the commit.

```
git commit -m 'initial commit'
```

The following image summarizes some of the core concepts of the git workflow.

![GitWorkflow](https://cdn-media-1.freecodecamp.org/images/1*iL2J8k4ygQlg3xriKGimbQ.png)

## Linking the repo to GitHub

Currently we only have what called a *Local Repository* because it only exists on our local machine. We can sync our new repo to an new *Remote Repository* on GitHub so that anyone can access our work from anywhere and always get the most up to date versions:

1. Log in to your GitHub account online
2. Create a new repository (via plus sign in upper right corner)
3. Name the repo `git_tutorial`
4. Leave all other options default and select "Create Repository"

We can now upload or *push* the new changes made in our local repo to the remote repo. GitHub has conveniently provided the code for us. Copy the command from GitHub and execute the following in the terminal:

```
git remote add origin https://github.com/winsl0w/git_tutorial.git
git push -u origin master
```

After entering your user credentials when prompted, you should have made your first successful commit to GitHub. The first command adds the URL for our repository and saves it to the tag *origin*. The second pushes to our current branch *master* to the URL stored in *origin*.

## Creating a new branch

One of the most compelling reasons is the ability to branch your repository. This allows the freedom to experiment with new modifications on your work while maintaining the ability of reverting to past states of the project. Stable versions of your code can be made available while experimenting with new features. Moreoever multiple people can work on the same project in parallel, later merging their work together.

Create a new branch of our repo named `test_branch`.

```
git checkout -b test_branch
```

The `checkout` command is used to swap between branches. The `-b` flag tells Git to create the branch before switching. If we inspect the files in our repo with `ls`, we can see that all the work from our `master` branch has been copied into the new branch. From this new branch, we can safely make changes without editing `master`.

Let's make some changes to our code. Reopen the file with Vim (`vi myfunc.m`), add a new line of code to the function, and save the changes (*Esc* + `:wq`).

```Matlab
function out = myfunc(arg)

arg = arg + 2:
out = arg + 2;
```

Inspecting the status of the repo (`git status`) confirms that our file has been modified. Now stage the file and commit the changes.

```
git add myfunc.m
git commit -m 'added two to arg'
```

## Merging branches

The `git merge` command allows us to merge to branches in our repo together, pulling any changes not made is one branch into the other. Additionally, it will prompt us to resolve any conflicts between the two branches.

Currently, the master branch does not have any changes that the test branch does not. Try merging master into test:

```
git merge master
```

We receive a message telling us that `test_branch` is already up to date with `master`. It is important to note that this merge is a *directional* operation. Because we are currently on the test branch, we are merging `master` into `test_branch` and not the other way around. Now checkout the master branch and perform the reverse operation.

```
git checkout master
git merge test_branch
```

Because `test_branch` contains changes not present in `master`, we see a message notifying us that a single change has been made to our function.

```
Fast-forward
 myfunc.m               |   1 +
 1 files changed, 1 insertion(+)
```

## Merge conflicts

Sometimes our files cannot be directly merged because they contain changes that are in direct conflict with one another. Let's deliberately create a conflict between our two branches. From `master`, open our function with Vim and make the below edits to the file. Note that what was previously an addition operation has now been replaced with division.

```Matlab
function out = myfunc(arg)

arg = arg / 2:
out = arg + 2;
```

 Now add and commit the changes. Then checkout the test branch again.

 ```
 git add myfunc.m
 git commit -m 'replaced addition with division'
 git checkout test_branch
 ```

 Now make the following conflicting to changes to the test branch.

 ```Matlab
function out = myfunc(arg)

arg = arg - 2:
out = arg + 2;
```

Add and commit the changes to the test branch, then checkout the master branch again.

 ```
 git add myfunc.m
 git commit -m 'replaced addition with division'
 git checkout master
 ```

 Now when we attempt to merge `test_branch` into `master` we receive the following warning.

 ```
 $ git merge test_branch
Auto-merging myfunc.m
CONFLICT (content): Merge conflict in myfunc.m
Automatic merge failed; fix conflicts and then commit the result.
```

If we inspect the status of the repo, we see that `myfunc.m` is not merged:

```
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   myfunc.m
```

Opening the file in Vim to resolve the conflict, we can see that Git has automatically inserted some lines to flag the specific conflict between the two branches as well as marking to source branch for each edit. *Note*: HEAD indicates the head of the current branch, in this case `master`.

```Matlab
function out = myfunc(arg)

<<<<<<< HEAD
arg = arg / 2;
=======
arg = arg - 2;
>>>>>>> test_branch
out = arg + 2;
```

Resolve the conflict by rejecting the changes from the master branch, resulting in the function below. Then save and quit the file.

 ```Matlab
function out = myfunc(arg)

arg = arg - 2:
out = arg + 2;
```

Now we can add and commit the changes to complete the merge.

```
git add myfunc.m
git commit -m 'merge test_branch'
```

## Pushing and pulling the remote repository

Now that we have made some changes to our local repository, we update the remote repo with our changes.

```
git push origin master
```

If we refresh the GitHub remote repo, we should see our updated files as well as our most recent commit messages for each file on the remote repository.

In addition to pushing our work to the repo, we may want to *pull* work from the remote repository to our local repository. From the remote repo, open `myfunc.m` and select to edit the file (pencil icon on the upper right). Edit the file to include a print statement with the output value and then select *Commit changes* at the bottom of the screen.

```Matlab
function out = myfunc(arg)

arg = arg - 2;
out = arg + 2;
fprintf('The value of out is %i\n',out);
```

Now from the local repository, open our file again in Vim and add another print statement indicating the start of the function.

```Matlab
function out = myfunc(arg)

fprintf('Starting up the function\n');
arg = arg - 2;
out = arg + 2;
```

Now add and commit the changes. We now have a conflict between the remote repo master branch and our local master branch. Pull the remote changes via:

```
git pull origin master
```





