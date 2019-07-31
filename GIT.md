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

- Use rebasing when pulling (`git pull --rebase`)

- Use clear and exhaustive commit messages, with the story code as
  title

  ```
  [ZUPIT-01]

  This is the first commit of Zupit!
  ```

- Use local commits as *checkpoints* during your workflow

- Push on origin private branches as backup if working on a long
  story.

### Dont's

- Don't use git branches for environment configuration (e.g. branch
  `env-prod` and `env-staging` with different configuration files)

- Avoid merges when possible

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

### Create a new branch

  ```
  git checkout -b <branch>
  ```

### How to merge a local branch on master

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

4. **Fast-forward master**

   ```
   git checkout master
   git merge <branch>
   ```

5. **Push master**

   ```
   git push origin master
   ```


### Create release branches and tags

   ```
   git checkout master
   git pull --rebase
   git checkout -b 'Sprint-19.12'
   git tag -a "Sprint-19.12" -m "Release after sprint 19.12"
   git push origin Sprint-19.12
   git push --all
   ```
   
### Release features and bugfixes

1. Fix the bug or implement the feature as normal, put it on `master`.

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
