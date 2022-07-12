# Neuroscience Articles Impact vs Value
## **Overview**

I completed this 4-week solo project as my final assignment for the General Assembly Data Science Immersive course.  This document addresses the problem, aims, methodology, findings, conclusions and tools used.

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

A dangerous example of this phenomenon is the infamous Wakefield study (1998), which wrongly implied a link between the measles, mumps and rubella (MMR) vaccination and autism. While the scientific community widely discredited the researcher and the article was subsequently retracted, the original article was a bombshell in the media, turning tens of thousands of parents around the world [against vaccinating their children](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0256395). The ripple effect of this 20+-year-old article is evidently present today, with the so-called anti-vax movements alive, thriving and endangering public health through [preventable measles outbreaks](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5489284/) and [COVID-19 vaccination hesitancy](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0264019).

Within this project, I wished to explore this sometimes monstrous disparity between the “impact” a scientific claim may have on the general public, and the genuine “value” it brings to scientific research. My main goal was to discover which features of a neuroscience article most contribute to its impact and value, whether the two are related, and could be predicted.

## **Aims & Objectives**

The specific questions I aimed to answer were:

1. Which factors contribute the most to the *value* of an article to the scientific community?
    - *Target: Value factor measured as the article reference counts within further academic work in published academic journals.*
2. Which factors contribute the most to the *impact* of an article on the general public?
    - *Target: Impact factor measured as the mention counts on social media, news outlets, and other non-academic sources.*
3. Which article features result in the largest disparity between the scientific and general communities?

## Tools & Libraries

- Requests
- Beautiful Soup
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- WordCloud
- TextWrap
- NLTK
- Imbalanced-learn
- SciPy
- XGBoost
- Tableau

## **Data Acquisition**

### **Article Data Retrieval**

I collected most of my data from the [Springer Nature API](https://dev.springernature.com/) – a leading global scientific publisher of books and journals. I have selected a range of journals available on the API, keeping their [Scimago](https://www.scimagojr.com/journalrank.php?category=2801) ratings in mind.

### **Article Metrics Retrieval**

Most APIs that provide article metrics data are for institutional use only. I tackled this by web-scraping the individual articles' web pages straight from the two publishers’ websites: Springer and Nature.

## Data Cleaning

I cleaned the collected data by:

- Removing duplicates, rows with missing values, non-unique values across several features, articles written in languages other than English and all but the desired genres
- Encoding the publishers, open access, issue types and genres
- Removing undesired characters from all object-type columns (authors, title, keywords, abstracts etc.)
- Splitting the online metrics (e.g. '262 tweeters & 75 blogs & 51 Facebook pages') into their individual source columns
- Extracting the years published and the author counts

Statistically speaking, there were many outliers within the years, author counts and article length columns. However, these were all real data points when looked up, so I did not remove them to preserve the data variance.

## The dataset

The resulting dataset contained information on 103985 Neuroscience articles. The data dictionaries are enclosed below.

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

## Exploratory Data Analysis (EDA)

### General Dataset Features

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6f763185-b097-43ec-aa67-d3055cd065b9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220706%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220706T165903Z&X-Amz-Expires=86400&X-Amz-Signature=24a624bcd4b79b503a045a98e5068a5c71b896a36143fcf9166cdc92180c3c81&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject"
     alt="The journal sources of the articles within this dataset. The size of each bubble corresponds to the article quantity from a given journal. "
     style="float: left; margin-right: 10px;" />

*Figure 1: The publishing journals of the articles within this dataset. The size of each bubble corresponds to the article quantity from a given journal.*

- The vast majority of the articles in this dataset were original research papers.
- 2/3 published by Springer, the rest by Nature.
- Only 13% were open-access.
- The oldest article was published in 1870, the newest in 2022, and the vast majority were released in the 21st century.
- An average article within this dataset was written by a group of 6 authors, while the largest team it took to produce a research article was 559 authors.
- An average article within this dataset was 10 pages long, while the longest article reached 101 pages.
- On average, reviews are both more valuable and impactful than original research papers. Likewise, Nature journals have a much higher average impact and value factors than those published by Springer. Open access articles were on average much more impactful, but less valuable.
- The view count was much higher correlated with the impact factor (0.72) than the value factor (0.28), thus how many times an article is read does not necessarily reflect its face value to the scientific community. This was further supported by the near-0 correlation between the value and impact factors themselves.

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/efacb9e4-6f34-40a7-9d39-84c0820dc352/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220706%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220706T170244Z&X-Amz-Expires=86400&X-Amz-Signature=d7e6f62b25142d910728641c77909c22b34126aa9f894a8af6ec772454322004&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject"
     alt="The journal sources of the articles within this dataset. The size of each bubble corresponds to the article quantity from a given journal. "
     style="float: left; margin-right: 10px;" />

*Figure 2: An example word cloud trained on the titles of all articles. Expectedly, the words brain, cell and disease dominated the articles.*

### Verifying the dataset’s integrity

To confirm that my dataset truly represents the neuroscientific research of the past century, I fitted an explorative multi-class Logistic Regression model using the decade as a target and the abstract as the predictor. The model's feature coefficients allowed me to highlight some of the "decade-defining" features.

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e161ea38-47fd-4f34-96dd-cad3476db099/Screenshot_2022-07-06_at_17.57.45.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220706%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220706T170533Z&X-Amz-Expires=86400&X-Amz-Signature=bc64e433acd60691ce7bf0e195d8abde924f0d4c1fe66d88130ddaee15da2eca&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screenshot%25202022-07-06%2520at%252017.57.45.png%22&x-id=GetObject"
     alt=""
     style="float: left; margin-right: 10px;" />
     
*Figure 3: The feature coefficients of an exploratory Logistic Regression model, demonstrating the defining article topics by decade.*

For example, the top defining feature of the 1980s was “ct”, which is in line with the invention of the first commercially available CT scanner around that time and the resulting [Nobel Prize](http://www.nobelprize.org/prizes/medicine/1979/summary/). Likewise, the [Human Genome Project](https://en.wikipedia.org/wiki/Human_Genome_Project#:~:text=The%20Human%20Genome%20Project) was launched in 1990, and "gene" was the top-defining feature of neuroscience research in this dataset. The current decade was, of course, defined by the words "covid", "19", and "covid 19"; within neuroscience research and far beyond.

## Model Selection

The test regression models I fitted generalised poorly on the testing dataset. Furthermore, the size of the dataset meant that these models were running for several days with no result. Therefore, I decided to tackle these problems by  predicting whether an article was “high-impact” / “low-impact” or “high-value” / “low-value”. I binarised my targets around the target mean.

The datasets produced after dummifying and frequency-vectorising the data were 100,000 rows x 1m+ columns in size. This meant that my kernel crashed and burned in the pre-processing stages, let alone in the modelling stages. Thus, I have decided to undersample the data to achieve target class balance using a random undersampler. This resulted in impact-targeting training-testing dataframes of size `(18772, 218372), (4694, 218372)`; and value-targeting training-testing dataframes of size `(33289, 367654), (8323, 367654)`.

To further assist the preprocessing and modelling stages of this project, I decided to work in the sparse matrix format. This allowed me to model on all my variables, text included, at once and allowed me to fit my models in hours, not days. 

In the past, I have found that meticulous hyperparameter tuning only ever slightly improves the model but takes a long time to run, so for this project, I defined a “baseline algorithm testing” function that ran a variety of models under basic parameters. These included the K-Nearest Neighbors Classifier, Logistic Regression, Decision Tree Classifier, Random Forest Classifier, Gradient Boosting Classifier and XGB Classifier. Of these, I selected to tune the most promising model based on the highest mean cross-validated accuracy score with the lowest standard deviation.

For my **impact factor** predicting model, my model of choice was the Lasso-like Logistic Regression with the following parameters:

```
penalty = 'l2'
solver = 'saga'
max_iter = 1000
C = 166.81005372
```

This model achieved an accuracy score of 0.925 on the training set, 0.889 on the testing set and 0.885±2.85e-05 in stratified 4-fold cross-validation. 

For my **value factor** predicting model, my model of choice was the Lasso-like Logistic Regression with the following parameters:

```
penalty='l2'
solver='saga'
max_iter = 1000
C= 10000
```

This model achieved an accuracy score of 0.89 on the training set, 0.86 on the testing set and 0.86±1.51e-05 in stratified 4-fold cross-validation.

## **Model Evaluation: Classification Reports**

### **Impact Factor**

**Train set precision 0.93, test set precision 0.89:**

- In the training set, 93% of the labels retrieved by my model were relevant; thus, 7% were false positives.
- In the testing set, 89% of the labels retrieved by my model were relevant; thus, 11% were false positives.

**Train set recall 0.93, test set recall 0.89:**

- In the training set, 93% of the relevant labels were retrieved by my model; thus, 7% were false negatives.
- In the testing set, 89% of the relevant labels were retrieved by my model; thus, 11% were false negatives.

**Train set ROC score 0.98, test set ROC score 0.95.**

- This score is an indicator of a successful tradeoff between false positives and false negatives.
- The impact predictions of my model were 98% correct on the training set.
- The impact predictions of my model were 95% correct on the training set.

### **Value Factor**

**Train set precision 0.89, test set precision 0.86:**

- In the training set, 89% of the labels retrieved by my model were relevant; thus, 11% were false positives.
- In the testing set, 86% of the labels retrieved by my model were relevant; thus, 13% were false positives.

**Train set recall 0.89, test set recall 0.86:**

- In the training set, 89% of the relevant labels were retrieved by my model; thus, 11% were false negatives.
- In the testing set, 86% of the relevant labels were retrieved by my model; thus, 14% were false negatives.

**Train set ROC score 0.96, test set ROC score 0.94:**

- This score is an indicator of a successful tradeoff between false positives and false negatives.
- The impact predictions of my model were 96% correct on the training set
- The impact predictions of my model were 94% correct on the training set

## Findings

Exploring the feature coefficients of these models uncovered the article attributes that most differently affect their impact and value. Preferential differences between the scientific community and the general public were observed within the articles’ decades, journals, publishers and genres.

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6a934d4e-dd88-4399-beb0-b0ae0f35d9b5/topics_scatter.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220706%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220706T181630Z&X-Amz-Expires=86400&X-Amz-Signature=94fc053fcde49854372a499c2809fd6e5262c9b40c7eb4ba5ee702270272d86c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22topics%2520scatter.png%22&x-id=GetObject"
     alt=""
     style="float: left; margin-right: 10px;" />

*Figure 4: Topic Differences in Value vs Impact Coefficients*

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/bfb55d58-0bdf-4193-885c-6f6fa68b9071/Screenshot_2022-07-06_at_11.50.48.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220706%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220706T181642Z&X-Amz-Expires=86400&X-Amz-Signature=9c7d163603e7c9be625b56dcdbd7bb257b65d8fa7d00aea104247f9ec155002a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screenshot%25202022-07-06%2520at%252011.50.48.png%22&x-id=GetObject"
     alt=""
     style="float: left; margin-right: 10px;" />

*Figure 5: All Differences in Impact vs Value Coefficients*

Please refer to my [Modelling Notebook](https://github.com/anastasiya-kuzmich/Neuroscience-Article-Project/blob/main/Anastasiya_Capstone_3_Modelling.ipynb) for a full descriptive breakdown of the results. 

## Limitations & Future Work

1. The dataset was collected from 2 publishers only; thus, it might not reflect all neuroscience research carried out over the past years. *While this was enough to reveal interesting trends within this study, further research should include a wider range of articles. Further work may also explore all general biomedical trends.*
2. Social media and news outlets do not always represent the "general public", as the people discussing an article online are just as likely (if not more likely) to be within the scientific community themselves. *This doesn’t necessarily affect my model, as the impact factor simply reflected “unpublished” opinions. However, further work could be refined by including specific tweets and information on the tweeters.*
3. What is “value”? An article could be objectively valuable without being referenced in further work, while another may be fruitful by informing future researchers to follow a different route. *This question is open-ended, but my metric is an undeniable measure of attention by the scientific community. Further work could include a sentiment analysis of both the online mentions and academic references.*
4. My model was a binary classification model, which might be too simplistic. *Future work can apply a regression model to data with wider impact and value factors variability.*

## Conclusions

Within this project, I successfully identified the features that most contribute to the disparity between the “impact” a scientific article has on the general public, and the genuine “value” it brings to scientific research. My machine learning models can identify these article qualities with 86-89% accuracy, while their coefficients define the journals, topics and other features with the highest contributions to this phenomenon.

My models can be used in tandem by publishers to “flag” high-impact & low-value articles for further peer review and fact-checking, as this class of articles is most likely to be picked up by the media, and less likely to be valuable to the scientific community. Such further verification could improve the publishers’ credibility and, more importantly, prevent the consequences of publishing potentially harmful articles (e.g. Wakefield’s MMR-autism study). 

## Contact

If you found this project interesting or would like to reach out, please don’t hesitate to contact me on [LinkedIn](https://www.linkedin.com/in/anastasiya-kuzmich/).
