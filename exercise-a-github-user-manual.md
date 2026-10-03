# How to Create a GitHub Repository from Scratch and Make Your First Commit

**Audience:** First-semester computing students with basic computer literacy and no prior Git or GitHub experience
**Estimated time:** 25 to 30 minutes
**Document version:** 1.0

---

## 1. Introduction

This procedure shows you how to create a new repository (a project folder whose history is tracked) on GitHub, connect it to a folder on your computer, and upload your first commit (a saved snapshot of your work).

By the end, you will have a repository on GitHub that contains a `README.md` file.

**Key terms**

| Term | Meaning |
|---|---|
| Git | Free software on your computer that tracks changes to files. |
| GitHub | A website that stores Git repositories online. |
| Repository (repo) | A project folder whose changes are tracked by Git. |
| Commit | A saved snapshot of your files, with a short message describing it. |
| Remote | The online copy of your repository (on GitHub). |
| Push | Upload your local commits to the remote. |

---

## 2. Prerequisites

Before you begin, make sure you have **all** of the following.

1. **A computer** running Windows, macOS, or Linux, with an internet connection.
2. **A GitHub account.** Sign up for free at <https://github.com/signup> and verify your email address.
3. **Git installed.** Download it from <https://git-scm.com/downloads> and install it with the default options. To check, open a terminal and type `git --version`. If a version number appears (for example `git version 2.45.0`), Git is installed.
4. **A terminal application.**
   - Windows: *Git Bash* (installed with Git) or PowerShell
   - macOS: *Terminal*
   - Linux: any terminal
5. **A GitHub Personal Access Token (PAT).** GitHub no longer accepts your account password when you upload from the terminal. Create a token first:
   1. On GitHub, click your profile picture (top right) and choose **Settings**.
   2. Scroll to **Developer settings** (bottom of the left menu) and choose **Personal access tokens > Tokens (classic)**.
   3. Click **Generate new token (classic)**.
   4. Enter a note such as `first-repo`, set an expiration, and tick the **repo** checkbox.
   5. Click **Generate token** and **copy the token immediately**. GitHub shows it only once. Store it somewhere safe, such as a password manager.
6. **Basic terminal skills:** you should know that you type a command and press **Enter** to run it.

---

## 3. Procedure

Each step contains one action. The **Expected result** tells you what you should see before moving on. If your result differs, go to [Section 4: Troubleshooting](#4-troubleshooting).

Commands appear in `code blocks`. Type them exactly as shown. Do not type the `$` symbol if you see one.

### Part 1: Create the repository on GitHub

**Step 1.** Open a web browser, go to <https://github.com>, and sign in.
**Expected result:** You see your GitHub dashboard.

**Step 2.** Click the **+** icon in the top-right corner of the page, then click **New repository**.
**Expected result:** The page titled "Create a new repository" opens.

**Step 3.** In the **Repository name** field, type `my-first-repo`.
**Expected result:** A green message appears saying `my-first-repo is available`.

> **Screenshot 1: "Create a new repository" page**
> *Description:* A full-width browser screenshot of the "Create a new repository" form. Red rectangles with numbered labels should highlight (1) the **Repository name** field containing `my-first-repo` with the green "is available" message beneath it, (2) the **Public** radio button, (3) the three initialization checkboxes (README, .gitignore, license), all **unticked**, and (4) the green **Create repository** button at the bottom. The caption reads: *"Figure 1. Complete these fields exactly as shown, then click Create repository."*

**Step 4.** Select the **Public** option under the visibility settings.
**Expected result:** The **Public** radio button is filled in.

**Step 5.** Make sure the **Add a README file**, **Add .gitignore**, and **Choose a license** options are all left unticked.
**Expected result:** All three checkboxes are empty. (You will create the README yourself in Step 16.)

**Step 6.** Click the green **Create repository** button.
**Expected result:** A "Quick setup" page appears showing a URL that looks like `https://github.com/YOUR-USERNAME/my-first-repo.git`.

**Step 7.** Click the copy icon beside the HTTPS URL to copy it.
**Expected result:** The icon briefly changes to a green check mark, confirming the URL is on your clipboard.

### Part 2: Prepare your computer

**Step 8.** Open your terminal application.
**Expected result:** A window opens showing a command prompt waiting for input.

**Step 9.** Type `git --version` and press **Enter**.
**Expected result:** The terminal prints a version number, such as `git version 2.45.0`.

**Step 10.** Tell Git your name by typing the following command (replace `Your Name` with your real name, keeping the quotation marks) and pressing **Enter**:

```bash
git config --global user.name "Your Name"
```

**Expected result:** No message appears. A silent result means success.

**Step 11.** Tell Git your email by typing the following command (use the same email as your GitHub account) and pressing **Enter**:

```bash
git config --global user.email "you@example.com"
```

**Expected result:** No message appears.

### Part 3: Create and commit your project locally

**Step 12.** Create a new project folder by typing the following and pressing **Enter**:

```bash
mkdir my-first-repo
```

**Expected result:** No message appears, but a folder named `my-first-repo` now exists in your current location.

**Step 13.** Move into the new folder by typing the following and pressing **Enter**:

```bash
cd my-first-repo
```

**Expected result:** The prompt now ends with `my-first-repo`, showing that you are inside the folder.

**Step 14.** Turn the folder into a Git repository by typing the following and pressing **Enter**:

```bash
git init
```

**Expected result:** The terminal prints `Initialized empty Git repository in .../my-first-repo/.git/`.

**Step 15.** Name your main branch `main` by typing the following and pressing **Enter**:

```bash
git branch -M main
```

**Expected result:** No message appears.

**Step 16.** Create a README file by typing the following and pressing **Enter**:

```bash
echo "# My First Repo" > README.md
```

**Expected result:** No message appears, but a file named `README.md` now exists in the folder.

**Step 17.** Check the repository's status by typing the following and pressing **Enter**:

```bash
git status
```

**Expected result:** The terminal lists `README.md` under **Untracked files**, usually in red. This means Git sees the file but is not yet tracking it.

**Step 18.** Add the file to the staging area (the list of changes that will go into your next commit) by typing the following and pressing **Enter**:

```bash
git add README.md
```

**Expected result:** No message appears.

**Step 19.** Check the status again by typing `git status` and pressing **Enter**.
**Expected result:** `README.md` now appears under **Changes to be committed**, usually in green, with the label `new file:`.

> **Screenshot 2: Terminal after staging the file**
> *Description:* A cropped terminal screenshot showing the output of `git status` after Step 18. The text `On branch main`, `No commits yet`, and `Changes to be committed:` should be visible, with the line `new file:   README.md` in green. A callout arrow points to the green line with the label *"Green = staged and ready to commit."* The caption reads: *"Figure 2. Expected output of git status after git add."*

**Step 20.** Create your first commit by typing the following and pressing **Enter**:

```bash
git commit -m "Add README"
```

**Expected result:** The terminal prints a summary such as `[main (root-commit) a1b2c3d] Add README` followed by `1 file changed, 1 insertion(+)`.

### Part 4: Connect to GitHub and upload

**Step 21.** Connect your local repository to the GitHub repository by typing the following (paste the URL you copied in Step 7 in place of the example) and pressing **Enter**:

```bash
git remote add origin https://github.com/YOUR-USERNAME/my-first-repo.git
```

**Expected result:** No message appears.

**Step 22.** Start the upload by typing the following and pressing **Enter**:

```bash
git push -u origin main
```

**Expected result:** The terminal asks for your GitHub **username** (or a sign-in window opens; if so, follow its prompts and skip to Step 25).

**Step 23.** Type your GitHub username and press **Enter**.
**Expected result:** The terminal asks for your **password**.

**Step 24.** Paste your **Personal Access Token** (not your account password) and press **Enter**.
**Expected result:** Nothing is shown as you paste (this is normal; the characters are hidden). The terminal then prints progress lines and ends with `Branch 'main' set up to track remote branch 'main' from 'origin'.`

**Step 25.** Return to your browser and refresh the `my-first-repo` page on GitHub.
**Expected result:** The repository page now lists `README.md` and shows your commit message, "Add README".

> **Screenshot 3: Completed repository on GitHub**
> *Description:* A browser screenshot of `github.com/YOUR-USERNAME/my-first-repo` after the push. The file list should show `README.md` with the commit message "Add README" and a timestamp such as "now". Below the file list, the rendered README should display the heading **My First Repo**. Highlight the file row and the rendered heading with red boxes. The caption reads: *"Figure 3. Your first commit visible on GitHub."*

**Congratulations.** You have created a repository and pushed your first commit.

---

## 4. Troubleshooting

### Most common error: `remote: Support for password authentication was removed` / `fatal: Authentication failed`

**When it appears:** At Step 24, after you enter your credentials.

**Cause:** You typed your GitHub *account password*. GitHub no longer accepts account passwords for Git operations over HTTPS. It requires a Personal Access Token instead.

**Fix:**

1. Create a token by following Prerequisite 5.
2. Run `git push -u origin main` again.
3. When asked for a password, paste the **token**.

**Still failing?** Check that the token has the **repo** scope ticked and has not expired, and that the username you typed is spelled exactly as it appears on GitHub.

---

## 5. Summary of Commands

| Purpose | Command |
|---|---|
| Check Git is installed | `git --version` |
| Set your identity | `git config --global user.name "..."` and `git config --global user.email "..."` |
| Start tracking a folder | `git init` |
| See what changed | `git status` |
| Stage a file | `git add <file>` |
| Save a snapshot | `git commit -m "message"` |
| Link to GitHub | `git remote add origin <URL>` |
| Upload commits | `git push -u origin main` |
