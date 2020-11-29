# Hachathon-2020

This repository stores the code and results in Hackathon-2020, aiming to analyse the real dataset  and synthetic dataset if diabetes.

The dataset and descriptions can be found at https://biolib.com/GATTAC/Diabetes-Machine-Learning-Data-ornq/ or in the folder "data".

Our team page is at https://biolib.com/SVM/Spaghetti-Vector-Monster/

An outline of our work and result can be found in the file "Presentation_Slides.pdf".

A more detailed description is shown below.

<br/><br/>

## An outline of our work

- Train a model on synthetic data to predict re-admission of the patients, compare the performance on synthetic data and real data.
- Train a Neuron Net to classify real data and synthetic data.
- Analyze the result of the last step, find the most significant difference between two datasets.

## Detailed description of each step

### 1. Train a model to predict re-admission of the patient.

#### 1.1 PCA

By PCA, we reduce 41 dimensions(except readmit) to 10 dimensions, then calculate the correlation coefficient between each principle component and each raw dimensions.
Find that there are some dimensions that are very un-related with all those 10 principle components, they are "chlorpropamide", "acetohexamide", "tolbutamide", "miglitol", "tolazamide", "glyburide-metformin", "glipizide-metformin". 
So in **all** of the following analysis, we teased out these dimensions.

#### 1.2 Model structure 

The structure during training is shown as follows:

![image](https://github.com/hejj16/Hachathon-2020/blob/main/model-structure1.png)

When predicting, use discrete fratures by Random Forest2 to classify, and use continuous features by Neuron Network to further classify those data being predicted as class -1.

#### 1.3 Result

|            |  Synthetic Data (Training set)   |  Synthetic Data (Val set)  |  Real Data  |
| --------   | -----:   | ----: |  | :----: |
| Accuracy       | 71%      |   68%   |  53% |




