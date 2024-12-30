
## Git and GitHub Setup, Syncing, and Authentication Overview  

### Setup
```
git init
git config --global --list
git remote add origin https://github.com/
git remote
git remote -v
git remote remove origin
```

Troubleshooting If git push Didn't Work
```
git config --global credential.username "<github ID>"
git remote add origin https://yourusername@github.com/username/git-tutorial.git # changed 

Use a Personal Access Token
Ensure You Ran git config
```
**Online Backup 2-Way Sync**

Sync Changes from local to remote
```
git commit -am "version 1"           # Initial commit
git push orgin main --set-upstream
git push -u origin main              # Push to remote


git commit --amend -m "version 2"     # Amend commit message to "version 2"
git push origin main -force               # Force-push amended commit to remote
```

Sync Changes from remote to local
```
git clone <url> new-repo         # Clone the repository into a folder named 'new-repo'
cd new-repo                      # Change into the cloned repository directory
git fetch                        # Fetch updates from the remote repository
git pull origin main --set-upstream  # Pull changes from the 'main' branch and set 'origin/main' as the upstream branch
```

#### Difference between git fetch and git pull:

- git fetch: Downloads changes from the remote repository to your local machine but does not merge them into your working directory. It updates your remote-tracking branches.

- git pull: Downloads and merges changes from the remote repository into your current branch, essentially combining git fetch and git merge in one step.

In short:
- git fetch = Download changes without merging.
- git pull = Download and merge changes.

**"Understanding Git Push, Amend, and Remote Tracking Branches"**

- `git push` only pushes commits, it doesn't push changes
- Remote Tracking Branches don’t update automatically
- When you `amend` a commit, Git creates a branch-like situation because the commit history on the remote (main branch) remains unchanged. Afterward, you need to use `git push --force` to overwrite the remote history with the amended commit.
- When you use `git commit --amend` to overwrite a previous commit, the commit message does not have to be the same. 


### What is GitHub SSH?
GitHub SSH (Secure Shell) is a protocol that allows you to securely interact with GitHub repositories without needing to enter your username and password every time you push, pull, or clone a repository. Instead of using HTTPS, which requires you to authenticate with your username and password (or personal access token), SSH uses cryptographic keys for authentication.

### Git Credentials and Authentication

When you clone a GitHub repository using HTTPS:

    git clone https://github.com/username/repository.git

You are prompted to enter your GitHub username and password (or personal access token if you have 2FA enabled) every time you push, pull, or interact with the remote repository.

This is because HTTPS requires you to authenticate with your GitHub account using your credentials each time.

### SSH Authentication vs. HTTPS Authentication
When you set up SSH authentication, you use SSH keys (public and private) to authenticate instead of entering your username and password.

With SSH, you don’t have to enter your credentials for each Git operation (push, pull, clone, etc.) because your SSH key is used for authentication.

In other words:
- With HTTPS: 
  You need to enter your username and password (or token) every time you interact with GitHub.
- With SSH: 
  Once you set up SSH keys, GitHub recognizes your identity automatically, so you don’t need to re-enter your credentials when pushing, pulling, or cloning.

When you set up SSH for GitHub, you only need to set a password (also known as a passphrase) once during the SSH key creation process.

1. Creating the SSH Key Pair
   
   When you run the command to generate a new SSH key pair: `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
- During this process, you will be prompted to enter a passphrase (optional).
- The passphrase is used to encrypt the private key on your local machine, adding an extra layer of security. This is not the same as your GitHub account password, but a separate passphrase for your SSH key.

1. When the Passphrase is Used

- First Time: 
  You enter the passphrase when generating the SSH key.
- Subsequent Use: 
  Each time you use the private SSH key (e.g., when pushing or pulling from GitHub), you'll be asked to enter the passphrase once per session (if you’re using an SSH agent like ssh-agent to manage your keys).
  - SSH Agent: 
  If you've set up an SSH agent, it will store your decrypted private key in memory, so you don’t need to re-enter the passphrase every time you use SSH within that session.

1. Using SSH for GitHub Operations
   
   Once the SSH key is set up and the passphrase is entered (if applicable), GitHub will authenticate you using the SSH key instead of your GitHub username and password. After the first time, you won’t need to re-enter the passphrase unless the SSH agent session is reset or you restart your system.

**Summary**
- once the user has set up their SSH key and added it to GitHub, they can push freely to repositories they have access to.
- The repository owner does not need to do anything to manage the SSH keys themselves for users who are already authenticated via their GitHub accounts. The user's SSH key is used to authenticate them automatically.
- No, the repository owner does not need to add users' SSH keys to GitHub. The user should add their own SSH key to their own GitHub account under Settings > SSH and GPG keys. The owner just needs to grant the user access to the repository.

### Scenario

In a scenario where a team is working on a single project hosted in one GitHub repository, the process for managing access and using SSH keys is as follows:

- User Setup: Each team member needs to generate their own SSH key pair (if they haven't already) on their local machine. This typically involves using the ssh-keygen command.

- Adding SSH Key to GitHub: Each team member must add their public SSH key to their own GitHub account. This is done by navigating to Settings > SSH and GPG keys in their GitHub account and adding the key there.

- Repository Access: The repository owner (or an administrator) must grant access to the repository for each team member. This is done by adding the users as collaborators or by adding them to a team if the repository is part of an organization.

- Cloning the Repository: Once access is granted, team members can clone the repository using the SSH URL. This allows them to push and pull changes without needing to enter their username and password each time, as the SSH key will handle authentication.

- Collaboration: Team members can now freely push their changes to the repository, provided they have the necessary permissions. The repository owner does not need to manage individual SSH keys; they only need to manage user access to the repository.

- Best Practices: It's a good practice for team members to regularly check their SSH key configurations and ensure they are using the correct key when interacting with GitHub. Additionally, if a team member leaves the project, the repository owner should revoke their access to maintain security.

In summary, in a team working on a single GitHub repository, each user manages their own SSH keys, and the repository owner manages user access to the repository. This setup allows for efficient collaboration without the need for the repository owner to handle SSH keys directly.