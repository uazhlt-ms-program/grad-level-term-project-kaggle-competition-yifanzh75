# Competition Summary: Movie/TV Review Classification

## Task summary

This project focuses on classifying the reviews of a movie/TV show as positive or negative or non-review. 
Reviews are simple texts with ID and their true labels attached in the training dataset. 
The training dataset contains 70317 rows while the test dataset contains 17580 rows. 
Both datasets have two columns ID and TEXT while the training also includes the column of true LABEL.
The final result will be an output file containing ID and the predicted label generated from the test dataset. 
F1 score will be calculated to measure the performance of the fitted model.
One challenge of the task is that the problem is not a simple binary classification problem. 
Except for classifying the reviews as positive or negative, the model also needs to identify texts that are not reviews for a movie or TV show.
Texts with label 0 in the training dataset may contain words that have positive or negative meanings that lead the model to incorrectly classify them to the other two groups. 
However, the text are actually the reviews for other things instead of a movie/TV show.
This project uses methods such as TF-IDF feature extraction, n-grams and logistic regression classifier.

## Exploratory data analysis
The training dataset contains 70317 rows while the test dataset contains 17580 rows. 
Both datasets have two columns ID and TEXT while the training also includes the column of true LABEL.
Label 0 means it is not a movie/TV show review.Label 1 means it is a positive movie/TV show review, and label 2 means it is a negative movie/TV show review. 
For example, ID 70037 wrote 
"This is the worst movie I have seen in years...it is, sort of, a takeoff of the TV show "Southland" which I think is an excellent series.<br />Even paying the $2.99 with my Prime membership was a waste of money.<br />I did watch the preview first and thought it was questionable...so I thought the movie would be better...not so.<br />So, in my opinion...don't waste your time on this one..." which is labeled as 2. 
ID 70056 wrote 
"Great read as usual." which is label as 0. 
ID 69604 wrote 
"I got a chance to see this movie at an early screening in Brea and I have been crazy for it ever since. The film is based on Shakespeare's Twelfth Night which I have read and loved and seen on stage a few times so I certainly liked the references. But whether you like Shakespeare or not it won't matter - the movie stands on it's on. It is super funny, witty and charming. Amanda Bynes is hilarious and so was David Cross. Actually the whole cast is great - I just happen to be a huge David Cross fanatic. The cast is hot and the soundtrack kicks lots of cool bands and a few I hadn't heard before but I know they have a CD coming out so I will definitely buy it. Everyone in our audience laughed from start to finish - all age groups. !!!!" which is label as 1.
Out of 70317 rows, 32289 rows are labeled 0, and 19139 rows are labeled 1, and 18889 rows are labeled 2. The dataset is a little unbalanced as the non-review rows are almost double of the size of the positive or negative rows. 

## Approach
a) Data pre-processing 
Missing texts from both training and test datasets are filled with empty strings. 
Texts are converted to the string type. 
HTML character references are converted to normal characters. 
Useless whitespaces are removed.

b) Model Building 
Using unigram and bigram TF-IDF features. 
Removing overly common words and really rare words. 
Building multinomial logistic regression with a balanced class weight. 

## Results
The result csv file has been uploaded to Kaggle site and gets a score of 0.92683.
To evaluate the robustness of the model, a stratified 5-fold cross validation is applied. Stratification method is used to preserve the distribution among different labels.
Two evaluation metrics are display below:
Accuracy score: 0.9331598645927552
Macro F1: 0.9233553605924273
Both high scores across folds indicate that the model fits well and is robust enough to present. 

## Error analysis
After fitting the model with the held-out data, some errors are identified.

a) Non-reviews and reviews
Example: ID 7577 wrote 
"This game is full of levels, characters, and has a great story line. Chaos Bleeds was originally an episode in season 5 but never made it. Basically if you watch all of the story line parts it's almost like a new buffy episode. Of coures you have to beat the game to see the what happens. I beat it and will probably play it again to discover more secrets and extras! I think everyone who loves these kinds of video games will love this one. You don't even have to be a buffy fan to play. A+ Game!"
This is a review of a video game, but it is mistakenly identified as a positive movie/TV review. This may be because this type of review contains words such as “characters” and “story line”, which usually appear in movie or TV show reviews.
Example: ID 494 wrote 
"The mail people had to send this copy of dark descent back/ THey said it was not deliverable."
This is a positive review; however, the model classifies it as a non-review. It suggests that the model struggles with ambiguity in the text as it seems to focus on "the mail people". 

This type of error between non-reviews and reviews usually occurs with very short text, indicating that not enough information is provided for the model to correctly differentiate the label.

b) Positive and negative reviews
Example: ID 59553 wrote 
“if any of you are from staten island you could see the guy choping the sign that almost reads staten island when he is on his way too n.y. to see get too his kids”
The true label is positive, but it is classified as a negative review. There are several typos, and the sentence is not grammatically correct which could confuse the model. The sentiment is also not obvious to identify.
Example: ID 52919 wrote 
“This is the best movie I've seen since I got the scope at the proctologists office.............this morning.”
The true label is negative, but it is classified as a positive review. This is a good example showing that the model struggles with human sarcasm because it can only rely on the literal meanings of the text.


## Reproducibility
The Jupyter notebook containing all the code is uploaded to the GitHub page. The dependencies that need to be installed are listed in the code. Step-by-step instructions are also included in each chunk of code chunk. The leaderboard numbers should be reproducible.

## Leaderboard result
A CSV file containing the final results has been submitted to Kaggle website under the name Yifan Zhang.
Leaderboard score is 0.92683.


## Link to code repository
Code repository:


