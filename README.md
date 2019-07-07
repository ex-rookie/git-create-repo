
Prerequisites:

 - Git username should be setup:
 ```
 git config --global user.name "YOUR-GIT-USERNAME"
 ```
 - Git repository actions(push, pull, commit) using either SSH key or HTTPS must be setup to be non-interactive.
 - To avoid any email privacy exceptions, please use the "noreply.github.com" email id. Found here:
 https://github.com/settings/emails
 


Instructions:

1. Use this procedure to create the GITHUB_API_TOKEN:

https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line

2. Append the below lines into the environment file ( for e.g. ~/.bashrc):

Use the GITHUB_API_TOKEN from step 1

```
GIT_USER_NAME=`git config --global user.name`
GITHUB_API_TOKEN=XXXXXXXXX

git-create-repo() {
  mkdir $1; cd $1; echo "$1" >> README.md
  curl -X POST https://api.github.com/user/repos -u $GIT_USER_NAME:$GITHUB_API_TOKEN -d '{"name":"'$1'"}'
  git init
  git add .
  git commit -m "Initial Commit"
  git remote add origin git@github.com:$GIT_USER_NAME/$1.git
  git push -u origin master
}

```

3. Either reopen a shell terminal or source the environment file:
```
$ . ~/.bashrc
```
4. Usage: The below command creates a repository named NEW_REPO on GitHub, and a local project directory with the same name:
```
$ git-create-repo NEW_REPO
```

Further development:

- Can be improved by adding Repo description and more details in the README.md file using the respective switches from GIT API.
- Once the API token is known, Steps 2 and 3 can be incorporated in a single shell script with enough error handling for resilience, in order to make it a single package solution.
