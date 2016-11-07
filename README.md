# Ready For My Close Up

#Abhishek Varma, 2014

Predicting the optimum month to release a movie

Introduction
Movies are serious business. As a multibillion dollar industry, they represent a massive chunk of America’s greatest export, culture. As with any industry, each studio strives to ensure that its investment is recouped and profit is maximized. Many of the decisions made to influence the Return on Investment (ROI) are made prior to the production of the film. These include greenlighting a screenplay, hiring a director, hiring a cast and giving the film a budget. 
However, once the movie is made, the studio still has to face one of its greatest challenges. The two biggest factors that influence the box-office earnings of a film are the marketing budget and the month in which it is released. This study will try and determine the optimum month in which to distribute your film.
A word of caution, however. There will be certain factors that will be very difficult to account for. These primarily include if the film aims to be in award contention (they are released towards the end of the year to gain box office success based on award nominations) or if they are primarily focused on a particular holiday (for example, Christmas themed movies like “Jingle all the way “will be released in December)

Methodology: 
Getting access to the relevant data proved to be more difficult than anticipated. IMDB has an absolutely massive database, but their unique id number has no discernible order. I wrote a script, with the aid of the Python package IMDBpy, which would scan through a range of id numbers and pick the more relevant films. This proved to be inefficient, since a lot of the ids are TV episodes, Games, Adult Films with hilarious titles, or films in which the budget information was not present. 
I then manually got the URLs for 1000 films from the database. I sorted through popular films that had done well in the box office. I didn’t focus on balancing the data set with financial flops since the aim of this research isn’t to predict box office success, but rather what the optimum month of release for your film should be.
Data
I pruned the 1000 titles to include those which had relevant box office information, and did a screen to ensure that every film met a minimum gross per screen that it was released. Certain films, usually blockbusters, release on greater number of screens than others. Since releasing on a greater number of screens is a larger risk, smaller, independents films are not released at the same scale as something with a larger audience like a Harry Potter film, for instance. I ended up with a training set of 700 films and a testing set of 165.
Algorithms 
Since I was trying to determine months of release, which is a discrete attribute, I decided to use classification algorithms. I used Decision trees and a Linear Kernel based SVM to do the classification. The attributes for each film were the genres in binary format, where if IMDB classified it as a particular genre, then it would have a 1, if not it got a 0. For example, the film “Men in Black 3” is given the following genres on IMDB : Action, Comedy, Scifi. 
This translates to :

[1,0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,1]

There were 25 possible genres, and I appended a 1 on the end of all, ending with 26 features.

Execution and results

SVM
After iterating through possible values of the regularization parameter in the SVM
          1       0.00      0.00      0.00         9
          2       0.00      0.00      0.00        15
          3       0.00      0.00      0.00        13
          4       0.00      0.00      0.00        12
          5       0.00      0.00      0.00         8
          6       0.20      0.07      0.11        14
          7       0.20      0.65      0.31        17
          8       1.00      0.07      0.13        14
          9       0.14      0.17      0.15         6
         10       0.50      0.14      0.22        14
         11       0.17      0.33      0.22        12
         12       0.28      0.57      0.37        21

avg / total       0.23      0.21      0.15       155

The SVM classifier failed to predict films released in the earlier parts of the year. I will discuss my suspicions as to why this is so later on. 
However, it was able to perform much better in the latter half of the year. The average precision of the SVM was 0.23, while the average for the latter half of the year was 0.415. This suggests that films that are released during the end of year are more predictable based on genre. This is inline with the r3elease of dramatic films during award season and horror films near Halloween. 
Decision Trees
The decision trees performed better than the SVM. Below is the classification report for the decision trees. 
precision    recall  f1-score   support

          1       0.11      0.22      0.15         9
          2       0.39      0.47      0.42        15
          3       0.11      0.08      0.09        13
          4       0.29      0.17      0.21        12
          5       0.21      0.38      0.27         8
          6       0.31      0.29      0.30        14
          7       0.32      0.41      0.36        17
          8       0.50      0.36      0.42        14
          9       0.00      0.00      0.00         6
         10       0.42      0.36      0.38        14
         11       1.00      0.17      0.29        12
         12       0.38      0.52      0.44        21

avg / total       0.36      0.32      0.31       155


The decision trees seem to be a more effective way to predict the month within the context of this discussion. Again the algorithm performed better during the latter half of the year.

Please note that the confusion matrix of both algorithms are attached as an appendix. However, it is difficult to visually learn anything from them since there are so many classes.


Discussion
As noted earlier, the algorithm performed a lot better for the films in the latter half of the year. Along with the awards consideration and seasonal films, it should be noted that Hollywood is notorious for ignoring the early parts of the year. The spring season is considered to be the weakest time for Hollywood to release their films. Films are usually released in the spring when it is determined that they would be too weak to go against the heavy hitters of the summer blockbuster season, or they will fail to resonate during award season towards the tail end of the year. Therefore, both the algorithms rarely suggested that a film be released early in the year.
Seasonality of Hollywood
It should be noted that there is a concept of the seasonality of Hollywood, where often a decision is made to release a film during a particular season, and then adjusted depending on various factors such as other similar films that the studio might be releasing, or blockbusters that other studios might be releasing during the same time period. In hindsight, I should have divided the classes into 4 seasons rather than 12 months. The accuracy of the results would have increased for both the algorithms, though the bias towards the latter half of the year would have remained. 

Confusion matrix of SVM 
[[ 0  0  0  0  1  1  4  0  0  0  2  1]
 [ 0  0  0  0  0  0  5  0  1  1  4  4]
 [ 0  0  0  0  2  0  6  0  0  0  0  5]
 [ 0  1  0  0  1  0  7  0  0  1  0  2]
 [ 0  0  0  0  0  0  5  0  0  0  1  2]
 [ 0  0  0  0  2  1  6  0  1  0  1  3]
 [ 0  0  0  0  1  1 11  0  1  0  0  3]
 [ 0  0  1  0  2  0  3  1  2  0  3  2]
 [ 0  0  0  0  0  1  1  0  1  0  1  2]
 [ 0  0  0  0  2  1  1  0  1  2  4  3]
 [ 0  0  1  0  2  0  1  0  0  0  4  4]
 [ 0  0  0  0  0  0  5  0  0  0  4 12]]

Confusion Matrix of Decision trees
[[ 2  0  1  0  0  1  4  0  0  0  0  1]
 [ 0  7  0  1  1  0  2  0  0  0  0  4]
 [ 2  3  1  0  2  2  2  0  0  0  0  1]
 [ 1  1  3  2  0  0  2  1  0  2  0  0]
 [ 3  0  0  0  3  0  1  0  0  0  0  1]
 [ 2  0  1  2  1  4  0  1  0  0  0  3]
 [ 4  0  1  0  1  1  7  0  0  0  0  3]
 [ 1  3  0  0  1  0  1  5  0  2  0  1]
 [ 0  2  0  0  1  2  0  1  0  0  0  0]
 [ 2  1  0  0  0  1  0  2  1  5  0  2]
 [ 0  1  0  2  2  0  2  0  0  1  2  2]
 [ 1  0  2  0  2  2  1  0  0  2  0 11]]
