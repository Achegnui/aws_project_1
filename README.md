# Bitbucket to GitHub Repository Synchronization

![WhatsApp Image 2025-04-16 at 04 35 29_b29e7417](https://github.com/user-attachments/assets/c987d035-ae0c-4a5b-b7fd-186e591556af)

### OBJECTIVE: Migrating your existing bitbucket repo to GitHub repo, synchronizing them in such a way that when ever a change is made in the source repository (Bitbucket) the same change will be replicated in GitHub without any manual intervention.

## Setting up Bitbucket and GitHub

- On your Bitbucket, create a new repository.
  Navigate to your Bitbucket repository and Create an access token under Repository settings > Security > Access tokens.
  Create Repository Access Token with selecting all the "READ" Permissions.  
   Make sure to save it somewhere because you can view it just once.
  Copy the last Access token

  ![image](/images/image%201.png)

  Navigate To Github and Import the Repository while keeping the same name

- On Bitbucket, Enable Pipelines under Repository settings > Pipelines > Settings

![image](/images/image%202.png)

- On Bitbucket, Generate keys under Repository settings > Pipelines > SSH keys. Copy the public key to clipboard
- On the same page, under Known hosts enter github.com as the Host address and then click Fetch followed by Add host
- On GitHub, create a github repository and add the public key under repository settings > Security > Deploy keys > Add deploy key. Tick the checkbox to Allow write access

  ![Screen Shot 2024-01-04 at 00 35 39](</images/image%203%20(public%20keys).png>>)

- On Bitbucket, add the public key under Repository settings > Security > Access keys > Add key

  ![image](</images/image%204%20(deploy%20keys).png>)

- On Bitbucket, Create an access tokens under Repository settings > Security > Access tokens. Create Repository Access Token with selecting all the "READ"
  Permissions and tick the 'Read and write checkbox under Webhooks

  ![image](</images/image%205%20(added%20public%20key%20to%20bitbucket%20repo).png>)

- Copy the first Token

![image](<images/image%206%20(githubsync%20access%20token).png>)

- On Bitbucket, Create a Repository variable under Repository Settings > Pipeline > Repository variable. You can name it " BITBUCKET_VARIABLE"
- The value will be the access token you just created in the previous step

  ![image](</images/image%207%20(bitbucket%20variable%20for%20pipeline).png>)

- On Github, At the top right click on your Profile, Scroll down at the bottom click on settings

  ![Screen Shot 2024-01-04 at 00 45 35](</images/image%208(move%20to%20github%20settings).png>)

- At the bottom left of the page click Developer Settings, Create a Personal access tokens Under Personal access tokens > Token (classic) > Generate new token >
  Generate new token (classic)

  ![image](/images/generating%20classic%20token%20image%209.png)

- Check the "repo" box, "Workflow" and the "write:package" box.

- Scroll down and click Genarate Token and copy the token

- On Bitbucket, Create a Repository variables with the Personal Access Token from Github as the value. Repository Settings > Pipeline > Repository variable. You
  can name it "GITHUB_VARIABLE".

  ![image](/images/variables.png)

## Creating a bitbucket-pipelines.yml file

![image](/images/image%2011%20creating%20yml%20file.png)

Paste the following:

```
  pipelines:
    default:
      - step:
          name: Sync GitHub Mirror
          image: alpine/git:latest
          clone:
            enabled: false
          script:
            - git clone --mirror https://x-token-auth:"$BITBUCKET_VARIABLES"@bitbucket.org/juvenachegnui/migration_repo_2.git ## @bitbucket.org follow by your Bitbucket repository path
            - cd migration_repo_2.git ## cd followed by your Bitbucket repository Name
            - git push --mirror https://x-token-auth:"$GITHUB_VARIABLES"@github.com/Achegnui/aws_project_1 ## @github.com followed by your Github repository path
```

On your code replace the "$BITBUCKET_VARIABLE" and "$GITHUB_VARIABLE" with your corresponding variable names while keeping the $ and the "" sign.

Bitbucket repository path: ![image](/images/bitbucket%20file%20path.png)

GitHub repository path: ![image](/images/githun%20repo%20file%20path.png)

GitHub repository name: ![image](/images/repo%20name.png)

![image](/images/yml%20code.png)

## Running the pipeline in Bitbucket

![image](/images/completion.png)

## Now check your GitHub repository and see that the bitbucket-pipelines.yml was automatically added!

![image](https://github.com/user-attachments/assets/8a498d1b-01c8-4c7e-8594-db7937ccdfd1)
