# Introduction to MLOps-
- MLOps stands for Machine Learning Operations.
- MLOps is a combination of machine learning, software engineering, DevOps, automation, infrastructure, and operational practices used to take machine learning models from development into production and keep them reliable over time.
- A data scientist can build a model that works well on a dataset, but that does not automatically mean the model is ready for real-world use.
- MLOps is the practice of managing the complete lifecycle of machine learning systems from development through deployment, monitoring, maintenance, and continuous improvement.

# Why is MLOps Required?
- Traditional software generally follows:
        
        Code
         ↓
        Build
         ↓
        Test
         ↓
        Deploy
         ↓
        Monitor

- Machine learning systems are more complicated because their behavior depends not only on code but also on data and models.
- An ML system can be represented as:

        Code + Data + Model + Infrastructure

- For example, suppose we create a house-price prediction model.
- The model may work correctly today:
- But after several years, house prices and customer behavior may change.
- The same input patterns may no longer produce the same outcomes.
- Therefore:

      New Data
         ↓
      Model Performance Changes
         ↓
      Monitoring
         ↓
      Retraining
         ↓
      New Model

- This continuous process is one of the major reasons MLOps is important.

## Traditional Software Development vs ML Development
- In traditional software:

      Developer writes rules
              ↓
      Application follows those rules

- In machine learning:

      Data
       ↓
      Learning Algorithm
       ↓
      Model
       ↓
      Predictions

- The important difference is that the behavior of an ML system is learned from data.


# MLOps Lifecycle
- The MLOps lifecycle is the continuous process of developing, deploying, monitoring, and improving a machine learning model in a production environment.


        Problem Definition
               ↓
        Data Collection
               ↓
        Data Preparation
               ↓
        Model Training
               ↓
        Model Evaluation
               ↓
        Model Deployment
               ↓
        Model Monitoring
               ↓
        Retraining / Improvement
               ↺

- Problem Definition – Defines the business objective and determines what the ML model needs to solve or predict.
- Data Collection – Gathers relevant and reliable data from sources such as databases, files, APIs, or applications.
- Data Preparation – Cleans, transforms, and organizes raw data so it becomes suitable for machine learning.
- Model Training – Applies a machine learning algorithm to training data so the model can learn patterns and relationships.
- Model Evaluation – Tests the trained model using suitable metrics to determine whether its performance is accurate and reliable.
- Model Deployment – Makes the validated model available in a production environment so it can generate predictions for real users or applications.
- Model Monitoring – Continuously observes model performance, data changes, errors, latency, and system health after deployment.
- Retraining – Updates the model using new data when performance decreases or real-world patterns change, followed by evaluation and redeployment.




