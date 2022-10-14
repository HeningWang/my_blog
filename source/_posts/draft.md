---
title: How to build a personal blog with hexo
date: 2022-08-11 16:13:06
tags: [technical, blogs]
---
# Install and Initialise Hexo on Mac OS
## Install and setting up nvm 
Follow the tutorial on this website: https://github.com/nvm-sh/nvm# to install nvm. (nvm, Node Version Manager, allows you to avoid EACES permission issues on Mac.)
After successfully installing nvm, add the following lines to .zprofile to launch nvm in every new zsh terminal session.
```
# open a new terminal window
cd ~/
touch .zprofile # skip this if the file is already there 
open .zprofile
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```
Using the following lines to install node, and check if node.js and nvm are successfully installed:
```
nvm install node
node -v
command -v nvm
``` 
## Install hexo
Follow the tutorial on this website: https://hexo.io/docs/
```
npm install -g hexo-cli
hexo #verify installation
```
## Initialise the blog and some relevant init configurations
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
## Set up Github Page
To set up a git page as a free user, you need to change the visibility of your repository from private to public: go to Settings -> General-> change visibility
Then go to Settings -> Pages -> Build and deployment, set the branch to master
add the following configurations in _config.yml
```
deploy:
  type: git
  repo: <repository url> 
  branch: [branch]
  message: [message]
```
Deploy your site hexo clean && hexo deploy.
## Fix deploy problem of node 12 to node 16
See this article in Chinese: https://www.zywvvd.com/notes/hexo/website/36-vercel-node-upgrade/vercel-node-upgrade/
//TODO: English below: 

# Generate workflow guidelines
## Hexo related
This command has the similar function like "git add .", should be used each time after you made some changes and want to save them statically.
```
hexo g
```
This command has the similar function like "git commit". This can push the changes to the Github Page branch and make it public.
```
hexo d
```
To create a new post or a new page, you can run the following command
```
hexo new [layout] <title>
```
## Markdown related
### Add a code chunk in md
To add a code chunk, you need 
```
    ``` [specify the language ]
    ```
```

## add super link
```
[Link text Here](https://link-url-here.org)
```