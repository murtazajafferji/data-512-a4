
# Ranking JavaScript Packages by Number of Dependent Projects 

## Problem

JavaScript is the most popular programming language on Earth. There are thousands of open source packages released for developers to use in their projects. When developers are deciding between similar packages that fulfill the needs of the project, they often desire to choose the most *popular* package with the largest community size, which often translates to the highest probability of future growth/support. Since *popularity* of the package is not easily measurable, users often use npm downloads or stars on Github as a proxy. However, the number of downloads is an unreliable indicator because download counts are incremented by [automated build processes/robots](https://blog.npmjs.org/post/92574016600/numeric-precision-matters-how-npm-download-counts) and GitHub stars are [indicative of the traffic or attention a package receives on GitHub](https://help.github.com/articles/about-stars/), but not necessarily usage. Additionally, since stars and download counts are nondecreasing, users can incorrectly assume that packages with higher star and download counts currently have larger communities or are more well maintained. 

## Goals

Stars, commits, pull-requests, issues, and downloads are commmonly used to measure popularity of JavaScript packages on GitHub. We will attempt to show that the number references to these packages by other repositories on GitHub is a more useful metric for determining a package's *popularity*. 

The dependency graph generated as part of this analysis can not only be used by developers to help determine which JavaScript package to choose, but it can also give insights into the dynamics of the JavaScript ecosystem, the adoption trends of JavaScript packages, and the nature of the projects consuming these packages.

**Human-centered design inspiration**

The decision to pursue this project was inspired by a common issue that people face when trying to evaluate packages on GitHub and the problem some developers have with having the importance of their work recognized. We will gather feedback from JavaScript developers during the formulation of research questions and while evaluating results of the analysis to determine if the new metric provides any additional value to them.


## Research questions

A similar study, [NPM By Numbers](http://npmbynumbers.bocoup.com/), was peformed in 2014, but with the significant difference of only analyzing packages published on NPM and not the applications which consume those packages. We intend to answer similar questions, using GitHub data, that were asked in the exploratory analysis for that study. 

>How many packages have more than 3 versions?
>
>How many packages have any direct dependents?
>
>How many packages are in the top 5% of direct dependents? (> 4)
>
>How many packages have any deep dependents?
>
>How many packages are in the top 5% of deep dependents? ( > 7)
>
>How many packages have a difference of > 1000 between their direct and >deep dependents?
>
>How many packages are older than one year?
>
>How many packages are older than two years?
>
>How many packages are older than three years?
>
>How many packages haven't been updated in longer than a year?
>
>Longest running projects [50] - delta between now and created date
>
>Most used packages [50] - based on deep dependencies
>
>Most releases [50] - packages with the most versions
>
>Least known, most valuable packages [50] - those packages that have few direct dependents but have high deep dependencies.
>
>[Source](https://github.com/iros/npm-by-numbers-analysis/blob/master/analysis/Analysis%20Sketch%20Book%20%28unstructured%29.ipynb)



## Dataset
**Datset used in this analysis**
>This 3TB+ dataset comprises the largest released source of GitHub activity to date. It contains a full snapshot of the content of more than 2.8 million open source GitHub repositories including more than 145 million unique commits, over 2 billion different file paths, and the contents of the latest revision for 163 million files, all of which are searchable with regular expressions.
>
>This public dataset is hosted in Google BigQuery and is included in BigQuery's 1TB/mo of free tier processing. This means that each user receives 1TB of free BigQuery processing every month, which can be used to run queries on this public dataset. 
>
>[Source](https://console.cloud.google.com/marketplace/details/github/github-repos?filter=solution-type:dataset&q=github&id=46ee22ab-2ca4-4750-81a7-3ee0f0150dcb)

**What is BigQuery?**
>BigQuery is Google's fully managed, NoOps, low cost analytics database. With BigQuery you can query terabytes of data without needing a database administrator or any infrastructure to manage. BigQuery uses familiar SQL and a pay-only-for-what-you-use charging model. BigQuery allows you to focus on analyzing data to find meaningful insights.
>
>[Source](https://codelabs.developers.google.com/codelabs/bigquery-github/index.html?index=..%2F..%2Findex#0)

**Data sources from which Google BigQuery aggregates data**

>**GH Archive**
>
>A log of all public events happening on GitHub.
>
>* https://www.githubarchive.org/
>* https://bigquery.cloud.google.com/table/githubarchive:month.201808
>
>Updated: Hourly
>
>**GHTorrent**
>
>A relational dataset that adds more data from GitHub's knowledge graph:
>
>* http://ghtorrent.org/gcloud.html
>* https://bigquery.cloud.google.com/table/ghtorrent-bq:ght_2018_04_01.users
>
>Updated: ~Monthly (depending on the project owner availability).
>
>**GitHub contents**
>
>A snapshot of the open source contents of GitHub, ready to be analyzed.
>
>* https://medium.com/google-cloud/github-on-bigquery-analyze-all-the-code-b3576fd2b150
>* https://bigquery.cloud.google.com/table/bigquery-public-data:github_repos.files
>
>Updated: ~Weekly.
>  
>[Source](https://github.com/fhoffa/analyzing_github)

**Scope of the data included in this dataset**
> * Projects [hosted on Github] that have a [clear open source license](https://blog.github.com/2016-09-21-license-now-displayed-on-repository-overview/).
> * Forks and/or un-notable projects not included.
> * Nevertheless, it represents terabytes of code.
>
>[Source](https://medium.com/google-cloud/github-on-bigquery-analyze-all-the-code-b3576fd2b150) 
>
>**Which percentage of the repositories of GitHub exists on the data set?**
>
>The results say that 6 months ago (June 2017) ~21,890 repositories got more than 15 stars. Of those ~21,890 repositories, ~53.86% are mirrored on bigquery-public-data.github_repos.contents.
>
>**How do you refresh the data? Do you use a selected list of projects or do you refresh the list each time?**
>
>The pipeline looks periodically for new projects. Old projects that change their license to a valid open source one (according to the API) might be missed and there's an option to add them manually.
>
>[Source](https://github.com/fhoffa/analyzing_github)

**License**
>This dataset is publicly available for anyone to use under the following terms provided by the Dataset Source - https://help.github.com/articles/github-terms-of-service/ - and is provided "AS IS" without any warranty, express or implied, from Google. Google disclaims all liability for any damages, direct or indirect, resulting from the use of the dataset.
>
> [Source](https://console.cloud.google.com/marketplace/details/github/github-repos?filter=solution-type:dataset&q=github&id=46ee22ab-2ca4-4750-81a7-3ee0f0150dcb&project=api-8220795080820646267-783724&folder&organizationId)


## Alternate dataset
**[Libraries.io Open Source Repository and Dependency Metadata](https://libraries.io/data)**
> Libraries.io gathers data from 36 package managers and 3 source code repositories. We track over 2.7m unique open source packages, 33m repositories and 235m interdependencies between them. This gives Libraries.io a unique understanding of open source software. An understanding that we want to share with you. 
>
>In this release you will find data about software distributed and/or crafted publicly on the Internet. You will find information about its development, its distribution and its relationship with other software included as a dependency. You will not find any information about the individuals who create and maintain these projects.

**License**
>This dataset is released under the [Creative Commons Attribution-ShareAlike 4.0 International Licence](https://creativecommons.org/licenses/by-sa/4.0/).
>
>This licence provides the user with the freedom to use, adapt and redistribute this data. In return the user must publish any derivative work under a similarly open licence, attributing Libraries.io as a data source. The full text of the licence is included in the data.
>
> [Source](https://zenodo.org/record/1196312#.W_v3OHdFzZs)



## Method

We will use the following [method](http://jbeckwith.com/2016/08/13/bigquery-github/) to get the ranking of packages with the largest number of dependencies by other repos:
1. Get all top level package.json files from [bigquery-public-data:github_repos.files]
2. Get the contents of these files from [bigquery-public-data:github_repos.contents]
3. Run a JavaScript user-defined-function to parse the dependencies within the contents
4. Store the results in a temp table
5. GROUP BY / ORDER BY n the temp table to get 



## Limitations of approach

* Since we are relying on references contained packages.json data, we are excluding use of packages via CDN
* Packages that have many contributors but are not included as dependencies, possibly due to being unfinished, will not be ranked 
* Projects that do not have an Open Source license are ignored
* Projects that have packages.json files below the top level will be ignored

## Potential data limitations

* 5 TB of data can be processed through queries in BigQuery for free per month. Beyond that, if costs are prohibitive, we may not be able to complete more in depth analysis
* Semantic versioning syntax allows for the same string to point to different package versions, depending on when npm install was run. Thus, we cannot reliably consume the package version.

## Requirements
* A device with a browser
* A Google Cloud Platform Project

## Analysis (placeholder)
1. Using the [BigQuery web UI](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=github_repos&t=commits&page=table), Google allows 1TB of data to be queried for free every month
2. Alternatively, each Kaggle user can query 5TB of data for free every 30 days, with the help of Sohier's BigQuery helper module. Setup environment to run BigQueries in Kaggle Kernels:
https://www.kaggle.com/poonaml/analyzing-3-million-github-repos-using-bigquery

