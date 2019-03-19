# Stack Overflow: Tag Prediction
<h1>1. Business Problem </h1>
<h2> 1.1 Description </h2>
<p style='font-size:18px'><b> Description </b></p>
<p>
Stack Overflow is the largest, most trusted online community for developers to learn, share their programming knowledge, and build their careers.<br />
<br />
Stack Overflow is something which every programmer use one way or another. Each month, over 50 million developers come to Stack Overflow to learn, share their knowledge, and build their careers. It features questions and answers on a wide range of topics in computer programming. The website serves as a platform for users to ask and answer questions, and, through membership and active participation, to vote questions and answers up or down and edit questions and answers in a fashion similar to a wiki or Digg. As of April 2014 Stack Overflow has over 4,000,000 registered users, and it exceeded 10,000,000 questions in late August 2015. Based on the type of tags assigned to questions, the top eight most discussed topics on the site are: Java, JavaScript, C#, PHP, Android, jQuery, Python and HTML.<br />
<br />
</p>

<p style='font-size:18px'><b> Problem Statemtent </b></p>
Suggest the tags based on the content that was there in the question posted on Stackoverflow.

<p style='font-size:18px'><b> Source:  </b> https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/</p>

<h2> 1.2 Source / useful links </h2>

Data Source : https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data <br>
Youtube : https://youtu.be/nNDqbUhtIRg <br>
Research paper : https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tagging-1.pdf <br>
Research paper : https://dl.acm.org/citation.cfm?id=2660970&dl=ACM&coll=DL

<h2> 1.3 Real World / Business Objectives and Constraints </h2>

1. Predict as many tags as possible with high precision and recall.
2. Incorrect tags could impact customer experience on StackOverflow.
3. No strict latency constraints.

<h1>2. Machine Learning problem </h1>

<h2> 2.1 Data </h2>
<h3> 2.1.1 Data Overview </h3>

Refer: https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data
<br>
All of the data is in 2 files: Train and Test.<br />
<pre>
<b>Train.csv</b> contains 4 columns: Id,Title,Body,Tags.<br />
<b>Test.csv</b> contains the same columns but without the Tags, which you are to predict.<br />
<b>Size of Train.csv</b> - 6.75GB<br />
<b>Size of Test.csv</b> - 2GB<br />
<b>Number of rows in Train.csv</b> = 6034195<br />
</pre>
The questions are randomized and contains a mix of verbose text sites as well as sites related to math and programming. The number of questions from each site may vary, and no filtering has been performed on the questions (such as closed questions).<br />
<br />
__Data Field Explaination__

Dataset contains 6,034,195 rows. The columns in the table are:<br />
<pre>
<b>Id</b> - Unique identifier for each question<br />
<b>Title</b> - The question's title<br />
<b>Body</b> - The body of the question<br />
<b>Tags</b> - The tags associated with the question in a space-seperated format (all lowercase, should not contain tabs '\t' or ampersands '&')<br />
</pre>

<br />

<h3>2.1.2 Example Data point </h3>

<pre>
<b>Title</b>:  Implementing Boundary Value Analysis of Software Testing in a C++ program?
<b>Body </b>: <pre><code>
        #include&lt;
        iostream&gt;\n
        #include&lt;
        stdlib.h&gt;\n\n
        using namespace std;\n\n
        int main()\n
        {\n
                 int n,a[n],x,c,u[n],m[n],e[n][4];\n         
                 cout&lt;&lt;"Enter the number of variables";\n         cin&gt;&gt;n;\n\n         
                 cout&lt;&lt;"Enter the Lower, and Upper Limits of the variables";\n         
                 for(int y=1; y&lt;n+1; y++)\n         
                 {\n                 
                    cin&gt;&gt;m[y];\n                 
                    cin&gt;&gt;u[y];\n         
                 }\n         
                 for(x=1; x&lt;n+1; x++)\n         
                 {\n                 
                    a[x] = (m[x] + u[x])/2;\n         
                 }\n         
                 c=(n*4)-4;\n         
                 for(int a1=1; a1&lt;n+1; a1++)\n         
                 {\n\n             
                    e[a1][0] = m[a1];\n             
                    e[a1][1] = m[a1]+1;\n             
                    e[a1][2] = u[a1]-1;\n             
                    e[a1][3] = u[a1];\n         
                 }\n         
                 for(int i=1; i&lt;n+1; i++)\n         
                 {\n            
                    for(int l=1; l&lt;=i; l++)\n            
                    {\n                 
                        if(l!=1)\n                 
                        {\n                    
                            cout&lt;&lt;a[l]&lt;&lt;"\\t";\n                 
                        }\n            
                    }\n            
                    for(int j=0; j&lt;4; j++)\n            
                    {\n                
                        cout&lt;&lt;e[i][j];\n                
                        for(int k=0; k&lt;n-(i+1); k++)\n                
                        {\n                    
                            cout&lt;&lt;a[k]&lt;&lt;"\\t";\n               
                        }\n                
                        cout&lt;&lt;"\\n";\n            
                    }\n        
                 }    \n\n        
                 system("PAUSE");\n        
                 return 0;    \n
        }\n
        </code></pre>\n\n
        <p>The answer should come in the form of a table like</p>\n\n
        <pre><code>       
        1            50              50\n       
        2            50              50\n       
        99           50              50\n       
        100          50              50\n       
        50           1               50\n       
        50           2               50\n       
        50           99              50\n       
        50           100             50\n       
        50           50              1\n       
        50           50              2\n       
        50           50              99\n       
        50           50              100\n
        </code></pre>\n\n
        <p>if the no of inputs is 3 and their ranges are\n
        1,100\n
        1,100\n
        1,100\n
        (could be varied too)</p>\n\n
        <p>The output is not coming,can anyone correct the code or tell me what\'s wrong?</p>\n'
<b>Tags </b>: 'c++ c'
</pre>

<h2>2.2 Mapping the real-world problem to a Machine Learning Problem </h2>
<h3> 2.2.1 Type of Machine Learning Problem </h3>
<p> It is a multi-label classification problem  <br>
<b>Multi-label Classification</b>: Multilabel classification assigns to each sample a set of target labels. This can be thought as predicting properties of a data-point that are not mutually exclusive, such as topics that are relevant for a document. A question on Stackoverflow might be about any of C, Pointers, FileIO and/or memory-management at the same time or none of these. <br>
__Credit__: http://scikit-learn.org/stable/modules/multiclass.html
</p>

<h3>2.2.2 Performance metric </h3>

<b>Micro-Averaged F1-Score (Mean F Score) </b>: 
The F1 score can be interpreted as a weighted average of the precision and recall, where an F1 score reaches its best value at 1 and worst score at 0. The relative contribution of precision and recall to the F1 score are equal. The formula for the F1 score is:

<i>F1 = 2 * (precision * recall) / (precision + recall)</i><br>

In the multi-class and multi-label case, this is the weighted average of the F1 score of each class. <br>

<b>'Micro f1 score': </b><br>
Calculate metrics globally by counting the total true positives, false negatives and false positives. This is a better metric when we have class imbalance.
<br>

<b>'Macro f1 score': </b><br>
Calculate metrics for each label, and find their unweighted mean. This does not take label imbalance into account.
<br>

https://www.kaggle.com/wiki/MeanFScore <br>
http://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html <br>
<br>
<b> Hamming loss </b>: The Hamming loss is the fraction of labels that are incorrectly predicted. <br>
https://www.kaggle.com/wiki/HammingLoss <br>


## Conclusion: 

#### What we did throughout this experiment:

The objective of this experiment was to suggest tags based on the questions that are posted in StackOverflow.
StackOverflow, as we know, is a website which serves as a platform of millions to programmers around the globe to ask and answer questions. 
There are a wide range of questions that are asked in StackOverflow, from simple computer science questions to the most advance topics in programming. There's
a multitude of different domains that are there in StackOverflow. In order to correctly classify a question to it's correct domain, StackOverflow uses this wonderful 
system of predicting tags based on the query questions. In this way, questions are given tags which results in the questions being answered by relevant people.
Imagine, we have a question on Python - "What is the Pythonic way to scrap web pages?". Now, based on this questions StackOverflow may give it tags like 'Python', 'Web-Scrapping' etc.
Now, had there been no system of tags, this questions would have been sent to anyone who is a member of StackOverflow. However, since we have these tags 'Python' and 'Web-Scrapping' as 
suggested by StackOverflow, the questions will only go to people who are interested in 'Python' and 'Web-Scrapping'. This increases productivity. It also reduces the hassle of a question being answered 
by an user who doesn't work with Python. Overall, this increases the user experience of all the users who come to StackOverflow.

Here, we are given a dataset which contains questions from almost 6 million users. Each question has corresponding tags associated to it. Based on the given data, we have to build a system which 
will predict tags based on new unseen questions. Each question is described by the following features - 'Title', 'Body', 'Tags'. The title and body text will be used to predict the tags. Remember, there
may be more than one tags associated to a question. So it's not only a multiclass problem, but it's also a multilabel problem. In such a scenario the best metric that we would chose is the Micro Average F1 Score.
We have chosen this metric to make use of the weighted average of the F1 score of each class. Micro averaged F1 score calculates metrics globally by counting the total true positives, false negatives and false positives. 
This is a better metric when we have class imbalance. We have to keep the following things in mind:

1. We have to predict as many tags as possible with high precision and recall.
2. Incorrect tags could impact customer experience on StackOverflow.
3. There is no strict latency constraints, which means given a query point, the program can take few minutes to correctly predict the tags.

The first thing we have done is perform some basic Exploratory Data Analysis on the given dataset. EDA is an important step to understand which features are important in the context of the
problem and which features are not. In the EDA section we got information about the following: 

1. Number of times each question appeared in the database
2. Distribution of number of tags per question. (Avg. number of tags per question: 2.899440)
3. Total number of unique tags present in the dataset (42048)
4. Number of times each tag has appeared across the entire dataset. 
5. c#, java, php are the most frequent tags in StackOverflow. (c# occurs 331505 number of times)

From, what we have observed till now is each question can be associated to multiple tags. So we can treat this as a multilabel problem. Afer the EDA is done, we have performed some data cleaning tasks.
Due to limitation in computing power, we will sample only 1 million data points. In the cleaning phase, we have seperated the code snippets from the body, we have removed special characters from
the title as well as the body. We have removed all the necessary stopwords, we have used Snowball stemmer to stem the words.

After the data cleaning step has been done we will plot a variance plot which explains how much information across the entire dataset is retained with the number of tags. Rememeber there are 42000 
unique tags that we have. We see that more than 99% variance is retained with number of tags = 5500 and more than 90% variance is retained when we use 500 tags. On building ML models with 5500
tags we see that it takes roughly 6-8 hours to train a Logistic Regression model. So, for the sake of time, we will use only 500 tags to train all our future models.

We will convert each of these 500 tags to binary outputs using the Count Vectorizer object of scikit learn. We will sample 0.5 million data points with 500 tags, and 3 times more weight added
to the titles. We have featurized the data using TFIDF and BOW (1-4 grams) vectorizers and use several models with Hyperparameter tuning to get the highest value of micro f1 score. We will use a simple Logistic
Regression model, we will use SGD Classifier with both 'log' loss and 'hinge' loss. At the end we have done a comparison between all the models that we have used to conclude which one actually
performed better for our problem. It turns out that the simple Logistic Regression model works best for our problem set. Since, this is a multilabel problem we will use the OnvVsRest classifier 
along with our base estimators to train the model. Please find below the list of models that we have trained along with their class precision, recall values and micro f1 score.	











