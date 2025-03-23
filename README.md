# CICD_CT_airflow
# Building a continuous training pipeline using Cloud Composer 
What is Cloud Composer?
Could Composer is a fully managed workflow orchestration service built on Google Cloud Platform. It is built on top of Apache airflow, which allows you to define, schedule, and monitor your complex workflows.
The below is the architecture of the pipeline:
![Picture1](https://github.com/user-attachments/assets/75336997-01bc-4c25-aeb7-846f9266ea32)
Here are the steps that we follow to build this pipeline. 
1.	Initially after training, evaluating and testing the model it(model.joblb) gets loaded into the gcs bucket.  
2.	But the model might become stale if not trained continuously as the new data comes in. This is where the above pipeline comes into picture.
3.	When there is a new data present in the GCS bucket, the Cloud composer takes the new data and validates, preprocess the data to make it fit to the model. 
4.	 Re-train the previous model on the new data and compare the metrics, which are logged in the BQ table, with the previous version model. 
5.	Set a minimum threshold for the evaluation metrics. And if the evaluation metrics for the new model is greater than the threshold set and the previous model’s metric value then push the model to gcs. 
6.	We can also set alerts on log-based metrics to let user know if anything fails in this process. 
What if we make any changes in the train.py code? How does the newly made changes get into the train .py file in the composer. This is where the CICD part comes in. 
7.	Include the docker file , cloudbuild.yaml file as well as test_training.py file  along with the train.py file in the github repo and connect this repo to the cloud build. 
Why we need dockerfile? So in order to run the train.py file the cloud build needs the dockerfile to run them in a containerized environment (working nature of cloud build). 
8.	Finally create a trigger on this build. 
