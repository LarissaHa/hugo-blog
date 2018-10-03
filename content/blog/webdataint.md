+++
author = "Larissa"
categories = ["Paper"]
tags = ["integration", "web-data", "fall2017"]
date = "2017-12-01"
description = "Project Paper - Lecture Web Data Integration - Fall Semester 2017"
featured = "DSC_0010.JPG"
featuredpath = "date"
linktitle = ""
title = "Integration of Book Data Sets"
type = "post"

+++

> presented by\\
> Larissa Haas\\
> Larissa Heissler\\
> Jan Wagner\\
> Julian Weller
> 
> submitted to the Data and Web Science Group\\
> Prof. Dr. Christian Bizer\\
> Anna Primpeli\\
> Oliver Lehmberg\\
> University of Mannheim

Introduction
============

In this report, we want to outline how we successfully applied the
concepts introduced in the course “Web Data Integration” offered in the
fall/winter term 2017.  
Assuming that a concrete information need triggered the necessity to
integrate data from multiple sources on books and authors, we describe
how we went through the entire data integration process to overcome
different types of heterogeneity. Chapter \[cha:phase1\] describes how
we accessed and preprocessed the data, before we finally performed
schema mapping to translate it into our target schema. More
specifically, we addressed technical, syntactical, data model,
structural and schema-related (that is, structural) semantic
heterogeneity. As we integrated data from more than five sources and
identity resolution (see chapter \[cha:phase2\]) as well as data fusion
(see chapter \[cha:phase3\]) were important parts of the integration
process, we decided to carry out materialized integration, that is, our
final results are actual physical files. In the latter two phases, we
had to deal with semantic heterogeneity on record- and value-level to
ultimately come up with single fused records for each real-world entity
considered, that is, books and authors. In our last chapter
(\[cha:results\]), we identify potentials for optimization.[1]

In the following report we want to provide information about our project
done for the course “Web Data Integration”, offered in the fall/winter
semester 2017, taught by Professor Christian Bizer, who was assisted by
Oliver (XX) and Anna (XX). The goal of this project was to deal with
different sources of data sets, different exchange formats, formatting
styles, measurements and differences in values describing the same
entity. To solve this problems, we crossed three big phases, first the
data collection and translation, second the identity resolution and
third the data fusion. This report will show what we have done within
these phases and which results we archived. The topic of our project is
all about books and authors. We gathered data from different data
sources for example like XX and XX and also consulted DBpedia, to enrich
our data in multiple ways. The goal is to get a data set about books
(and their authors) as complete as possible.

Phase I: Data Collection and Data Translation
=============================================

Data sets used for the Project
------------------------------

<table>
<caption>Data Profile.<span data-label="my-label"></span></caption>
<thead>
<tr class="header">
<th align="left"><strong>No.</strong></th>
<th align="left"><strong>Data Set</strong></th>
<th align="left"><strong>Format</strong></th>
<th align="left"><strong>Class</strong></th>
<th align="left"><strong>#Entities</strong></th>
<th align="left"><strong>#Attributes</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">1</td>
<td align="left">Amazon Product Data (Reviews)</td>
<td align="left">JSON</td>
<td align="left">(Review)</td>
<td align="left">8,898,041</td>
<td align="left">9</td>
</tr>
<tr class="even">
<td align="left">1</td>
<td align="left">Amazon Product Data (Meta Data)</td>
<td align="left">JSON</td>
<td align="left">Book</td>
<td align="left">2,370,585</td>
<td align="left">9</td>
</tr>
<tr class="odd">
<td align="left">2</td>
<td align="left">Book-Crossing Data Set</td>
<td align="left">CSV</td>
<td align="left">Book</td>
<td align="left">271,379</td>
<td align="left">8</td>
</tr>
<tr class="even">
<td align="left">2</td>
<td align="left">Book-Crossing Data Set</td>
<td align="left">CSV</td>
<td align="left">(Rating)</td>
<td align="left">1,149,780</td>
<td align="left">3</td>
</tr>
<tr class="odd">
<td align="left">3</td>
<td align="left">CMU Book Summary Data Set</td>
<td align="left">TXT</td>
<td align="left">Book</td>
<td align="left">16,558</td>
<td align="left">7</td>
</tr>
<tr class="even">
<td align="left">4</td>
<td align="left">Goodbooks-10k</td>
<td align="left">CSV</td>
<td align="left">Book</td>
<td align="left">10,000</td>
<td align="left">23</td>
</tr>
<tr class="odd">
<td align="left">4</td>
<td align="left">Goodbooks-10k</td>
<td align="left">CSV</td>
<td align="left">(Rating)</td>
<td align="left">5,976,479</td>
<td align="left">3</td>
</tr>
<tr class="even">
<td align="left">4</td>
<td align="left">Goodbooks-10k</td>
<td align="left">CSV</td>
<td align="left">(Tag)</td>
<td align="left">34,252</td>
<td align="left">2</td>
</tr>
<tr class="odd">
<td align="left">5</td>
<td align="left">DBpedia Books (Gathered for each</td>
<td align="left">CSV</td>
<td align="left">Book</td>
<td align="left">24,648</td>
<td align="left">10</td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">attribute seperately from DBpedia)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">6</td>
<td align="left">DBpedia Authors</td>
<td align="left">CSV</td>
<td align="left">Author</td>
<td align="left">36,446</td>
<td align="left">4</td>
</tr>
<tr class="even">
<td align="left">7</td>
<td align="left">OpenLibrary</td>
<td align="left">JSON</td>
<td align="left">Book</td>
<td align="left">20,493,415</td>
<td align="left">23</td>
</tr>
<tr class="odd">
<td align="left">8</td>
<td align="left">OpenLibrary</td>
<td align="left">JSON</td>
<td align="left">Author</td>
<td align="left">6,977,708</td>
<td align="left">4</td>
</tr>
</tbody>
</table>

The table shown above contains the data sets in the form we obtained
them from the web. Of course, not all the information contained in the
data sets were interesting for our project. The following sections
describe what we did with each data set and how we prepared them for the
upcoming second phase.

### Amazon Product Data

For the calculation of the average rating, we looped over all ratings of
the amazon rating data set and summed up the scores of a specific book
(identified by the unique asin). Simultaneously we created a new
variable to count the number of ratings which were made to a specific
book. Afterwards we calculated the average rating by dividing the sum of
the rating scores by those counts and linked them with the amazon
metadata set via asin.

### Book-Crossing Data Set

The rating-file from the BX data set contains three attributes:
“User-ID”, “ISBN” and “Book-Rating”. First, we dropped the “User-ID”
column, then we filtered the “Book-Rating” column so that only rows with
book ratings that are not equal to 0 are left in the table. The source
files’ web page describes “0” values as “implicit” ratings, but we have
no further information about these implicit ratings. In a next step we
introduced two new columns: one for the average rating and one for the
count of ratings. So for each ISBN, we now have the average rating and
the number of ratings.

With the “main” BX data set we performed a left join on the ISBN so that
for each row in it, we retrieved the respective rating together with the
count of ratings.

### CMU Book Summary Data Set

The CMU data set remained quite the same, except for the columns
“Wikipedia ID”, “Freebase ID” and “Plot summary”, which we dropped
because we couldn’t use them in the upcoming process.

### Goodbooks-10k

The Goodbooks data came in three data sets, which we combined to one
complete data set. The first file contained the most interesting
attributes, while the third contained some tags (with their IDs), which
were connected to the books via the second data set. So we joined the
three data sets together resulting in a single file, where the tags are
listed in a single cell with comma separated. The title value of this
data set came with some additional information about the series in
brackets, so we parsed the title and wrote the series information in the
according attribute. The same procedure was made with the language, for
in the language value was information written whether the book was
published in “canadian”, “british” or “us” english. This information was
transferred to the publication location. Then we reduced the final data
set in its dimensions, so that only the attributes remained you can see
in Table 2.2.

### DBpedia Books

To get some additional and maybe more complete data, as from the data
sources we found in the internet, we also consulted DBpedia via the
Sparkle endpoint. When we tried to get some data from it, we quickly
discovered that there is an information limit of 40,000 data sets you
can get as maximum from the endpoint. We reached this limit mainly
because the entities got multiplied if there were multiple values
available for one “cell” in the data set. For example: One book was
published by two different publishers and two different counts of page
numbers. The data set received from this constellation would consist of
four entries, one for each possible combination of publisher and page
number. This sounds not that bad as long as you don’t look at books like
H.P. Lovecrafts “Cthulhu Mythos”, which was published in many versions,
languages, editions, what results easily in more than 20,000 entries. So
we decided to get the values for each attribute separately and combined
them later by ISBN. This, of course, resulted in the fact, that the
DBpedia data set has no missing ISBN, because this was the only
necessary attribute.

### DBpedia Authors

Also from DBpedia via their SPARQL-Endpoint, we used the ontology
“Writer” and a Jupyter Notebook (Python) to gather information. DBpedia
contains 36445 entities with the ontology “Writer”

### OpenLibrary Books

The https://openlibrary.org/ website offers a data-dump from their
Database, as JSON file. Beside a lot of information, the
Author-Attribute only contains one or multiple keys, that match against
the Key-Attribute in the OpenLibrary authors data set. One of the
preprocessing steps was to replace the keys with the actual names. The
data set contained a lot of records. To shrink the size, we also agreed
on removing records without ISBN, records without Authors, as well as
all columns that were not needed for the target schema.

### OpenLibrary Authors

The OpenLibrary Author data set looked quite well, but just when you not
looked deeper into it. The data from OpenLibrary is gathered
collaboratively, what means that nearly everybody can add some
information and not everything is revised. This resulted, for example,
in a huge number of different date formats, because the dates where not
collected in one format or even one language. We even encountered
entries like “death date: as soon as possible” to name at least one of
the many different none-sense entries we encountered. Therefore we
worked with regular expressions to forget these entries and to get as
much information from this date attribute as possible. Because of the
large size of the data set we also decided to remove those entries where
we had only the name, because in the later steps these entries would not
gain any additional information, but only increase the number of
comparisons we will have to make. During these steps we were able to
reduce the number of entries from nearly 7 millions to 1.3 million.

Final Target Schema
-------------------

<table>
<caption>Integrated Target Schema.</caption>
<thead>
<tr class="header">
<th align="left"><strong>Class Name</strong></th>
<th align="left"><strong>Attribute Name</strong></th>
<th align="left"><strong>Data Set in which Attribute is found</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">book</td>
<td align="left">id</td>
<td align="left">generated for each data set separately</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">title</td>
<td align="left">data sets # 1, # 2, # 3, # 4, # 5, # 7</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">author name</td>
<td align="left">data sets # 2, # 3, # 4, # 5, # 7</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">ISBN13</td>
<td align="left">data sets # 1, # 2, (# 4), # 5, # 7</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">genre</td>
<td align="left">data sets # 3, # 5, # 7</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">publisher</td>
<td align="left">data sets # 2, # 5, # 7</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">publication location</td>
<td align="left">data sets # 4, # 5, # 7</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">publication date</td>
<td align="left">data sets # 2, # 3, # 4, # 7</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">average rating</td>
<td align="left">data sets # 1, # 2, # 4</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">number of ratings</td>
<td align="left">data sets # 1, # 2, # 4</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">number of pages</td>
<td align="left">data sets # 5, # 7</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">language</td>
<td align="left">data sets # 4, # 5, # 7</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">format</td>
<td align="left">data set # 7</td>
</tr>
<tr class="even">
<td align="left">book</td>
<td align="left">series</td>
<td align="left">data sets # 4, # 5, # 7</td>
</tr>
<tr class="odd">
<td align="left">book</td>
<td align="left">tags</td>
<td align="left">data set # 4</td>
</tr>
<tr class="even">
<td align="left">author</td>
<td align="left">name</td>
<td align="left">data sets # 6, # 8</td>
</tr>
<tr class="odd">
<td align="left">author</td>
<td align="left">birth date</td>
<td align="left">data sets # 6, # 8</td>
</tr>
<tr class="even">
<td align="left">author</td>
<td align="left">death date</td>
<td align="left">data sets # 6, # 8</td>
</tr>
<tr class="odd">
<td align="left">author</td>
<td align="left">gender</td>
<td align="left">data set # 6</td>
</tr>
<tr class="even">
<td align="left">author</td>
<td align="left">location</td>
<td align="left">data set # 8</td>
</tr>
</tbody>
</table>

Phase II: Identity Resolution
=============================

Data Preprocessing for Comparison
---------------------------------

To prepare the data sets for the matching step, we lowercased everything
and removed unnecessary characters from the strings, when it made sense.
With the python library isbnlib we transformed all questionable ISBN
numbers to the correct ISBN13 form. We did not perform any stopword
removal, because we are dealing with titles, where stopwords could carry
meaning and the power to distinguish, too. Other attributes than title
did not contain stopwords at all.

\[my-label\]

<table>
<caption>Dimensions of Data sets after Preprocessing</caption>
<thead>
<tr class="header">
<th align="left"><strong>Dataset</strong></th>
<th align="left"><strong>Filled Cells</strong></th>
<th align="left"><strong>Sparseness</strong></th>
<th align="left"><strong># Attributes</strong></th>
<th align="left"><strong># Entries</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><strong>Goodbooks</strong></td>
<td align="left">95454 / 100000</td>
<td align="left">95,5 %</td>
<td align="left">10 / 13</td>
<td align="left">10000</td>
</tr>
<tr class="even">
<td align="left"><strong>DBpedia</strong></td>
<td align="left">161298 / 221841</td>
<td align="left">72.8 %</td>
<td align="left">9 / 13</td>
<td align="left">24649</td>
</tr>
<tr class="odd">
<td align="left"><strong>CMU</strong></td>
<td align="left">54545 / 66240</td>
<td align="left">82,3 %</td>
<td align="left">4 / 13</td>
<td align="left">16560</td>
</tr>
<tr class="even">
<td align="left"><strong>BX</strong></td>
<td align="left">1651560 / 1899331</td>
<td align="left">87 %</td>
<td align="left">7 / 13</td>
<td align="left">271333</td>
</tr>
<tr class="odd">
<td align="left"><strong>DBpedia Authors</strong></td>
<td align="left">104808 / 145784</td>
<td align="left">71,9 %</td>
<td align="left">4 / 5</td>
<td align="left">36445</td>
</tr>
<tr class="even">
<td align="left"><strong>OL-Authors</strong></td>
<td align="left">2343114 / 4194304</td>
<td align="left">55,9 %</td>
<td align="left">4 / 5</td>
<td align="left">1302128</td>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">(calculated with the</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">first 1048576 entries)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

With the above mentioned data sets and the shown dimensions we started
in the second phase. We decided to remove two of our data sets (Amazon
Product Data and OpenLibrary Books) due to their size and their (in
comparison) relatively low information gain. With the four remaining
book data sets we are able to meet the expectations of this student
project as well.

Dealing with Authors
--------------------

Since we combine multiple data sets we need to face the issue of
duplicates. To ensure that the two entities refer to the same real world
entity we do not only compare the author name but also take the birth
date into account. We therefore split the string of the birth date into
day, month and year and using a linearly matching rule to combine and
weight the different similarity functions. In detail, we weight the
similarity function of name and year higher with a score of 0.4 and
because the mass must sum up to 1 we assign a lower weight of 0.1 to day
and month.

Since one entity from the first database can match with multiple
entities in the second database, we perform local matching. We therefor
treat two records as duplicates if their similarity value hits the
threshold of 0.7. To calculate the similarity values we use Levenshtein
similarity for names, which we assume to be good with typos and leads to
accurate results. We took a small sample to investigate whether the
wrong orders of name play a role in our set. Fortunately we discovered
that forename-middlename-lastname is the most common name format so that
wrong order might be a minor problem. For the birth date attributes we
apply a numerical similarity metric where we allow the dates to differ
about 20%.

As we have very large data sets, DBpedia with 65535 entities and
OpenLibrary with about 1.3 million records, we struggled with the
quadratic complexity of the matching process. To handle this problem, we
applied a sorted neighborhood blocking algorithm. To guarantee a high
reduction ratio which implies that the records are equally distributed,
we use multi-pass. We therefor generated the blocking keys
*NameYearMonthDay*, *BackwardnameYearMonthDay* and to ensure a high
recall, which means we don’t miss pairs that are actually matching, we
also included the key *FirstcharNameYearMonthDay* to our list of
blocking keys as there appear less typos at the first character of a
name.[2] We then sort the entities by the keys and slide over the sorted
list with a window of size 10.

But even with the sorted neighborhood blocking algorithm we got problems
with memory, so we have chosen storage over runtime and compare only
10000 records of the bigger set, OpenLibrary, with the whole DBpedia set
per run. We repeat this till no more entities from the OpenLibrary set
are left to be compared with the records of the DBpedia set. On the one
hand side, we do need more runtime but on the other hand side we keep
the memory on a suitable level and avoid a memory overflow.

Dealing with Books
------------------

We decided to perform the identity resolution for the books by using the
*BookCrossing* data set as a reference and then separately comparing the
*CMU*, *DBpedia* and *GoodBooks* data sets with it. Our approach,
including the creation of the gold standards, was finally implemented in
Jupyter Notebooks for Python. Table \[tbl:ir\_books\_overview\] provides
an overview of the results described in this section.

### Gold Standard

The three gold standards, that is, for (1) *BookCrossing* vs. *CMU*, (2)
*BookCrossing* vs. *DBpedia* and (3) *BookCrossing* vs. *GoodBooks* were
created as follows: For a pair of books to be included in the respective
gold standard, we required two things: (i) The edit distance between the
book titles divided by the length of the longer one of the two book
titles compared had to be smaller than 0.2 and (ii) the edit distance
between the book titles had to be greater than 0. After we had
identified a hundred of such pairs, we stopped our search. Corner cases
that were included were e.g. “harry potter and the order of the phoenix
book 5” vs. “harry potter and the order of the phoenix” (true match), or
“romeo und julia.” vs. “romeo and juliet” (true match). We wrote all the
records from the pairs to a CSV-file, imported the data into Excel and
asked each team member to label the pairs as either true matches or
false matches. Afterwards, we calculated the average pairwise
inter-annotator agreement: For gold standard (1), it is 92,41% for (2)
92,15% and for (3) 90,41%. The scores are reasonably high enough to
conclude that the annotators agree quite strongly. For a pair to finally
be labelled as a true match, we required that at least three out of the
four annotators had assigned the true label.

### Identity Resolution

For the identity resolution, we first determined the optimal similarity
threshold for a pair of books to be considered as describing the same
book such that the F1-Score in the respective gold standard is maximal
by linearly going through all possible similarity threshold values
between 0.01 and 1.00 at steps of 0.01. For (1), this resulted in a
maximum F1-Score of 95% at an optimal threshold of 0.66. For (2), the
F1-Score was 97.3% at a threshold of 0.70 and for (3), the F1-Score was
97.3% at a threshold of 0.52. We used these thresholds in combination
with weights of 0.5 for the similarity score of the book titles, 0.3 for
the author names and 0.2 for the year of publication for performing the
actual identity resolution. The rationale behind this is that we expect
the titles to be most discriminative and the year to be the least, as we
do not want to differentiate between different editions. As similarity
measures, we used Levenshtein distance for both the title and the author
names. For comparing the years of publication, we used maximum
percentage distance with a maximal percentage of deviation that should
be tolerated of 20%.

We also implemented an optimization algorithm that maximizes the
F1-Score by varying the following at the same time for the respective
gold standards: (a) the similarity threshold for considering two records
to describe the same book, (b) the weights that should be assigned to
the similarity scores for the titles, author names and year values, (c)
the similarity metrics used (either Levenshtein similarity or Jaccard
similarity for the titles, either Levenshtein similarity or Jaro-Winkler
similarity (at a p-value of 0.1) for the author names and always maximum
percentage distance for the years (always at 20% allowed tolerance)).
Unfortunately, we did not have time to implement cross validation in
this context which is why we did not further use the respective results
due to potential overfitting issues. Moreover, that is the reason why we
decided to stick with Levenshtein similarity for both the title as well
as the author names, as it is quite basic and thus suitable for a
baseline.

For the *DBpedia* data set, it should be pointed out, that no years of
publication were available. Thus, we decided to distribute the remaining
weight of 0.2 to the title similarity score and author name similarity
score according to the ratio of 0.5 (title weight) to 0.3 (author name
weight). Consequently, the weight for the title similarity score was
0.625 and the weight for the author name similarity score 0.375.

We did not perform de-duplication on each data set separately, before
running the actual identity resolution between different data sets as we
wanted to retain as much data (that is “opinions”) for the data fusion
as possible.

To speed up the comparisons, we applied partitional blocking. As a key,
we used the first character of the book title concatenated with the
first character of the author name. Only pairs of books having this key
in common were compared through our linear matching rule. Pairs that did
not share this key, were implicitly considered to have a similarity
score of 0. The decrease in recall with blocking was 92.7% - 73.2% =
19.5% for case (1), 97.3% - 78.4% = 18.9% for (2) and 100% - 82.2% =
17.8% for (3).

<table>
<caption>Overview of Identity Resolution Results for the Books<span data-label="tbl:ir_books_overview"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Case</strong></td>
<td align="left"><strong>AVG</strong></td>
<td align="left"><strong>Opt.</strong></td>
<td align="left"><strong>Max.</strong></td>
<td align="left"><strong>Recall w/o</strong></td>
<td align="left"><strong>Recall with</strong></td>
<td align="left"><strong>Delta</strong></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left"><strong>IAA</strong></td>
<td align="left"><strong>Th.</strong></td>
<td align="left"><strong>F1</strong></td>
<td align="left"><strong>Blocking</strong></td>
<td align="left"><strong>Blocking</strong></td>
<td align="left"><strong>Recall</strong></td>
</tr>
<tr class="odd">
<td align="left">(1) BX_CMU</td>
<td align="left">92.41%</td>
<td align="left">66%</td>
<td align="left">95.0%</td>
<td align="left">92.7%</td>
<td align="left">73.2%</td>
<td align="left">-19.5%</td>
</tr>
<tr class="even">
<td align="left">(2) BX_DB</td>
<td align="left">92.15%</td>
<td align="left">70%</td>
<td align="left">97.3%</td>
<td align="left">97.3%</td>
<td align="left">78.4%</td>
<td align="left">-18.9%</td>
</tr>
<tr class="odd">
<td align="left">(3) BX_GB</td>
<td align="left">90.41%</td>
<td align="left">52%</td>
<td align="left">97.3%</td>
<td align="left">100.0%</td>
<td align="left">82.2%</td>
<td align="left">-17.8%</td>
</tr>
</tbody>
</table>

Phase III: Data Fusion
======================

Conflict Resolution
-------------------

\[my-label\]

<table>
<caption>Functions for Conflict Resolution</caption>
<thead>
<tr class="header">
<th align="left"><strong>Attribute</strong></th>
<th align="left"><strong>Function</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Title</td>
<td align="left">String Comparison</td>
</tr>
<tr class="even">
<td align="left">Author Name</td>
<td align="left">String Comparison</td>
</tr>
<tr class="odd">
<td align="left">ISBN13</td>
<td align="left">Validation &amp; Replacement</td>
</tr>
<tr class="even">
<td align="left">Genre</td>
<td align="left">Union</td>
</tr>
<tr class="odd">
<td align="left">Publisher</td>
<td align="left">String Comparison</td>
</tr>
<tr class="even">
<td align="left">Publication Location</td>
<td align="left">String Comparison</td>
</tr>
<tr class="odd">
<td align="left">Publication Date</td>
<td align="left">Date Comparison</td>
</tr>
<tr class="even">
<td align="left">Average Rating</td>
<td align="left">Weighted Average - Upscale to 1-10 Rating</td>
</tr>
<tr class="odd">
<td align="left">Number of Ratings</td>
<td align="left">Addition</td>
</tr>
<tr class="even">
<td align="left">Number of Pages</td>
<td align="left">Highest Number</td>
</tr>
<tr class="odd">
<td align="left">Language</td>
<td align="left">String Comparison</td>
</tr>
<tr class="even">
<td align="left">Series</td>
<td align="left">Union</td>
</tr>
<tr class="odd">
<td align="left">Tags</td>
<td align="left">Union</td>
</tr>
<tr class="even">
<td align="left">Author Name</td>
<td align="left">String Comparison</td>
</tr>
<tr class="odd">
<td align="left">Birth date</td>
<td align="left">Date Comparison</td>
</tr>
<tr class="even">
<td align="left">Death date</td>
<td align="left">Date Comparison</td>
</tr>
<tr class="odd">
<td align="left">Gender</td>
<td align="left">String Comparison</td>
</tr>
<tr class="even">
<td align="left">Location</td>
<td align="left">String Comparison</td>
</tr>
</tbody>
</table>

The following fusion strategies were used:

In general, when an entity had more then one match (similarity above
threshold) within the next data set, only the match with the highest
similarity was taken into account. All other matches were ignored. Table
4.1 shows the different attributes and the respective fusion-strategy
that was used. For the string comparison we decided to always use the
longest string. With help of an external library, the ISBN13 was
validated. If only one ISBN13 was valid, it was used. If both were valid
and equal or unequal, the number from the first data set was used (this
decision was also made because we knew that the ISBN of some data sets
were broken). Genres, series, tags were just added to each other. The
available ratings were scaled up to an 1 - 10 rating. For birth dates,
we used the earlier date of the two compared dates respectively for the
death date, the later date of the two compared dates. The data fusion
was implemented with Python 3.6.3, as Jupyter Notebook. Again, the BX
data set was chosen as reference data set. With the results from the
Identity Resolution, first the CMU data set was fused, the resulting
data set was then fused with the DBpedia data set and that result was
then fused with the Goodbooks data set.

Density
-------

The weighted average density of our final input data sets of books is
around 46.8%. During our Data Fusion we gained two more points and
resulted in a density of 48.8%. One would say that 2% are not that much,
but this gain means also, that we had a lot of overlapping values within
our 27,309 overlapping entries (what makes roughly 9% of our data). If
we looked only at the part of the fused data set, where we did actually
some fusion, we find a density of 71,4%. For the author data sets we
started with a weighted average density of 45.1% and were able to
increase it to 45.5%. Here, the overlap between the two data sets is
just about 2.7%, which leaves not much opportunities to increase the
density.

Gold Standard
-------------

Because we were in the comfortable position to observe our entities in
the data sets directly, we created our gold standard simply by
observing. We gathered a sample of 85 books mainly from our reference
data set book crossing and retrieved the attributes author, title and
(first) publication year by hand to compare them with the final data
set. After the comparison we calculated the accuracy per cell. All in
all, we achieved an accuracy of 89.7%, i.e. 89.7% of the cells in our
gold standard were found in our fused data set. But of course, as
mentioned in the lecture, it is very difficult to define a “general”
truth for the entities.

Conclusion
==========

In our report, we outlined how we went through each phase of the
integration process in the domain of book and author data. Finally, we
want to point out directions for further improvement that we
identified.  
**I. Data Collection and Data Translation.** (1) As the original *CMU
Book Summary Dataset* contained plot summaries, it might have been
interesting to exploit this data for learning single-or multi-labelled
topics as kind of additional genres or tags. (2) On a larger scale, it
might be interesting to infer from the actual text corpora of the books
missing data, like e.g. genres or tags. (3) As ISBNs implicitly contain
book meta data like for example the respective language or geographic
region as well as publisher data, we could have resolved them further.
(4) Additionally, we could have inferred e.g. country data from the
publisher data with a list of respective mappings.  
**II. Identity Resolution.** Regarding the identity resolution, it could
be beneficial (1) to optimize the weights for the linear matching rules
by means of cross validation, (2) to choose from a wider range of
similarity measures, including optimized similarity measure
hyperparameters, and (3) to experiment with non-linear matching rules.
For the books, we could additionally (i) try several different
partitional blocking keys, or (ii) apply the (multi-pass) sorted
neighborhood method for blocking at different windows sizes, instead of
plain partitional blocking.  
**III. Data Fusion.** The data fusion could be further optimized through
(1) experimenting with different rules, for example including voting
with regard to the genres and tags, or (2) by verifying the author names
in the actual author data sets.

### Things we could have done, but we didn’t

- do some text analysis of the summaries to predict genre or tags (or
even author?!) - do fine grained feature parsing (e.g. get the country
from the ISBN, get country from city etc.)

Further Experimental Results
============================

Phase II: Identity Resolution for Books
---------------------------------------

**Baseline.** For our baseline approach, we only optimized the
similarity threshold for considering two records to be describing the
same book, such that the F1-score is maximized. Please note that only
similarity thresholds are included in tables
\[app:p2ir\_books\_tbl\_baseline\_bx\_cmu\],
\[app:p2ir\_books\_tbl\_baseline\_bx\_DBpedia\] and
\[app:p2ir\_books\_tbl\_baseline\_bx\_GoodBooks\] if they increase the
associated F1-score.

<table>
<caption>Determination of Optimal Similarity Threshold for Case (1): BX vs. CMU<span data-label="app:p2ir_books_tbl_baseline_bx_cmu"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Similarity</strong></td>
<td align="left"><strong>F1</strong></td>
</tr>
<tr class="even">
<td align="left"><strong>Threshold</strong></td>
<td align="left"><strong>Score</strong></td>
</tr>
<tr class="odd">
<td align="left">0.01</td>
<td align="left">0.58</td>
</tr>
<tr class="even">
<td align="left">0.43</td>
<td align="left">0.58</td>
</tr>
<tr class="odd">
<td align="left">0.44</td>
<td align="left">0.60</td>
</tr>
<tr class="even">
<td align="left">0.45</td>
<td align="left">0.61</td>
</tr>
<tr class="odd">
<td align="left">0.46</td>
<td align="left">0.63</td>
</tr>
<tr class="even">
<td align="left">0.47</td>
<td align="left">0.65</td>
</tr>
<tr class="odd">
<td align="left">0.48</td>
<td align="left">0.70</td>
</tr>
<tr class="even">
<td align="left">0.49</td>
<td align="left">0.72</td>
</tr>
<tr class="odd">
<td align="left">0.50</td>
<td align="left">0.78</td>
</tr>
<tr class="even">
<td align="left">0.51</td>
<td align="left">0.82</td>
</tr>
<tr class="odd">
<td align="left">0.52</td>
<td align="left">0.86</td>
</tr>
<tr class="even">
<td align="left">0.53</td>
<td align="left">0.88</td>
</tr>
<tr class="odd">
<td align="left">0.56</td>
<td align="left">0.89</td>
</tr>
<tr class="even">
<td align="left">0.58</td>
<td align="left">0.91</td>
</tr>
<tr class="odd">
<td align="left">0.60</td>
<td align="left">0.93</td>
</tr>
<tr class="even">
<td align="left">0.64</td>
<td align="left">0.94</td>
</tr>
<tr class="odd">
<td align="left">0.66</td>
<td align="left">0.95</td>
</tr>
</tbody>
</table>

<table>
<caption>Determination of Optimal Similarity Threshold for Case (2): BX vs. DBpedia<span data-label="app:p2ir_books_tbl_baseline_bx_DBpedia"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Similarity</strong></td>
<td align="left"><strong>F1</strong></td>
</tr>
<tr class="even">
<td align="left"><strong>Threshold</strong></td>
<td align="left"><strong>Score</strong></td>
</tr>
<tr class="odd">
<td align="left">0,01</td>
<td align="left">0,54</td>
</tr>
<tr class="even">
<td align="left">0,53</td>
<td align="left">0,54</td>
</tr>
<tr class="odd">
<td align="left">0,54</td>
<td align="left">0,55</td>
</tr>
<tr class="even">
<td align="left">0,55</td>
<td align="left">0,56</td>
</tr>
<tr class="odd">
<td align="left">0,56</td>
<td align="left">0,58</td>
</tr>
<tr class="even">
<td align="left">0,57</td>
<td align="left">0,61</td>
</tr>
<tr class="odd">
<td align="left">0,58</td>
<td align="left">0,64</td>
</tr>
<tr class="even">
<td align="left">0,59</td>
<td align="left">0,68</td>
</tr>
<tr class="odd">
<td align="left">0,60</td>
<td align="left">0,70</td>
</tr>
<tr class="even">
<td align="left">0,61</td>
<td align="left">0,77</td>
</tr>
<tr class="odd">
<td align="left">0,62</td>
<td align="left">0,82</td>
</tr>
<tr class="even">
<td align="left">0,63</td>
<td align="left">0,87</td>
</tr>
<tr class="odd">
<td align="left">0,64</td>
<td align="left">0,90</td>
</tr>
<tr class="even">
<td align="left">0,65</td>
<td align="left">0,94</td>
</tr>
<tr class="odd">
<td align="left">0,66</td>
<td align="left">0,95</td>
</tr>
<tr class="even">
<td align="left">0,68</td>
<td align="left">0,96</td>
</tr>
<tr class="odd">
<td align="left">0,70</td>
<td align="left">0,97</td>
</tr>
</tbody>
</table>

<table>
<caption>Determination of Optimal Similarity Threshold for Case (3): BX vs. GoodBooks<span data-label="app:p2ir_books_tbl_baseline_bx_GoodBooks"></span></caption>
<tbody>
<tr class="odd">
<td align="left"><strong>Similarity</strong></td>
<td align="left"><strong>F1</strong></td>
</tr>
<tr class="even">
<td align="left"><strong>Threshold</strong></td>
<td align="left"><strong>Score</strong></td>
</tr>
<tr class="odd">
<td align="left">0,01</td>
<td align="left">0,84</td>
</tr>
<tr class="even">
<td align="left">0,41</td>
<td align="left">0,85</td>
</tr>
<tr class="odd">
<td align="left">0,43</td>
<td align="left">0,85</td>
</tr>
<tr class="even">
<td align="left">0,44</td>
<td align="left">0,86</td>
</tr>
<tr class="odd">
<td align="left">0,45</td>
<td align="left">0,87</td>
</tr>
<tr class="even">
<td align="left">0,46</td>
<td align="left">0,88</td>
</tr>
<tr class="odd">
<td align="left">0,47</td>
<td align="left">0,88</td>
</tr>
<tr class="even">
<td align="left">0,48</td>
<td align="left">0,90</td>
</tr>
<tr class="odd">
<td align="left">0,49</td>
<td align="left">0,92</td>
</tr>
<tr class="even">
<td align="left">0,50</td>
<td align="left">0,95</td>
</tr>
<tr class="odd">
<td align="left">0,51</td>
<td align="left">0,96</td>
</tr>
<tr class="even">
<td align="left">0,52</td>
<td align="left">0,97</td>
</tr>
</tbody>
</table>

[1] \[ftn:i ntro\_source\]The contents of the introduction are mainly
taken from/derived from the lecture slides (“source”).

[2] Tobias Vogal and Felix Naumann. *Automatic blocking key selection
for duplicate detection based on unigram combinations*. Proceedings of
the International Workshop on Quality in Databases (QDB). 2012.
