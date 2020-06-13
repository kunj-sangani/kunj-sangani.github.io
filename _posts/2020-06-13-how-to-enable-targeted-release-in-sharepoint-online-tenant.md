---
date: 2020-06-13 09:34:59
layout: post
title: "How To Enable Targeted Release In SharePoint Online Tenant"
subtitle:
description:
image: 
optimized_image: /assets/img/Blogs/Blog1/f.png
category:
tags:
  - SharePoint
  - Administration
author: kunjsangani
paginate: false
---

# Overview

This blog will walk us through changing the release preference for SharePoint Online tenants.

For changing Release preference in Microsoft365 Tenant follow the below steps.

## Step 1

Login with the tenant admin account

![1st Image](/assets/img/Blogs/Blog1/a.png)

## Step 2

Navigate to the Microsoft365 Tenant admin center

![2nd Image](/assets/img/Blogs/Blog1/b.png)

## Step 3

Click on show all to see all the navigations in admin center

![3rd Image](/assets/img/Blogs/Blog1/c.png)

## Step 4

Expand the settings section and select the setting tab inside it to get all the relevant settings for M365 Tenant.

![4th Image](/assets/img/Blogs/Blog1/d.png)

## Step 5

Select the organization Profile Tab and click on release preferences.

![5th Image](/assets/img/Blogs/Blog1/e.png)

## Step 6

There are three options in the release preference --  let us go through it in detail.

![6th Image](/assets/img/Blogs/Blog1/f.png)

* Standart release for everyone
  This should be used for Production Tenant where we would not require any new updates other than the stable updates.

* Targeted release for everyone
  This should be used for Development Tenant where we would require all the new updates even when it is in Preview mode. This will help us know  the latest features developed by Microsoft

* Targeted release for selected users
  This should be used when we won't require all the users in the tenant to get the new updates or the features in the Preview mode. We can specify which users should have that access