---
title: "ML-Model design steps(starting-2-end) with simple Linear Regression"
date: 2024-04-21
categories: DeepLearning
---

# Step1 : Problem statement  : 
 - What is the Objective?
 - What is the expected output? for example: Binary variable (example: 0 = "first prediction", 1 = "second prediction").

# Step2 : Data Collection : 
 - Ensure all the data is collected from all the possible sources, data can be strcuctured, unstructured, semi-structured or all.

# Step3 : Data Preprocessing :
 - 3.1 Data Cleaning:
  -- Handling missing values :
    --- It depends on the type of data. For example: For numerical type column one could choose mean/median/mode depending 
    on the requirements.
   --- Or if it is string type data, one can fill it with N/A, unknown etc or data can be dropped.
   --- This preprocessing usually depends on the requirements.

  -- Removing duplicates: Ensure that there are no duplicates in the dataset, to avoid skewness of the results.
 
 - 3.2 Data Transformation
  -- Feature Engineering : This step involves (if needed) create new features that could help in the prediction.
  -- Normalization/Standardization: Scale the data if you're using models that are sensitive to the magnitude of input values.
 
 - 3.3 Handling Dates(if needed) : Data parsing should be correct, consistent and extract useful information from dates.

 - 3.4 Encoding Categorical Data: 
  Convert categorical variables into a form that can be provided to ML models by using one-hot encoding, label encoding, or using embedding layers for deep learning models.
   
   -- 3.4.1 : Label Encoding : It is usually prefered when the categorical feature is ordinal (the categories have a meaningful order or rank).
    ```python
        from sklearn.preprocessing import LabelEncoder
        encoder = LabelEncoder()
        # encoder.fit_transform(...)
    ```
   -- 3.4.2 :  One-Hot Encoding with sklearn : It is usually preferred when you are implementing a pipeline or need to handle unseen data during 
              model deployment. Similar to `pd.get_dummies()`
     ```python
            from sklearn.preprocessing import OneHotEncoder
            encoder = OneHotEncoder(drop='first', sparse=False)
            # encoder.fit_transform(...)
     ```
   -- 3.4.3 :  Hashing : Preferred when number of unique categories is large and dimentionality reduction is needed.
     ```python
            from sklearn.feature_extraction import FeatureHasher
            hasher = FeatureHasher(n_features=6, input_type='string')
            # hasher.transform(.....)
     ```
   -- 3.4.4 : Category Encoders : Advanced encoding techniques for categorical variables
     ```python
            import category_encoders as ce
            encoder = ce.BinaryEncoder(cols=['month', 'day_of_week'])
            # encoder.fit_transform(df)
     ```


# Step4 :  Feature Selection : 
 - 4.1 Correlation Analysis: Remove highly correlated features to reduce multicollinearity.
 - 4.2 Importance Metrics: Utilize model-based metrics like feature importance or etc.

# Step5 : Data Partitioning : 
 Split the data into training, validation, and test sets.

# Step6 : Developing a Baseline Model :
 Start with a simple model to establish a baseline performance for example Logistic regression or a basic decision tree.

# Step7 : Model Training and Evaluation : 
 - Train your model on the training set and evaluate it using the validation set.
 - Adjust parameters, as needed, and consider upgrading model based on the parameters by fine-tuninig or model enhancement.

# Step8 : Final Evaluation and Deployment!!!

## Note: The key in machine learning, particularly for predictions like this, is iteration. So, Steps(3-8) can be repeated until suitable performance is achieved. And, these steps depends on the problem statements.

---

## [.ipynb Notebook](https://github.com/05satyam/blogs/blob/main/PredictionModelDesignWithStepAndExample.ipynb)
