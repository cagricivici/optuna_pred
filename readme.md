# Optuna Optimization:
- Every model has strong and weak points. When you train the model, they come together. What is the most tricky poins is that you can assume the model working better looking at metric results, but it cannot reflect the certain success in test. Because it may convey that your model went 'overfitting'. It happened to me in this study a lot.
- Addition to that, after using optuna, since metric results are more or less the same, test prediction goes better and better. Here is the magic of blending stragety. Optuna is in charge of optimizing the parameters model is using. But as the nature of model type, for example LGB has better points which XGB does not. The opposite points are also present in vice versa. In blending the predictions, I want the results to cancel each other at this weakness or make the strong point even more succesful.
- First of all, I tried to find best parameters while using Optuna. Trial searched the best one by focusing at the metric that objective method returns.
- After finding the best results, I let the model predict at the same time. But critical point is that:
* Stratified CV is used here because my data is imbalance. In every fold, i let the model predict test data then save it.
* I let them have done until the very end of K Fold. It continued at every model i put them into system. Every model create their own prediction in each fold as well.
   - For example: 3 model will search while 5 fold running -> it gives me 15 different test prediction which can be awesome in blending strategy.
 
Takeaways for me: I was assuming that predicting the test data can be misleading when each model's training is not done yet. However it is giving me the model variety. Because in each fold, different model would be training then predicting. If it contributes some weakness into predictions, next model runs can cancel it. It is **the magic of blend strategy**. 

On the other hands, If you let the model finish its own training, the final model might have been overfitted. Because I have allowed the model see all training data. With the help of stratified kfold, the model predicts the test data until it sees the all training data.

- Also we could use random search cv or grid search but the way they are working is not intelligent. They search randomly also *they are not able to think that it went better or worse*, that's why they dont direct the search into wise direction. But Optuna can do that. Optuna is looking forward to seeing the metrics that the model gave. If it catches good results, then it go deeper into that direction. With the formal way to say: *Optuna focuses the search in those promising regions, rather than exploring blindly.*

I truncated my code so that it will focus on only optuna optimizations.
