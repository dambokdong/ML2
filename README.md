# ML2 Take-Home Project: ECG Classification

## Overview 
The task given is a multiclass classification problem, where the class distribution of the training data is highly imbalanced. Nature of the data is a segmented time-series, where each sample row is 187-element vector containing the ECG lead II signal for one heartbeat. The last column denotes the class to which a sample belongs. Labels [0, ..., 4] correspond to ['N', 'S', 'V', 'F', 'Q'] respectively, with majority(normal) sample accounts for 83% and the reest with varying proportions. 

First, we aim to handle the imbalance in the dataset via oversampling. We test different methods of oversampling, from random upsampling to SMOTE-based variations. Further description is included in the section "Oversampling Methods"

Next, we implement 3 different classifiers(k-NN, Random Forest, CNN) to tackle the classification problem. We feed the original training data and different sets of data generated by various oversampling methods, and validate the trained model against the test set. Further description is included in the section "Classifiers".

Lastly, model performance is gauged by different evaluation metrics with consideration to the imbalanced nature of the data. We compare how different oversampling methods affect the performance in each classification model. 

## Oversampling Methods 
To provide a wide range of relevant methods, we considered the following reference algorithms: 

- SMOTE and ADASYN as general and widely used methods of dealing with imbalance in data
- Borderline-SMOTE as a method designed with specific focus on borderline minority data
- SMOTE with Tomek links as an ensemble method of oversampling + cleaning of difficult examples(undersampling)
- ProWSyn as a method taking into account proximities to the border as weight values

In addition, we included the baseline case where no resampling was applied, and Random Upsampling where no specific algorithm was applied yet the minority samples were randomly chosen and duplicated. 

## Classifiers
3 different algorithms were considered: k-NN, Random Forest and 1-D CNN. 

Detailed hyperparameter tuning for k-nn and random forest classifiers are lacking in this analysis, with stronger focus on quick comparison between affects of different oversampling methods in model performance. 

For the 1-d Convolutional Neural Net, the original model was referred from https://www.kaggle.com/code/gregoiredc/arrhythmia-on-ecg-classification-using-cnn. 

The network consists of:  
  - 3 sets of 1-D convolutional layer with a batch normalization and a max pool layer. 
  - 1 fully-connected layer
  - 1 linear layer with softmax output
  - No regularization was used except for early stopping

## Performance Evaluation
We have included several metrics which could accurately reflect the efficacy against imbalanced classification problem, along with classic confusion matrix. First category of metric is computed on the individual class level, whereas the second category is computed over all classes to show overall performance. 

Amongst several metrics that were initially considered, below are the metrics that we set to focus on:
- Class Level: Recall and F-2 Measure
- Overall Level: Weighted Accuracy, Geometric Mean of Recalls, Macro-averaged F-2 Measure
 
Every oversampling method we tested has shown improvement against the baseline case(no resampling), and exhibited sufficient capacity to learn the training distribution. The error rates for all models were highest for the classes with the fewest examples, namely S-and F-. 

To evaluate each oversampling method across different classifiers, we used the average ranking where the results are shown as below. 

<img width="675" alt="Screen Shot 2022-08-10 at 12 10 48 PM" src="https://user-images.githubusercontent.com/110798353/183801901-33ddd634-b094-4e2d-9014-a5b61ac08ccc.png">

There was no single method that donimates the rest in all metrics, yet we find ProWSyn and SMOTETomek to perform consistently well across different metrics. ADASYN and SMOTE were the second-best group with no notable preference over one another to be confirmed, while BorderlineSMOTE did not show tangible benefit over random upsampling in our case.

On class-level metrics, in most cases higher recall is accompanied by lower precision. However, for our classification problem, we believe the recall value carries more importance as for health issues it is deemed to be more costly to generate false negatives. Additionally, the trade-off turns out to be beneficial with regards to the value of combined-level metrics.

## References 
Learning from Imbalanced Data (Haibo He, Edwardo A.Garcia)

Metrics for Mlti-class Classification: An Overview (Margherita Grandini, Enrico Bagli, Giorgio Bisani)
  
Combined Cleaning and Resampling Algorithm for Multi-Class Imbalanced Data with Label Noise (Michał Koziarskia, Michał Woźniakb, Bartosz Krawczykc)

ProWSyn: Proximity Weighted Synthetic Oversampling Technique for Imbalanced Data Set Learning (Sukarna Barua, Kazuyuki Murase)
  
1D Convolutional Neural Network Models for Human Activity Recognition: https://machinelearningmastery.com/cnn-models-for-human-activity-recognition-time-series-classification/

CNN Model: https://www.kaggle.com/code/gregoiredc/arrhythmia-on-ecg-classification-using-cnn

Confusion Matrix: https://www.kaggle.com/code/coni57/model-from-arxiv-1805-00794/notebook

  
