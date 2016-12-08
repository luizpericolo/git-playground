# Git-Playground

This is a playground repository with the purpose of learning and testing git commands!

# Git-Playground

This is a playground repository with the purpose of learning and testing git commands!

##### Scenario 01 - Removing pushed HEAD commit into own branch
###### Problem:

Luiz accidentally commited a new feature into master and pushed it upstream. However, the requirement is still being discussed between teams and it might have to be implemented by another team in another project. Now Luiz has to remove this feature, that thankfully* was commited in a single commit, and save it in its **own branch** (**feature/requirement-x** in this scenario) so that any other new features and fixes aren't blocked going to **master** before this discussion ends.

Checking our scenario:

    $ git branch
    * master
    $ git log
    commit 060ca9ec404e1ab22c84f1b86486daa4f40bb4af
    Author: Luiz <luiz@luiz.com>
    Date: Wed Dec 7 14:00:00 2016 -0200  

        add awesome requirement-x

    commit 060ca9ec404e1ab22c84f1b86486daa4f40bb4af
    Author: Luiz <luiz@luiz.com>
    Data: Tue Dec 6 10:00:00 2016 -0200

        suggest that coconuts migrate

    ...

\* thankfully because we only have to revert one commit. And even more thankfully because there are no commits after it in master.

So, how should Luiz go about doing this?  
If Luiz opens a new branch called **feature/requirement-x** from master's **HEAD** it is guaranteed that **feature/requirement-x** will have the implementation that was wrongfully pushed to upstream's **master** branch.  

So Luiz can start off with a simple:

    $ git checkout -b feature/requirement-x
    Switched to a new branch 'feature/requirement-x'

After creating the feature branch, we can check that it does indeed have the commit adding the requirement we want to save (and all of master's history aswell):

    $ git log
     commit 060ca9ec404e1ab22c84f1b86486daa4f40bb4af
        Author: Luiz <luiz@luiz.com>
        Date: Wed Dec 7 14:00:00 2016 -0200  

            add awesome requirement-x

        commit 060ca9ec404e1ab22c84f1b86486daa4f40bb4af
        Author: Luiz <luiz@luiz.com>
        Data: Tue Dec 6 10:00:00 2016 -0200

            suggest that coconuts migrate

        ...

Luiz can now push this branch to the remote repository in order to save it:

    $ git push origin feature/requirement-x
    Total 0 (delta 0), reused 0 (delta 0)
    remote:
    remote: Create pull request for feature/requirement-x:
    remote:   http://github.com/project/compare/commits?sourceBranch=refs/heads/feature/requirement-x
    remote:
    To ssh://github.com/project.git
     * [new branch]      feature/requirement-x -> feature/requirement-x

If we list all branches we get:

    $ git branch -a
    * feature/requirement-x
    master
    remotes/origin/HEAD -> origin/master
    remotes/origin/feature/requirement-x
    remotes/origin/master

Now we have to switch back to **master** and revert the commit that adds the requirement x, which has a hashcode of **060ca9ec404e1ab22c84f1b86486daa4f40bb4af**:

    $ git checkout master
    Switched to branch 'master'
    Your branch is up-to-date with 'origin/master'.
    $ git revert 060ca9ec404e1ab22c84f1b86486daa4f40bb4af


Luiz has successfully reverted commit **060ca9ec404e1ab22c84f1b86486daa4f40bb4af** in **master**. Here's proof:

    $ git log
    commit cc566ae7dabad572b97ee59043ef6edbef07007d
    Author: Luiz <luiz@luiz.com>
    Date:   Thu Dec 8 15:31:25 2016 -0200

        Revert "add awesome requirement-x"

        This reverts commit 060ca9ec404e1ab22c84f1b86486daa4f40bb4af.

The only thing left to do is push **master** to origin and we'll be done.

    $ git push origin master
