---
title: Environment variables and Path variable
date: '2019-09-24T08:11:33.626Z'
template: "post"
draft: false
slug: "/posts/environment-variables-and-path-variable/"
category: "Random"
tags:
  - "environmentVariable"
  - "pathVariable"
description: 
socialImage: "/media/image-2.jpg"
---
## Get all the Environment variables
```command
SET
```
The above command will also give the value of `PATH` variable.

## Get the PATH variable value
```command
path
```

## Updating the PATH variable temporary
```command
setx path "%path%;c:\directoryPath"
```
