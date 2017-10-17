The Golden Rule of Rebasing: never use it on public branches
------------------------------------------------------------

Do not rebase commits that exist outside your repository. If you’re collaborating with other developers via the same feature branch, that branch is public, and you’re not allowed to re-write its history.

Any changes from other developers need to be incorporated with `git merge` instead of `git rebase`. It’s usually a good idea to clean up your code with an interactive rebase before submitting your pull request.


Keeping Fork up to date
-----------------------

Configure a remote that points to the `upstream` repo in Git to sync changes made in the original repo with the fork:

``` bash
# List the current configured remote repo for the fork
git remote -v

# Specify a new remote 'upstream' repo
# that will be synced with the fork
git remote add upstream https://github.com/orig-user/orig-project.git

# Verify the new 'upstream' repo is specified for our fork
git remote -v
```

We can use `set-branches` to change the list of branches tracked by the named remote. This can be used to track a specific subset of the available remote branches after the initial setup for a remote:

- The named branches will be interpreted as if specified with the `-t` option on the `git remote add` command line.

- With `--add`, instead of replacing the list of currently tracked branches, adds to that list.

``` bash
# To update existing remote to track specific branch only
# git remote set-branches <remote-name> <branch-name>
git remote set-branches upstream master
```

Sync a fork to keep it up-to-date with the `upstream` repo. Fetch the branches and their respective commits from the `upstream`. Commits to `master` will be stored in a local branch `upstream/master`.

``` bash
git fetch upstream
# If the project has tags that have not merged to master we should do
git fetch upstream --tags
# Fetch specific branch with specific depth of commits
git fetch --depth=3 upstream master

# View all branches, including those from upstream
git branch -va
```

Check out our fork's local `master` branch. Merge the changes from `upstream/master` into our local `master` branch. This brings our fork's `master` branch into sync with the `upstream` repository, without losing our local changes:

``` bash
git checkout master
git merge upstream/master
```

> NOTE: If our local `master` branch didn't have any unique commits, Git will instead perform a "fast-forward". However, if we have been making changes on `master`, we may have to deal with conflicts.


Submitting a Pull Request
-------------------------

### Create a Branch

Whenever we begin work on a new feature or bugfix, it's important that we create a new branch. It keeps our changes organized and separated from the `master` branch so that we can easily submit and manage multiple pull requests for every task we complete:

``` bash
# Checkout the master branch - we want our new branch to come from master
git checkout master

# Create and switch to our new branch
git branch new_feature
git checkout new_feature
```

### Cleaning up

Prior to submitting PR we need to clean up our branch and make it as simple as possible for the original repo's maintainer to test, accept and merge our work.

If any commits have been made to the `upstream/master` branch, we should rebase our `new_feature` branch so that merging it will be a simple fast-forward that won't require any conflict resolution work:

``` bash
# Fetch upstream master and merge with our repo's master branch
git fetch upstream master
git checkout master
git merge upstream/master

# If there were any new commits, rebase our feature branch
git checkout new_feature
git rebase master
```

It may be desirable to squash some of our commits into a single commit. Instead of seeing all of a contributor's individual commits from a topic branch, the commits are combined into one commit. Pull requests with squashed commits are merged using the fast-forward option.

We can do this with an interactive rebase. This will open up a text editor where you can specify which commits to squash:

``` bash
# Rebase all commits on feature branch
git checkout
git rebase -i master
```

To publish our local work to our remote fork:
``` bash
git push origin new_feature
```

Once we've committed and pushed all changes to GitHub, select `new_feature` branch of our fork and create new pull request. If we need to make any adjustments to our pull request, just push the updates to GitHub. Our pull request will automatically track the changes on our `new_feature` branch and update.
