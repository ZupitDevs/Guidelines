# Git - Version Control

The goal of this document is to describe a way of using GIT that
should:

- Improve the GIT history log
- Avoid *spaghetti branching*
- Avoid merge commits
- Have less conflicts
- Simplify the release of bugfixes

## Netiquette

### Do's

- Use rebasing when pulling
  * Command line: `git pull --rebase`
            
  * GitKraken: Select the "Pull" menu and then select "Pull (rebase)"

- Use clear and exhaustive commit messages, with the story code as
  title

  ```
  ZUPIT-01 Add feature XYZ
  ```

  Use the following commit message syntax: https://chris.beams.io/posts/git-commit/

- Use local commits as *checkpoints* during your workflow

- Push on origin private branches as backup at the end of the day.

### Dont's

- Don't use git branches for environment configuration (e.g. branch
  `env-prod` and `env-staging` with different configuration files)

- Avoid merges when possible (allowed in long living branches)

- Don't break the **golden rule**

#### The Golden Rule

The golden rule of git rebase is to never use it on public
branches. For example, think about what would happen if you rebased
master onto your feature branch: The rebase moves all of the commits
in master onto the tip of feature . The problem is that this only
happened in your repository.

## General workflow

- The `master` branch should be the common development branch.

- Work on a local branch, named as the story you are in charge
  of. (e.g. `ZU-123`), then rebase it on top of `master`

- Commit your intermediate changes on the branch. When the story is
  done, and tests pass, move the changes in master and delete your
  local branch

- At the end of a sprint (or more generally, when releasing), create a
  branch starting from `master` with a valid name (e.g. `sprint-1904`
  or `release-20190403`)

### Create a new branch - Command Line

  ```
  git checkout -b <branch>
  ```
### Create a new branch - Git Kraken
  ```
   Click Branch button
  ```

------------


### How to merge a local branch on master - Command Line

1. **Make sure your master branch is up-to-date**

    ```
    git checkout master
    git pull --rebase
    ```

2. **Checkout your local branch**

    ```
    git checkout <branch>
    ```

3. **Squash and rebase.**

    Here you `pick` the first commit and `squash` all the others. You
    can do this interactively:

    ```
    git rebase -i master
    ```

    Remember to change your new squashed commit message with a
    meaningful text.
    - First row: StoryId + Mini description
    - From the second row: complete description  

4. **Fast-forward master**

    ```
    git checkout master
    git merge <branch>
    ```

5. **Push master**

    ```
    git push origin master
    ```

### How to merge a local branch on master - Git Kraken

1. **Make sure your master branch is up-to-date**
    ```
    Double click master
    Click Pull button
    ```

2. **Checkout your local branch**
    ```
    Double click <your_branch>
    ```

3. **Squash and rebase.**

    Here you `pick` the first commit and `squash` all the others. You
    can do this interactively:
    ```
    Select all your local commits
    Right click --> "Squash <x> commits"
    Right click your branch --> Rebase <your_branch> onto master
    ```
    Remember to change your new squashed commit message with a
    meaningful text.
    - Summary: StoryId + Mini description
    - Description: complete description

4. **Fast-forward master**
    ```
    Double click master
    Right click master --> Fast-forward master to <your_branch>
    ```

5. **Push master**
    ```
    Click Push button
    ```

------------


### Create release branches and tags - Command Line

   ```
   git checkout master
   git pull --rebase
   git checkout -b 'Sprint-19.12'
   git tag -a "RelSprint-19.12" -m "Release after sprint 19.12"
   git push origin Sprint-19.12
   git push --all
   ```
   

### Create release branches and tags - Git Kraken
   ```
   Double click master
   Click Pull button
   Click Branch button --> 'Sprint-19.12'
   Right click <new_branch> --> Create annotated tag here
       Name --> 'RelSprint-19.12'
       Annotation message --> 'Release after sprint 19.12'
   Click Push button
   ```

------------


### Release features and bugfixes - Command Line

1. Fix the bug or implement the feature as normal and put it on `master`.

2. Checkout the current release/sprint branch

   ```
   git checkout Sprint-19.12
   ```

3. Cherry pick the commit in to the branch (use ```git log master```
   to get the commit hash)

   ```
   git cherry-pick <commit-hash>
   ```

4. Push & deploy!

   ```
   git push origin Sprint-19.12
   ```

### Release features and bugfixes - Git Kraken

1. Fix the bug or implement the feature as normal and put it on `master`.

2. Checkout the current release/sprint branch

   ```
   Double click <sprint_branch>
   ```

3. Cherry pick the commit in to the branch

   ```
   Right click the commit you wanna cherrypick
   Click "Cherrypick commit"
   "Do you want to immediately commit the cherrypicked changes?" --> Yes
   ```

4. Push & deploy!

   ```
   Click Push button
   ```

## Exceptions

### Long Living Branches

When you are working on a really long feature, maybe with other developers, the general workflow method is not applicable.
Rebasing on a shared branch is prohibited and you do not want to lose every intermediate commit.
The solution is using long living branches.

1. Create a branch named "LLB-|feature-name|"

2. When you pull the branch from remote you still have to use the rebase mode.

3. When you want to get the latest commits from master, use merge instead of rebase.

4. When the really long feature is ready and stable merge it into master (Not rebase!). 
