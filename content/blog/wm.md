+++
author = "Larissa"
categories = ["Paper", "Rapid Miner"]
tags = ["network-analysis", "twitter", "topology", "spring2017"]
date = "2017-05-27"
description = "Web Mining - Project Report - Spring Semester 2017"
featured = ""
featuredpath = "date"
linktitle = ""
title = "Analyzing Topological Positions Based on Numerical User Information on Twitter"
type = "post"

+++

> presented by\\
> Toka Abdelgabar\\
> Lena Burger \\
> Larissa Haas \\
> Larissa Heissler \\
> Jan Wagner
> 
> submitted to the Data and Web Science Group\\
> Prof. Dr. Christian Bizer\\
> Prof. Dr. Simone Ponzetto\\
> Dr. Stefano Faralli\\
> Dr. Goran Glavas\\
> University of Mannheim

Application Area and Goals
==========================

Real-time micro-blogging services, such as Twitter, have been used
nowadays as the means of keeping up with current information streams.
Twitter has over 338 million active users within a month and this number
increases every day. The users tweet about various topics with a limit
of 140 characters. The information on twitter gets shared through the
follower/followee social network structure where a follower receives all
tweets from the users they follow. The more users tweet and blog,
networks and relations form. These can be used and visualized to provide
insights about different user roles in the diffusion of information
((Kwak et al. 2010b), (Armentano, Godoy, and Amandi 2011)).

The goal of our project is to investigate the relationship between the
position or the role of a twitter user within the twitter network (i.e.
the topological positioning of a node in that network) and its numerical
user information. For this investigation we use three datasets, a first
one to develop our topological model of different roles in a network and
a combination of a second and third one to apply this topological model
and to test our assumption that there is also a difference of feature
values between the topological roles. We want to evaluate our findings
based on the results of our classification model.

The report is structured as follows. First, Chapter \[cha:datasets\]
depicts the size and structure of the datasets we used for our
investigation. Chapter \[cha:preprocessing\] then describes our
preprocessing steps and how we worked with the derived networks.
Afterwards, Chapter \[cha:webmining\] comprises our actual web mining
methods, including our role definitions, the application of these roles
and our classification model. Finally, Chapter \[cha:discussion\] gives
a short conclusion.

Datasets
========

We used three different twitter datasets. First, the Higgs twitter
dataset, second the www2010 twitter dataset and third the twitter7
dataset. The www2010 and twitter7 dataset were provided by Dr. Stefano
Faralli, together with the file ’t2009-userinfo’, that contains twitter
user information and was used as foundation for the final classification
dataset. The following sections will briefly describe the datasets.

Higgs Twitter Dataset
---------------------

The Higgs twitter dataset includes information about users tweeting
about the keywords or hashtags ’lhc’, ’cern’, ’boson’ or ’higgs’ before,
during and after the announcement of the discovery of the higgs boson in
July 2012 (resulting in a total time span of seven days). This
announcement “was the first of this kind in the era of global online
social media” (Domenico et al. 2013).

The network is provided by the Stanford Network Analysis Project (SNAP)
(“SNAP Higgs Twitter Dataset,” n.d.) and it is divided into four
datasets: a social network and a retweet, reply and mention network. The
social network consists of the follower/followee relationships. All
networks contain directed edges. Within this dataset, all userIDs are
anonymized. The number of nodes and edges in the social, retweet and
mention networks is more or less comparable in relation to the largest
Weakly Connected Component (WCC) and the total network. The social
network contains a total number of 456626 nodes and the number of nodes
within the largest WCC is 456290. The reply network, in contrast, has a
remarkably (relative) smaller WCC. This pattern changes when we have a
look at the Strong Connected Components: For the social network, the
relationship between SCC and total network is still high, but for the
other three networks the biggest SCC are not bigger than 4,7 % of the
total network (in this case: 4,7 % of edges in the mention network are
part of the largest SCC). This implies that retweet, reply and mention
networks are not as tightly connected as the social network. By looking
at the other given numbers and calculations, we can confirm this
findings.

Within a first investigation of the given dataset, another researcher
found a prevailing pattern of “information exchange between high-degree
nodes (information hubs) and low-degree nodes (information consumers)”
(Domenico et al. 2013). Instead of analyzing this relationship more
deeply, the paper of Domenico et al. looks at the temporal aspect and
the activation rate of the users tweeting at this topic.

Twitter7 Dataset
----------------

The twitter7 dataset consists of over 476 million tweets, collected
during seven months (June - December) in 2009 (“SNAP 476 Million Twitter
Tweets,” n.d.). Every tweet contains a timestamp, the author (user) and
the tweet content. A tweet can be a normal message, a reply, a mention,
or an retweet. The dataset was also crawled by the Stanford Network
Analysis Project (SNAP), but is no longer available for the public. The
dataset is not anonymized and therefore, we were able to extract the
usernames.

www2010 Dataset
---------------

The www2010 dataset contains 1.4 billion directed follow edges between
41 million Twitter users (Kwak et al. 2010a), crawled in 2010. The
userIDs were not anonymized, therefore we were also able to extract
these information.

’t2009-userinfo’ File
---------------------

The provided file contains user information of about 3 million twitter
users. The available information includes the name, the location, a
freetext description, the number of followers, number of followees,
number of tweets, along with the userID and username, which were used to
create the excerpts of twitter7 and www2010 datasets.

Preprocessing
=============

For the classification part, we edited and expanded the ’t2009-userinfo’
file with the information if an user is a ’Broker’, ’Influencer’ or
’Expert’. We also added the number of replies a user published and
received, the number of retweets a user published and received, as well
as the number of mentions a user published and received. These numbers
were extracted during the preprocessing which is explained in the next
sections.

Building Networks to Work with
------------------------------

To expand the ’t2009-userinfo’ file with the information, if an user is
an ’Expert’, ’Influencer’ or ’Broker’, we performed different
preprocessing steps. The steps are described below, but can also be
found in more depth alongside the respective Python-Code, in the
submitted Jupyter notebook.

For the whole preprocessing, we defined the usernames and userIDs from
the ’t2009-userinfo’ file as ’known ids’ respectively ’known usernames’.

First, we extracted all lines with known ids from the www2010 dataset.
This was the foundation for the follower/followee networkview.
Afterwards, we shrinked the size of the twitter7 dataset, by deleting
empty lines and the timestamp. In another step, the username was
extracted, resulting in one file per month, only containing ’username
tweet’ per line.

With these new files, it was possible to extract the different
networkviews, as well as counting up the different kind of tweets per
user, i.e. the retweets, replies and mentions. To identify the different
kind of tweets, we implemented the following three simple rules:

1.  If Tweet starts with ’RT: @Name’ = Retweet

2.  If 1. does not fit, but Tweet starts with ’@Name’ = Reply

3.  If 1. and 2. do not fit, but ’@Name’ is somewhere in a Tweet =
    Mention

Among others, one shortcoming of these simple rules is that, e.g., in a
tweet from User\_A: ’Jupyter from @Jan is for @Uni\_Mannheim’, only the
mention-relation ’User\_A to Jan’ was extracted.

The next step was to clean the newly extracted networks from unkown
usernames, respectively unknown userIDs.

Afterwards, the cleaned networks were used to count the number of
different tweets and if the user was actively or passively involved in
the specific tweet (e.g. retweeted, or was retweeted)

The extracted networks were also used to calculate the role/position
within the network, resulting in a binary specification if the user has
the specific role (Expert, Influencer, Broker).

The last step was to manipulate and extend the ’t2009-userinfo’ file,
resulting in the following columns: userid, username, no\_follows,
no\_followers, no\_tweets, replied, got\_reply, mentioned,
was\_mentioned, retweeted, got\_retweeted, isBroker, isInfluencer,
isExpert.

Working with the Networks
-------------------------

The processing of the networks was done with the NetworkX package for
Python. It includes several options to generate, manipulate and analyze
graphs in various formats, which is very helpful while working with
other tools like Pajek in parallel.

By investigating the graphs of the Higgs networks more deeply, we found
out that there are loops, where nodes are connected directly to
themselves. This is, especially for the following-follower network, very
uncommon and provides no further information for the position of this
node within the network. Therefore, we decided to delete the self loops
before applying any measurements.

The exact and detailed way of network processing as well as the
labelling of nodes can be viewed step by step in the additionally
provided Jupyter notebook.

Actual Web Mining
=================

Role Definitions
----------------

##### *Expert*

  
The role expert is defined as a “person who has knowledge about a topic
discussed inside a social network, and, as such her opinions and ideas
can be trusted” (Forestier et al. 2012). The methodology of identifying
experts was developed by finding roles within Forum Networks (Forestier
et al. 2012). To apply the methodology on a twitter network we adapt the
question-answer behavior, but only focus on the replies to a tweet. We
assume a user to be an expert when the user replies more often to a
tweet than getting replies by other users. Therefore, we look on the
replies, depending on the direction, as ingoing and outgoing
relationships.

Hence, we first will analyze the outgoing relationships. In case of an
expert, the outgoing relationships appears if User A replies to a Tweet
of User B. In contrary, the ingoing relationships of User A exists when
User B replies to a tweet of User A. The defined direction of
relationships allows us to calculate the outdegree and indegree of User
A. The outdegree is the “number of ties the node directs to others”
(Kosorukoff and Passmore 2011). Applied to our problem, the outdegree
measures the number of outgoing replies. On the other hand, indegree is
a “count of the number of ties directed to the node” (Kosorukoff and
Passmore 2011) which is the number of replies User A received.

To get an implication for the role identification from the degree
centrality concept, we need to measure the relation between the ingoing
and outgoing relationships using a simple z-score. It has been shown
that the simple z-score performs very well on classifying experts
(Zhang, n.d.). The formula is:
$$z\_{r} = \\frac{r - \\frac{n}{2}}{\\sqrt{\\frac{n}{2}}}$$
 Here, ’r’ depicts the number of replies the user himself made, while
’n’ is the total number of ingoing and outgoing relations the user has
with other users.

The z-score divides the replies in two cases: the number of replies User
A published and the number of replies User A received (Zhang, n.d.). It
measures how far above over the average User A is in comparison to the
number of received replies (Zhang, n.d.).

We assume that a user is more likely to be an expert if the outgoing
relationships outweigh, what is depicted by a negative z-score.

##### *Influencer*

  
An influencer is defined as person whose action has the potential to
initiate an action from another user (LEAVITT et al. 2009). In our
project we see replies, mentions and retweets as possible reaction of
another user. To be able to react to a possible influencer, the user
must receive the message first. In general, we assume that a user who
has a lot of followers is more likely to influence other users than
someone who has only a small number of followers. This concept is
depicted in the ’Influence degree’ which calculates the number of
followers of a user (Cha et al. 2010). A high influence degree indicates
that a user is able to spread information widely.

After a user receives a message from a potential influencer the possible
reaction is considered in the ’Retweet Influence’ and the ’Mention
Influence’. The first is measured by the number of retweets and
implicates that a user with a high retweet influence is able to generate
content with pass-along value (Cha et al. 2010). The latter is
calculated by the total number of mentions and shows the ability of a
user to engage others in a conversation (Cha et al. 2010).

To bring the three measurements together, we first rank the users
according to their Reply-, Retweet-, and Mention-Influence. Afterwards
we compare the ranks of the three measurement with each other by using
Spearman’s rank correlation coefficient. This calculates the strength of
the association between two rank sets (Cha et al. 2010).

<table>
<caption>Spearman’s rank correlation coefficient.<span data-label="tab:spear_correlation"></span></caption>
<thead>
<tr class="header">
<th align="left">Correlation</th>
<th align="left">All</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Reply / Retweet</td>
<td align="left">0.529</td>
</tr>
<tr class="even">
<td align="left">Reply / Mention</td>
<td align="left">- 0.32</td>
</tr>
<tr class="odd">
<td align="left">Retweet / Mention</td>
<td align="left">- 0.151</td>
</tr>
</tbody>
</table>

In Table \[tab:spear\_correlation\], we can see a moderate high
correlation between reply and retweet. From the correlation of 0,529 we
can derive that a user who often gets replies is also often retweeted
and vice versa.

To get the labeled nodes for our classification model we fall back on
the relative ranks. For each node, we normalize the values due to the
total number of, e.g., replies. Afterwards, we sum up the normalized
values for the three measurements to a final value per node. If a node
appears in the top 1% of the final ranking, we label this node as
influencer.

##### *Brokerage*

  
A broker is ’a person who is connected to people who are themselves not
directly connected has opportunities to mediate between them and profit
from his or her mediation’ ((De Nooy, Mrvar, and Batagelj 2011)). In
detail, a broker can connect hubs, because he operates as a bridge. Due
to his advantageous structural position, he is able to control the
information flow between these hubs.

To analyze the general present of hubs we use the indegree distribution.
The indegree distribution gives us the total number of connections into
a specific node. The distribution of the Higgs Network is left skewed
and it shows that we have many highly connected nodes, which highlight
the existence of hubs. In Contrary, the Twitter7 Network shows a
comparable distribution, despite less highly connected nodes are
present. Additionally, we can see a significant fraction with a very low
degree in both networks which indicates nodes that are almost isolated.

Since we observe the existence of hubs, we can identify users who
connect these hubs and will therefore be viewed as brokers. We use
’Betweeness-Centrality’ for this measurement. Betweeness-Centrality
measures the actor’s ability to control others (Rowley 1997). Therefore,
users with a high Betweeness-Centrality are brokers in a sense that they
support exchanges between less central actors (Scott 2012).

Application
-----------

The application of our role definitions to the graphs of our second data
set is also done with NetworkX, as described for the Higgs graph above.
The nodes, which fit in our role definitions, were ranked and the top
percent of each rank was labeled with the respective role. Using a
proportionate threshold for labeling a node as ’special’ was mandatory,
for our networks being too different to compare directly in concrete
numbers.

<table>
<caption>Role distribution.<span data-label="tab:role_correlation"></span></caption>
<thead>
<tr class="header">
<th align="left">Network</th>
<th align="left">Expert</th>
<th align="left">Influencer</th>
<th align="left">Broker</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Higgs</td>
<td align="left">1652</td>
<td align="left">4566</td>
<td align="left">4566</td>
</tr>
<tr class="even">
<td align="left">Twitter7</td>
<td align="left">81</td>
<td align="left">6549</td>
<td align="left">2208</td>
</tr>
</tbody>
</table>

Due to the fact that our user data only shows ’active’ users, i.e. users
who mentioned, retweeted, replied at least once in the given timespan,
we lost some labeled nodes by merging them with the user data.
Especially experts appear not that often what may also be caused by a
different topological structure within the networks. In Table
\[tab:role\_correlation\], we can see the different counts of labeled
nodes in comparison. The provided Jupyter notebook provides more
detailed information regarding the labeling of nodes. In Figure
\[fig:test1\] and \[fig:test2\], the different ego networks (within the
Higgs network) of a typical broker and a typical influencer, found on
top of the ranking by our measurements, are illustrated. One can clearly
see that there is an actual topological difference between the two
roles.

![png](/img/2017/05/Broker_76531_2_ego.png)

![png](/img/2017/05/Influencer_220_2_ego.png)

Model
-----

This section describes the algorithms and approaches we applied to form
our classification models. We proceed three independent models to
classify the three roles based on the attributes ’number of followers’,
’number follows’, ’number of tweets’, ’number of replies’ and ’number of
retweets’. These attributes represent the users content and based on
these features, a classification model will be developed. Due to the
binary characteristic of the labeled roles we use Decision Trees, Naive
Bayes and Logistic Regression for the classification. To evaluate our
models, we used Cross Validation with ten folds and respectively 90%
training and 10% testing data.

The results can be seen in Table \[tab:eva\]. The results for the
Decision Trees are not given below, because they did not deliver
satisfying results.

<table>
<caption>Evaluation of the Classification.<span data-label="tab:eva"></span></caption>
<thead>
<tr class="header">
<th align="left">F1-Measure</th>
<th align="left">Expert</th>
<th align="left">Influencer</th>
<th align="left">Broker</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Naive Bayes</td>
<td align="left">58.70%</td>
<td align="left">59.66%</td>
<td align="left">1.08%</td>
</tr>
<tr class="even">
<td align="left">Logistic Regression</td>
<td align="left">9.41%</td>
<td align="left">74.25%</td>
<td align="left">50.14%</td>
</tr>
</tbody>
</table>

Conclusion
==========

Our project investigated the relationship between user roles within a
network and the numerical user information.

The Logistic Regression gives us the implication that only the number of
tweets and the number of retweets have a significant impact on the
classification of a broker (p-value &lt;0.001). This confirms the
theoretically assumptions of information control for the broker
position. Within the logistic model for the expert classification we can
see three highly significant attributes, the number of replies, number
of mentions and number of retweets, which also verifies the theory of an
expert. In contrast, all attributes are significant for the influencer
prediction in the logistic regression Model.

The most difficult part was to find the experts, first in the network
and then within the classification. This maybe is also a special pattern
of twitter, because it is a tool to spread information, and interesting
accounts (i.e. Experts) may also have a lot of social influence (like an
Influencer). Besides that, we have a lot of silent consumers, who are
not part of the network (in our research) and sometimes not even part of
twitter itself and just reading from ’the outside’.

The results of our research are quite satisfying, but could be even
better with different or additional preprocessing steps. For example,
the simple rules to identify the type of tweet within the twitter7
dataset, have room for improvement.

Future work might be done concerning the classification model based on
the user features available in the twitter7 and www2010 datasets.
Additional Text Mining methods could be applied to the available
’status’ feature, e.g., to distinguish between the experts and
influencers.

# References

Armentano, Marcelo G, Daniela L Godoy, and Analía A Amandi. 2011. “A
Topology-Based Approach for Followees Recommendation in Twitter.” In
*Workshop Chairs*, 22.

Cha, Meeyoung, Hamed Haddadi, Fabricio Benevenuto, and P Krishna
Gummadi. 2010. “Measuring User Influence in Twitter: The Million
Follower Fallacy.” *Icwsm* 10 (10-17): 30.

De Nooy, Wouter, Andrej Mrvar, and Vladimir Batagelj. 2011. *Exploratory
Social Network Analysis with Pajek*. Vol. 27. Cambridge University
Press.

Domenico, M. De, A. Lima, P. Mougel, and M. Musolesi. 2013. “The Anatomy
of a Scientific Rumor.” *Scientific Reports* 3.

Forestier, Mathilde, Anna Stavrianou, Julien Velcin, and Djamel A
Zighed. 2012. “Roles in Social Networks: Methodologies and Research
Issues.” *Web Intelligence and Agent Systems: An International Journal*
10 (1). IOS Press: 117–33.

Kosorukoff, A., and D.L. Passmore. 2011. *Social Network Analysis:
Theory and Applications*. Passmore, D. L.
<https://books.google.de/books?id=LrAnswEACAAJ>.

Kwak, Haewoon, Changhyun Lee, Hosung Park, and Sue Moon. 2010a. “What Is
Twitter, a Social Network or a News Media?” In *WWW ’10: Proceedings of
the 19th International Conference on World Wide Web*, 591–600. New York,
NY, USA: ACM.
doi:[http://doi.acm.org/10.1145/1772690.1772751](https://doi.org/http://doi.acm.org/10.1145/1772690.1772751).

———. 2010b. “What Is Twitter, a Social Network or a News Media?” In
*Proceedings of the 19th International Conference on World Wide Web*,
591–600. ACM.

LEAVITT, A., E. BURCHARD, D. FISHER, and S. GILBERT. 2009. “The
Influentials: New Approaches for Analyzing Influence on Twitter.”
Webecology Project.

Rowley, Timothy J. 1997. “Moving Beyond Dyadic Ties: A Network Theory of
Stakeholder Influences.” *Academy of Management Review* 22 (4). Academy
of Management: 887–910.

Scott, John. 2012. *Social Network Analysis*. Sage.

“SNAP 476 Million Twitter Tweets.” n.d.
<https://snap.stanford.edu/data/twitter7.html>.

“SNAP Higgs Twitter Dataset.” n.d.
<https://snap.stanford.edu/data/higgs-twitter.html>.

Zhang, Michael. n.d. “Finding Experts in Unstructured Communities
Through Relationships and Topics.”
