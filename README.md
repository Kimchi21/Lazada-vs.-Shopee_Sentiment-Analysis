# Lazada vs. Shopee - Sentiment Analysis

<p align="center">
  <img src="https://cdn-oss.ginee.com/official/wp-content/uploads/2021/10/Screen-Shot-2021-10-11-at-6.00.15-PM.png" alt="vs" width="450">
</p>

## Table of Contents
1. [Overview](#overview)
2. [Methodology](#methodology)
3. [Exploratory Data Analysis](#exploratory-data-analysis)
4. [Results and Conclusion](#results-and-conclusion)
5. [Recommendations](#recommendations)
6. [Dashboards](#dashboards)

## Overview<a name="overview"></a>
Undoubtedly, online shopping has emerged as one of the most transformative modern conveniences. Among the Philippines' top e-commerce platforms, **Lazada** and **Shopee** stand out as highly favored choices. The e-commerce industry is rapidly evolving, and platforms like Lazada and Shopee have solidified their positions as pivotal contenders in the online shopping market. To guide informed decision-making, enhance user experiences, and shape effective marketing strategies, gaining a deep understanding of how users perceive and engage with these platforms is imperative.

Understanding customer sentiment and preferences on particular platforms is essential in the fiercely competitive e-commerce industry. The challenge is to apply sentiment analysis to evaluate and compare user sentiments on Lazada and Shopee. This entails determining the dominant emotions, such as good, negative, or neutral emotions, and evaluating the underlying causes of these emotions.

## Methodology<a name="methodology"></a>

<p align="center">
  <img src="Resources/Laz vs. Shopee - Methodology.png" alt="methodology" width="650">
</p>

### Data Source
Google play store provides access to content on Google Play, including apps, books, magazines, music, movies, and television programs. Users can leave reviews and rate the Shopee and Lazada apps on Google Play. Ratings are given on a 5-point scale, with 1 representing the lowest possible score and 5 representing the highest.

### Data Collection
To conduct sentiment analysis and topic modeling using textual data, it is essential to acquire authentic user reviews of Lazada and Shopee. To obtain these reviews, a web scraping process was employed. This process was facilitated by the utilization of [Google-Play-Scraper](https://github.com/JoMingyu/google-play-scraper), which offers APIs for seamless data extraction from the Google Play Store in Python, eliminating the need for external dependencies. The scraping of data was performed on the end of the month September 30, 2023.

```python
from google_play_scraper import Sort, reviews_all

result = reviews_all(
    'insert id app link here',
    sleep_milliseconds=0, # defaults to 0
    lang='en', # defaults to 'en'
    country='us', # defaults to 'us'
    sort=Sort.MOST_RELEVANT, # defaults to Sort.MOST_RELEVANT
    filter_score_with=5 # defaults to None(means all score)
)
```

The code block above showcases the web scraping function in which it returns all the reviews of the app in .json format. The id app link of the corresponding apps (Lazada and Shopee) as shown below and country was set to ph for Philippines was change adjusted accordingly.

<p align="center">
  <img src="Resources/Lazada link.png" alt="laz link" width="450">
</p>

<p align="center">
  <img src="Resources/Shopee link.png" alt="laz link" width="450">
</p>



### Data Cleaning and Preprocessing
After obtaining the data, the process of data cleaning is performed in which duplicate reviews, reviews that don't have any meaningful input and reviews that are not in English will be removed. Preprocessing was employed after data cleaning this step included removing HTML tags, removing non-letters or emojis, changing the text into lowercase, and removing stop words that appear regularly.

### Sentiment Analysis
Sentiment analysis was performed using two models. The first is the VADER (Valence Aware Dictionary and sEntiment Reasoner) is a sentiment analysis tool **VADER** is lexicon-based and adapted to social media sentiments and is also a bag of words approach.  VADER uses a lexicon of words and phrases, each assigned a polarity score from -1 (negative) to 1 (positive), with 0 for neutrality. After analyzing the text, VADER provides a final sentiment classification, which can be positive, negative, neutral, or a compound score that quantifies the overall sentiment's intensity.

The second one is **RoBERTa** pretrained model that has been trained on a sizable body of data. The word context in relation to other words is taken into account by the transformer model. The RoBERTa model also assings polarity scores of -1 (negative) to 1 (positive), with 0 for neutrality, however this model does not have a compound does the sentiment will analyzed on either negative or the positive sentiment.

### Topic Modeling
This final step was taken to identify which are the main causes of the sentiments to the users of the app. This may be useful to the management of both apps as they can improve the app based on the negative reviews and view feedback based on the positive reviews.

## Exploratory Data Analysis<a name="exploratory-data-analysis"></a>
The initial dataset for Lazada contained 788,257 rows. After preprocessing, which involved removing duplicate user reviews and applying filters, the dataset was reduced to 336,202 rows. In the case of Shopee, the original dataset had 316,410 rows. Preprocessing, which included eliminating duplicate user reviews and applying filters, the dataset was reduced to 155,583 rows. Upon inspecting the Shopee data it was found out that it was only able to scrape data until June 15, 2021 unlike Lazada who was able to scrape data all the way back in 2013.

### Key Insights from EDA

#### 1. **Distribution of reviews according to when they were made**

<p align="center">
  <img src="Resources\laz_date_distribution.png" alt="lazdatedib" width="400">
</p>

 The distribution of reviews by user rating in Lazada go way back in 2013 in which it started off with mainly positive reviews. Reviews gained traction at the end of 2018 and steadily had reviews during and after the pandemic hit. 
 
<p align="center">
  <img src="Resources\shopee_date_distribution.png" alt="shodatedib" width="400">
</p>

 As for Shopee the distribution of reviews by user rating in Shopee unfortunately go back only to 2021 this might be because of the data being scraped. It had steady positive reviews at the mid of 2021 however there can be seen a surgence of negative reviews at the month of April and September of 2022.

#### 2. **What trends could be seen over time based on the reviews**

<p align="center">
  <img src="Resources\laz_rty.png" alt="lazrty" width="600">
</p>

<p align="center">
  <img src="Resources\laz_rtq.png" alt="lazrtq" width="600">
</p>

<p align="center">
  <img src="Resources\laz_rtm.png" alt="lazrtm" width="600">
</p>

<p align="center">
  <img src="Resources\laz_rtd.png" alt="lazrtd" width="600">
</p>

The peak in reviews occurred during November 2019, particularly on the 11th day of that month, coinciding with Lazada's significant sales events. Lazada is known for its monthly sales, adopting a format where the day matches the month number (e.g., 1.1 for January, 2.2 for February). During these events, products are offered at discounted rates, accompanied by free shipping vouchers, cashback, and Lazada bonuses, making it the best time for consumers to make purchases. This surge in reviews during sales periods suggests a correlation between consumer feedback and the occurrence of these promotional events, indicating levels of satisfaction or dissatisfaction. Does it mean that during other years, the same trend could be seen? In my opinion, I don't think so, because drilling down into other years will provide insights into that specific year, but this overall top-down view of the whole review trend provides a good surface-level view of the analysis.

<p align="center">
  <img src="Resources\shopee_rty.png" alt="shopeerty" width="600">
</p>

<p align="center">
  <img src="Resources\shopee_rtq.png" alt="shopeertq" width="600">
</p>

<p align="center">
  <img src="Resources\shopee_rtm.png" alt="shopeertm" width="600">
</p>

<p align="center">
  <img src="Resources\shopee_rtd.png" alt="shopeertd" width="600">
</p>

In 2022, reviews surged to a total of 71,297, peaking notably in the 3rd quarter with 54,657 reviews, particularly in August, July, and September (18,262, 18,236, and 18,159, respectively). The 18th day of each month notably stood out as the peak day with 5,942 reviews, while review spikes were also noticeable at the start, middle, and end of the month. These fluctuations appear to align with both quarterly and yearly trends in review activity. Shopee's diverse range of sales events, including Shopee Flash Sale, ShopeePay Exclusives, Lowest Price Guaranteed, Shopee Double Double Sale, and Shopee Prizes, likely contribute to the heightened activity on the platform. This could potentially correlate with observed peaks in review submissions across various periods throughout the year.

#### 3. **Which reviews garnered the most number of thumbs up from their reviews both positive and negative**
 
<p align="center">
  <img src="Resources\Laz_TUC.png" alt="vs" width="500">
</p>

 Based on Lazada reviews, negative ratings tend to receive more thumbs up on average, than positive ratings. 

 <p align="center">
  <img src="Resources\shopee_TUC.png" alt="vs" width="500">
</p>
 
 The same trend can be seen with Shopee reviews, in which negative ratings tend to receive more thumbs up on average than positive ratings.

#### 4. **Most frequently used words for positive and negative reviews**

<p align="center">
  <img src="Resources\laz_freq_pos.png" alt="vs" width="500">
</p>

<p align="center">
  <img src="Resources\laz_freq_neg.png" alt="vs" width="500">
</p>

 After adding some custom stopwords the word cloud is finally giving cohesive sentiments fleshing out the reviews based on positive and negative content. It can be seen in Lazada the most frequently used words for positive reviews mainly suggest positive qualities like the app is easy to use, good service and great. While, the most frequently used words for negative reviews are even, order, and product.

<p align="center">
  <img src="Resources\shopee_freq_pos.png" alt="vs" width="500">
</p>

<p align="center">
  <img src="Resources\shopee_freq_neg.png" alt="vs" width="500">
</p>

 Same thing with shopee by adding some custom stop words the sentiments become a lot fleshed out. The most frequently used words for positive reviews in shopee are good, thank and nice. On the other hand, the most frequently used words for negative reviews in shopee are order, even and customer service.

#### 5. **What version of the app had the most positive and negative reviews**

<p align="center">
  <img src="Resources\laz_appv_distribution.png" alt="vs" width="550">
</p>

 Lazada app version **6.38.2** had the most positive reviews, and app version **6.28.1** had the most negative reviews.

<p align="center">
  <img src="Resources\shopee_appv_distribution.png" alt="vs" width="550">
</p>

 As for Shopee, the app version with the most positive reviews is version **2.80.30** and the version with the most negative reviews is version **2.86.11**.

#### Downsampling the Data
Before proceeding to sentiment analysis and topic modeling. It would be unfair to comapre the two if there is a big discrepancy in the number of number of data for both e-commerce platforms. Downsampling the data into 150,000 rows for each was done to have a fair evaluation of both.

#### Data Dictionary
The dataset contains the following features:

| Feature         | Type      | Description |
|:----------------|:---------:|-----------|
| content         |   obj   |   Raw text of user reviews   |
| contentAdj      |   obj   |   Preprocessed text of user reivews   |
| score           |   int   |   Star rating of users gave (1-5)   |
| sentimentLabel           |   int   |   Sentiment label according to the stars given   <br> Positive Sentiment: 1 ----> 4 - 5 star ratings<br>Negative Sentiment: 0 ----> 1-3 star ratings| 

## Results and Conclusion<a name="results-and-conclusion"></a>

### Lazada Sentiment Analysis Report

#### Accuracy
|                   | Best Accuracy  | Best Threshold |
|:------------------|:--------------:|:---------:|
| VADER             |    85.68       |   -0.003   |
| RoBERTa           |    91.24       |   0.08   |

Based on the results, VADER is able to classify 85.68% of sentiments correctly with -0.003 being its best threshold. On the other hand, the RoBERTa model correctly classified 91.24% of sentiments with 0.08  as its best threshold. This shows that the RoBERTa model was better at determining which sentiments are negative and positive.

#### VADER Confusion Matrix
|                       | Predicted Positive Review  | Predicted Negative Review |
|:----------------------|:--------------------------:|:-------------------------:|
| Actual Positive Review|    17149	                 |   17554                   |
| Actual Negative Review|    3928                    |   111369                  |

#### RoBERTa Confusion Matrix
|                       | Predicted Positive Review  | Predicted Negative Review |
|:----------------------|:--------------------------:|:-------------------------:|
| Actual Positive Review|    26808	                 |   7895                   |
| Actual Negative Review|    5241                    |   110056                  |

In VADER's confusion matrix, it correctly predicted 17,149 positive reviews but misclassified 17,554 positive reviews as negative. Similarly, VADER predicted 111,369 negative reviews correctly but misclassified 3,928 negative reviews as positive. These misclassifications led to a lower accuracy. RoBERTa demonstrated better performance in its confusion matrix. It correctly predicted 26,808 positive reviews and 110,056 negative reviews, resulting in a more balanced and accurate prediction.

#### VADER Classification Report
|               | Precision | Recall | F1-Score | Support |
|---------------|-----------|--------|----------|---------|
| 0             | 0.81      | 0.49   | 0.61     | 34,703  |
| 1             | 0.86      | 0.97   | 0.91     | 115,297 |
|---------------|-----------|--------|----------|---------|
| Accuracy      |           |        | 0.86     |150,000  |
| Macro Avg     | 0.84      | 0.73   | 0.76     | 150,000 |
| Weighted Avg  |   0.85    | 0.86    | 0.84    | 150,000 |

#### RoBERTa Classification Report
|               | Precision | Recall | F1-Score | Support |
|---------------|-----------|--------|----------|---------|
| 0             | 0.84      | 0.77   | 0.80     | 34, 703  |
| 1             | 0.93      | 0.95   | 0.94     | 115,297 |
|---------------|-----------|--------|----------|---------|
| Accuracy      |           |        | 0.91     |150,000  |
| Macro Avg     | 0.88      | 0.86   | 0.87     | 150,000 |
| Weighted Avg  |   0.91    | 0.91    | 0.91    | 150,000 |

VADER's classification report shows precision, recall, and F1-Score for both classes (0 and 1). The model has relatively low recall for Class 0 (0.49), suggesting it struggles to identify negative reviews. It achieves a high recall for Class 1 (0.97), indicating its ability to accurately identify positive reviews. RoBERTa's classification report highlights its strengths. It achieves higher precision, recall, and F1-Score for both classes. Particularly, its recall for Class 0 is significantly better (0.77) than VADER's. The macro average and weighted average metrics also reflect RoBERTa's superior performance in achieving balanced results.

### Shopee Sentiment Analysis Report

#### Accuracy
|                   | Best Accuracy  | Best Threshold |
|:------------------|:--------------:|:---------:|
| VADER             |    82.52       |   -0.001   |
| RoBERTa           |    90.93       |   0.11   |

VADER achieved an accuracy of 82.52%, while RoBERTa exhibited superior performance with an accuracy of 90.93%, indicating its better predictive accuracy.

#### VADER Confusion Matrix
|                       | Predicted Positive Review  | Predicted Negative Review |
|:----------------------|:--------------------------:|:-------------------------:|
| Actual Positive Review|    20955		               |   21901                   |
| Actual Negative Review|    4314                    |   102830                  |

#### RoBERTa Confusion Matrix
|                       | Predicted Positive Review  | Predicted Negative Review |
|:----------------------|:--------------------------:|:-------------------------:|
| Actual Positive Review|    35185			             |   7671                    |
| Actual Negative Review|    5929                    |   101215                  |

In VADER's confusion matrix, it correctly predicted 20,955 positive reviews but misclassified 21,901 positive reviews as negative. It accurately predicted 102,830 negative reviews but also misclassified 4,314 negative reviews as positive. These misclassifications contributed to a lower accuracy. RoBERTa's confusion matrix shows a substantial improvement. It correctly predicted 35,185 positive reviews and 101,215 negative reviews, demonstrating more accurate classification in both positive and negative categories.

#### VADER Classification Report
|               | Precision | Recall | F1-Score | Support |
|---------------|-----------|--------|----------|---------|
| 0             | 0.83      | 0.49   | 0.62     | 42856   |
| 1             | 0.82      | 0.96   | 0.89     | 107144  |
|---------------|-----------|--------|----------|---------|
| Accuracy      |           |        | 0.83     |150,000  |
| Macro Avg     | 0.83      | 0.72   | 0.75     | 150,000 |
| Weighted Avg  |   0.83    | 0.83    | 0.81    | 150,000 |

#### RoBERTa Classification Report
|               | Precision | Recall | F1-Score | Support |
|---------------|-----------|--------|----------|---------|
| 0             | 0.86      | 0.82   | 0.84     | 42856   |
| 1             | 0.93      | 0.94   | 0.94     | 107144  |
|---------------|-----------|--------|----------|---------|
| Accuracy      |           |        | 0.91     |150,000  |
| Macro Avg     | 0.89      | 0.88   | 0.89     | 150,000 |
| Weighted Avg  |   0.91    | 0.91   | 0.91     | 150,000 |

RoBERTa's classification report presents its strengths. It demonstrates higher precision, recall, and F1-Score for both classes, particularly an improved recall for Class 0 (0.82) compared to VADER. The macro average and weighted average metrics also reflect RoBERTa's superior performance in achieving balanced results.

In summary, RoBERTa outperforms VADER with a significantly higher accuracy, precision, recall, and F1-Score for both positive and negative classes. RoBERTa's performance in sentiment analysis is superior to VADER, as it achieves better accuracy and a more balanced classification of positive and negative reviews.

### Lazada Topic Modeling Report

#### Negative Topics
| Topic_Num | Topic Name                         | Topic_Perc_Contribution | Keywords                                           |
|-----------|-----------------------------------|-------------------------|---------------------------------------------------|
| 1         | Ad Annoyance and App Performance   | 0.9862                  | ad, good, annoying, pop, open, stop, download, hate, user, price |
| 2         | User Experience and Feature Requests | 0.9705                  | use, product, update, buy, need, try, fix, want, work, time |
| 3         | Order and Delivery Issues          | 0.9814                  | order, bad, service, delivery, customer, time, deliver, cancel, receive, courier |

- In the "Ad Annoyance and App Performance" topic (Topic 0), users express frustration with ad-related issues, app performance, and pop-ups. The presence of keywords like "annoying," "hate," and "stop" suggests a negative sentiment regarding the ad experience and app performance.

- The "User Experience and Feature Requests" topic (Topic 1) highlights users' concerns about the app's usability, the need for updates, and feature requests. Keywords like "need," "fix," and "work" indicate areas where users find room for improvement.

- The "Order and Delivery Issues" topic (Topic 2) revolves around problems related to orders, deliveries, and customer service. Keywords such as "bad," "cancel," and "courier" imply negative experiences with these aspects of the service.

#### Positive Topics
| Topic_Num | Topic Name                     | Topic_Perc_Contribution | Keywords                                             |
|-----------|--------------------------------|-------------------------|-----------------------------------------------------|
| 1         | User Satisfaction              | 0.9718                  | good, love, nice, service, buy, quality, satisfied, need, experience, customer |
| 2         | Accessibility and Convenience   | 0.9779                  | thank, product, order, time, price, convenient, 😊, useful, want, far |
| 3         | User-Friendly Experience        | 0.9684                  | easy, great, use, shop, delivery, fast, find, 🏻, cool, friendly |

- In the "User Satisfaction" topic (Topic 0), users express satisfaction with various aspects of the app, including service quality, buying experience, and customer satisfaction. Keywords like "good," "love," and "nice" suggest a positive sentiment in this context.

- The "Accessibility and Convenience" topic (Topic 1) highlights users' appreciation for the app's accessibility, convenience, and affordability. Keywords like "thank," "useful," and "😊" indicate a positive sentiment related to these factors.

- The "User-Friendly Experience" topic (Topic 2) revolves around positive experiences with the app, emphasizing its user-friendliness, delivery speed, and overall ease of use. Keywords such as "easy," "great," and "friendly" suggest a positive sentiment toward these aspects.


### Shopee Topic Modeling Report

#### Negative Topics
| Topic_Num | Topic Name                         | Topic_Perc_Contribution | Keywords                                           |
|-----------|-----------------------------------|-------------------------|---------------------------------------------------|
| 1         | Account and Payment Inconvenience  | 0.9754                  | use, good, time, account, pay, option, star, buy, shop, change |
| 2         | Delivery and Service Issue         | 0.9824                  | order, service, delivery, customer, bad, time, receive, poor, refund, parcel |
| 3         | App Problems and Product Concerns   | 0.9853                  | update, fix, product, work, try, phone, add, problem, ad, review |

- The "Account and Payment Inconvenience" topic, with a topic contribution of 0.9754, highlights user concerns regarding account management and payment-related issues. Keywords like "time," "account," and "pay" indicate difficulties users face in these areas. Interestingly, the presence of terms like "good" and "option" suggests that users may have experienced both positive and negative aspects related to account and payment management.

- The "Delivery and Service Issue" topic, with a substantial topic contribution of 0.9824, revolves around problems users encounter in the context of delivery and customer service. Keywords such as "bad," "receive," and "refund" reflect negative experiences with the service. Users express dissatisfaction with delivery times and service quality, making it a prominent topic with clear areas for improvement.

- In the "App Problems and Product Concerns" topic, with a topic contribution of 0.9853, users express challenges related to app functionality and product issues. Keywords like "update," "problem," and "ad" indicate frustration with app-related problems and concerns about product quality. Users may have encountered issues with app updates, functionality, and advertising, contributing to a negative sentiment.

#### Positive Topics
| Topic_Num | Topic Name                                    | Topic_Perc_Contribution | Keywords                                           |
|-----------|----------------------------------------------|-------------------------|---------------------------------------------------|
| 1         | Accessability and Convenience                  | 0.9654                  | thank, easy, use, shop, product, buy, need, convenient, lot, want |
| 2         | Positive Ordering and Delivery Experience     | 0.9880                  | love, order, delivery, happy, fast, time, enjoy, star, deliver, hope |
| 3         | Satisfaction with Quality and Service         | 0.9608                  | good, nice, great, satisfied, quality, service, 😊, experience, useful, customer |

- The "Accessability and Convenience" topic, with a topic contribution of 0.9654, is characterized by positive sentiments related to accessibility and convenience. Users express gratitude and appreciation ("thank," "easy") for the ease of use, shopping, and product selection. The presence of keywords like "convenient" and "lot" suggests that users find the platform convenient and versatile for their needs.

- The "Positive Ordering and Delivery Experience" topic stands out with a high topic contribution of 0.9880. Users in this topic express delight and satisfaction with the ordering and delivery process, using words like "love," "happy," and "enjoy." Keywords like "fast," "deliver," and "hope" reflect a positive sentiment, indicating a seamless and pleasant experience with ordering and receiving products.

- In the "Satisfaction with Quality and Service" topic, users express contentment and approval of the quality and service provided. Keywords like "good," "nice," and "great" indicate a high level of satisfaction. Users also use emoticons like "😊" to express their positive feelings. This topic contributes significantly to the overall sentiment, with a topic contribution of 0.9608.


**Conclusion**

The analysis of user sentiment and feedback on the platform reveals valuable insights into the user experience and satisfaction. The sentiment analysis models, particularly RoBERTa, demonstrate superior accuracy and sentiment classification compared to VADER. User feedback is a valuable resource for identifying areas of improvement, as negative topics pinpoint specific pain points that need attention. Additionally, positive topics highlight areas of strength and positive user experiences that should be maintained and promoted.

The platforms can leverage these insights to focus on addressing negative sentiments and enhancing positive aspects to provide an overall improved user experience. By continuously monitoring and analyzing user sentiment, the platform can adapt to changing user needs and preferences, ultimately leading to increased user satisfaction and loyalty.

All in all, both Lazada and Shopee as e-commerce platforms has its strengths and weaknesses and doing online shopping on one of these platforms is a matter of preferences or based on which gives the better experience.

Further analysis can be made by improving some of the stages performed. During data cleaning and preprocessing langdetect was used to filter only english reviews in the data however there are instances of reviews being in "Taglish" or Tagalog mixed with English this means that if the review starts of with english and ends in Tagalog it will not be removed. The reason for using langdetect is for ease of use for filtering although its language detection accuracy may be affected because of length of review see [FAQ](Resources/langdetect%20FAQ.png). Making a purely Tagalog/Filipino sentiment analysis could be another way of specifically targeting the Philippine buyer demographic.



## Recommendations<a name="recommendations"></a>

To relieve some of the pain points that customers had encountered in their corresponding platforms here are some actions that could be taken:

#### Lazada
**Ad Annoyance and App Performance**
- Optimize ad placement to reduce annoyance.
- Improve app performance, addressing slow loading and pop-up issues.
- Collect and act on user feedback to target improvements.

**User Experience and Feature Requests**
- Regularly update the app to fix bugs and enhance user experience.
- Consider implementing user-requested features aligned with app goals.
- Focus on usability improvements and pain point identification.

**Order and Delivery Issues**
- Strengthen customer support to address order and delivery problems.
- Collaborate with couriers to ensure reliable deliveries.
- Enhance real-time order tracking for transparent delivery.

#### Shopee
**Account and Payment Inconvenience**
- Address user concerns about account and payment difficulties.
- Enhance account management options and payment processes.
- Investigate both positive and negative aspects of account and payment experiences.

**Delivery and Service Issues**
- Improve delivery and customer service to address user dissatisfaction.
- Focus on resolving issues related to delivery times, service quality, and customer refunds.
- Prioritize enhancing the overall service experience.

**App Problems and Product Concerns**
- Resolve app-related problems and address concerns about product quality.
- Concentrate on app functionality, updates, and the impact of advertising on users.
- Work to improve the app and product experience for users.

Identifying and addressing customer pain points on e-commerce platforms like Lazada and Shopee is crucial. It directly impacts user satisfaction and the success of these platforms. By tackling issues like ad annoyance, app performance, order and delivery problems (Lazada), and account inconveniences, delivery issues, and app functionality (Shopee), both platforms can significantly improve the user experience. Monitoring user feedback and taking prompt actions to resolve these concerns not only enhance user satisfaction but also contribute to their continued growth and competitiveness in the e-commerce market.


## Dashboards<a name="dashboards"></a>

<p align="center">
  <img src="Resources\Lazada Dashboard2.png" alt="vs" width="650">
</p>

<p align="center">
  <img src="Resources\Shopee Dashboard2.png" alt="vs" width="650">
</p>

