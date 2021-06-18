# Basic Principles of Git, Github and Github Desktop

[Press here to return to main page.](https://github.com/iron-claw-972/Curriculum2020)

## 1. Purpose

This activity will introduce several key concepts related to version control and Github. In particular the following will be covered:
- Creating repositories
- Cloning repositories
- Pushing and pulling from repositories
- Creating and using branches
- Pull requests

## 2. Materials

- A Github account
  - In order to create a Github account go to [this link](https://github.com/join).
- Github Desktop
  - Downloading Github desktop is detailed [here](https://github.com/iron-claw-972/Curriculum2020/blob/master/InstallingFrcPrereqs.md#github-desktop).

## 3. Instructions

### 3a. Signing In To Github Desktop

1. Open Github desktop
2. In the top left corner left click on `File`

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/gituhb_desktop_file_options.png)

3. Left click on `Options...` in the dropdown menu

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_file_options_selected.png)

4. Left click on `Sign in` under GitHub.com

  - Note: Do not press the `Sign in` under GitHub Enterprise Server

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_sign_in.png)

5. Enter the email address you used when creating your GitHub account and the password associated with it. Then left click on `Sign in`.

  - Note: In the event that you are already signed in to GitHub on your browser, you may press the `Sign in using your browser` link. This will open a webpage and will create a pop-up, to which you should select `Open GitHubDesktop.exe`. The webpage will close and you will be signed in.
  - Note: In the event that you forget your password, you can use the `Forgot Password?` dialog to recover your account.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_sign_in_menu.png)

At this point you should be signed in to GitHub Desktop.

### 3b. Creating a repository

1. Left click on the `Current Repository` area in the top left corner. This opens a dropdown, which shows a list of the repositories that GitHub desktop knows about.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_current_repository.png)

2. Left click on the `Add` dropdown to open the adding dropdown.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_add_dropdown.png)

3. Left click on the `Create new repository...` option. This opens a menu in the center of the window.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_create_new_repo_menu.png)

4. The opened menu has several fields:

- `Name` - The name of the repository.
- `Description` - A short description of the contents of the repository.
- `Local path` - The location on your computer where the repository will be stored.

The other options are out of the scope of this basic tutorial, they should not be modified without understanding what they do.

5. Below is an example of what a filled out menu looks like.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_create_new_repo_menu_filled.png)

6. Fill in the `Name` field with a name for your repository. It should be short and descriptive of the contents. For example: `MyFirstRepo`. Then, without changing any other options, left click on `Create repository`.

7. Left click on the `Current Repository` area to close the dropdown.

8. In order to publish your repository to GitHub, press the `Publish repository` button at the top of the window.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_publish_repository.png)

9. A menu will open in the center of the window. The first two fields are the same as described in step 4, do not change them. The field `Keep this code private` is the most important of the fields in this menu. If it is checked, your code will only be visible to you (with configuration this can be circumvented however, this is out of the scope of this tutorial). If it is not checked, anyone can see your code. The `Organization` field allows for the repository to be put under an GitHub organization, such as `IronClaw972`.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_publish_repo_menu.png)

10. Uncheck the `Keep this code private` field and left click on `Public repository`. After a few seconds, your repository will be visible on GitHub. Go to [github.com](https://github.com). If you are not already logged in to GitHub in your browser, log in, then navigate to [github.com](https://github.com). On the left of the webpage you will see a list of repositories. The repository you have just created should appear in this list. Left clicking on it will link you to the repository page. There you can see the contents of the repository.

### 3c. Committing and pushing to a repository

1. Follow the instructions in 3b to create a repository to commit to.

2. Open the `Current Repository` dropdown as detailed in section 3b, step 1.

3. Right click on the repository you created. In the image below, this repository is `ExampleRepo`.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_right_click_repo.png)

4. Left click on `Show in Explorer`. This will open a File Explorer tab at the repository on your computer.

5. Right click in the space under the files visible in the file explorer.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_create_txt_menu.png)

6. Hover your mouse over the `New >` option.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_create_txt_menu_new.png)

7. Left click on `Text Document`. And enter a name for the document, for example `ExampleDocument`, while not changing the `.txt` extension.

8. Double left click on the new text document and type "Hello World."

9. Close the text editor you were using to write into the document, by default this editor is Notepad.

10. Navigate back to GitHub Desktop, it should now look approximately like this:

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_new_change_to_commit.png)

11. In the bottom left corner, name your commit. In the case below, the commit is called `add text file`. Ideally commit names are short and descriptive.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_commit_corner.png)

12. Left click on `Commit to master`.

13. At the top of the GitHub desktop window, the option `Fetch origin` has changed to `Push origin`. Left click on it to push your code to GitHub. If you navigate to your repository on [github.com](https://github.com), as detailed in section 3b, step 10, you should now see your text file in the list of items.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_push_origin.png)

### 3d. Cloning a repository

1. Navigate to any GitHub repository. This tutorial will use this repository [Curriculum2020](https://github.com/iron-claw-972/Curriculum2020).

2. Near the top of the page, left click on `Clone` to open the dropdown. Then left click on `Open in Desktop`. Finally, select `Open GitHubDesktop.exe` on the dropdown.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_clone_repository_website.png)

3. At this point, GitHub desktop will appear with a menu in the center of its window. As seen below. Select `Clone` to download the repository.

### 3e. Fetching and Pulling a repository

1. It is important to keep the local version of your repository up to date. This is very simple to do with GitHub desktop. First, left click the `Fetch origin` button near the top of the GitHub desktop window to check for changes to the code on GitHub.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_fetch_origin.png)

2. If there are changes, the `Fetch origin` will change to `Pull origin`. Left click the button again to download all of the latest changes to the repository.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_pull_origin.png)

### 3f. Creating a branch

1. Navigate to the repository you created in 3b and committed to in 3c.

2. Left click on the `Current branch` area to open the branches dropdown.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_branches_dropdown.png)

3. Left click on the `New branch` button to open the `create a branch` menu.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_create_branch_menu.png)

4. Enter a name for your branch, for example `edited-text-file`.

5. Left click on the `publish branch` button.

6. Edit the text file you created in section 3c, following steps 2, 3, 4, 7, and 8 if necessary to locate and edit the file. Change the text in the document.

7. Commit the new changes to GitHub, following steps 10, 11, 12, and 13 in section 3c if necessary.

8. Using the branches dropdown, opened in 3f step 2, you can change which branch you are on. Navigating to your repo on GitHub, you can also view the different branches.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_github_branches_dropdown.png)

### 3g. Creating a pull request

When writing code with a team, often times pull requests are used. First a branch is created with some code changes. Ten, a pull request is created. This pull request allows for the code changes to be reviewed before being added to the main code base. This process of review greatly reduces the chance of poorly written or faulty code being put into the master branch.

1. Navigate to your repository on GitHub.

2. Go to the `Pull requests` tab near the top of the page. However, not the one at the top of the page.

![image](https://github.com/iron-claw-972/Curriculum2020/blob/master/images/github_desktop_pull_requests_tab.png)

3. On the right, left click on the `New pull request` button.

4. Left click on `compare: master` to open a dropdown. Then left click on the branch you want to pull into master, in the case of this tutorial it is the branch you created in 3f.

5. Left click on `Create pull request`. Write a short description in the `Leave a comment` area and left click on the new `Create pull request` button.

6. Typically, someone else will review and approve your pull request, however you can just left click on `Merge pull request` and `Confirm merge`.

7. Navigating back to your repository, you should see that the changes you made in the branch are now a part of the master branch.

## 4. Further reading

[The GitHub Desktop Getting Started](https://help.github.com/en/desktop/getting-started-with-github-desktop) is a useful resource that discusses these topics in more detail, in particular [this page](https://help.github.com/en/desktop/getting-started-with-github-desktop/creating-your-first-repository-using-github-desktop).

[The GitHub Hello World Activity](https://guides.github.com/activities/hello-world/) also covers some of these topics in greater detail.
