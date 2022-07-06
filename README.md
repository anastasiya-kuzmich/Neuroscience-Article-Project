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

I collected most of my data from the Springer Nature API, which is a leading global scientific publisher of books and journals. I have selected a range of journals available on the API, keeping their Scimago ratings in mind.

### **Article Metrics Retrieval**

Most APIs that provide article metrics data are for institutional use only. I tackled this by web-scraping the individual articles' web pages straight from the two publishers’ websites: Springer and Nature.
