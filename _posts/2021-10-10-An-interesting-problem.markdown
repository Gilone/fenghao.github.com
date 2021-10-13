---
title: "Detailed proof of the optimality property of mean absolute error"
layout: post
date: 2021-10-10 20:59
image: https://upload.wikimedia.org/wikipedia/commons/thumb/a/ad/Gottfried-Wilhelm-Leibniz-Louis-Dutens-opera-omnia_MG_1180.tif/lossy-page1-220px-Gottfried-Wilhelm-Leibniz-Louis-Dutens-opera-omnia_MG_1180.tif.jpg
headerImage: True
tag:
- Tech
category: blog
projects: false
author: Hao Feng
description: Detailed proof of the optimality property of Mean Absolute Error
---

In the homework of *CSE258 Data Mining and Recommendation System*, there is a very interesting question.

>Show that for a trivial predictor, i.e., y = θ0, the best possible value of θ0 in terms of the Mean Absolute Error is the median of the label y.

Where to start this proof? How to get its derivative?

The breakthrough of the problem is to treat MAE as a expectation of a random variable. Therefore, I have my idea.

![1]({{site.url}}/assets/images/proof/1.png)
![2]({{site.url}}/assets/images/proof/2.png)
![3]({{site.url}}/assets/images/proof/3.png)

**Thank you, Mr.Leibniz!**
