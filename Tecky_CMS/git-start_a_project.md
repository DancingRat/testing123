# Gitlab Cheat Sheet

&nbsp;

## Procedure for starting a project on Gitlab

&nbsp;

1. **[gitlab]** Select "Create blank project" in gitlab.com
   (this instruction DO NOT initialize repository with a README)
   &nbsp;

2. **[local]** Create a folder (e.g. myProject)

    &nbsp;

    ```bash
    mkdir myProject
    ```

    &nbsp;

3. **[local]** Enter the folder

    &nbsp;

    ```bash
    cd myProject
    ```

    &nbsp;

4. **[local]** Initialize git with initial branch "main"

    &nbsp;

    ```bash
    git init --initial-branch=main
    ```

    &nbsp;

5. **[local]** Attach the folder to the git project (repository)

    &nbsp;

    ```bash
    git remote add origin git@gitlab.com:<git-account>/myproject.git
    ```

    &nbsp;

6. **[local]** [optional] Add a testing file to the repository

    &nbsp;

    ```bash
    touch testing.txt
    ```

    &nbsp;

7. **[local]** Add all current changes to 'staging area'

    &nbsp;

    ```bash
    git add .
    ```

    &nbsp;

8. **[local]** Commit all changes with commit message

    &nbsp;

    ```bash
    git commit -m 'added testing.txt for testing'
    ```

    &nbsp;

9. **[local]** Push local repository to remote repository (gitlab)

    &nbsp;

    ```bash
    git push -u origin main

    ```

    &nbsp;

## Clone an existing project

0. **[gitlab]** Project owner grant the assess right to you, if needed

    &nbsp;

1. **[gitlab]** Obtain the repository's SSH link

    &nbsp;

2. **[local]** Clone the project (you don't need to create the project folder yourself. Git will do it for you)

    &nbsp;

    ```bash
    git clone git@gitlab.com:<git-account>/myproject.git

    ```

    &nbsp;
