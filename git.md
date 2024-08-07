# Git rules

## 1. Repository

## 2. Branch

- Using 2 branches to record the history of the project.
  - The `main` branch stores the official release history.
  - The `develop` branch serves as an integration branch for features.
- Tag all commits in the `main` branch with a version number.

### 2.1. Naming

- **SHORT** and Descriptive names.
- Branch prefix:
  - 1. Feature branches: `feature/`
  - 2. Release branches: `release/`
  - 3. Hotfix branches: `hotfix/`
  - 4. Support branches: `support/`
  - 5. Fix bug branches: `bug/`

- Several people work on the same feature: `feature/main`
  - Personal branch: `feature/<name>`

- Characters:
  - All lowercase.
  - To separate words using `-` instead of `_`.
  - No special characters.

## 3. Commit

- Each commit should be a single *logical change*.
  - For example: a patch fixes a bug and optimize the performance, it should be split into 2 commits.
  - Don't split a single logical change to several commits. Avoid **Work-In-Progress** commit.
- To sync on top of other brach:

    ```bash
    git fetch
    git rebase origin/main
    ```

### 3.1. Commit messages

- Refer using editor, with message template:

    ```bash
    # good
    $ git commit

    # bad
    $ git commit -m "Quick fix"
    ```

- Using git message template:
  - Create template file: `vim ~/.git_message_template`

    ```text
    # First line is Title: Summary, imperative, start upper case.

    # Remember blank line between title and body.

    # Body: Explain *what* and *why* not *how*. Include task ID (Jira issue).

    # More empty line before the end.
    # Co-authored-by: name <user@users.noreply.github.com>
    ```

  - Enable template: `git config --global commit.template ~/.git_message_template`

- If commit A solves a bug introduced by commit B, it should also be stated in the message.

### 3.2. Squash commits

- Squash a commit with another commit: `git commit --squash f387cab2`

## 4. Merging

- Each new feature should reside in its own branch, and using `develop` as their parent branch.
- When the feature is completed, it gets merged back into develop.
  - 1. To avoid conflict: Merge `develop` to current branch: `git merge develop`
  - 1. Create a Merge Request.
  - 2. Select Squash to merge commits become only one in the `develop` if required.

- If your branch should be ahead. Rebase it onto the branch it's going to be merged to:

    ```bash
    git fetch
    git rebase origin/main
    # then merge
    ```

  - This results in a branch that can be applied directly to the end of the "main" branch and results in a very simple history.
  - NOTE: `Rebase` is better suited for projects with **short-running** branches. Otherwise it should be `merge` to another branch instead of rebasing onto it.

## 5. PR styles

```text
# Description

Please include a summary of the changes and the related issue. Please also include relevant motivation and context. List any dependencies that are required for this change.

Fixes # (issue)

## Type of change

Please delete options that are not relevant.

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] This change requires a documentation update

# How Has This Been Tested?

Please describe the tests that you ran to verify your changes. Provide instructions so we can reproduce. Please also list any relevant details for your test configuration

- [ ] Test A
- [ ] Test B

**Test Configuration**:
* Firmware version:
* Hardware:
* Toolchain:
* SDK:

# Checklist:

- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Any dependent changes have been merged and published in downstream modules
```

- Prefer `squash merge` instead of `rebase-merge`.
