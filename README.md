Lab - Git
======================

This lab explores the use of git (and GitHub) by a team. It assumes you are already familiar with the basics of git. Make sure to look at the details of assessment at the end of this file.

You must first join a "group" on Canvas (details on the Assignment page for the lab). Then you will need to set up your teams. This process is described [here](JoiningTeams.md).      
Issues arise when multiple people use the same git repository. The exercises in the lab explore some of them. It assumes you are working in a team of 3, consisting of team members Dev1, Dev2, and Dev 3.

## Important note about branching

**Main** - This branch is your "local" or "remote" main branch. Note that GitHub used to call it "master" until 2020. So, do not get confused to see the term "master" in some of the GitHub screenshots below. On a side note, here is an interesting read about this name change: https://www.theserverside.com/feature/Why-GitHub-renamed-its-master-branch-to-main

**feature1, feature2 and feature3** - These branches are to be created and used once you get to Exercise 2.

**Submission** - This branch is only for making your final submission for this lab, and must be used only after you have made all code corrections (for Exercise 1) AND implemented all specified features (for Exercise), and only one of the team members must push to this branch on behalf of the team. Read carefully the instructions and rules associated with this special branch in Exercise 2 description.

## Exercise 1 - Basic Git

For this exercise the three team members will individually complete the tasks
below to fix faults in their own copy of the repository. The issue is then how
to combine all of the changes into a single version on the remote repository.

***Task 1: Increment Fix***
Find and fix the fault in Counter increment(). All code changes and relevant commits must be performed on the main branch.

***Task 2: Decrement Fix***
Find and fix the fault in Counter decrement(). All code changes and relevant commits must be performed on the main branch.

***Task 3: Reset Fix***
Find and fix the fault in Counter reset(). All code changes and relevant commits must be performed on the main branch.

Each team member makes the change, commits it to their local repository (of
course making meaningful commit messages!) and then attempts to push the
changes to the remote repository. The first one should work without problems,
but for the second and third, the local repositories are now out of date with
respect to the remote repository. Note that all of this should be done on the main branch. **Using separate branches is for a later exercise.**

<ol>
  <li>Dev1,2,3 - clone the project to the local repository. Doing this
  in Eclipse as follows (note that use of Eclipse is not required):
  		<ul>
  			<li>import as a project from Git</li>
  			<li>Right click on project, Configure > Convert to Maven project</li>
  			<li>Run the project with "package" goal, all tests should fail</li>
  		</ul>
  <li>Dev1,2,3 - performs tasks 1, 2 and 3 (below) respectively on their own local source code</li>
  <li>Dev1 - stage, commit and push the changes for task 1</li>
  <li>Dev2 - perform code synchonisation as explained below and push the changes for task 2</li>
  <li>Dev3 - perform code synchonisation as explained below and push the changes for task 3</li>
</ol>

#### Testing ####
There are four test scripts in /src/test folder. The `TestCounter` is for testing changes you have to perform for this exercise, while the others are for the next exercise. You should read the source code in this test script and find the test method that test your task, for example, `testIncrement()` is for testing Task1. After you make the change according to your task, you can execute this test script by running Maven with the `test` goal. This will compile and run all tests on the project. The task is complete when the test relevant to the task passes.

#### Continuous Integration ####

In COMPSCI331 we will be using the continuous integration (CI) feature of
GitHub. Basically this means that whenever you push something to your
repository, some tests will be run. You will receive email summarising the
results of the tests. The results are also available on GitHub. **Note that for this course, CI workflow tests will only execute when pushing to the "submission" branch, and that there is a restriction that you may only push to submission 3 times max for all assessments in this course. Read the Assessment section for more details.** Further
information about CI are below and more will be provided in later labs. For
now, just be aware that any push to the "submission" branch (even if you are not changing code) will
cause the tests to be run and email sent to you. 

#### Code Synchonisation

When Dev2 and Dev3 try to commit and push their changes, the push should fail and show the error as shown in the figure below (Eclipse). This is because Dev1 has already pushed the source code to Git so the source code that Dev2 and Dev3 have is not in sync with the code on the remote repository. Github does not allow you to push the source code for this reason and therefore rejects the attempt.

![](src/resources/rejected-commit.png)

The term "non-fast-forward" is a complicated way to say that there is a newer version of the file being pushed on the remote repository, usually because someone else has changed the file and pushed it to the remote repository. The command line message is slightly more useful:

```
! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/COMPSCI331-2022/week02lab-git.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Note the comment about <tt>git pull...</tt>, and you can ignore the
"fast-forwards" hint.

What Dev2 and Dev3 now need to do (first one, then the other, otherwise the
same problem will occur) is to perform a 'pull to merge'. This is done by issuing
a "pull" either through the IDE or command line (<tt>git pull</tt>).

From Eclipse, ensure "Merge" is selected when doing the merge.

![](src/resources/pullmerge.png)

From the command line when you run this command you will see something like:
```
Auto-merging filethatwaschanged.txt
CONFLICT (content): Merge conflict in filethatwaschanged.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Now the file affected (in this case <tt>filethatwaschanged.txt</tt>) will have both sets of changes. When the <tt>pull</tt> is done (really the <tt>merge</tt> that is part of the <tt>pull</tt>), the two sets of changes are merged into the file, with annotations to show which is which. This will be shown in the file with something like:

```
<<<<<<< HEAD
one set of changes
=======
the other set of changes
>>>>>>> main
```

What the dev needs to do is figure out how to integrate these two sets of
changes, generally referred to as resolving the conflict. How this is done depends on exactly what the changes are. The
main thing is that the annotations (<tt>&lt;&lt;&lt;&lt;&lt;&lt;&lt;</tt>,
<tt>&gt;&gt;&gt;&gt;&gt;&gt;&gt;</tt>,
<tt>======</tt>) have to be removed. For example:
```
the other set of changes
one set of changes
```
In this case the decision was made to alter the order the changes appear in the
file. Now the file can be committed and pushed to the remote repository.

Once both Dev2 and Dev3 have resolved the conflicts this exercise is complete. 

## Exercise 2 - Git Branches

For this exercise, the three developers will again make three changes (this time adding features), but on different branches. 
**Before starting this exercise, please make sure that all three developers pull the latest source code from the repository.**

Feature 1 by Dev1 is to implement the increment method **incrementToEven()** that increases the counter to the next even number, and implement the decrement method **decrementToEven()** that decreases the counter to the previous even number.

Feature 2 by Dev2 is to implement the increment method **incrementToPrime()** that increases the counter to the next prime number, and implement the decrement method **decrementToPrime()** that decreases the counter to the previous prime number.

Feature 3 by Dev3 is to implement the **countFrequency()** method. This method counts the number of words in the given sentence. Dev3 should also consider refactoring the code implemented by Dev1 and Dev2. The code refactoring should improve the overall quality of source code such as getting rid of duplicate code, apply the standard code convention, and so on.

#### Development Process

Each dev works on these features on three separate branches, namely feature1, feature2 and feature3, before merging them into the main branch. The overall process is:

<ol>
  <li>Dev1,2,3 - clone the project to local repository</li>
  <li>Dev1,2,3 - implement feature 1, 2 and 3 respectively locally</li>
  <li>Dev1 - run tests locally (Maven 'Verify'), stage, commit and push changes on the local feature1 branch</li>
  <li>Dev2 - run tests locally (Maven 'Verify'), stage, commit and push changes on the local feature2 branch</li>
  <li>Dev3 - run tests locally (Maven 'Verify'), stage, commit and push changes on the local feature3 branch</li>
  <li>Dev1,2,3 - run tests locally again (just to make sure), and then create a "pull request" (GitHub) to merge from your own branch to the main branch</li>
  <li>team leader approves the pull requests</li>
  <li>**IMPORTANT:** The final step will be to push your (**ONLY one of the team members; remember this lab requires a team submission**) code to the **"submission"** branch, which will trigger the CI workflow leading to you receiving an email about if your submission passed the required unit tests or not (build failed or passed). A fail message means one or more of the tests failed, and you must make further changes to your code before again committing and pushing to the "submission" branch. Detailed steps for how to create and manage the submission branch are listed in pts 1, 3 and 4 at https://www.cs.auckland.ac.nz/~ewan/teaching/submission-branch.html. Note that pt 2 is not valid for this exercise as the submission branch is not provided to you as the part of the repo.
</ol>

#### New Branch, Build and Test

Each dev needs to create a branch for the feature they are implementing as named above. Creating a branch from the command line is done with:
```
prompt> git branch feature1
```
In Eclipse, you can create a new branch by going to Team > switch To > New
Branch.

Once you have made the changes needed, commit them.  Make sure you are on your
own branch before making a commit.

There are three test scripts in place namely TestFeature1, TestFeature2 and TestFeature3 for testing each feature. A feature can be tested on a branch by using the goal in maven as **-Dtest=[test script] test**. For example, **-Dtest=TestFeature1 test** is for testing feature 1. Note that this is only to run the tests individually locally. The CI Workflow script only runs Maven's Verify goal once for all 9 tests. See the .github/workflows/autotest.yml. **You must not change the contents of this file.**

After committing the source code to a branch, Github classroom workflow (CI) will be executed. The figure below shows the log file (it also can be accessed from Github's Actions tab) after Dev1 has committed on feature1 branch; this shows testfeature1 has succeeded, while testfeature2 and testfeature3 failed. Similarly, the execution of feature2 branch should have testfeature2 succeeded, while testfeature1 and testfeature3 failed.  

![](src/resources/testrun-github.png)

There are a number of tutorials available on-line for how to use Git branching. A reasonable one
(although with more detail than needed for this lab) is by [Atlassian](https://www.atlassian.com/git/tutorials/using-branches)

#### Pull Request ####

The implementation of new features are separately stored on different branches. In order to combine all implementations, the branches for each feature needs to be merged into the main branch. To achieve this, each dev creates a pull requests on Github by going to Pull Request tab and click 'new pull request'. Then, select the branch to merge into main branch. Github will show the comparison of files on main branch and feature branch as the figure below.

![](src/resources/pull-request1.png)

If there is no conflict in the files, the branches can be automatically merged. However, if there is any conflict, the developer must resolve it when approving the pull request. This is done by clicking on 'Create pull request' and entering the message of this pull request for later approval.

![](src/resources/pull-request2.png)

On the approval as shown above, Github informs us that there is no conflict so we can choose to merge the pull request. However, if there is conflict as sample shown below, the conflict must be resolved before it can be merged into the main.

![](src/resources/pull-request3.png)


After that, the implementation of the feature will be added into the main branch. This process must be repeated for all three features.
  
## Build & Run project on GitHub ##

To see the results of building and running tests on Github (using the "**submission**" branch only), go to the Action tab. GitHub Action is the CI-CD (continuous integration - continuous deployment) pipeline provided by GitHub. It is similar to other CI-CD pipeline platforms, e.g. Travis CI or Jenkins. In this project, there is a workflow already defined namely Github Classroom, as shown in the figure below. Every time code is pushed to the repository, this workflow will be queued to execute automatically. When this workflow runs successfully (as shown below), the exercise is complete.

![](src/resources/test-success.png)

<h2>Assessment</h2>

The marking of this lab will be based on your team repository as of **Friday 18
March 1700hrs NZ time**. As well as the changes made to it for the above exercises, you
must include a file <tt>Team.md</tt> containing the list of members in your
team and a brief summary of what role each member played. For example:

* Ewan - Dev1 in exercise 1, Dev3 in exercise 2
* Paramvir - Dev3 in exercise 1, Dev2 in exercise 2
* Nick - Dev2 in exercise 1, Dev1 in exercise 1

If this file is not provided then there will be a **50% penalty**.

**You must enter your initial team communication via GitHub Discussions before Thursday, 10 Feb 17:00hrs NZ time**. Some examples of this evidence could be: who does what, by when you plan to finish the exercises, constraints, planning notes, etc. Additionally, feel free to use GitHub Issues to report and discuss specific issues that you face while working on the exercises. If you end up using both Discussions and Issues, clearly state this in Team.md. If you fail to add any notes to Discussions before the scheduled lab closing time, **25% penalty** will be applied to the lab marks.

**Note that your team (one of the team members to push on behalf of the team) must NOT push to the "submission" branch on remote (GitHub repo) more than 3 times**. Executing workflows incurs cost, and you must make sure you do not violate this rule while making your submissions. **Violating this rule will bring penalties too**.

Overall, the assessment will be performed by examining the **commit logs, Team.md, comments recorded in GitHub Discussions and other
information associated with your team repository**. You must demonstrate that
you have engaged with the lab material and fully participated with the
team. This means we expect to see non-trivial commits, with meaningful commit
messages, corresponding to each exercise. Different team members will do
different things and different times, but we will be looking for evidence that
there was team cooperation and collaboration. Examples including making useful
commits, communications via GitHub Discussions e.g. commenting on actions by other team members, resolving conflicts, etc.
