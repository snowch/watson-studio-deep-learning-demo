# Watson Studio Deep Learning Demo

### Overview

In this demo you will:

- Create a neural network for classifying hand written digits
- Create an experiment to train the model
- Create a web service to classify

### Pre-requisites

The following IBM Service Instances are required:

- Watson Studio
- Watson Machine Learning
- Cloud Object Storage (COS)

Ensure that all of the service instances are all in the same IBM Cloud Resource Group, Organisation and Space.

### Watson Studio Steps

This section lists the required Watson Studio setup steps.

#### Project Setup

 - Cloud Object Storage connection
   - Navigate to the Watson Studio Project **Overview** page  
   - Create a connection to your Cloud Object Storage with the name **COS Images**
 - Access Token
   - Navigate to the Watson Studio Project **Settings** page 
   - Create a Project Token with **Editor** Role
 - Import notebooks
   - Import the notebooks [Prepare MNIST Data.ipynb](./Prepare MNIST Data.ipynb) and [Create Test Image.ipynb](./Create Test Image.ipynb)
   - You can use a Python Anaconda Environment (any size will do)

#### Upload MNIST Images

In this section we upload the sample MNIST images to Cloud Object Storage

 - Open the notebook **Prepare MNIST Data**
 - Set the following variables:
   - project_id (this can be found in the URL)
   - project_access_token (this was created in the previous section)
 - Run all cells in the notebook
 - Make a note of the COS bucket names printed at the end

#### Create Neural Network and Training Definition

 - Navigate to the Watson Studio Project **Assets** page
 - Create a new Modeler flow
    - Select **From Example**
    - Select **NEURAL NETWORK DESIGN | Single Convolution layer on MNIST**
    - Click Create
 - Right click the **Image Data** node
 - Select **Open**
    - Under *Data Connections* select **COS Images**
    - Under *Buckets* select [[project-id]]-training
    - Under *Train data file* select **mnist-tf-train.pkl** 
    - Under *Test data file* select **mnist-tf-test.pkl**
    - Under *Validation data file* select **mnist-tf-valid.pkl**
    - Click **Save**
 - On the toolbar, click: **Publish Training Definition**
    - Accept the defaults
    - Click **Publish**
 - In the pop up message click the link **train it in an experiment**
    - Experiment Name: **MNIST Experiment**
    - Under the heading *Cloud Object Storage bucket for storing training source and results files*
       - Click **select**
       - On *Select Cloud Object Storage connection* select **COS Images**
       - Select existing bucket for training data - use training bucket name output from notebook: **Prepare MNIST Data**
       - Select existing bucket for results data - use results bucket name output from notebook: **Prepare MNIST Data**
       - Click **Select**
    - Click **Add training definition**
       - Select **Existing training definition**
       - Under *Existing training definitions* select the training definition exported from the neural network editor
       - Under *Compute plan* select **1/2 x NVIDIA® Tesla® K80 (1 GPU)**
       - Under *Hyperparameter optimization method* select **none**
       - Click **Select**
       - Click **Create and run**
       - When the training run has completed goto the section **Deploy Model**
       
#### Deploy Model
  
   - Click **Actions** on the right of the table for the completed training run **Single Convolution layer on MNIST**
   - Click **Save model**
   - Provide a model name: **MNIST Model**
   - Click **Save**
      - In the message *Model successfully saved. View model details here.* click **here**
      - Click **Deployments**
      - Click **Add Deployment**
         - Provide name **MNIST Deployment**
         - Click **Save**
            - When status is: **DEPLOY_SUCCESS**
            - Select **MNIST Service**
            - Click **Test**
            - Click **Provide Input Data as Json**
            
#### Test deployment
   
   - Open a new Watson Studio browser tab
   - Open the notebook **Create Test Image**
   - Run all cells
   - Copy the output: **{"values": [...]}**
   - Go back the *Deployment Test* tab
       - In the box *Enter input data* paste the copied output
       - Click **Predict*
       
  Note the output probabilities.
