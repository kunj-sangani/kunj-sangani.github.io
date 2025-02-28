---
weight: 4
title: "How To Use PnP Search Filters And Search Vertical Web Parts"
date: 2021-05-27T22:57:40+08:00
lastmod: 2021-05-22T27:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "In this article, we will learn how to use the PnP search filter and search a vertical web part"

canonicalurl: "https://www.c-sharpcorner.com/article/how-to-use-pnp-search-filters-and-search-vertical-webpart/"

tags: ["PnP Search", "SPFx", "SharePoint Online"]
categories: ["SharePoint Online", "SPFx"]

lightgallery: true
---

Introduction
------------

In this article, we will learn how to use the PnP search filter and search a vertical web part  
  
**Prerequisites**

  
\- PnP search result and search box web part added in Modern Page

  
\- To understand better, check out my article on [how to use and configure the PnP search](https://www.c-sharpcorner.com/article/how-to-use-and-configure-pnp-search-webpart/) Web part

Setup
-----

Below are the steps to use PnP Search Filters and Search Vertical Web part

**Step 1**
------------

Add PnP Search Filters and Search Vertical Web part to an existing search modern page

**Step 2 - Configure the web parts**
------------

*   PnP Search Filters Webpart  
    This web part is used for providing a filter to the search result so that the user can drill down their results to fetch their desired result.

*   _Refiners_  
    We can select the managed property for displaying the search filter. This managed property should be refinable. We can even provide custom labels for the managed property. We can specify the sort type and sort order for the refiner.

*   _Connect to search results Web Part_
    _The search results from Web Part to use on the current page to get filters.

*   _Web part title_  
    Displays the web part title we can keep it blank if we do not want to display the title

*   _Show blank if no result_  
    It would not display any filters if there are no filter results for the search term

*   _Filter layout_  
    We can choose from the two different layout present  
      
    

*   Vertical - It would display the filter result in the page itself
*   Panel - It would open a panel on click of filter icon and we can select the filter result from the panel displayed

![How To Use PnP Search Filters And Search Vertical Webpart](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-search-filters-and-search-vertical-webpart/Images/1_SearchFilter.png)

*   _PnP Search Vertical Web part_  
    This web part is used for providing different scopes which can be used by users to fetch their desired result  
      
    

*   _Search verticals_  
    We can configure different scopes by providing the Tab name(Display name), Query Template, result source identified, Office UI Fabric icon (this icon would be displayed). If we want to open the scope on another page then we can select Is hyperlink and provide the Link URL which would be used for redirecting the user. We can even select open behavior that could be open in a new tab or open in the current tab.

*   _Show result count_  
    If we want to display the result count against the scope we can toggle it to on

*   _Connect to a search results Web Part_  
    The search results from Web Part to use on the current page.

![How To Use PnP Search Filters And Search Vertical Webpart](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-search-filters-and-search-vertical-webpart/Images/2_SearchVerticals.png)

**Step 3 - Update the Search Result web part wit the below setting**
------------ 

Edit the search result web part and navigate to the second page or section and update the below settings:

*   _Use refiners from this component_  
    In the dropdown, select the Search Filters so that it would connect to the search filters web part.

*   _Use verticals from this component_  
    In the dropdown, select the Search Verticals so that it would connect to the search verticals.

![How To Use PnP Search Filters And Search Vertical Webpart](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-search-filters-and-search-vertical-webpart/Images/3_SearchResults.png)

**Outcome**
------------

![How To Use PnP Search Filters And Search Vertical Webpart](https://f4n3x6c5.stackpathcdn.com/article/how-to-use-pnp-search-filters-and-search-vertical-webpart/Images/4_Outcome.png)