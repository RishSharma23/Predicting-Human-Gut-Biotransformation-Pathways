
# Prediction of Human Gut Biotransformation Pathways

## **Student Name(s)** 

Yashas Raman, 49323976, yraman@uci.edu Raghav Sriram, 28137297, sriramr2@uci.edu Rishit Sharma, 45329724, rishits3@uci.edu 

## **1. Introduction and Problem Statement** 

This project will use machine learning techniques on a dataset consisting of human gut biotransformation reactions to predict how the human gut microbiome transforms chemical compounds. By analyzing chemical and biological data, we aim to forecast the end products of these transformations and the pathways they take. Success will be measured by comparing our predictions with real data to see how closely they match. 

The goal of this project is to predict what happens to chemical compounds when they interact with the human gut microbiome. In the gut, microbes break down and modify compounds through a series of chemical reactions, resulting in new molecules called metabolites. Understanding these changes is essential for drug development, assessing chemical safety, and personalized medicine. Our system will take in the chemical structure of a compound and predict the sequence of transformations it undergoes, identifying the most likely pathways and end products. 

## **2. Related Work** 

The prediction of chemical transformations in the human gut microbiome has been explored through innovative approaches. Rule-based systems, like cheminformatics libraries such as RDKit, have been widely used for their clear and interpretable reaction templates. Schwaller et al. (2018) made a significant breakthrough by introducing a sequence-to-sequence neural translation model which treated chemical reactions as language-like sequences. A popular approach building off this research, such as by Kaggle user Poma Mikhail, was to apply reaction templates to generate a random walk, which essentially generates a pathway by taking a sequence of random steps. However, this approach faced challenges in generalizing to rare or unseen reactions, which are crucial in biochemical contexts, as it is not unable to properly identify trends in the data. 

Our approach establishes a thorough multi-step pathway prediction methodology for chemical transformations occurring within the human gut. Our system mimics transformation sequences using a hybrid strategy by combining template-based prediction and supervised machine learning. Drawing from Rogers and Hahn's (2010) molecular fingerprint techniques and inspired by MoleculeNet's evaluation strategies (Wu et al., 2018), we utilize classification methods such as Random Forest and LightGBM to predict the most likely pathways for a chemical compound in the gut. By combining sophisticated machine learning techniques with interpretable models, we attempt to create a more nuanced way of mapping out complex chemical transformations. 

## **3. Data Sets** 

Dataset URL: 

https://www.kaggle.com/competitions/prediction-human-gut-biotransformation-pathways/data 

There are three files contained in our dataset, a training csv, a test csv, and sample submission csv. The training dataset contains 17301 gut biotransformation reactions. Each reaction is uniquely identified with an ID and the SMILES of reactants and products of each reaction. The test csv contains an ID of the pathway, the number of steps each pathway has (ie number of metabolites), and the first compound in the pathway. The csv file our model produced from running on the test dataset contains the ID of the pathway and then the first, second, third, fourth and fifth ranked predictions made by the model. The figures below depict the length and quantity of our SMILES, describing the general shape of our dataset: 

<img width="627" height="220" alt="image" src="https://github.com/user-attachments/assets/f6b99b35-532d-4193-885f-0a67b61f691f" />

The figures below show what our raw training data looks like (CSV file): 
<img width="633" height="338" alt="image" src="https://github.com/user-attachments/assets/e1a03246-e2da-45e9-a3de-80c5b12da939" />

## **4. Technical Approach** 

Our project aims to predict human gut biotransformation pathways for given chemical compounds. The overarching goal is to start with a known reactant and determine the sequence of transformations it might undergo in the human gut environment, producing a set of likely metabolites. 

## **Data Representation:** 

We use SMILES (Simplified Molecular Input Line Entry System) strings to represent chemical compounds. These textual representations capture the structure of molecules in a linear form. By processing the training data, we attempted to establish a mapping between a reactant and product SMILES. We removed invalid SMILES and ensured only valid chemical structures remained. We also removed duplicate reactant/product pairs to avoid bias. For modeling, we used feature selection, cutting down the number of features from 2048 to just 500, in order to reduce dimensionality and improve training speeds. 

## **Core Algorithms and Techniques:** 

We employed the RDKit library, a powerful toolkit for cheminformatics, to parse and handle chemical structures. Each reactant/product pair in the training set was used to extract a reaction template. A reaction template provides a generalized rule representing how a certain reactant turns into a certain product. Given a starting molecule, we iteratively apply our reaction templates. By chaining these transformations step-by-step, we simulate a pathway. We generate multiple candidate pathways and then use scoring methods to rank the most likely ones. 

To improve predictions and possibly rank outcomes, we used molecular fingerprints as features. Specifically, we used Morgan fingerprints (circular fingerprints), which were generated from the SMILES using RDKit. These fingerprints transform the molecular structure into a high-dimensional binary vector that represents chemical structures. We then combined reactant and product fingerprints to create training examples for supervised learning models. 

We experimented with two machine learning algorithms, LightGBM and Random Forest. LightGBM is a machine learning model that improves its predictions by focusing on the most difficult examples during training, a process known as gradient boosting. In our project, it is used to predict and prioritize the most likely chemical reactions, especially when the data is uneven or imbalanced. Random Forest is a machine learning method that combines the results of many decision trees to make better predictions. For our project, it helps rank and classify the possible chemical transformations by analyzing patterns in the molecular features of reactants and products. We expanded on Random Forest by creating two models, one using 100 estimators and another using 200 estimators, in order to see the effects of increasing the amount of estimators on model performance. The figure below outlines our general system pipeline used in this project: 

<img width="441" height="215" alt="image" src="https://github.com/user-attachments/assets/a83c1c64-56b8-4f98-bd4a-cd28fe39c73c" />


## **5. Software** 

The table below outlines all major pieces of software and code involved in the execution of this project: 

|**Code Component**|**Author**|**Functionality**|**Input**|**Output**|
|---|---|---|---|---|
|Data Cleaning<br>Script|50% team, 50%<br>external code<br>derived from<br>Kaggle user<br>Poma Mikhail|Loads and parses data and<br>validates SMILES strings.|Training and<br>testing data|Cleaned<br>dataset|
|Reaction<br>Template Code|External (Kaggle<br>user Poma<br>Mikhail)|Extracts reaction templates<br>from reactant-product pairs<br>(via RDKit)|Training data|List of<br>reaction<br>templates|
|Fingerprint<br>Generation|Team (written)|Functions using RDKit to<br>generate Morgan fingerprints<br>from SMILES.|SMILES string|Numpy array<br>of features|
|ML Training<br>Scripts|Team (written)|Code that fits LightGBM and<br>Random Forest model to our<br>training data, and evaluates<br>them when applied to our<br>testing data|Feature arrays<br>and labels|Fitted<br>model,<br>metrics|
|Visualization<br>Scripts|Team (written)|Scripts to plot histograms, bar<br>charts, and confusion<br>matrices for interpretability|Dataframes,<br>predictions|Charts/Plots<br>needed to<br>visualize<br>results|
|RDKit Library|External|SMILES parsing, fingerprint<br>generation, reaction<br>templates|SMILES strings|Molecules,<br>fingerprints|
|Sklearn/NumPy/<br>Pandas|External|Data manipulation, ML<br>algorithms, metrics|CSV data,<br>arrays|Processed<br>data, ML<br>models|



## **6. Experiments and Evaluation** 

We chose to evaluate each of our models by using the Final Score (FS) metric as outlined by the Kaggle competition organizers for this specific project. The formula for the metric can be seen below: 
<img width="166" height="44" alt="image" src="https://github.com/user-attachments/assets/c845332d-ed12-46fe-a6d8-287624fc8b1d" />

This metric is an accuracy score that represents the average of the pathways’ similarity in the test dataset. The value of FS is between 0 and 1, with higher values indicating better performance. The PS in the formula represents pathway similarity, which compares how similar the pathways predicted by our model are with the true reaction pathway. PS is calculated using the following formula: 
<img width="191" height="36" alt="image" src="https://github.com/user-attachments/assets/c8690339-0d6f-4627-93db-07f823644832" />

The PMS in the formula represents the similarity between a predicted metabolite and the true metabolite in a reaction pathway. PMS is calculated using the following formula: 
<img width="240" height="37" alt="image" src="https://github.com/user-attachments/assets/a3f8179b-b683-4ff2-9968-a8c9637786eb" />

In this formula LD represents the Levenshtein Distance (LD), which is the difference between two string sequences, between the ground truth metabolite and the predicted metabolite. Essentially, the overall FS metric essentially compares our predicted pathways to the true pathways character-by-character using LD, allowing us to determine the overall effectiveness of our machine learning models. We used an 80/20 train/validation split of the training data, and evaluated our model’s performance using accuracy and F1 precision score (weighted). 

## **Results of the Models:** 

LightGBM Model: 

   - Accuracy: 0.7107241788682231 

   - F1 Score: 0.6498877034018843 

- Random Forest (100 Estimators): 

   - Accuracy: 0.7855164226355362 

   - F1 Score: 0.7365284432960275 

- Random Forest (200 Estimators): 

   - Accuracy: 0.7859121487930352 

   - F1 Score: 0.7371349093358137 

The figure below shows how Random Forest had a higher density of correct predictions (1.0) while LightGBM had a higher density of incorrect predictions (0.0), which is consistent with our results: 

<img width="465" height="260" alt="image" src="https://github.com/user-attachments/assets/d07235e7-e675-4df7-abd2-7fc98cbe616f" />


We can see that both Random Forest models significantly outperformed the LightGBM model in both accuracy and F1 score. This is likely due to the nature of our dataset, as Random Forest models tend to perform better than LightGBM models on complex datasets. As random forest models train each decision tree independently, as opposed to LGBM which uses gradient boosting, it produces more robust models that tend to fit complex data better. However, we found that increasing the amount of estimators from 100 to 200 did not seem to cause any significant improvement. This suggests that the model had already reached a point of diminishing returns in terms of additional estimators, with further increases providing minimal benefits. Overall, these results highlight the robustness of Random Forest for handling complex, high-dimensional datasets, making it a strong choice for our project. 

Sensitivity to algorithmic choices played a critical role in shaping the performance of our models. We initially intended for our Random Forest models to use all 2048 features from the dataset. However, we found that the models simply would not run with all the features present, as they took far too much time to even produce results.Therefore, we found it necessary to use feature selection, and cut down the number of features to 500. This likely affected our model performance, as the remaining 1548 features may have contained crucial information that could have improved our models’ performance. Normalization and feature selection were critical for model stability. Techniques like SKLearn’s SelectKBest improved training, and we found that proper data cleaning significantly enhanced model performance. Our template selection strategy required careful calibration: more restrictive templates increased precision but limited pathway generation, while broader templates produced more diverse results at the cost of accuracy. Ultimately, finding the right balance between these approaches was key to optimizing our model's effectiveness. 

The figure below depicts the trend seen in feature importance for our 500 features. The exponential curve shows that the most important features had by far the most significant impact on the models: 

<img width="501" height="312" alt="image" src="https://github.com/user-attachments/assets/23e1dd05-77ea-4527-90f3-1d2d3f83fc4c" />

More figures pertaining to our results can be found in the appendix section of this report. 

## **7. Discussion and Conclusion** 

We learned that the complexity of chemical transformations can be partially captured by reaction templates and molecule fingerprints. The ML models managed to capture some structural transformation patterns but struggled with rare transformations. The Random Forest’s stable performance suggests that ensemble methods, which combine the predictions of multiple trees, handle complex, high-dimensional cheminformatics data well especially compared to gradient boosting methods such as LightGBM. 

We initially expected our LightGBM model to outperform our Random Forest models, due to the large size of our dataset. However, we failed to account for the highly complex nature of our data, which is likely the reason why our Random Forest models actually performed better. We also expected the model to perform slightly better on our test data. This suggests that our pipeline heavily depended on known transformations from training data. Without similar patterns in the training set, predictions were less accurate. 

Despite our large dataset, our models may have still been limited by our level of data coverage, as space of possible chemical transformations is vast. Our model only learned from 17301 known reaction pairs, which is still an extremely small subset of all possible reactions. Because of this, the model likely struggled when a test compound’s transformations were not sufficiently represented by similar examples in the training data. Our use of feature selection may have also inhibited the performance of our models. In order to have our model run in a timely manner, we had to reduce the number of features in our dataset from 2048 to 500. This may have caused important information in some of our data to be lost, causing our model to perform worse. 

Future steps we could take to improve our approach involve exploring advanced computational techniques and expanding the scope of our data. One promising direction is investing in Graph Neural Networks (GNNs) or other deep learning models specifically designed for chemical reaction prediction. These models excel at capturing the relationships between molecular components, making them ideal for complex reaction pathways. Additionally, gathering a more comprehensive reaction database would ensure broader coverage of the total possible reactions, ideally leading to improved model performance. Another avenue worth investigating is the use of reinforcement learning agents, which could iteratively select transformations, potentially generating more accurate results. 

In conclusion, our exploration dove into the fascinating complexity of how chemical compounds transform within the human gut, revealing both exciting possibilities and important limitations in our current understanding. 

## **8. Individual Contributions** 

**Raghav Sriram:** I handled notebook hosting on Kaggle and wrote code to train, develop, and test the models used in this project. I also ensured the notebook was well organized, easy to follow, and that our pipeline was well detailed and clear. I prepared the project.zip file by creating the project.html, README.txt file, and organizing the datasets. Furthermore, I contributed to writing sections 4 and 5 of the project proposal and assisted Yashas in editing the slideshow for the final presentation. For the final report, I helped write sections 3 and 5a and copy edited the final report so that it was ready for submission. 

**Rishit Sharma:** I contributed extensively to the visualization aspects of this project, creating the code for all figures, diagrams, and tables showcased in the final report, and made several suggestions and improvements to the notebook code. I took the lead in interpreting the output of our experiments, ensuring that the results were clearly understood and integrated into our conclusions. I authored the majority of the final report’s written content, including sections 1, 3, 4a, 4b, 5, and 6. 

**Yashas Raman:** I assisted Raghav in writing the code for data preprocessing as well as all three of our models used in this project. I also oversaw and edited the final project report to ensure it was well-organized and ready for submission, along with adding useful contributions to essentially every section in the report. Additionally, I helped make the final presentation slideshow, ensuring it effectively showcased our project's goals, methods, and results in a clear and engaging manner. 

## **Appendix** 

This figure shows the top 20 features integrated into our Random Forest models, giving us a closer look at feature importance: 

<img width="538" height="315" alt="image" src="https://github.com/user-attachments/assets/8d125a5d-f639-4b77-8f07-8451a96130b9" />

This figure is a confusion matrix showing the number of correct predictions for each of the first 10 classes in our LightGBM mode. You can see that the LGBM model does not predict accurately as there are not correct predictions for classes 0 through 7. This trend appeared for the rest of the classes: 

<img width="412" height="437" alt="image" src="https://github.com/user-attachments/assets/e88da7f2-5110-42ea-a292-8cd95d5c8d4e" />


This figure is a confusion matrix showing the number of correct predictions for each of the first 10 classes in our Random Forest (200 estimators) model. We can see that our Random Forest model either equalled or outperformed our LightGBM model in every class: 

<img width="459" height="449" alt="image" src="https://github.com/user-attachments/assets/f0bf2b7d-c22d-493d-b1ba-8bc440a47006" />


## **Works Cited** 

Schwaller, P., Gaudin, T., Lanyi, D., Bekas, C., & Laino, T. (2018). Molecular transformer: A model for uncertainty-calibrated chemical reaction prediction. _arXiv preprint_ . https://arxiv.org/abs/1811.02633 

Rogers, D., & Hahn, M. (2010). Extended-connectivity fingerprints. _Journal of Chemical Information and Modeling, 50_ (5), 742–754. https://doi.org/10.1021/ci100050t 

Wu, Z., Ramsundar, B., Feinberg, E. N., Gomes, J., Geniesse, C., Pappu, A. S., ... & Pande, V. (2018). MoleculeNet: A benchmark for molecular machine learning. _Chemical Science, 9_ (2), 513–530. https://doi.org/10.1039/C7SC02664A 



The goal of this project is to predict what happens to chemical compounds when they interact with thehuman gut microbiome. In the gut, microbes break down and modify compounds through a series of chemical reactions, resulting in new molecules called metabolites. Understanding these changes is essential for drug development, assessing chemical safety, and personalized medicine. Our system will take in the chemical structure of a compound and predict the sequence of transformations it undergoes, identifying the most likely pathways and end products.

Directory Organization:
project.html: File contains all the output of our notebook
project.ipynb: File contains the notebook where we performed all of our computational tasks
test.csv: File contains testing data. Test dataset includes the starting metabolite and the ending metabolite our model is supposed to predict.

In Archive folder:
test_demo.csv: Contains an example of how test data looks like. There are three columns, PWY_ID, Steps, and Compound
train.csv: Training set which consists of a list of metabolic reactions (17,320 reactions total)

IMPORTANT INFO: project.ipynb takes 1-2 minutes when run on a GPU but takes around 8 minutes when run on CPU only.
