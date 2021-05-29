# Introduction
Genetic testing is improving how diseases like cancer are treated everyday. Sequencing cancer tumors reveals thousands of genetic mutations. Challenge remains to distinguish the cancer-causing mutations from the silent ones. Advancing our understanding of cancer and designing more precise and effective treatments holds much promise for the patients. To this end, we created a model to speed up a time-consuming manual workflow of a clinical pathologist by classifying genetic mutations from text-based clinical literature.

# Data Source
Memorial Sloan Kettering Cancer Center has created an expert-annotated precision oncology knowledge base that contains several thousand annotations clinically actionable genes (available on [Kaggle](https://www.kaggle.com/c/msk-redefining-cancer-treatment/data?select=training_variants.zip)). Each genetic variation is placed into 1 of 9 categories, persumably corresponding to a treatment type. Our goal is to build an ML model to classify these annotations 

# Quantification of Results
We used various text embeddings to vectorize the text data, and for each embedding, we built multiple models for classification. Due to the nature of this multiple classification problem and the fact that the data was very unbalanced, it is inappropriate to use accuracy or log-loss as metrics to measure performance of any of the machine learning or neural network approaches. Rather, [ROC curves](https://en.wikipedia.org/wiki/Receiver_operating_characteristic), which indicate the proportion of true positive and false positive rates (i.e., sensitivity vs false alarm), is a more appropriate measure. We present our results in terms of the area under the ROC curve (AUC). It is desireable for the AUC to be close to 1.

# Main Results
Our aggregate results show that Doc2Vec clearly outperformed Bag of Words and TF-IDF embeddings. However, with an AUC of ~0.8, we do not believe that our classifier is good enough to guide clinical decision making yet.

![Results1](https://github.com/kfchou/PersonalizedMedicine/blob/main/figures/AUC%20v%20Encoder.png)

Interestingly, there is no difference in performance across the various classifiers, either classical machine learning or deep learning.

![Results2](https://github.com/kfchou/PersonalizedMedicine/blob/main/figures/AUC%20v%20Classifier.png)

# Actionable Findings
We found that within the dataset, several gene variations did not have associated text descriptors. Additionally, several genetic variations share the same text descriptors, which may or may not belong to the same class. These issues with the dataset make it difficult for both humans and machines to correctly label similar genetic variations. Based on this, we recommend augmenting the dataset to fill empty text descriptors, and adding data to prevent different genetic variations belonging to different classes from having the same text descriptor. This can be done by scraping research literature from the web, such as via pub med.

Our classification results found that several classes appear highly correlated (classes 1&4), and hence easily confused. This result was observed in multiple text embedding schemes. It is possible that these genetic variations share similar scientific literature, and are correlated biologically as well. **Practitioners should take care to not make the same mistake as the ML models when placing a genetic varient into one of these classes.** In order to improve classification performance, we recommend exploring methods to decorrelate these classes. This may be accomplished by obtaining more data for these classes via web scraping.

|*Doc2Vec + Random Forest*|*TF-IDF + Random Forest*|*Bag of Words + Random Forest*|
|:------:|:------:|:------:|
|<img src="https://github.com/kfchou/PersonalizedMedicine/blob/main/figures/CM_D2V_RF_train.png" alt="alt text" width="100%">|<img src="https://github.com/kfchou/PersonalizedMedicine/blob/main/figures/CM_TFIDF_RF_train.png" alt="alt text" width="100%">|<img src="https://github.com/kfchou/PersonalizedMedicine/blob/main/figures/CM_BOW_RF_train.png" alt="alt text" width="1000%">|

The confusion matrices also show a skew toward class 7, most likely due to that fact that our training data distribution is heavily skewed toward that class. For future work, we suggest either undersampling class 7, or collect more data for other classes to even out the training distribution.

# Other thoughts
It is unclear what each "class" represents. Hence it is difficult to understand, based on the accompyning literature, what defines a class. Having this knowledge may help design custom features (e.g., key words or words that identify relationships between genes, proteins, or other structures), which may enable a more reliable and accurate classifier.

While we have implemented several classical models and LSTMs, it may be worth building a more modern NLP-based model, such as using transfer learning from BERT, then fine-tuning that model using this dataset.
