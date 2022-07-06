# Neuroscience Articles Impact vs Value
## **Overview**

This project was completed as my final project of the General Assembly Data Science Immersive course. This document addresses the problem, aims, methodology, findings, conclusions and tools used.

## **Notebook Contents**

| File      | Contents |
| ----------- | ----------- |
| Anastasiya_Capstone_1_Acquisition_and_Cleaning.ipynb      | Acquisition and cleaning notebook       |
| Anastasiya_Capstone_2_EDA.ipynb   | Exploratory Data Analysis (EDA) Notebook        |
| Anastasiya_Capstone_2-1_EDA_Supplementary.ipynb  | Supplementary EDA: Modelling Topics by Decade        |
| Anastasiya_Capstone_3_Modelling.ipynb   | Modelling & results        |
| NonTechPresentation_NeuroscienceArticles_AnastasiyaKuzmich.ppsx   | A non-technical PowerPoint show recording        |

*Please note the data files are not included but are available upon request.*

## **Background**

As a big fan of all things science, I've noticed that catchy headlines about scientific “breakthroughs” and “successes” often contradict the information in the original article, as the media oversensionalises scientific discoveries to generate clicks and views. This effect is further amplified by the unstoppable machine of social media.

A dangerous example of this phenomenon is the infamous Wakefield study (1998), which wrongly implied a link between the measles, mumps and rubella (MMR) vaccination and autism. While the scientific community widely discredited the researcher and the article was subsequently retracted, the original article was a bombshell in the media, turning tens of thousands of parents around the world [against vaccinating their children](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0256395). The ripple effect of this 20+-year-old article is noticeably present today, with so-called anti-vax movements alive, thriving and endangering public health through [preventable measles outbreaks](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5489284/) and [COVID-19 vaccination hesitancy](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0264019).

Within this project, I wished to explore this sometimes monstrous disparity between the “impact” that a scientific claim may have on the general impact, and the genuine “value” it brings to scientific research. My main goal was to discover what features of a neuroscience article most contribute to its impact and value, and whether the two are related and could be predicted.

## **Aims & Objectives**

The specific questions I aimed to answer were:

1. Which factors contribute most to the *value* of an article to the scientific community?
    - *Target: value factor measured as article reference counts within further academic work in published academic journals*
2. Which factors contribute most to the *impact* of an article on the general public?
    - *Target: impact factor measured as mention counts on social media, news outlets, and other non-academic sources.*
3. Which article features result in the biggest disparities between the scientific and general communities?

## **Data Acquisition**

### **Article Data Retrieval**

I collected most of my data from the [Springer Nature API](https://dev.springernature.com/), which is a leading global scientific publisher of books and journals. I have selected a range of journals available on the API, keeping their [Scimago](https://www.scimagojr.com/journalrank.php?category=2801) ratings in mind.

### **Article Metrics Retrieval**

Most APIs that provide article metrics data are for institutional use only. I tackled this by web-scraping the individual articles' web pages straight from the two publishers’ websites: Springer and Nature.

## Data Cleaning

I cleaned the collected data by:

- Dropping duplicates, rows with missing values, non-unique values across several features, articles written in languages other than English and all but the desired genres.
- Encoding the publishers, open access, issue type, genres,
- Removing undesired characters from all object-type columns (authors, title, keywords, abstracts etc).
- Splitting the onilne metrics (e.g. '262 tweeters & 75 blogs & 51 Facebook pages') into their individual source columns.
- Extracting the years published and the author counts.

Statistically speaking, there was a lot of outliers within the years, author counts and article length columns. However, these were all real data points when looked up, so I did not remove them to preserve the data variance.

## The dataset

The resulting dataset contained information on 103985 Neuroscience articles, including:

### **Article Content**

| Variable      | Description | Type |
| ----------- | ----------- | ----------- |
| **doi** | Digital Object Identifier, a unique string of numbers, letters and symbols used to uniquely identify an article or document & provide it with a permanent web address | object |
|**title**| Article title| object|
|**journal**| The journal in which the article was published |object|
|**year_published**| The year the article was published |int64|
|**first_author**| Name of the first author, usually the person who contributed most to the work |object|
|**second_author**| Name of the second author |object|
|**third_author**|Name of the third author |object|
|**author_count**| Total number of authors |int64|
|**genre**| Article genre (0: Original Paper, 1: Review Paper, 2: Brief Communication) |int64|
|**publisher**| Article publisher (0: Springer, 1: Nature)|int64|
|**issue_type**| Issue type (0: Regular, 1: Combined, 2: Supplement, 3:Unknown) |int64|
|**article_length**| Length of the article in pages |float64|
|**is_open_access**| Whether the article can be freely accessed by anyone or just the academic institutions (0: Closed-access, 1: Open-access) |int64|
|**keywords**| Article keywords |object|
|**abstract**| Brief summary of the main article |object|

### **Article Metrics**
*as of 07th June 2022*

| Variable      | Description | Type |
| ----------- | ----------- | ----------- |
| **article_accesses** | Number of times an article has been accessed on the publisher's website | float64 |
| **web_of_science** | Number of times Clarivate users clicked on the available Web of Science links for the optimal, legitimate full-text version of the article, or took steps to save the item in a bibliographic management tool e.g. on EndNote | float64 |
| **cross_ref** | Published citations counts for each work a.k.a how many scientific articles/other publications have cited that work since | float64 |
| **twitter** | Number of mentions on Twitter | int64 |
| **facebook** | Number of mentions on Facebook | int64 |
| **blogs** | Number of mentions in blogs | int64 |
| **google_plus** | Number of mentions on Google+ | int64 |
| **news** | All media mentions | int64 |
| **reddit** | Number of mentions on Reddit | int64 |
| **pinterest** | Number of mentions on Pinterest | int64 |
| **video**| Number of mentions on videos published online | int64 |
| **wiki** | Number of mentions on the Wikipedia website | int64 |
| **f1000** | Faculty Opinions (formerly f1000), essentially a number of "community endorsements" | int64 |
| **mendeley** | Total Mendeley reader count | int64 |
| **citeulike** | Total CiteULike saves, essentially "bookmarks" for academic research | int64 |

### **Engineered Features**

I additionally engineered the following features during EDA and throughout modelling:

| Variable      | Description | Type |
| ----------- | ----------- | ----------- |
| view_count | Article access count from the publisher's website | float64 |
| value_factor | Citation counts within further published work to reflect the "value" to the scientific community a.k.a the cross-ref metric | float64 |
| impact_factor | Mention counts from non-academic sources reflecting the article's "popularity" with the wider community e.g. social media, news etc; calculated using an Altmetric-like weighting system reflective of the reach of each type of source e.g. news = 8, Wikipedia = 3, Pinterest = 0.25. | float64 |
| time_period | The decade an article was published | int64 |
| topics | A lemmatised combination of keywords and title textual data, reflecting the key topics of the article | object |

