# Codex Tutorial
 
Codex is an AI coding assistant from OpenAI. In the terminal you describe what you want in plain English, and Codex proposes the commands or file edits, then asks for your permission before it runs them. This chapter shows how to install and configure Codex, then use it for the **version control** and **pull request** workflows.
 
> **Note:** Codex works with a free ChatGPT account, but free usage is limited. Messages are counted in a five-hour window, and weekly limits may also apply. If you see a usage-limit message, wait for the window to reset and continue later.
 
## Installation

Official Repository: [OpenAI Codex on GitHub](https://github.com/openai/codex)
 
**Before you start**, check that Node.js and git are installed:
 
 Type the following commands into PowerShell which can be accessed via the Windows button on a PC.

![Click Windows](images/1.%20Click%20Windows.png)
![Type PowerShell](images/2.%20Type%20PowerShell.png)

```
git --version
node --version
npm --version
```

![Type Git](images/4.%20Type%20git--version.png)
 
If any of these print "command not found," install git from <https://git-scm.com> and the LTS version of Node.js from <https://nodejs.org>, then open a new terminal and recheck the downloads again using the same commands.
 
![What it looks like if Git is not downloaded](images/5.%20Press%20enter%20git%20not%20downloaded.png)

**Install Codex** using one of the following:
 
- Any computer with Node.js:
```
  npm install -g @openai/codex
```
 
- Mac with Homebrew:
```
  brew install --cask codex
```
 
**Check that it worked:**
 
Type the following command into PowerShell to see if the install was successful.

```
codex --version
```
 
If a version number prints, Codex is installed.
 
**Troubleshooting**
 
- `codex: command not found`: close the terminal, open a new one, and try again.

## Configuration
 
Two separate logins are needed:
 
| Login | What it is for | How to set it up |
|---|---|---|
| ChatGPT login | Lets Codex (the AI) work for you | Sign in when Codex first starts |
| GitHub login | Lets your computer push to GitHub and open pull requests | `gh auth login` |
 
The Codex command-line tool does not have its own "connect GitHub" button. When you ask Codex to commit, push, or open a pull request, it runs the same git and GitHub commands you could run yourself, using the GitHub login on your computer. So linking Codex to GitHub means setting up git and the GitHub CLI as shown below.
 
### Fork and clone the repository
 
1. On GitHub, open the course repository and click **Fork**, then **Create fork**.

![Fork Created](images/32.%20Fork%20created.png)

2. On your fork, click the green **Code** button and copy the HTTPS link.
3. In the terminal, go to the folder where you keep projects, then clone your fork:
```
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
```
 
   ![Clone your Repo](images/35.%20Clone%20your%20repo.png)
 
4. Add the original repository as `upstream` so you can pull in new changes later:
```
   git remote add upstream https://github.com/<instructor-username>/<repo-name>.git
   git remote -v
```
 
### Sign in to Codex with a ChatGPT account
 
1. In the terminal, make sure you are inside the repo folder.
2. Type `codex` and press Enter.
3. Choose **Sign in with ChatGPT**.
4. A browser window opens. Log in with your ChatGPT account (a free account works) and approve access.
   ![Sign in with ChatGPT](images/42.%20Web%20browser%20sign-in.png)
5. Return to the terminal. When a message box appears, Codex is ready.
6. To leave Codex, press Ctrl + C (twice if needed).
### Tell git who you are
 
Every commit is stamped with a name and email. Check whether they are set to your personal information:
 
```
git config --global user.name
git config --global user.email
```
 
If either prints nothing, set them (use the email tied to your GitHub account):
 
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
 
### Log your computer in to GitHub
 
This step links your computer, and therefore Codex, to your GitHub account. It uses the GitHub CLI (`gh`).
 
1. Install the GitHub CLI:
   - Mac with Homebrew: `brew install gh`
   - Windows: `winget install --id GitHub.cli`
   - Other: download from <https://cli.github.com>
2. Close and reopen the terminal, then run `gh --version` to confirm.
3. Run:
```
   gh auth login
```
 
4. Answer the questions:
   - *Where do you use GitHub?* **GitHub.com**
   - *What is your preferred protocol for Git operations?* **HTTPS**
   - *Authenticate Git with your GitHub credentials?* **Yes**
   - *How would you like to authenticate GitHub CLI?* **Login with a web browser**
5. Copy the one-time code shown in the terminal, press Enter, and paste the code into the GitHub page that opens in your browser. Click **Continue**, then **Authorize**.
   ![One-Time Code](images/newonetimecode.png)
6. Verify the login:
```
   gh auth status
```
 
   ![Device Activation Confirmed](images/52.%20Device%20connection%20confirmed.png)
 
### Where Codex keeps its settings
 
Codex stores its settings in `~/.codex/config.toml`. No changes are needed for this walkthrough. Never copy anything from the `~/.codex` folder into a repository, because it can contain login information.
 
## Usage
 
The workflows below use a throwaway practice branch so nothing affects the real project. Simple steps are typed directly with git, and Codex is used for the steps that show what it can do.
 
### Version Control Workflow
 
1. **Create a branch (typed directly).**
```
   git checkout -b codex-practice
```
 
   Git replies "Switched to a new branch."
 
   ![New Branch](images/newbranch.png)
 
2. **Start Codex.**
```
   codex
```
 
3. **Ask Codex to create a file.** Type this prompt:
   > Create a file called codex-test.md containing a short paragraph explaining what a git branch is.
   Codex shows what it wants to do and asks for permission. Nothing is changed until you approve, which is an important safety feature. Review the request, then approve it.
4. **Look at what changed.** Type this prompt:
   > Run git status and show me the diff.
   `git status` lists the files that changed. A diff shows exactly which lines were added or removed.
5. **Ask Codex to commit.** Type this prompt:
   > Stage codex-test.md and commit it with the message "Add codex test file".
   Staging marks a file to be included in the next snapshot, and committing takes the snapshot. Approve the commands when Codex asks.

6. **Check the history (typed directly).** Press Ctrl + C to leave Codex, then run:
```
   git log --oneline
```
 
   The new commit message appears at the top. Press `q` if the list stays open.
 
   
 
7. **Push the branch to GitHub (typed directly).**
```
   git push -u origin codex-practice
```
 
8. **Confirm on GitHub.** Open your fork, use the branch dropdown to switch to `codex-practice`, and check that `codex-test.md` is listed.
   ![codex-test Dropdown](images/64.%20Codex%20test%20file%20md%20in%20dropdown.png)
### Pull Request Workflow
 
This practice pull request is opened inside your own fork, so it does not add clutter to the original repository.
 
1. **Ask Codex to open the pull request.** Start Codex again with `codex` and type this prompt:
   > Use the GitHub CLI to open a pull request from the current branch into main on my fork. Write a clear title and a short description.
   Approve the commands. Codex runs `gh pr create`. If it asks which repository to use, choose your own fork, not the original.
   If Codex has trouble, leave Codex and run the command yourself:
```
   gh pr create --repo <your-username>/<repo-name> --base main --head codex-practice --title "Add Codex test file" --body "Practice pull request."
```
 
   ![Creating Pull Request](images/66.%20PR%20prompt.png)
 
2. **View the pull request on GitHub.** Open the link that was printed, or go to the **Pull requests** tab on your fork. Check the title, description, and the **Files changed** tab.
   ![Looking at PR on Browser](images/68.%20Looking%20at%20PR%20on%20browser.png)
3. **Clean up.**
```
   git checkout main
   git branch -D codex-practice
   git push origin --delete codex-practice
```
 
   If the default branch is named `master`, use that name instead of `main`.
 
## Tips and troubleshooting
 
- **Plan your prompts.** On a free account, write prompts in advance and keep them short and specific to save usage.
- **Review before approving.** Codex asks before it runs commands or edits files. Read each request before approving.
- **Authentication errors on push.** Run `gh auth login` again, then `gh auth status`.
- **`fatal: not a git repository`.** You are not inside the repo folder. Use `cd <repo-name>` first.
- **Pull request aimed at the wrong repository.** On the pull request page, change the base repository, or close it and create a new one.
 