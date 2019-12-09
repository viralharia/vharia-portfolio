---
title: REST - PUT vs PATCH
date: '2019-09-30T14:09:05.760Z'
template: "post"
draft: false
slug: "/posts/rest-put-vs-patch/"
category: "REST"
tags:
  - "REST"
description:
socialImage: "/media/image-2.jpg"
---
* When designing API endpoints, there’s always the need to specify what http method to use for CRUD (Create, Read/Retrieve, Update, Delete) operations. 
* Commonly, this is nailed down as:
  * Create — POST
  * Read/Retrieve — GET
  * Update — PUT/PATCH
  * Delete — DELETE
* for update we have 2 operations - `PUT` and `PATCH` and sometimes they are used interchangeably. Both methods update the resource at a location, but they do so differently.

## PUT request
* `PUT` request replaces the content at the resource. If nothing is found at the location, it’ll simply PUT the resource at the location. 
* PUT requests always contains a full resource. This is necessary because, a necessary quality of PUT requests is **_idempotence_** — the quality of producing the same result even if the same request is made multiple times.

## PATCH request
* A `PATCH` request on the other hand, is used to make changes to part of the resource at a location. 
* That is, it **PATCHES** the resource — changing its properties. It is used to make minor updates to resources and it’s not required to be idempotent.
* if a `PATCH` request is made to a non-existent url, it should simply fail without creating a new resource unlike PUT, which would create a new one using the payload.
