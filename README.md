# H-M-Kaggle-Competition-Submission

The most interesting part of this competition is we need to generate train and test by ourself, the candinate generation strategy is the key to beyond limitation of accuracy ,good feature engineering or modeling could be close to the limitation .Our solution is using various retrieval strategies + feature engineering + GBDT, seems simple but powerful.

We mainly generate recent popular items because fashion changing fast and has seasonality,tried to add cold start items but they will never be ranked to top12 due to lacking of interaction information.
user and item interaction information are always the most important of recommendation problem,the features we created are almost interaction features,image and text features didn't help but should be useful for cold start problem.

Almost 50% users have no transactions in recent 3months,so we created many cumulative features for them,and last week,last month,last season features for active users.

We use 6 weeks data as train,last week as valid,retrieve 100 caninates for each user,it has stable cv-lb correlation.We focus on improving single lightgbm model to the last week, cv is 0.0430 and lb is 0.0362.At last week I have to rent gcp's big memory server and vast.ai's gpu server to run bigger models to get higher accuracy.


### Optimization

- we use TreeLite to accelerate lightgbm inference speed (2X faster),catboost-gpu is 30X faster than lightgbm-cpu inference.
- transform all the categorical features(including two way) to label encoding,use reduce_mem_usage,
- create a feature store,save intermediate features files to dictionary , final features to feather,exsiting features will not be create again
- split all the users to 28 group,inference simultaneously with multiple servers.
