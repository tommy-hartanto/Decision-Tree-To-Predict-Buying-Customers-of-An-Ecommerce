# Decision Tree To Predict Buying Customers of An Ecommerce
personal project by Tommy Hartanto
data source: https://www.kaggle.com/datasets/imakash3011/online-shoppers-purchasing-intention-dataset

## Background
An online store (let's name it KagStore) has 12,330 sessions worth of data. Each session represents a customer's behavior on the web page over a year.

Typical online stores have lots of people visiting. Some of them leave as they go along each step of the customer journey, and in the end only few of them end up buying. KagStore is no exception.

The upside of this industry is they have virtually zero variable cost to reach hundreds of thousands of people everyday. Thus broadcasting is not an issue. However, with the advent of personalized marketing (treating different people differently) embraced by everyone in the marketplace, KagStore was somehow triggered to do the same.

With me at their disposal, I developed a machine learning to suggest which visitor is going to buy - and not going to buy. So KagStore can send more relevant message to identified visitors to improve conversion rate.

## Goal & Objective
Goal: Automatically predict a visitor's probability to buy.
Objective: Create a classification model with as minimum False Negative as possible.
Metrics: Recall & ROC-AUC score.
Additional note: Because losing a more prospective buyer (False Negative) is more disastrous than losing a less prospective one (False Positive), I attempt to avoid wrongly labeling the prospective buyer as possible (Recall) - without completely ignoring the other one (ROC-AUC).

## Data Overview
See the file named 'Data Analysis'

## Data Pre-Processing

### Data Cleaning

#### Null Values
There is no null value.

#### Duplicated Data
There are 125 duplicated data. All of them are succesfully removed.

#### Multicolinearity
These 5 features are removed due to medium to high correlation with others, worse data distribution and less contextual value:
- ProductRelated_Duration
- Administrative_Duration
- Informational_Duration
- BounceRates
- PageValues

#### Outlier Handling
These 2 numerical features have their outliers removed:
- ProductRelated
- ExitRates

Both of them are transformed with Cube Root before filtered by 3x z-score.

#### Scaling
These 2 features are scaled with StandardScaler:
- ProductRelated
- ExitRates

These 3 features are scaled with MinMaxScaler:
- Administrative
- Informational
- Special Day

#### Encoding
These 3 features are label encoded:
- Month
- Weekend
- Revenue

These 1 feature is One-Hot encoded:
- VisitorType

## Machine Learning Modeling
With abundant algorithms available to use, I pick Decision Tree for its lightweight-processing and understandability to get useful insights.

### Model Fitting & Hyperparameter Tuning
After several iterations of tuning parameters, the best paramaters I used are these:
- random_state=33
- class_weight={0:0.15,1:0.85}
- max_depth=3
- max_features='sqrt'
- min_samples_leaf=156
- min_samples_split=66

Threshold set at 0.2.

The score now is:
- Recall:
  - Training score: 99.9%
  - Testing score:99.3
- ROC-AUC:
  - Training score: 55.8%
  - Testing score: 55.6%

### Feature Importance
![image](https://user-images.githubusercontent.com/107762612/179357184-c25245a7-1891-4ce1-9e76-4e644e415b88.png)

I used Feature Importance to see if deleting one or some of the bottom of the list may improve the model performance.
After experimenting, I decided to remove 2 features:
- VisitorType_Returning_Visitor
- VisitorType_Other'

The recall score is reduced a little by 1.2%, the ROC_AUC score improved considerably better by 5%.

This is the revised Feature Importance:

![image](https://user-images.githubusercontent.com/107762612/179357170-46a651b2-fa72-47d7-91e5-1f4f3513a568.png)

### Decision Tree Visualized
![image](https://user-images.githubusercontent.com/107762612/179357219-07b5a696-b8dd-4286-8fba-f87cd4ca4037.png)

### Final Model Score
- Recall:
  - Training score: 99.0%
  - Testing score:98.1
- ROC-AUC:
  - Training score: 60.7%
  - Testing score: 60.6%

### Insights & Recommendations
- 
