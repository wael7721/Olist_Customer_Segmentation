# Olist Customer Segmentation using K-means Clustering

> This project uses K-means to segment customers based on their location to assist the marketing team to make decisions determined by the data analysis deduced. The choice of numbers of cluster was made using evaluation metrics that will help us achieve a better understanding of our customers.

## Table of Contents
1. [Business Understanding](#1-business-understanding)
2. [Data Understanding](#2-data-understanding)
3. [Data Preparation and Pre-processing](#3-data-preparation-and-pre-processing)
4. [Modeling](#4-modeling)
5. [Evaluation](#5-evaluation)
6. [Insights, Conclusions and Business Recommendations](#6-insights-conclusions-and-business-recommendations)

---

## 1. Business Understanding

### 1.1 Project Description

Olist is a Brazilian e-commerce founded in 2015 platform that operates as a marketplace connecting businesses to an online store, to allow sellers to place their products on the market and reach an expanded customer base.

The platform is known for its focus on enabling smaller businesses to stand out in the competitive e-commerce landscape by providing tools and services that simplify the selling process. Olist has become one of the most improving players in the e-commerce industry.

However, Olist has a diverse customer base with varying preferences, behaviors, and needs. The company currently lacks a granular understanding of its customer segments, making it difficult to tailor marketing efforts, product recommendations, and customer service to individual customer needs. As a result, Olist faces several operational and marketing challenges.

By addressing this business problem through improved customer segmentation, Olist can enhance its competitive position and provide a superior shopping experience to its customers, ultimately driving higher revenue and profitability in the Brazilian e-commerce market.

### 1.2 Project Objectives

With the substantial growth of Olist in the competitive E-commerce landscape, the need to identify its users has become vital to keep up with its competitors.

To continue with the rising trajectory of Olist and assisting its users to find their needs, Olist needs to determine its users preferences, behaviour and pattern based on its collected data to be able to provide personalized product recommendations.

Utilizing the features provided, we can segment customers based on location to provide a better customized experience and determine the most purchased and least purchased products, determine number of customers in each cluster and help the **Marketing team** make a decision that will help Olist's growth and customer satisfaction.

**Project Goals:**
- Segment customers based on location to provide a customized experience and recommend them tailored products.
- Identify weak categories that have extremely low purchase rate and make the marketing team take a decision.
- Optimize product distribution for the sellers to move to for less time delays of delivery.

*Note: While the third goal may not be a recommendation for a marketing team, it will also be achieved directly through our interpretation of the two former goals.*

---

## 2. Data Understanding

### 2.1 Importing Data

To be able to interpret the data we need to first understand it by knowing its type (nominal or ordinal) and identify key characteristics.

The following Olist data was provided from [Kaggle.com](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), which is a popular online platform for data science competitions, machine learning projects, and data analytics.

**Datasets Used:**
- customers.csv
- geolocation.csv
- orderItems.csv
- payment.csv
- orderReviews.csv
- orders.csv
- products.csv
- sellers.csv
- productCategoryName.csv

### 2.2 Data Exploration

Olist has provided multiple datasets publicly to be analysed and conclude ways to improve its targeted user recommendation.

#### Customers Dataset
This dataset contains information about the customers such as orders, location and zip code which will help us classify users based on location.

#### Geolocation Dataset
The geolocation dataset contains the location of Olist users with latitude and longitude values. This may cause security concerns, however Olist has already taken into account users' safety and removed the total user zipcode and kept only a prefix.

#### Order Items Dataset
This dataset contains details of orders users and the prices of the order and can be traced to its user using order_id.

#### Order Payment Dataset
Order payments contains details of how orders are paid and the method of payment.

#### Order Reviews Dataset
The order reviews dataset contains comments and ratings given by users and the date of review.

#### Order Dataset
Contains general information on the order such as date of request and the items.

#### Products Dataset
Products dataset delivers information on the products such as weight, size and its category.

#### Sellers Dataset
The sellers dataset provide information on the sellers of the products.

#### Category Name Translation Dataset
Because Olist is a Brazilian e-commerce company, it provides its category details in Portuguese. Olist has provided a translation for all its categories in this dataset.

---

## 3. Data Preparation and Pre-processing

### 3.1 Data Cleaning

The geolocation data was cleaned by:
- Removing duplicate latitude-longitude pairs
- Keeping only essential columns: `geolocation_lat`, `geolocation_lng`, and `geolocation_zip_code_prefix`
- Preparing data for clustering with latitude and longitude coordinates

### 3.2 Data Visualization

Initial clustering visualization was performed to understand the geographic distribution of data points across different cluster counts (3-6 clusters).

### 3.3 Data Reduction & Data Transformation

The following transformations were applied:

1. **Merged customer and geolocation dataframes** using zip code prefix as the join key to create `geo_customer` dataframe
2. **Merged with orders dataframe** to determine the orders that each cluster made
3. **Merged with order items** to obtain product IDs
4. **Merged with products** to obtain product categories
5. **Translated product categories** from Portuguese to English using the category translation dataset

---

## 4. Modeling

### K-Means Clustering

K-Means clustering was applied to segment customers based on geographic location (latitude and longitude).

**Optimal Cluster Selection:**
- Tested cluster numbers ranging from 2 to 11
- Evaluated using the **Elbow Method** to identify the "elbow point" where inertia decrease slows
- **Final choice: 4 clusters** - identified as optimal based on evaluation metrics

**Model Parameters:**
- Algorithm: Lloyd's K-Means with K-Means++ initialization
- Random state: 42 (for reproducibility)
- Number of initializations: 10

---

## 5. Evaluation

### Evaluation Metrics

#### Inertia
Inertia quantifies how internally coherent the clusters are. It **measures how close the data points are to their respective cluster centers**.

A good model is one with low inertia AND a low number of clusters.

**Formula:** $$\sum_{i=1}^n (X_i - C_k)^2$$

- n = total number of data points
- $C_k$ = centroid of the cluster X is assigned to

**Average Inertia (used in analysis):** $$\frac{\sum_{i=1}^n (X_i - C_k)^2}{K}$$

Where K = number of clusters

#### Silhouette Score
The silhouette score is a metric used in K-means with values ranging between -1 and 1 that determines how distinct the clusters are from each other.

- **Score near +1:** The sample is far away from neighboring clusters (well-defined cluster)
- **Score near 0:** The sample is on or very close to the decision boundary between two clusters
- **Score near -1:** The sample might have been assigned to the wrong cluster

#### Number of Clusters
The cluster number itself is also an evaluation metric because choosing a high number of clusters would result in **overfitting**, where the model captures noise and variability in the data rather than meaningful patterns.

#### Model Selection Criteria
**A good model has:** Low inertia + Low cluster number + High silhouette score

**Decision:** We selected **4 clusters** as it is one of the optimal choices according to the elbow method, cluster number, and silhouette score.

---

## 6. Insights, Conclusions and Business Recommendations

### Cluster Analysis Results

Through geographic segmentation and product category analysis, the following insights were deduced:

#### **Cluster 0: Bottom of Brazil**
- **Customer Count:** ~15,000 customers
- **Top 5 Categories to Keep Recommending:**
  1. Bed, Bath & Table
  2. Sports & Leisure
  3. Furniture & Décor
  4. Health & Beauty
  5. Computer Accessories
- **Least Bought Categories (Areas for Improvement):**
  1. Security and Services
  2. Arts and Craftmanship
  3. Fashion Children Clothes
  4. Home Comfort
  5. CDs, DVDs and Musicals

#### **Cluster 1: Heart of Brazil**
- **Customer Count:** ~70,000 customers (largest segment)
- **Top 5 Categories to Keep Recommending:**
  1. Bed, Bath & Table
  2. Health & Beauty
  3. Sports & Leisure
  4. Computer Accessories
  5. Furniture & Décor
- **Least Bought Categories (Areas for Improvement):**
  1. Security and Services
  2. Fashion Children Clothes
  3. CDs, DVDs and Musicals
  4. Kitchen Items
  5. Fashion Sports

#### **Cluster 2: Top Left of Brazil**
- **Customer Count:** ~9,130 customers
- **Top 5 Categories to Keep Recommending:**
  1. Health & Beauty
  2. Watches and Gifts
  3. Sports & Leisure
  4. Telephones
  5. Computer Accessories
- **Least Bought Categories (Areas for Improvement):**
  1. Security and Services
  2. Kitchen Items
  3. Arts and Craftmanship
  4. CDs, DVDs and Musicals
  5. Fashion Children Clothes

#### **Cluster 3: Top Right of Brazil**
- **Customer Count:** ~2,892 customers (smallest segment)
- **Top 5 Categories to Keep Recommending:**
  1. Health & Beauty
  2. Sports & Leisure
  3. Telephones
  4. Computer Accessories
  5. Watches and Gifts
- **Least Bought Categories (Areas for Improvement):**
  1. Arts and Craftmanship
  2. CDs, DVDs and Musicals
  3. Kitchen Items
  4. Security and Services
  5. Fashion Children Clothes

---

## Key Recommendations for the Marketing Team

1. **Cluster-Specific Product Recommendations:** Use the top categories for each cluster to customize product recommendations and marketing campaigns based on geographic location.

2. **Address Weak Categories:** Implement targeted marketing strategies for underperforming categories across all clusters, particularly:
   - Security and Services (weak across all clusters)
   - Fashion Children Clothes (weak across all clusters)
   - CDs, DVDs and Musicals (weak across all clusters)

3. **Geographic Targeting:** Focus marketing efforts on Cluster 1 (Heart of Brazil) with 70,000 customers for maximum impact, while developing specific strategies for smaller regional clusters.

4. **Inventory Optimization:** Optimize product distribution for sellers based on regional preferences to reduce delivery times and improve customer satisfaction.

5. **Regional Customization:** Tailor the e-commerce experience, product visibility, and recommendations based on the unique preferences of each geographic cluster.

---

## Project Files

- `last.ipynb` - Complete Jupyter notebook with all analysis, visualizations, and code
- `README.md` - This file, containing project summary and findings
- `Data/` - Directory containing all CSV datasets used in the analysis

---

## Technology Stack

- **Python 3.x**
- **Libraries:**
  - Pandas (Data manipulation)
  - Scikit-learn (Machine Learning - K-Means clustering)
  - Matplotlib (Data visualization)
  - NumPy (Numerical computations)

---

## Authors

Team 10 - Olist Customer Segmentation Project

---

**Last Updated:** December 2025
