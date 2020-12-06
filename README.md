# moa_kaggle2020
In this competition, many people trained their models with `SmoothBCEwLogits(smoothing =1E-3)` (or with other similar smoothing values).

I did that and, I think, **it was one of my biggest mistakes!**. 

As Chris Deotte says in [3rd Place Public - We Should Have Trusted CV - 118th Private](https://www.kaggle.com/c/lish-moa/discussion/200614), **we Should Have Trusted CV!** but I think that **it was a more common mistake than usual due to the BIG difference between the [Public Test and the Private Test](https://www.kaggle.com/c/lish-moa/discussion/200832)**.

First, I have been trying to understand **why the hosts decided to create a public test with a different distribution to the private test, and why they did not publish 'train_drug.csv' at the beginning of the competition**. 

I am not complaining about that because I know it is typical in Kaggle competitions, but I would like to expose it because many competitors spent a lot of time (and wasting a lot of submissions) trying to understand the relation between the Local CV and the Public LB.

After the publication of 'train_drug.csv' and, with the excellent @Chris's code [Drug and Multilabel Stratification Code](https://www.kaggle.com/c/lish-moa/discussion/195195), competitors were able to make better decisions. 

However, in my case, the big difference observed in LB Public with different Smooth Values and the bad correspondence between CV and Public LB, **laid me to the wrong decision of using SmoothBCEwLogits(smoothing =1E-3)**.

![](https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F276462%2F4366a1912ce1a2f9d0a15ceebe6dd3a0%2Fold_ann_smooth.png?generation=1607256086941517&alt=media)

The next figure shows my best three models (ANN, RESNET and TABNET) trained with smooth=1E-3 and without smooth. 

"4x4 10-CV LogLoss" corresponds to the logloss of the average of oofs using 4 different distribution of folds with 4 different seeds.  (and with 10-fold CV). The fold distributions were obtained with the @Chris code  [Drug and Multilabel Stratification Code](https://www.kaggle.com/c/lish-moa/discussion/195195) but using this version: ` vc1 = vc.loc[(vc==6)|(vc==12)|(vc==18)].index.sort_values()`.

![](https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F276462%2F58417ef758d18bb794e51461fe6d2981%2Fchris_validation.png?generation=1607256976822676&alt=media)

The weighted blending of the three models with smooth=1E-3 gave me 0.01614 but **I could have obtained 0.01609 without smoothing**.

My second and best submission was using the best model's parameters with @Chris code but trained with the original random stratified CV (the old CV).  In other to combine the models, I used the same weights that minimized the CV in @Chris validation code. **This weighted blending gave me my best Private LB (0.01612) but, without smoothing, I could have obtained 0.01607**, close to the gold zone.

![](https://www.googleapis.com/download/storage/v1/b/kaggle-forum-message-attachments/o/inbox%2F276462%2F315f056f71aa27f8e20b58c7844f2052%2Frandom_validation.png?generation=1607259248045328&alt=media)
