---
title: Cost-based Modeling and Optimization of Secure Matrix Multiplication in the Cloud
author: Richard Hernandez
date: '2024-05-03 14:10:00 +0800'
categories:
  - Research
  - MPC
tags:
  - research
  - multiparty computation
  - federate learning
  - privacy
  - security
image:
  path: ../assets/img/strassen.png
  alt: Other works vs mine
---

## Abstract

Machine Learning (ML) applications are prominent in many fields due to their ability to derive insights and automate processes. Many services utilize data from smart devices to offer personalized services to users. However, privacy concerns arise when sensitive data is collected by such devices for ML operations and outsourced to the cloud, along with the high liability costs associated with a security breach.
Multiparty computation (MPC) is a promising method for privacy-preserving ML. However, it has a significant computational overhead, mainly due to the required Matrix Multiplications (MM). This paper improves the secure MM performance by adopting and integrating the Strassen algorithm in the MPC protocols. The challenge is tuning the different cloud infrastructure parameters to maximize the Strassen algorithm's benefits. To address this challenge, we formulated a multiobjective optimization problem. The goal is to improve the MM's response time while minimizing the cloud resource costs and potential security losses, given a successful cyberattack on the cloud.
We then developed several solutions to identify the best parameters (e.g., matrix dimension, number of MPC nodes, among others) to optimize the trade-off among different optimization goals. Our implementation over SPDZ shows that we can significantly reduce resource costs and minimize potential security loss with respect to the naive MM over the MPC approach. 
