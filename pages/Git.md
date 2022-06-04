tags:: version control system

- Git ways to store credentials
  heading:: true
	- The best way I found to deal with github + git authentication is to use SSH: 
	  https://docs.github.com/en/authentication/connecting-to-github-with-ssh 
	  https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
	  ```bash
	  ssh-keygen -t ed25519 -C "your_email@example.com"
	  # start the ssh-agent in the background. And probably it is a good idea to add this line to ~/.bashrc
	  eval "$(ssh-agent -s)"
	  ssh-add ~/.ssh/id_ed25519
	  
	  # Then you need to add the key to your github account. See https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account 
	  # For this, in this case I used gh (github client)
	  
	  # Then I logged in gh changing my preferred protocol to SSH
	  gh auth login
	  ? What account do you want to log into? GitHub.com
	  ? You're already logged into github.com. Do you want to re-authenticate? Yes
	  ? What is your preferred protocol for Git operations? SSH
	  ? Upload your SSH public key to your GitHub account? Skip
	  ? How would you like to authenticate GitHub CLI? Paste an authentication token
	  Tip: you can generate a Personal Access Token here https://github.com/settings/tokens
	  The minimum required scopes are 'repo', 'read:org'.
	  ? Paste your authentication token: ****************************************
	  - gh config set -h github.com git_protocol ssh
	  ✓ Configured git protocol
	  ✓ Logged in as jfftonsic
	  
	  # Then I just cloned one of my repositories
	  gh repo clone jfftonsic/general
	  
	  # Then I was able to push things without worrying about or meddling with git authentication issues.
	  
	  However... at a later moment it did ask the ssh key’s passphrase again.
	  
	  ```
	- An unsafe form to do that
		- Say you have already cloned a repo and want to store its password locally and are on its root dir
		  ```bash
		  git config credential.helper 'store --file ~/.git_repo_credentials' # this will save it to this specific file
		  git config credential.https://github.com.username jftonsic # don’t replace ‘username’ on this command
		  ```
-