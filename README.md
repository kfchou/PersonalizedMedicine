# Introduction
 Genetic testing is improving how diseases like cancer are treated everyday. Sequencing cancer tumors reveals thousands of genetic mutations. Challenge remains to distinguish the cancer-causing mutations from the silent ones. Advancing our understanding of cancer and designing more precise and effective treatments holds much promise for the patients. To this end, we created a model to speed up a time-consuming manual workflow of a clinical pathologist by classifying genetic mutations from text-based clinical literature.

# Data Source
Memorial Sloan Kettering Cancer Center has created an expert-annotated precision oncology knowledge base that contains several thousand annotations clinically actionable genes (available on [Kaggle](https://www.kaggle.com/c/msk-redefining-cancer-treatment/data?select=training_variants.zip)). Each genetic variation is placed into 1 of 9 categories, persumably corresponding to a treatment type. Our goal is to build an ML model to classify these annotations 

# Main Results
Our aggregate results show that Doc2Vec clearly outperformed Bag of Words and TF-IDF embeddings. Interestingly, there is no difference in performance across the various classifiers, either classical machine learning or deep learning.

# Actionable Findings
We found that within the dataset, several gene variations did not have associated text descriptors. Additionally, several genetic variations share the same text descriptors, which may or may not belong to the same class. These issues with the dataset make it difficult for both humans and machines to correctly label similar genetic variations. Based on this, **we recommend augmenting the dataset to fill empty text descriptors, and adding data to prevent different genetic variations belonging to different classes from having the same text descriptor.** This can be done by scraping research literature from the web, such as via pub med.

Our classification results found that several classes appear highly correlated (classes 1&4, classes 2&7), and hence easily confused. This result was observed in multiple text embedding schemes (Add figure). In order to improve classification performance, **we recommend exploring methods to decorrelate these classes.** This may be accomplished by obtaining more data for these classes via web scraping.

# Recommendations for Performance Assessment
Due to the nature of this multiple classification problem and the fact that the data was very unbalanced, it is inappropriate to use accuracy or log-loss as metrics to measure performance of any of the machine learning or neural network approaches. Rather, ROC curves, which indicate the proportion of true positive and false positive rates, is a more appropriate measure.

# Other thoughts
It is unclear what each "class" represents. Hence it is difficult to understand, based on the accompyning literature, what defines a class. Having this knowledge may help design custom features (e.g., key words or words that identify relationships between genes, proteins, or other structures), which may enable a more reliable and accurate classifier.

While we have implemented several classical models and LSTMs, it may be worth building a more modern NLP-based model, such as using transfer learning from BERT, then fine-tuning that model using this dataset.
