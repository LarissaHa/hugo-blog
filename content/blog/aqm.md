+++
author = "Larissa"
categories = ["Paper", "R"]
tags = ["simulation", "first-differences", "machine-learning", "neural-nets", "regression", "spring2018"]
date = "2018-06-15"
description = "Comparing different ML Approaches on Data on Voluntary Action - Advanced Quantitative Methods - Spring Semester 2018"
featured = "age-volact.png"
featuredpath = "date"
linktitle = ""
title = "Neural Net - The ultimate Answer to all Machine Learning Problems?"
type = "post"

+++

Introduction
============

The buzzwords “Machine Learning”, “Neural Networks” and “Artifical
Intelligence” hover through the air wherever people are working with
large sets of data, also called Big Data. Such approaches are already in
use for friendship recommendations on Facebook, for improving the
quality of your smartphone pictures or for reducing waste by regulating
product orders of supermarkets. They improve predictions wherever there
is enough data to “learn” from. But this trend just starts to be
relevant for other scientific fields, for example in political science.
Even though there are tons of records where people were asked what they
think, how they behave, what they expect, only one set of machine
learning (ML) algorithms seems to be used: the regression.

The goal of this paper is to broaden the view on ML approaches existing
beyond regression, for example, Random Forests, Support Vector Machines,
and Neural Nets, and to compare these algorithms in terms of their
predictive power on a genuine political science topic: participation in
voluntary organizations. Therefore I will have a look at data from the
European Social Survey (ESS) from round 1 and 6 because they both
include a battery of survey questions about voluntary participation.
Based on the theoretical foundations extracted in chapter 1 and the
theoretical explanation of different ML approaches in chapter 2, I will
select and prepare variables (as described in chapter 3) and feed them
into ML algorithms. The results and their implications will be
interpreted in chapter 4. In the end, advantages (and disadvantages) of
the use of machine learning approaches in the field of political science
should be clear as well as why it should be worth for a political
scientist to have a look at these approaches.

Brief Theoretical Overview
==========================

In the broad field of political science, there are subfields with
concepts that can be measured quite well, and there are others, like
social participation. For a long time, this subfield was only viewed as
an attachment for analyzing political behavior, not realizing that this
participation is a distinct field of study on its own (Gabriel and Völkl
2008, 269). With the establishment of the term “civil society” the
literature begins to work on both fields of social and political
participation. As van Deth states, “the distinction between social and
political involvement of citizens does \[…\] rest \[…\] on the primary
goals of the groups involved.” (Deth 1997, 3). When we have a look at
voluntary organizations that are part of the social participation sphere
and of the civil society at the same moment, we may assume similar
mechanisms as for political participation, only assuming different
goals.

To localize the term “activity in voluntary associations” more clearly,
we can look at Dekkers and van den Broeks definition, which describes

> “*voluntariness* as the guiding principle of civil society and
> *associations* as its dominant collective actors. A prerequisite for
> partaking in civil society is *commitment*: the willingness to bind
> oneself to a common course and to take responsibilities” (original
> emphasis) (Dekker and Broek 1998, 13)

This willingness was questioned in parts of the literature, especially
in the American political science sphere, where Putnam stated that
Americans were “Bowling alone” instead of meeting in sports clubs as
they used to have in the past. For Europe on the other hand, no evidence
for such behavior could be found (Gabriel and Völkl 2008, 279), (Dekker
and Broek 1998, 35).

Sports clubs, churches, interest groups, etc. (see (Deth 1997, 1) for a
more detailed list) stand in the focus of the literature, because on the
one hand these associations fill gaps where the state fails to help
(e.g. food sharing, charity organizations), on the other hand the
participating citizens learn values and ways of behavior that are
important for the society in total (for more examples see (Deth and
Maloney 2015, 826)). Some scholars even see associations as “schools of
democracy”, where “they can promote positive feelings towards other
social and political institutions \[…\], towards other individuals
\[…\], and towards oneself as a politically capable actor” (Morales and
Geurts 2007, 135). It is not quite clear, though, if the domain of
active citizens has to be an officially organized group of people or if
work for informal groups of citizens counts as “voluntary activity”, too
(Gabriel and Völkl 2008, 270).

As many different notions of the concept “voluntary association” appear
in the literature, as many influence factors we see, that have an impact
on the willingness to participate. From narrowly defined lists of
factors to broadly defined concept groups (e.g. (Zukin et al. 2006,
124); (Badescu and Neller 2007, 159)), there is one model that seems to
concentrate all possible influences in three simple questions: Are they
able to participate? Do they want to participate? Did somebody ask them
to participate? (Dalton 2014, 64). This split in “resources”,
“engagement” and “recruitment” (Verba et al. 1995, 269) results in the
so-called “civic voluntarism model”, which defines participation
especially in terms of individual influence factors what seems to be
more appropriate in our individualized societies than an analysis in
terms of social class or other social groups (Gabriel and Völkl 2008,
288).

Even if the participation itself and the positive results of
participation are so highly relevant for democracies and societies, the
type and frequency of this social participation are measured not quite
well (Deth and Maloney 2015, 831). Most data that is available comes
from surveys, where they ask about participation and associational
involvement. But every country has an own understanding of
“associations” and “involvement” or “participation”, sometimes different
lists of example organizations where provided, or the question wording
was not clear enough (Deth and Maloney 2015, 832). Additionally, people
often don’t recognize their social participation when they are asked
within a survey, because they don’t know how the concept is defined and
that, for example, their activity in food sharing would count as well as
being active as a youth coach at the soccer club. These factors, most
often, lead to an underestimation of participation, but this “is not
constant across countries, since some nations \[…\] show a higher
proportion of citizens who are involved in associations without being
members” (Morales and Geurts 2007, 137). In addition, in most of the
European countries there is no organizational register (or it is not up
to date), and the associations do not count how many people are
organized within their structure.

This lack of “good” data leads to the problem that we must work very
effectively with the data sets we have at hand. And as regression seems
to be a multi-purpose tool in empirical political science, it’s time to
look at this data from a different point of view.

Explanation of Approaches
=========================

In the following section, I want to briefly describe the four different
methods that I will use later in the comparison. First, these methods
are all together supervised algorithms. Supervised (in contrast to
unsupervised) means, that we give the “right” solution with our data.
Besides the known Logistic Regression, I want to introduce the
algorithms “Random Forest”, “Support Vector Machines” and “Neural Nets”.
Of course, this section can’t serve the function of a total and
exhaustive explanation of all details of these approaches, there are
other very good books and articles which do this better and deeper as I
ever could do it in this paper. The following section should rather give
hints on the basic mechanisms at work, what to be aware of before using
these approaches, what to expect and which advantages and disadvantages
we may expect from the resulting models.

Logistic Regression
-------------------

The first approach for predicting voluntary action and analyzing the
factors that lead to a participation would be the “classical” Logistic
Regression. There are various articles in the respective literature
(Badescu and Neller 2007; Dekker and Broek 1998; Armingeon 2007) using
this Logistic Regression on this topic, so it is known and widely
accepted. In this case, I will use the Logistic Regression specified as
follows:

*Stochastic Component:*

$$
\begin{aligned}
Y\_i \\sim Y\_{Bern}(Y\_i|\\pi\_i) &= \\pi^{y\_i}\_i (1 - \\pi\_i)^{1-y\_i} \\
\end{aligned}
$$
$$
\begin{aligned}
= \\begin{cases} \\pi\_i &\\text{for } y\_i = 1 \\
1-\\pi\_i &\\text{for } y\_i = 0 \\end{cases}\\end{aligned}$$

*Systematic Component:*

$$
\begin{aligned}
Pr(Y\_i=1) = \\pi\_i = g(X\_i\\beta)
\end{aligned}
$$

Where g() is the cumulative standard logistic function, which leads me
to the according log-likelihood function, which is optimized in respect
to the beta coefficients.

$$
\begin{aligned}
Pr(Y\_i=1) &= \\pi\_i \\
\end{aligned}
$$
$$
\begin{aligned}
&= \\Lambda(\\beta\_0 + \\beta\_1x\_i) \\
\end{aligned}
$$
$$
\begin{aligned}
&= \\frac{1}{1 + e^{-X\_i \\beta}}
\end{aligned}
$$

$$
\begin{aligned}
lnL(\\pi | y) = &\\sum^n\_{i=1} (y\_i \* ln(\\pi\_i) + (1 - y\_i) \* ln(1 - \\pi\_i))\\
\end{aligned}
$$
$$
\begin{aligned}
lnL(\\beta | y) = &\\sum^n\_{i=1} (y\_i \* ln(\\frac{1}{1 + e^{-X\_i \\beta}}) + (1 - y\_i) \* ln(1 - \\frac{1}{1 + e^{-X\_i \\beta}}))
\end{aligned}
$$

The results of this function are optimized betas, which are coefficients
used to calculate the predictions of my model. The advantage of this
method is that I get significance values together with the coefficients,
so I can see how certain or uncertain I am about the effects I measured.
Additionally, I can interpret the sign of the coefficients, which is the
direction of the effect. The value of the coefficients is harder to
interpret, mathematically spoken they express the log odds that with an
increase in this variable the result will be a success (i.e. resulting
in the 1-class of my 2-class-scenario). Although the results seem to be
interpretable, too large tables with different models, coefficients,
standard errors and stars now and then appear to be daunting for the
average reader. Therefore, authors try to “simplify” their regression
results which lead to confusion, because now numbers are replaced with
pluses (Gabriel and Völkl 2008), are printed in bold (Zukin et al. 2006)
or represented with differently styled arrows (Dalton 2014). This may
enhance the understandability in the short term, but it also leads to
ambiguity and decreasing comparability across the studies.

Another big disadvantage of Logistic Regression is the absent
possibility to specify the model besides the data. For Logistic
Regression there exists only one model for one data set (the various
possibilities of variable selection and interaction terms not included),
the only adjustment is the assignment of the threshold for deciding on 1
and 0 at the end. In the following subsections, I will show that this
lacks flexibility in comparison to the other approaches.

Random Forest
-------------

The approach called “Random Forest” is, as the name already shows it, an
ensemble of decision trees. A decision tree grows from top to bottom. It
“guides” the data through its branches and splits the branches by
specific criteria to make the split data purer (in the distribution of
target classes). When applied to regression (rather than to
classification tasks), a so-called “Regression Tree” is built. In
contrast to the classical decision tree, here the leaves do not decide
on a predicted class but on a regression function applied to the data
that came through the according branches. A Random Forest now first
samples from records and variables and feeds these samples into various
regression trees. The results are then combined to the “best” possible
result over all trees (Dangeti 2017, 111). This method is called
“Ensemble” and it inherits characteristics of the “Bagging” approach.

There are two crucial options to specify when building a Random Forest:
the number of trees and the maximum depth of each tree. The depth
describes the number of splits the tree can make from trunk to leaves.
The more splits I allow, the more fine-grained is the result at the
leaves, but also the more decisions are to make.

![png](/img/2018/06/forest1-1prozent.png)

An advantage of Random Forests is their characteristic to combine the
best of Logistic Regression and decision trees:

> “Logistic regression has very high bias and low variance technique; on
> the other hand, decision trees have high variance and low bias, which
> makes decision trees unstable. By averaging decision trees, we will
> minimize the variance component the of model, which makes approximate
> nearest to an ideal model.” (Dangeti 2017, 111)

Unfortunately, there can no significances of variables be assigned, and
even if decision trees themselves are highly intuitive because they can
be plotted and interpreted by humans, Random Forests lack
interpretability because of their shire size and depth (Dangeti 2017,
113). Just to give an impression of the dimensions I am talking about:
Figure \[foresttree\] shows exact 1 percent of one tree out of 400 trees
within a Random Forest (with depth maximum at 30).

Support Vector Machines
-----------------------

We can see a Support Vector Machine (SVM) as the best possible line (or
hyperplane in more than two dimensions) to separate data. The goal is to
make this separated data on both sides of the line as homogeneous as
possible (Lantz 2015, 239).

![png](/img/2018/06/svmprinciple.png)

To understand the admittedly complex mechanism of SVMs, I will briefly
introduce three of the core concepts:

-   **Maximum margin**: In figure \[svmprinciple\], we can see three
    lines, equally able to separate between circles and squares. But
    only line b has the largest distance to the points and because “it
    is likely that the line that leads to the greatest separation will
    generalize the best to the future data” (Lantz 2015, 241), it is
    considered as the best separation.

-   **Support vectors**: The lines or hyperplanes are not calculated
    between all available data points, this would lead to an
    exceptionally high computational effort. Only the data points
    closest to the hyperplane are taken into consideration to define the
    separation. “This is a key feature of SVMs; the support vectors
    provide a very compact way to store a classification model, even if
    the number of features is extremely large." (Lantz 2015, 242)
	
	![png](/img/2018/06/kernel-trick-h.png)

-   **Kernel trick**: In practice, most data points are not separable by
    a line or a hyperplane, SVMs use various computational tricks to
    modify the data. They assume that a separation of classes is
    possible when the data is transformed to another dimension. Think of
    a map with an island pictured on it (Figure \[kerneltrick\]). There
    is no line that can separate island from water. But when you add a
    third dimension and examine the island again, you see that the water
    level cuts island from ocean. Kernel functions can be of different
    nature, there exist for example linear, polynomial or sigmoid
    kernels (Lantz 2015, 247f.).

These flexibilities in building a model according to your data make SVMs
especially powerful tools. Often, they achieve the best results in
predicting when compared with other methods, assumed they get enough
data to train on. Disadvantages are their complex computational effort
caused by the high dimensionality of data as well as the kernel
transformations. Additionally, their resulting models cannot be
interpreted by humans. I don’t get any significances, coefficients or
importance distributions for the variables, so I cannot conclude
anything substantial from the model.

Neural Net
----------

A Neural Net is a machine learning approach that can be used for both,
classification and regression problems. Just like a biological brain, it
connects inputs with “signals” or weights via hidden nodes to outputs.
Within these nodes, activation functions “decide” which information
passes the node, and which gets “forgotten”. The output is (just like
for every other considered approach) the prediction whether a person is
active in voluntary organizations or not.

![png](/img/2018/06/symbolic-nn.png)

As already mentioned, each node consists of an activation function,
which is fed with some input (the values in the first layer or some
transmitted “signals” in the hidden layers) and generates some output.
These nodes, or neurons as they are called, can be placed in different
patterns or structures within the Neural Net. Basically, I am estimating
various regressions in a hierarchized way:

$$\\begin{aligned}
Pr(Y\_i = 1)  &= \\pi\_i \\\\
&= \\frac{1}{1+e^{-X\_i\\beta}} \\\\
&= logit(X\_i\\beta) \\\\
&= logit(linear(X\_i\\beta))\\end{aligned}$$

This formula shows how a linear regression is encapsulated inside of the
Logistic Regression function. I could take a step further and add
another layer, the result would look like this:

$$\\begin{aligned}
Pr(Y\_i = 1) &= \\pi\_i \\\\
&=  logit(linear(logit(linear(X\_i))))\\end{aligned}$$

In fact, this is a simple “single hidden layer” network. If I had two
independent variables feeding in the network, I could visualize the
dependencies like in Figure \[simplenn\]. The figure shows also the
error weights (marked here with “B”).

![png](/img/2018/06/basic-nn-crop-sw.png)

Of course, I don’t want to build a Neural Net because of one single
hidden layer, but it takes some of the “black box” characteristics away.
In fact, I am calculating somehow chained functions (Logistic Regression
functions for the case above), until only one neuron is left and gives
us the result.

There are two crucial parameters that form the basic characteristic of a
Neural Network. I will briefly explain what these important parameters
are and what they basically do when I use them within our models.

-   **Activation function**: As already mentioned above, the activation
    function decides which neurons are activated and should send signals
    through their connections. One typical function we already know is
    the sigmoid function, which is a special case of the logistic
    function. Other widely used activation functions are the hyperbolic
    tangent or the Rectifier (or Relu for Rectified Linear Unit (Dangeti
    2017, 243)), which can be mathematically described as
    *f*(*x*)=*m**a**x*(0, *x*).

-   **Network architecture**: There are quite a lot ways of structuring
    a Neural Net. The only fixed points are the input nodes for each
    variable and the output node for the prediction results. In between
    everything is possible: several layers of hidden nodes, deeper (more
    layers), or wider (more nodes within one layer), fully connected or
    partly connected, with dropout ratios between the nodes and so on.
    There is no rule which combination of all these factors fits best to
    your data, but the structure is crucial for building the “best”
    model instead of building a “good” model.

Besides these two, Neural Networks have many more parameters, that can
be applied and tuned. For example the H2O deep learning function, I will
use for my Neural Net models later on, has in total 90 options
(mandatory and optional) to alter.[1] This results in the apparently
unsolvable task to test all possible combinations of parameters to find
the best working solution. Usually “grid searches” are used to complete
this task. They run through the specified options systematically and
rank the resulting models based on some criteria. Unfortunately, grid
searches are computationally highly intensive so that I cannot take this
method into account for my recorded solutions.

Data Preparation
================

Data Set and Chosen Variables
-----------------------------

For this paper, I worked with data from the European Social Survey
(short ESS). I used data from two rounds, namely round 1 and 6 (ESS
2002; ESS 2012). These two rounds include both questions about activity
in voluntary organizations. In ESS round 1 the question was asked
whether the respondent did the following this in the past 12 months:

-   Is he/she a member of an organization?

-   Did he/she participate within the organization?

-   Did he/she donate money to a organization?

-   Did he/she voluntarily work for a organization?[2]

Possible organizations were listed, where these actions could have taken
place, for example culture, sports or social clubs. I recoded these
variables into one binary variable, being 1 if the respondent answered
“yes” to the second or the forth question. Simply donating money or
being member of an organization is not enough to count as “being
active”.

For ESS round 6 the question was different, but captured the same
meaning. Again it was asked about the activity for voluntary
organizations within the last 12 months, the options to select ranged
from “at least once a week” to “less often” than once every six
months.[3] I recoded this variable into a binary variable, where all
positive answers were transformed into a 1 and “never” into a 0. We have
to keep in mind that these two questions are principally different, but
I decided to go with data from both years as it is known that ML
algorithms need a lot of training data to work well. By using both
rounds as one data set I am able to double the number of observations I
can use to feed in the algorithms. To make sure that this is not a
disadvantage for the predicting, I will also calculate models with the
two rounds separated in two data sets.

The independent variables were selected based on the “civic voluntarism
model” by (Verba et al. 1995) and were guided by the work of (Badescu
and Neller 2007). A detailed explanation of the variables and comments
on the pre-processing can be seen in the appendix \[preprocessing\].
Here I just want to list the variables based on their belonging to the
three levels of “civic voluntarism”:

-   **Resources**: gender, age, country, years in education, living
    area, marital status, children, being a houseperson, weekly work
    hours

-   **Engagement**: tolerance, self realization, solidarity, political
    interest, trust in executive, trust in legislative, trust in
    European Parliament, satisfaction with democracy, political TV
    consumption, total TV consumption

-   **Recruitment**: attending church, meet social contacts

By merging the two rounds together I added a binary variable showing to
which round the record belongs (this should also work against the error
based on taking the two rounds as one data set).

The ESS recommends to include population and design weight variables in
the model[4] to make the estimations representative for the population
of one country or the countries within the EU. As it is difficult to
include these weight variables to the same impact in all models, I will
not include them in the analysis. As my goal is in the first instance to
evaluate the models on their quality in prediction, it would be a
subsequent step to make the results representative for the population.
Therefore we have to keep in mind (especially for the Logistic
Regression, as I don’t get any interpretable results from the other
models), that the results from the different approaches will be not
representative neither for one country nor for the countries compared
within the EU.

Pre-Processing
--------------

### Missing Values

Unfortunately, the data was created with a lot of empty spaces in the
matrix. Therefore, I used the default approach, which can also be
applied within each algorithm itself, and replaced the missing spaces
with the average value of that respective variable, or the most frequent
value when it was a binary variable. This may lead to an underestimation
of the variance in the variable, so this must be remembered for the
Logistic Regression when we interpret the significance of the calculated
coefficients.

### Normalization

Especially Neural Networks are very sensitive to data measured on
different scales. The activation functions are sensitive around +/- 1,
so it is useful to re-scale the included variables.

> “By restricting the range of input values, the activation function
> will have action across the entire range, preventing large-valued
> features such as household income from dominating small-valued
> features such as the number of children in the household. A side
> benefit is that the model may also be faster to train since the
> algorithm can iterate more quickly through the actionable range of
> input values.” (Lantz 2015, 225)

I therefore scaled the data (where it was not categorical or binomial)
to a mean of 0 and a standard deviation of 1. These re-scaled values
will be used for all the approaches, again to make the results
comparable. The other approaches should not be influenced negatively by
normalized values, on the contrary, this step has the tendency to make
the prediction better.

There is one problem with normalizing the variable values: We have to
keep it in mind when we want to interpret the Logistic Regression
coefficients. I will remember the original distribution of the data,
such as mean, highest and lowest values and so on, so I am able to
interprete the scaled results with respect to the original ones.

For the categorical country variable I created dummy variables, which
are included in an extra model to test their influence (they easily
doubled the number of my independent variables, so I had to include them
separately).

### Splitting in Training and Test Data Sets

To avoid that an algorithm learns instances of the data it should
predict later, I clearly separated my data into a training and a test
data set. I randomly selected n/10 indices from my data set and excluded
them as “test data” from the rest, the “training data”. The algorithms
are then able to learn on the training data and to apply their
“knowledge” about the models on the unseen test data set. This
separation is very important as this is the only way to proof the
validity of the model and to prevent over-fitting on the training data
(which would be the case if the algorithm knows the data “by heart” and
performs way too good but is not able to deal with unseen data).

### Balancing

Because the not voluntary active citizens where nearly twice as much as
the participating citizenship (65% : 35%), I created an additional data
set for training, which had equally numbered target variable levels.
Sometimes this facilitates the work of the algorithm. I used
oversampling, so records of the underrepresented category of voluntary
activity were duplicated. These artificially added voluntary active
citizens to should equalize the relationship between the two categories.

Comparison of Results
=====================

Evaluation Metrics
------------------

To compare the different models and evaluate the best model for each
approach, I used two metrics, each one capturing a different concept
within the scoring process. I think the measurement **accuracy**, also
known as “Percent Correctly Predicted” (PCP), can be captured most
intuitively compared to the other two. You can just interpret it as the
percent of correctly predicted cases. In mathematical terms it looks
like this:

$$\\begin{aligned}
Accuracy = \\frac{( tp + tn )}{(tp+fp+tn+fn)}\\end{aligned}$$

Where tp (= true positives), tn (= true negatives), fp (= false
positives) and fn (= false negatives) are the cells of a confusion
matrix (see table \[confusion\]).

<table>
<caption>Confusion Matrix<span data-label="confusion"></span></caption>
<thead>
<tr class="header">
<th align="left"></th>
<th align="left"></th>
<th align="left">truth</th>
<th align="left"></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"></td>
<td align="left"></td>
<td align="left">0</td>
<td align="left">1</td>
</tr>
<tr class="even">
<td align="left">prediction</td>
<td align="left">0</td>
<td align="left">tn</td>
<td align="left">fn</td>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">1</td>
<td align="left">fp</td>
<td align="left">tp</td>
</tr>
</tbody>
</table>

Secondly, I will have a look at the **F1 measure**, which combines two
famous measurements called recall and precision. Precision captures the
error rate in the predictions, how often I am wrong when I predict a 1.
Recall measures how many of the true cases I can predict correctly.

$$\\begin{aligned}
F1 &= \\frac{(2\*Precision\*Recall)}{(Precision+Recall)} \\\\
Precision &= \\frac{tp}{(tp+fp)} \\\\
Recall &= \\frac{tp}{(tp+fn)}\\end{aligned}$$

The two parts are weighted equally, which is the reason why it is called
F1 (there are measurements like F0.5 and F2 existing, each weighing one
of the components more). The algorithms of the Neural Net and Random
Forest optimize according to F1, because it is regarded as the
“broadest” measurement. By focusing, for example, only on accuracy, I
could easily be distracted because with a highly unbalanced dependent
variable I gain a high accuracy measurement, even if I am just
predicting the most often occurring class (and doing no machine learning
at all).

In combination, these three measurements should give us a good notion of
which model could be evaluated as the “best”.

Evaluation of Results
---------------------

To evaluate the results from the four established approaches, we must
keep our baseline in mind. The distribution of voluntarily participating
citizens in the data is 35 percent against 65 percent not participating.
This implies that with a model, that just predicts the modal category
(i.e. the most often occurring category), I achieve an accuracy of 65
percent. The goal of the models is to top this baseline results. As
already said, I will compare the results based on three different
evaluation metrics. The given results in the following paragraphs are
the “maximized” results, i.e. the reported metrics were observed with a
threshold optimized especially for them.

![png](/img/2018/06/evaluation-log-threshold-unbal.png)

At first, the results from the **Logistic Regression** appeared pretty
bad in comparison to the other used approaches. But then I realized that
I did not optimize the threshold, where the decision was made which
record should count as 1 or as 0. Figure \[threshold\] shows that it
matters where I put the threshold. For an optimal model I would take a
value inbetween the maximum of accuracy and F1.

When I now compare the Logistic Regression results to the other results
in figure \[acc\] and \[f1\], the values show that it is not that far
away from the others. In terms of F1, the other models score slightly
better, but in terms of accuracy they are all equally good.

![png](/img/2018/06/acc.png)
![png](/img/2018/06/f1.png)

The best model for **Random Forest** was a forest with 400 trees and a
depth of 30. It scores highest in both, accuracy and F1, at the basic
model. All the Random Forests I built were very stable and did not tend
to over-fit, as the accuracy measured on in- and out-of-sample was
equally high.

Tuning the **Neural Net** models was really tricky because no parameter
change seemed to help. Figure \[tuning\] shows how adding more and more
neurons to the hidden layer makes the in-sample accuracy better and
better - but the out-of-sample accuracy actually decreases. Clearly,
this figure was not created with the powerful H2O-algorithm, but it
shows a crucial part of the story: Tuning the parameters did not help.

![png](/img/2018/06/test5150.png)

I could have done a grid search, but to do so I needed more (and longer)
server power. The best result was achieved with a 3-layer network with
200 neurons in the first and the second layer and 100 neurons in the
third layer. As activation function I used the “rectifier” function. I
also tried tuning the epoch number (usually the higher the better), but
it didn’t make any difference, so I stayed with 10, a relative low
number compared to models in other domains. The results of this Neural
Net (Basic Model) were not the highest, but also not the worst compared
to the other approaches. The accuracy value is about 68 percent, which
is higher than the 65 percent baseline, but only 3 percent.

I ran the **Support Vector Machine** models only on my R-studio server
capacity, because they took too long to calculate locally on my laptop.
The in-sample accuracy was pretty good, at 70 percent by first guess,
but the out-of-sample accuracy was worse than the baseline.
Unfortunately my server time ran up before I could test all data sets
and do some extensive grid search, therefore some values in figure
\[acc\] and all in figure \[f1\] are missing.

Besides the basic model, I mentioned in the sections above several
aspects in the data which I wanted to control. The results are already
pictured in the figures \[acc\] and \[f1\], but they will be also
analyzed in the following paragraphs.

**Does the (unequal) distribution of categories have an influence on the
prediction?**

As figure \[acc\] shows, in terms of accuracy, the results of the models
trained with a “balanced” data set do not differ from the results from
an “unbalanced” data set. When we compare the F1 values in figure \[f1\]
we see a slight improvement for the “balanced” data set for the Logistic
Regression. For the Neural Net, on the other hand, the F1-value was
reduced. This shows that balancing the data can improve your results,
but it really depends on your data and your model.

**Does the merging of data sets influence the prediction?**

In terms of accuracy there are no major changes when I built models for
the rounds separated in comparison to the merged data model. In terms of
F1, ESS round 1 scores not over 0.55 (worse than for the merged data
model), whereas ESS round 6 nearly reaches 0.60. In this case we can see
the big difference between accuracy and F1. For accuracy the models look
equally good, but not for F1. There has to be some potential for
improvement.

**Does the inclusion of the country variable can improve the results?**

Yes, it does. All three observed models score higher when the country
dummies are included (compared to the “ESS only” models). both for
accuracy and F1. With this data I observed the overall best scoring
models: For accuracy it is the Neural Net (on ESS1 data), for F1 it is
the Random Forest (on ESS6 data).

Evaluation of Variables
-----------------------

![png](/img/2018/06/var-imp.png)

In the end, I want to compare the models concerning the variable
importances. Maybe I can get a better image of the factors influencing
voluntary activity, when I take all three basic models together. For the
Logistic Regression, I summed up the absolute values of all significant
(p &gt; 0.05) coefficients (to assign predictive “power” to them, as
they are all scaled). Then I calculated relative importances[5] which
are comparable to the importances I get from the H2O Random Forest and
Neural Net algorithms. Table \[varimp\] shows these values compared to
each other. In all three approaches together, there are in total three
variables with an importance value over average: social trust, meet
social contacts, and years in education. In general, the “social”
variables tend to be more important than, for example, status
characteristics as the marital status, the existence of children or
being female. But, for the data set at hand, I cannot claim that the
importances follow patterns described in the literature. There is no
separation between “resource” and “engagement” variables, as some of
them seem important while others don’t. Only the “recruitment” part
(with “meet social contacts” and “church attendance”) is in complete
regarded as important for the three approaches.

![png](/img/2018/06/age-volact.png)

One last comparison of the three approaches concerns the marginal
effects. Which influence has one variable on the outcome, assuming the
other variables do not change? Figure \[age\] shows clearly where the
differences between Logistic Regression, Neural Networks and Random
Forests are. The line of the Logistic Regression is a straight monotone
increasing line. The Neural Net predicts lower participation
probabilities for smaller birth years (older respondents), the
probabilities increase the younger the respondent gets until, roughly
around 0, the curve starts to decrease. 0, the mean of the scaled
birth-year-variable, stands for the birth year 1961, therefore the
“average aged” respondents are between 41 (ESS round 1) and 51 (ESS
round 6) years old. The oldest person in the data is born 1893, while
the youngest is born in 1998. For both, Neural Net and Random Forest
this means that we start with a relatively high probability of voluntary
activity when we are young. Then we go to work, have a family, and at
some point the probability starts to decrease. For Neural Net slower and
steadier, for Random Forest drastically and with a positive tendency
towards the older age.

Conclusion
==========

From the last figure I can conclude that Neural Nets and Random Forests
can model different influences at different data values better and more
precise than a simple Logistic Regression could do. Nevertheless, the
prediction results of the Logistic Regression are not as bad as I
expected them to be at the beginning. Despite the precise modelling
potential of Neural Nets and Random Forests, these two models could not
take advantage of their characteristics: No parameter tuning helped to
improve the results.

Apart from that, Support Vector Machines could not convince at all
because they needed more time on the server to run than a Neural Net on
my laptop and could not achieve equally high results with default
settings.

But these findings provide valuable insights: When I cannot improve my
model, I have to go back to the data and start with the pre-processing
again. We have seen that the inclusion of other variables can make our
models work better than before. Possible additional strategies could be

-   a better missing value replacement,

-   automated parameter tuning and feature subset selection algorithms,

-   feature engineering, or

-   a different variable selection.

All in all, leaving the comfort zone of the “known” regression could be
helpful for every scientist working with an huge amount of data.
Depending on the data at hand, other algorithms like Random Forests or
Neural Nets could provide comparable and even better results, and are
still interpretable at the same moment (see for example figure \[age\]).
There hasn’t to be any fear when working with these unusual approaches,
of course they all have their own prerequisites and assumptions, but as
long as you keep them in mind, with suitable data they can be very
powerful tools.

Appendix
========

[R Code](/blog/aqm-r-code)
------

Statement of Authorship
=======================

I hereby declare that the paper presented is my own work and that I have
not called upon the help of a third party. In addition, I affirm that
neither I nor anybody else has submitted this paper or parts of it to
obtain credits elsewhere before. I have clearly marked and acknowledged
all quotations or references that have been taken from the works of
others. All secondary literature and other sources are marked and listed
in the bibliography. The same applies to all charts, diagrams and
illustrations as well as to all Internet resources. Moreover, I consent
to my paper being electronically stored and sent anonymously in order to
be checked for plagiarism. I am aware that the paper cannot be evaluated
and may be graded “failed” (“nicht ausreichend”) if the declaration is
not made.
  
Larissa Haas

June 6, 2018

# References

Armingeon, K. 2007. “Political Participation and Associational
Involvement.” In *Citizenship and Involvement in European Democracies: A
Comparative Analysis*, edited by J. W. van Deth, 358–83. London:
Routledge.

Badescu, G., and K. Neller. 2007. “Explaining Associational
Involvement.” In *Citizenship and Involvement in European Democracies: A
Comparative Analysis*, edited by J. W. van Deth, 158–87. London:
Routledge.

Cook, D. 2017. *Practical Machine Learning with H2o: Powerful, Scalable
Techniques for Ai and Deep Learning*. Sebastopol: O’Reilly Media.

Dalton, R. J. 2014. *Citizen Politics: Public Opinion and Political
Parties in Advanced Industrial Democracies*. Los Angeles: SAGE
Publications.

Dangeti, P. 2017. *Statistics for Machine Learning. Build Supervised,
Unsupervised, and Reinforcement Learning Models Using Both Python and
R*. Birmingham: Packt Publishing.

Dekker, P., and A. van den Broek. 1998. “Civil Society in Comparative
Perspective: Involvement in Voluntary Associations in North America and
Western Europe.” *Voluntas: International Journal of Voluntary and
Nonprofit Organizations* 9 (1): 11–38.

Deth, J. W. van. 1997. *Private Groups and Public Life*. London:
Routledge.

Deth, J. W. van, and W. A. Maloney. 2015. “Associations and
Associational Involvement in Europe.” In *Routledge Handbook of European
Politics*, edited by J. M. Magone, 826–42. Abingdon: Routledge.

Dr. Wiley, Joshua F. 2016. *R Deep Learning Essentials. Build Automatic
Classification and Prediction Models Using Unsupervised Learning*.
Birmingham: Packt Publishing.

ESS. 2002. “ESS Round 1: European Social Survey Round 1 Data.” NSD -
Norwegian Centre for Research Data, Norway – Data Archive and
distributor of ESS data for ESS ERIC.

———. 2012. “ESS Round 6: European Social Survey Round 6 Data.” NSD -
Norwegian Centre for Research Data, Norway – Data Archive and
distributor of ESS data for ESS ERIC.

Gabriel, O. W., and K. Völkl. 2008. “Politische Und Soziale
Partizipation.” In *Die Eu-Staaten Im Vergleich*, edited by O. W.
Gabriel and S. Kropp, 268–98. Wiesbaden: VS Verlag.

Lantz, B. 2015. *Machine Learning with R. Second Edition*. Birmingham:
Packt Publishing.

Morales, L., and P. Geurts. 2007. “Associational Involvement.” In
*Citizenship and Involvement in European Democracies: A Comparative
Analysis*, edited by J. W. van Deth, 135–57. London: Routledge.

Verba et al., S. 1995. *Voice and Equality: Civic Voluntarism in
American Politics*. Cambridge, MA: Harvard University Press.

Zukin et al., C. 2006. *A New Engagement?: Political Participation,
Civic Life, and the Changing American Citizen*. New York: Oxford
University Press.

# Footnotes

[1] H2O Documentation:
<http://docs.h2o.ai/h2o/latest-stable/h2o-docs/data-science/deep-learning.html>

[2] ESS1 Main Questionnaire:
<http://www.europeansocialsurvey.org/docs/round1/fieldwork/source/ESS1_source_main_questionnaire.pdf>

[3] ESS6 Main Questionnaire:
<http://www.europeansocialsurvey.org/docs/round6/fieldwork/source/ESS6_source_main_questionnaire.pdf>

[4] Weighting European Social Survey Data:
<http://www.europeansocialsurvey.org/docs/methodology/ESS_weighting_data_1.pdf>

[5] *relative* means: Their absolute values sum up to 1
