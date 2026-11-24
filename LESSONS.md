# FRAUD DETECTION PIPELINE - PAYSIM SYTHENTIC DATASET

# END-TO-END FRAUD MODEL: TIME-BASED VALIDATION, CALIBRATED THRESHOLD, DRIFT MONITORING, API AND DEPOLYMENT

## Day 1:
Created repo
smoke test data
pushed to repo

--------------------------------------------------------------------

## Day 2:
'''
RESULT OF PAYSIM

isFraud
0    6354407
1       8213

Accuracy of model that never flags a transaction as fraud: 0.9987091795518198
Frauds caught by model: 8213
'''

### 1 Why can a fraud model with 99.9% accuracy be worthless?
Since the model is to predict the frauds (versus no frauds), the class 'frauds' is highly outbalanced by 'no frauds' (0.13% v 99.87%). So, a classifer can just predict its not fraud all the time and still get it right 99.87% of the time. Hence, accuracy will not be a good metric when the class is highly imbalanced, and dominated by one or more classes.

### 2 What does precision mean for a fraud analyst reviewing alerts?
Precision tells us - out of all the frauds, how  many did we correctly guess i.e., my model has predicted N frauds. Out of those, how many were actually frauds and how many 'not frauds' were predicted as frauds. A fraud analyst would be getting false fraud alerts when the precision is low.  

### 3 What does recall mean for the business?
Recall tells us - of all the actual frauds, how many did the model actually predict correctly, i.e., how many were correctly identified as frauds, and how many actual frauds were identified as non-frauds. For a business where even a small amount of frauds cost significantly higher costs, it is important to have recall so that the no frauds are misclassified as non-frauds.

### 4 What does moving the threshold up or down do to precision and recall?
Increasing either one tradeoffs the other. Having high precision comes with increase in False Negatives, thus decreasing Recall while high recall comes with increase in False Postives this decrease in Precision.

### 5 Why is a precision-recall curve more informative than ROC when fraud is 0.13% of data?
ROC plots False Positive Rate (FP / (FP + TN)) v True Positive Rate (TP/ TP+FN). False Positive Rate is diluted by the massive number of True Negatives (TN), making the model look strong when positives are rare ie., classess are highly imbalanced. 
On the other hand, prcision-recall curve plots PR (TP/ TP+FP) v Recall (TP/TP+FN) so helps us choose the correct trade-offs, here Recall. So if we choose a threshold with high recall, then we can see how many false alerts we get since the precision will  drop.


----------------------------------------------

### The following is from another project that i worked on, with Faults A, B, C, D:
|Class|	Support|	TP (≈)|	Predicted (≈)|	FP (≈)|	FN (≈)|	Precision|	Recall|	F1
|none|	5,775|	5,763|	5,985|	222|	12|	0.963|	0.998|	0.981|
|A|	759|	|662	|803	|141	|97	|0.824	|0.872|	0.848|
|B|	615|	|291	|529	|238	|324	|0.550	|0.473|	0.509|
|C|	615|	245	|446	|201	|370	|0.549	|0.398	|0.462|
|D|	486|	486|	486|	0|	0|	1.000|	1.000|	1.000|





Correct fault rows: 291 + 245 + 662 + 486 = 1,684
Misclassified fault rows: 324 + 370 + 97 = 791 = 220 labelled "none" + 571 wrong fault type

Here are my takeaways from this project w.r.t classification results
### Lesson 1: accuracy flatters imbalanced data. 
A model that says "none" every time scores 5,775 / 8,250 = 70.0% accuracy, 
but I got macro F1 of only 0.165 . While my model says 90.3% accuracy, the macro F1 says 0.760. Accuracy rose 20 points while three of the five classes remain weak. 
Weighted F1 (0.896) is almost the same as accuracy, because both are pulled toward the large "none" class. Macro F1 treats every class equally, which is why I would rely on this.

### Lesson 2: Precision and Recall
C: precision 0.549, recall 0.398. When it says "C" it is right 55% of the time, but it only finds 40% of real stuck faults.
A is the opposite: recall 0.872 is higher than precision 0.824. The class is liberal and collects 141 false positives.
So which error costs more, a missed fault or a wrong label?
In my project's context, wrong label has more consequence than missed fault because it required fixing, hence having a good value get fixed not only let goes of the bad one but also damages the good one.

### Lesson 3 - F1 
F1 is a harmonic mean, so it punishes imbalance. 
F1 for B is: 2 × 0.550 × 0.473 / (0.550 + 0.473) = 0.509. 
A low value on either precision or recall drags F1 down, unlike an arithmetic mean.

### Lesson 4: micro, macro and weighted averages

For single-label multiclass, accuracy equals micro-averaged precision, recall and F1.
Macro asks "how well do I do on an average class?" and weighted asks "how well do I do on an average row?".
Macro Average matters when classes are rate.