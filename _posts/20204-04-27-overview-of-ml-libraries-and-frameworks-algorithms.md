---
title: "Overview of Machine Learning Libraries, Frameworks, Techniques and Over/Under Fitting"
date: 2024-04-27
categories: DeepLearning
---


# Overview of Machine Learning Libraries and Frameworks
 1. Scikit-learn:
     - Good for small to medium datasets and supports models like regression, classification, 
       clustering, and dimensionality reduction.
     - Not usually preferred for large data sets or deep learning tasks
     ``` python
        from sklearn.ensemble import RandomForestClassifier
        from sklearn.datasets import load_iris
        from sklearn.model_selection import train_test_split
        from sklearn.metrics import accuracy_score

        # Load dataset
        data = load_iris()
        X = data.data
        y = data.target

        # Split dataset
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

        # Train model
        model = RandomForestClassifier(n_estimators=100)
        model.fit(X_train, y_train)

        # Predict and evaluate
        predictions = model.predict(X_test)
        print("Accuracy:", accuracy_score(y_test, predictions))
     ```
     
 2. TensorFlow and Keras:
     - Used in high-performance numerical computation and deep learning tasks.
     - Supports GPUs

 3. PyTorch:  
     - Alternative of Tensorflow and used in high-performance numerical computation and deep 
       learning tasks.
     - [Data Preprocessing with Pytorch](https://05satyam.github.io/pytorch/2024/03/25/data-preprocessing-using-pytorch.html)
      - [Data Manipulation with Pytorch](https://05satyam.github.io/pytorch/2024/03/25/data-manipulation-using-pytorch.html)

 4. XGBoost: Extreme Gradient Boosting
     - Optimized distributed gradient boosting library.
     - Supports both classification and regression.
     - Supports regularization to prevent overfitting.
     ```python
        import xgboost as xgb
        from sklearn.model_selection import train_test_split
        from sklearn.metrics import mean_squared_error

        # Load data and create DMatrix
        data = xgb.DMatrix(data=X, label=y)

        # Split data
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25)

        # Set parameters
        params = {
            "max_depth": 10,
            "eta": 0.1,
            "objective": "reg:squarederror"
        }

        # Train model
        model = xgb.train(params, xgb.DMatrix(X_train, label=y_train), num_boost_round=10)

        # Predict and evaluate
        predictions = model.predict(xgb.DMatrix(X_test))
        print("RMSE:", mean_squared_error(y_test, predictions, squared=False))

     ```
 5. LightGBM:
     - It is a gradient boosting framework and uses tree based algorithms.
     - It is designed for distributed and efficient training for large datasets.
     - Supports categorical features.
     ```python
        import lightgbm as lgb
        from sklearn.model_selection import train_test_split
        from sklearn.metrics import accuracy_score

        # Load and prepare data
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

        # Create dataset for LightGBM
        train_data = lgb.Dataset(X_train, label=y_train)
        test_data = lgb.Dataset(X_test, label=y_test, reference=train_data)

        # Parameters
        params = { 
            'objective': 'binary', #objective function
            'metric': 'binary_logloss', # evaluation metrics
            'num_leaves': 31, 
            'learning_rate': 0.05
        }

        # Train model
        gbm = lgb.train(params, train_data, num_boost_round=100, valid_sets=[test_data])

        # Predict and evaluate
        predictions = gbm.predict(X_test)
        predicted_classes = [1 if prob > 0.5 else 0 for prob in predictions]
        print("Accuracy:", accuracy_score(y_test, predicted_classes))

     ```

# Classification and Regression Techniques
 ## Classification: 
     - Linear Models: Logistic Regression, Support Vector Machines (SVM).
     - Tree-Based Models: Decision Trees, Random Forest, Gradient Boosting Machines (GBM), XGBoost,
                          LightGBM.
     - Neural Networks: MLP (Multi-Layer Perceptrons), CNNs (Convolutional Neural Networks), RNNs  
                        (Recurrent Neural Networks).
     - Neighbors-Based: K-Nearest Neighbors (KNN).

 ## Regression:
     - Linear Models: Linear Regression, Ridge, Lasso
     - Tree-Based Models: Regression Trees, Random Forest Regression, XGBoost Regression, LightGBM 
                          Regression.
     - Neural Networks: Same as in `Classification`, but with different output layer configurations
                      Support Vector Regression (SVR).

# Handling Overfitting and Underfitting
 ## Overfitting:
     - It happens when a model learns the detail and noise in the training data to the extent that 
       it negatively impacts the performance of the model on new data.

 ### Strategies to Handle Overfitting:
     - Regularization: Techniques like L1, L2 and Elastic Net regularization regularization are 
                       commonly used in linear and logistic regression.
     - Pruning: Reducing the depth of the tree(Tree-based models).
     - Cross-validation: Using techniques like k-fold cross-validation{dividing the data into 
                         multiple parts}.
     - Ensemble Methods: Techniques like bagging and boosting reduce variance and bias.
     - Dropout: Used in neural networks to randomly drops out a certain number of neurons from the 
                network during each iteration of training.

 ## Underfitting:
     - It happens when a model is too simple to learn the underlying pattern of the data and fails 
       to capture the underlying trend.

 ### Strategies to Handle Underfitting:
     -  Increasing Model Complexity, Feature Engineering, Decreasing Regularization etc.


#### Note:
     -  Choosing the right model and techniques depends greatly on the nature of the data and the specific requirements of the application. Always start with simple!!!