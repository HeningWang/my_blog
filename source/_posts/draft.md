---
title: How to build a personal blog with hexo
date: 2022-08-11 16:13:06
tags: [technical, blogs]
---
# Install Hexo
Prerequisites for installing hexo are: Git und Nodejs
To install them on Windows: [Git](https://git-scm.com/download/win), [Node.js](https://nodejs.org/en/)
using Windows OS:

```
npm install hexo
```

TODO: Add MacOS

# Initialise the blog and some relevant init configurations
```
hexo init
```
will automatically set up the folder into a hexo blog project. **Important!** The folder must be a empty folder.

## Add git 
```
git init
```
Create an empty Git repository or reinitialize an existing one
Then open Github Desktop to publish an existing repository

TODO: How to add contents from a local repository to an existing online repository

## Set up Github Page
To set up a git page as a free user, you need to change the visibility of your repository from private to public: go to Settings -> General-> change visibility
Then go to Settings -> Pages -> Build and deployment, set the branch to master
add the following configurations in _config.yml
```
deploy:
  type: git
  repo: <repository url> # https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch]
  message: [message]
```
Deploy your site hexo clean && hexo deploy.



# Some useful commands
```
hexo g
```
This command has the similar function like "git add .", should be used each time after you made some changes and want to save them staticly.
```
hexo d
```
This command has the similar function like "git commit". This can push the changes to the Github Page branch and make it public.

## Add a code chunk
To add a code chunk, you need 
```
    ``` [specify the language ]
    ```
```

## add super link
```
[Link text Here](https://link-url-here.org)
```