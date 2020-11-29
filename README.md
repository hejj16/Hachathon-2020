# Hachathon-2020


This repository stores the code and results in Hackathon-2020, aiming to analyse the real dataset  and synthetic dataset if diabetes.

The dataset and descriptions can be found at https://biolib.com/GATTAC/Diabetes-Machine-Learning-Data-ornq/ or in the folder "data".

Our team page is at https://biolib.com/SVM/Spaghetti-Vector-Monster/ .
Teammates: [Jiajun He](https://github.com/hejj16), [Zelin Li](https://github.com/lzlniu)

An outline of our work and result can be found in the file "Presentation_Slides.pdf".

A more detailed description is shown below.

<br/><br/>

## An outline of our work

- Train a model on synthetic data to predict re-admission of the patients, compare the performance on synthetic data and real data.
- Train a Neuron Net to classify real data and synthetic data.
- Analyze the result of the last step, find the most significant difference between two datasets.

## Detailed description of each step

### 1. Train a model to predict re-admission of the patient

Train a model on synthetic data to predict re-admission of the patients, then valid on validation set of synthetic data and on real data.
Compare the difference in performance, to find if there is any difference between real data and syn data.


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
| :------:   | :----:   | :----: |  :----: |
| Accuracy   | 71%      |   68%   |  53% |

From the table, we find that real data is much **harder** to predict than synthetic data.

So only using synthetic data to conduct data analysis is **risky**.


### 2. Classify real data and synthetic data

Last step, we find that there are significant difference between 2 dataset, so in this step, we want to figure out if this difference is learnable by training a classifier to classify 2 dataset.

#### 2.1 Model structure

We use a 5-hidden-layer neuron network to classify, and the amount of neurons in each layer is 60, 40, 20, 10, 5.

#### 2.2 Result
|            |  Training set   |  Val set  |
| :------:   | :----:   | :----: | 
| Accuracy   | 90%      |   88%   |

From the table, we can tell that the difference between 2 datasets is learnable.


### 3. Analysis the result, find the most significant difference between 2 datasets

In last step, we find that the difference between 2 datasets is learnable. In this step, we use t-SNE to visualise the ability of decoupling of each layer in our trained neuron network.

#### 3.1 t-SNE embedding and visualization


![image](https://github.com/hejj16/Hachathon-2020/blob/main/tsne.png)

From the picture, we can see after 1st layer, the decoupling is pretty good. Though as network goes deeper, the result is getting better, what makes a significant difference is 1st layer. 

#### 3.2 Analyse what the 1st layer does

To see what the 1st layer does, we take pseudo-inverse to recoover input.

This inspiration comes from the paper *Visualizing and Understanding Convolutional Networks* by Zeiler M.D., Fergus R. in 2014. Though this is not a ConvNet, the idea is similar.

We take the samples that mostly active the neurons in 1st layer. The matrix of these samples are X. A is the outout of 1st layer. Then we use formula below to recover X. Here we take pseudo-inv as W is not square.
![image](https://github.com/hejj16/Hachathon-2020/blob/main/pinv.PNG)

Then compare the difference between input and input X. Check those dimensions significantly negative in recovered X. These dimensions can be interepreted as "useless" dimensions beacuse 1st layers are not activated by these dimensions.

By teasing these dimensions out, we get 22 dimensions remained.

#### 3.3 Find the most important dimensions amoung 22 dimensions

First, train model to classify real and syn data using only 1 dimension of these 22 dimensions, finding the accuracy are just a little bit higher than 50%, indecating that one dimension is not sufficient.

Second, train model by using 2 dimensions of these 22 dimensions, finding by combining "insulin" and "diabetesMed"/ "insulin" and "change" / "change" and "diabetesMed", the accuracy can achieve about 62-65%. While the combination of other dimensions seem no use.

Then we train model by using these 3 dimensions: "insulin", "change" and "diabetesMed". The accuracy is shown below.

|            |  All features   |  Only 3 features  |  All other features  |
| :------:   | :----:   | :----: |  :----: |
| Accuracy   | 88%      |   72%   |  74% |

This indecates that these 3 features really matter.

#### 3.4 Understand why these 3 features matter

We calculate the correlation coefficient between these 3 features, the result is shown as follow.

**Real Data**
|            |  insulin   |  change  |  diabetesMed  |
| :------:   | :----:   | :----: |  :----: |
| insulin   | 1.00      |   -0.14  |  0.26 |
| change  | -0.14     |   1.00   |  -0.51 |
| diabetesMed   | 0.26      |   -0.51   |  1.00 |

**Synthetic Data**
|            |  insulin   |  change  |  diabetesMed  |
| :------:   | :----:   | :----: |  :----: |
| insulin   | 1.00      |   -0.02  |  0.01 |
| change  | -0.02     |   1.00   |  -0.02 |
| diabetesMed   | 0.01      |   -0.02   |  1.00 |


In real data, these three dimensions are highly **related**;
while in synthetic data, these three dimensions seems to be generated **independently**.
This is the most significant difference between 2 datasets.


### 4. Conclusion

- The mainly difference between real data and synthetic data are dimensions “insulin”, “change” and “diabetesMed”;

- In real data, these three dimensions are highly related;

- While in synthetic data, these three dimensions seems to be generated independently.








