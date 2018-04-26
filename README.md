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

#### Create Neural Network

 - Navigate to the Watson Studio Project **Assets** page
 - Create a new Modeler flow
    - Select **From Example**
    - Select **NEURAL NETWORK DESIGN | Single Convolution layer on MNIST**
    - Click Create
 - Right click the **Image Data** node
 - Select **Open**
    - Under *Data Connections* select **COS Images**
    - Under *Buckets* select <<project-id>>-training
    - Under *Train data file* select **mnist-tf-train.pkl** 
    - Under *Test data file* select **mnist-tf-test.pkl**
    - Under *Validation data file* select **mnist-tf-valid.pkl**
    - Click **Save**
 - On the toolbar, click: **Publish Training Definition**
    - Accept the defaults
    - Click **Publish**
