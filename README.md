Beyond the binary: predicting the ideology of political tweets and newsletters
By Gabe Dorner and Ryan Foley
June 11, 2024

Question
Social media posts, especially on X and Facebook, as well as email newsletters are necessary in connecting politicians with their constituents, updating them on their party’s legislative agenda, asking for campaign donations etc. In an era of growing hyperpartisanship and social media usage for political discourse, we seek to answer the question if a deep learning model can predict a political X post’s (to which we will hereafter refer as ‘tweets’) discrete ideology score, as well as its ideological group on a scale between one  (very liberal) and seven (very conservative).



Theory
	For this project, we took inspiration from Daniel Preotiuc-Pietro’s paper: Beyond Binary Labels: Political Ideology Prediction of Twitter Users. The study used Word2Vec, LIWC, emotions, political terms, and unigrams to draw conclusions about different groups’ word choice and political engagement, as well as predicting the ideological grouping—again, from one to seven—of tweets. The study found these group predictions more nuanced and challenging than previously reported, due to the fact that X users in their dataset did not post about politics as much as they expected. Another issue with our unidimensional concept of ideology is: what does it mean to be ‘extreme’ (i.e. a one or a seven)? The paper had a difficult time with this as ‘self-reported political extremity is more an indication of political engagement than of ideological self-placement’ (Preotiuc-Pietro et al., 2017, p. 9). This certainly calls into question whether the paper was studying ideology at all.
 
Based on their findings, our project made a few changes to their design—which will be explained in the following section. Furthermore, we made the following hypotheses: (1) since we are using politicians’ tweets, we expect our models to outperform those in Preotiuc-Pietro’s paper;  (2) we expect a politician’s  tweets to be easier to predict than their newsletters, due the fact that tweets will be to the point and newsletters may lose their ideological cues in a sea of random information; (3) RoBERTa will outperform Word2Vec in ideological predictions.


 
Data & methods

We sourced our X data from GitHub (2018 and 2022 , and our newsletter data from DCInbox from 2021 until the end of May 2024. We combine this with Voteview’s DWNominate (dim1) scores, which score politicians on a continuous liberal-conserivative scale for how they vote on economic/redistribution issues. We used this as our ground truth for ideology in order to train our models.
 
Our first method, Word2Vec, was used in the previous study and we applied it in our scenario for comparative purposes. We used NLTK’s TweetTokenizer to tokenize the Twitter data and used the tokenized list of words to train the Word2Vec model. For this, we used a window size of  5, vector size of 100, and a minimum word instance of 100. The vector size of 100 was chosen as a compromise between computational efficiency and predictive performance. The other parameters were tweaked slightly but their effect on performance was minimal. For example, a minimum word instance of 500 did not change the performance at all. Using k-means clustering, we obtained 500 spectral clusters based on the vectors from our trained Word2Vec model. In order to predict the ideology of Twitter users, we had to represent each tweet as a fraction of the clusters its contents belonged to. Similarly, we represented each user as the aggregate distribution of their tweet-cluster fractions. We then used a logistic regression method utilizing the SAGA solver in order to train a classifier model. 
In addition to Word2Vec, which was used in Preotiuc-Pietro’s study, we used the pretrained model RoBERTa from Facebook AI to make predictions about the ideology of a political newsletter or tweet. In evaluating the models, we have acquired data on model performance as both a regression on the continuous ideology scale between negative and positive one, as well as accuracy scores for the seven ideological groups.

Our key improvements to Preotiuc-Pietro’s study are that we have a solid and meaningful ground truth by using the DWNominate data. Additionally, RoBERTa should be better suited to performing the ideology predictions than Word2Vec.



Findings

  Word2Vec: Testing on 20% of our data, we obtained accuracy scores that were as high as expected in some cases and higher than expected in others. For our binary prediction, a 97% accuracy was reported, which is exactly what Preotiuc-Pietro’s study found. However, when predicting the ideology of legislator twitter accounts on a seven-way scale, our model achieved a 76% accuracy score, while previous results have hovered around 30%. This is interesting, but it is likely due to our data, not improved methods. Previous studies have used random twitter accounts while our data consisted of politically engaged congress members, which is easier to predict ideologically than a random sample of users. Legislators are more likely to tweet information that is aligned with their ideological viewpoint, while an everyday user is less likely to talk about politics or relay their ideology in a concise manner. 
  
  RoBERTa: Predicted ideology of newsletters on discrete 1-7 scale with an accuracy of 91%, but struggled with twitter data (about 60% accuracy).



Language Analysis:
  Democrats were much more likely to tweet about issues such as health care, civil rights, women’s rights, LBTQ+ rights, gun control/violence, inequality, etc.
  Republicans were much more likely to tweet about issues such as Cuba, FoxNews, America-first, inflation, COVID-mandates, stolen election claims, tax-cuts, etc.

First Few Words of the Most Common Topics by Party:

Republican Topics:

Topic 0: people US need Ukraine FoxNews one get COVID China coronavirus letter Texas would

Topic 1: Act Pelosi Senate bill House Democrats vote pass Speaker Cruz Nancy Court legislation partisan

Topic 2: country day nation Today women honor service family Thank law us years every Happy lives

Topic 3: Alaska states weekend Day Election companies production July learning development safely rally

Topic 4: today great vote realDonaldTrump election County Great tonight Congratulations sure morning last day

Topic 5: students COVID19 know President one Trump like school education public Afghanistan story

Topic 6: Iowa people New House Republican Watch back Chairman GOP statement American America full

Topic 7: Biden American Americans Democrats border President crisis America energy people policies Joe inflation

Topic 8: support help work Thank forward working Congress need today great Senate state look

Topic 9: health care businesses small children EdLaborGOP North veterans schools protect workers fight kids rights


Democrat Topics:

Topic 0: Trump President people realDonaldTrump would Republicans GOP American administration president want Court Americans

Topic 1: Florida us Capitol today Join ossoff hearing Michigan tonight Watch Democratic live Please

Topic 2: school students children parents information kids Health free student food program residents available apply

Topic 3: vote make get voting ballot sure election today voters November right days help

Topic 4: need help Act communities bill support families crisis workers funding jobs climate economy working

Topic 5: Thank great today Great Thanks forward work community support join everyone see morning

Topic 6: Happy Day celebrate equality celebrating workers happy Today rights discrimination love Get

Topic 7: care health women fight rights access protect right fighting deserve affordable Medicare Security Social need

Topic 8: work country years people day families us proud Today American every honor first working nation

Topic 9: House Senate Act gun violence must bill pass passed law legislation time Rubio




Conclusion:

Hypothesis 1: Correct.
Hypothesis 2: Super incorrect. Further research needed to understand why roberta is bad at X data and so good at newsletters.
Hypothesis 3: Incorrect, but we did not analyze newsletter data with Word2Vec.

 Our study explored the feasibility of predicting the ideological leanings of political tweets and newsletters using Word2Vec and RoBERTa. We were inspired by Daniel Preotiuc-Pietro's work, and our approach incorporated several improvements, including the use of DWNominate scores as a firm ground truth for ideological positions.
 
  Our findings revealed that while Word2Vec provided significant accuracy in predicting binary ideological categories and performed notably well in distinguishing among seven ideological groups, its predictions were likely enhanced by the politically engaged nature of our dataset. RoBERTa, on the other hand, excelled in predicting the ideology of newsletters but struggled with tweets, suggesting a complex relationship between text format and ideological signaling. Further analysis or tweaking may improve RoBERTa’s performance with tweets.
  
  Notably, our hypotheses were partially validated: we confirmed that using politicians' tweets yielded higher accuracy compared to Preotiuc-Pietro's results. However, hypotheses (2) and (3) were incorrect. RoBERTa struggled with tweets compared to newsletters and Word2Vec outperformed RoBERTa. Our results underscore the potential of deep learning models in political text analysis while also pointing to the necessity for further research to understand the limitations and contextual factors affecting predictive performance.
