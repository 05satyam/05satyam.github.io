---
title: "Regularization in ML Models"
date: 2024-04-27
categories: DeepLearning
---

# Regularization
 - It is used to prevent Overfitting of ML models.
 - Overfitting is related to high variance.
     - High Variance means: Model's sensitivity to small fluctuations in the training dataset.
                            if you slightly change the training data=> the predictions change significantly.
     - Bias-Variance Tradeoff: Tradeoff between the error introduced by `bias` and the error 
                               introduced by `variance`.
                               - High bias can cause a model to miss relevant relations between features and target outputs (underfitting).
                               - High variance can cause a model to model random noise in the training data, rather than the intended outputs (overfitting).

      
## Criteria to use? 
 - Usually preffered to use if you see big difference in performance of model outcomes during 
    training and testing phase.
 
## Types:
 - L1, L2 and Elastic Net regularization {defined by us during model creation}
 - Model parameters, Dropout, early stoping etc {implicit by the model as hyper parameters}

### L1(Lasso) Regularization:
 -  Adds a penalty equal to the absolute value of the magnitude of coefficients/weights.
  ```python
     L = L0 + λ∑∣wi∣

     L0 = original loss function
     wi = model coefficients.
     λ  = regularization parameter hence, controls the strength of the penalty.
    ``` 
 - L1 can yield sparse models where some coefficient weights are exactly zero. 
 - L1 is useful for feature selection, effectively determining which features are important 
       for the prediction.
 - Usage: 
     - Use L1 if you need a sparse model, where feature selection is important and only a 
        subset of features are meaningful.

### L2(Ridge) Regularization: 
 - Adds a penalty equal to the square of the magnitude of coefficients/weights. 
    ```python
     L = L0 + λ∑w<sub>i</sub><sup>2</sup>

     The terms are the same as those defined for L1. 
   ```
 - L2 does not zero out coefficients. 
 - L2 encourages coefficients to be small.
 - All features get to contribute, albeit modestly, to the model.
 - L2 give better prediction results and is robust against outliers and multicollinearity.
 - Usage:
     - Use L2 when you expect that many small or medium effects are important.
     - L2 can perform better when `multicollinearity` is present.
     - L2 tends to have better prediction performance due to its ability to shrink 
        coefficients evenly.

### Elastic Net regularization:
 - Combination of both L1 and L2 regularization. 
 - For example:
    ```python
         L = L0 + λ∑∣wi∣ +  (1-λ)∑w<sub>i</sub><sup>2</sup>
    ```
 - Usage:
     - It is a good choice when you have correlated features and you want to balance the feature selection and overfitting prevention or not sure which one to use among L1 and L2 initially.

### Model parameters:
 - Adjusting hyperparameters given by respective model.
 - For example: model_depth in tree models, colsample_bytree in xgboost etc. or in neural 
    nets  number of layers and the number of neurons per layer and many more...

### Dropout regularization:
 - Randomly droping out a certain number of neurons from the network during each iteration of 
    training  and let model train on several subsets of data to lean and perform.
 - Adjust dropout parameters while design models.
 
### Early Stopping:
 - Using validation dataset to observe model performance while training and then stopping in between to before a  degradation in performance stars.
 - For example:  
     - In LightGBM, Xgboost set : early_stopping_rounds
     - In LLM, setup `Early Stopping Callback Function` 


#### Outliers:
 - Data points that deviate significantly from other observations.
 - Outliers can skew and mislead the training process.
 - To handle outliers: 
     - Transformation: Use transformation like logarithmic, square root, etc.
     - Remove outliers from the dataset if not needed.
     - Visualize: Visual methods like box plots, or clustering methods to identify outliers.

#### Multicollinearity:
 - Two or more predictor variables in a regression model are highly correlated.
 - To handle:
     - Use Regularization.
     - Visualization : Plot correlation for each parameters.



### Example Implementation:
  
#### Logistic Regression with L1 and L2:
 ```python
     from sklearn.linear_model import LogisticRegression
     from sklearn.model_selection import train_test_split

     # Sample data
     X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

     # L1 Regularization
     model_l1 = LogisticRegression(penalty='l1', solver='liblinear')
     model_l1.fit(X_train, y_train)

     # L2 Regularization
     model_l2 = LogisticRegression(penalty='l2', solver='liblinear')
     model_l2.fit(X_train, y_train)

 ```
   
#### XGBoost:
 ```python
     import xgboost as xgb
     # Create DMatrix
     d_train = xgb.DMatrix(X_train, label=y_train)

     # Set parameters for L1 and L2
     params = {
      'alpha': 1.0,   # L1 regularization term on weights
      'lambda': 1.0,  # L2 regularization term on weights
      'objective': 'reg:squarederror'
     }

     # Train model
     model = xgboost.train(params, d_train, num_boost_round=10)

 ```

#### LighBGM:
 ```python
     import lightgbm as lgb
     # Create dataset for LightGBM
     d_train = lgb.Dataset(X_train, label=y_train)

     # Set parameters for L1 and L2
     params = {
      'objective': 'binary',
      'lambda_l1': 0.5,  # L1 regularization
      'lambda_l2': 0.5,  # L2 regularization
     }

     # Train model
     gbm = lgb.train(params, d_train, num_boost_round=100)

 ```
    
#### Large Language Model(Trainer from Hugging face): from HF documentation
 ```python
            
     from transformers import TrainerCallback, TrainerControl, TrainerState
            
     # This class monitors the evaluation loss after each evaluation step.
     class EarlyStopping(TrainerCallback):
      def __init__(self, early_stopping_patience: int = 3, early_stopping_threshold: float = 0.0):
      self.early_stopping_patience = early_stopping_patience
      self.early_stopping_threshold = early_stopping_threshold
      self.best_metric = None
      self.patience_counter = 0

     # called after each evaluation
      def on_evaluate(self, args, state: TrainerState, control: TrainerControl, **kwargs):
      # Retrieve the evaluation metric from the state
       eval_metric = state.log_history[-1]['eval_loss']

       # Check if it's the best metric we've seen so far
       if self.best_metric is None or eval_metric < self.best_metric - self.early_stopping_threshold:
        self.best_metric = eval_metric
        self.patience_counter = 0
       else:
        self.patience_counter += 1

       # Check if we should stop training
       if self.patience_counter >= self.early_stopping_patience:
        print("No improvement in metric for", self.early_stopping_patience, "evaluation steps. Stopping training...")
        control.should_training_stop = True

     from transformers import Trainer, TrainingArguments, AutoModelForSequenceClassification

     model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)
     training_args = TrainingArguments(
      output_dir='./',
      evaluation_strategy="epoch",
      save_strategy="epoch",
      num_train_epochs=100,
      per_device_train_batch_size=4,
      per_device_eval_batch_size=2
     )

     trainer = Trainer(
      model=model,
      args=training_args,
      train_dataset=train_dataset,  
      eval_dataset=eval_dataset,    
      callbacks=[EarlyStopping(early_stopping_patience=3)]
     )

     trainer.train()
    
 ```  