# WS
WS_project


QUESTIONS: In task 4, is your prediction good or bad? Why? If good, what steps do you need to implement the model? If bad, what steps do you need to improve? How do you know you have done enough improvement? Lay out the steps and justify them as if you are presenting to your manager.

We have evidence to believe that the prediction is good. Final prediction accuracy on the test set (86.3%) compares well with established leaderboards (79%, https://www.kaggle.com/c/twitter-sentiment-analysis2/leaderboard). It’s important that the test dataset (“evaluate.csv”) was not used during training of the model. As long as the training and test datasets are representative of the tweets that we would want to run this model on in the future, then we have good reason to believe that the model will be useful in predicting sentiment for any future tweets. Overfitting is likely not an issue since loss on a validation set (a subsample of the training set) continued to drop throughout training (final accuracy = 86.0%), and was similar to performance on the test set (accuracy = 86.3%).

However, there are some issues that should be addressed before we put the model into production, and there are some likely methods we could use to improve the model even more. Above I mentioned that the model will be useful as long as the training and test datasets are representative of future tweets we’d like to predict on. However, the training and test sets have a class imbalance issue… there are many more ‘neutral’ tweets examples compared to ‘positive’ and ‘negative’ example tweets. This imbalance is drastically more pronounced for the test set (~6x fewer positive and ~20x fewer negative examples compared to neutral). Since the distribution of the training set and test set are drastically different, we have to assume that either or both datasets are not a representative sample of future tweets that we want to use the model to classify. This is an important issue, since the model will predict reflecting the balance of classes in the data fed into it, and we need that balance to be representative of the real world (aka the future tweets we are trying to predict on). Investigating this, and potentially drawing new training and test sets for training, would be the first steps I would work on next in this project.

There are a few additional steps we could take to improve model accuracy: 
1) Pretrain with more data: Transfer learning works best when there is as much data to pretrain on as possible. The model I use is pretrained on the Wikitext103 dataset, which is based on 100s of thousands of Wikipedia articles. I then fine-tune the language model on the tweets dataset that was provided. The weights from this model are then used as the backbone of a classification model for sentiment analysis, also trained on the tweets dataset. However, the tweets dataset that was provided includes only 160K tweets. There are other publicly available datasets (e.g. https://www.kaggle.com/kazanova/sentiment140, with 1.6M tweets), that contain much more very similar data. This data could be used both in the fine-tuning of the language model, and initial pretraining of the classification model before it is finally fine-tuned on the provided tweets dataset, to improve transfer learning and final accuracy.
2) Hyperparameter optimization: Optimize the hyperparameters through either a grid search or random search through reasonable ranges of values for hyperparameters including learning rate, momentum, etc., to find values that produce best final training results.
3) Ensembling: Train multiple types of neural networks (e.g. LSTMs and transformers, with different model sizes and complexities), and use ensembling to combine the predictions from each for an improved final prediction.


Submission:
	Model weights located here: https://s3-us-west-2.amazonaws.com/dbressbuck/data/WS/classifier_weights_001.pth
	Evaluation metrics: (Table of metrics presented in code located above)
	    Negative log-likelihood loss (NLL):
	        Final validation set loss:  0.381
	    Accuracy:
	        Final validation set accuracy: 86.0%
	        Final test set accuracy: 86.3%
	 
