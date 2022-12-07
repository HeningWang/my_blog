---
title: insert-image
date: 2022-12-07 15:27:52
tags: [hexo]
---
# How to insert images in hexo-blogs

## Via local resources
1. change the following setting in the writing session in _config.yml of hexo (At least for me, it's already there, just need to change the value. But if there isn't, add this command manually.)
```
post_asset_folder: true
# originally
# post_asset_folder: false
```

2. install plugin packages
```
npm install hexo-asset-image --save
```

3. Generate the resource folder

With asset folder management enabled, Hexo will create a folder every time you make a new post with the hexo new [layout] <title> command. This asset folder will have the same name as the markdown file associated with the post. Place all assets related to your post into the associated folder, and you will be able to reference them using a relative path, making for an easier and more convenient workflow.

4. Import images
Put images into the folder, then import the image in the following format.
```
![alt](folder/picture.png)
```