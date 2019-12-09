---
title: Travis CI - Job lifecycle
date: '2019-11-07 14:36'
template: "post"
draft: false
slug: "/posts/travis-ci-job-lifecycle/"
category: "Random"
tags:
  - "travisci"
description:
socialImage: "/media/image-2.jpg"
---
# Travis CI Job Lifecycle
A job on Travis CI is made up of two main parts:
- **install**: install any dependencies required
- **script**: run the build script

The complete job lifecycle, including three optional deployment phases and after checking out the git repository and changing to the repository directory, is:

1. OPTIONAL Install `apt addons`
2. OPTIONAL Install `cache components`
3. `before_install`
4. `install`
5. `before_script`
6. `script`
7. OPTIONAL `before_cache` (for cleaning up cache)
8. `after_success` or `after_failure`
9. OPTIONAL `before_deploy`
10. OPTIONAL `deploy`
11. OPTIONAL `after_deploy`
12. `after_script`
