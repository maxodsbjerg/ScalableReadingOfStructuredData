# Programming Historian English Language Lesson Template

This file can be used as a template for writing your lesson. It includes information and guidelines on formatting which supplement but do not replace the author's guidelines (/en/author-guidelines)

## Some Important Reminders:

*	Tutorials should not exceed 8,000 words (including code).
*	Keep your tone formal but accessible.
*	Talk to your reader in the second person (you).
*	Adopt a widely-used version of English (British, Canadian, Indian, South African etc).
*	The piece of writing is a "tutorial" or a "lesson" and not an "article".
*  Adopt open source principles
*  Write for a global audience
*  Write sustainably

# Lesson Metadata

**Delete everything above this line when ready to submit your lesson**.

title: Scalable Reading of Structured Data
collection: lessons
layout: lesson
slug: LEAVE BLANK
date: LEAVE BLANK
translation_date: LEAVE BLANK
authors:
- Max Odsbjerg Pedersen 1
- Josephine Møller Jensen 2
- Victor Harbo Johnston 3
- Alexander Ulrich Thygelsen 4
- Helle Strandgaard Jensen 5
reviewers:
- LEAVE BLANK
editors:
- LEAVE BLANK
translator:
translation-editor:
- LEAVE BLANK
translation-reviewer:
- LEAVE BLANK
original: LEAVE BLANK
review-ticket: LEAVE BLANK
difficulty: LEAVE BLANK
activity: LEAVE BLANK
topics: LEAVE BLANK
abstract: LEAVE BLANK
---

# Table of Contents

{% include toc.html %}

--
# Lesson Structure
In this lesson, we introduce a workflow for scalable reading of structured data. The lesson is structured in two parallel tracks: 
1. A general track suggesting a way to work analytically with structured where distant reading of a large dataset is used as context for a close reading of  distinctive datapoints. 
2. An example track which uses we use simple functions in the programming language R to analyze Twitter data. 
Combining these two tracks we show how scalable reading can be used to analyze a wide variety of structured data. Our suggested scalable reading workflow includes two types of distant readings that will help explore and analyze overall features in large data sets (timely dimensions and binary relationships), plus a way of using distant reading to select individual data points for close reading in a systematic and reproducible manner.
# Lesson Aims
* Setting up a workflow where exploratory, distant reading is used as a context that guides the selection of individual data points for close reading 
* Employ exploratory analyses to find patterns in structured data
* Apply and chain basic filtering and arranging functions in R (if you have no or little knowledge of R, we recommend looking at this lesson before you begin)

# Gateway for newcomers: bridging traditional and computational methods 
The combination of close and distant reading we suggest in this lesson is meant as a way for students and academics who are new to the kind of computational thinking embedded in digital methods. When connecting distant reading of large datasets to close reading of single data points, we bridge between digital and traditional methods. In our experience, the scalable reading where the analysis of the entire data sets represents a set of contexts for the close reading eases the difficulties newcomers might experience in asking questions of their material which can be solved using computational thinking. The reproducible way of selecting individual cases for closer inspection speaks, for instance, directly to central questions within the discipline of history and sociology regarding the representativity of case studies. 

# The Scalable Reading 
We originally used the workflow presented below to analyze the remembrance of the American children’s television program *Sesame Street* on Twitter. We used the combined close and distant reading to find out which events generated discussion of *Sesame Street’s* history, which twitter-users dominated the discourse about *Sesame Street’s* history, and which parts of the show's history they mentioned. However, the same analytical framework can also be used to analyze many other kinds of structured data. To demonstrate the applicability of the workflow to other kinds of data, we discuss how it could be applied to a set of structured data from the digitized collections from the National Gallery of Denmark. The data from the National Gallery is very different from the Twitter data used in the lesson's example track, but the general idea of using distant reading to contextualize close reading works equally well as with the Twitter data. 

The workflow for scalable reading of structured data has three steps: 
1. **Exploration of a dataset’s timely dimension.** <br>In the Twitter dataset, we explore how a specific phenomenon gains traction on the platform during a certain period of time. Had we worked with data from the National Gallery we could have analyzed the timely distribution of their collections e.g. accoring to acquisition year or when artworks were made.
 
2. **Exploration of binary relations in a dataset** <br>In the case of the Twitter data we explore the use of hash-tags (versus non-use), the distribution of tweets on verified versus non-verified accounts, and the interaction level of these two account types. If it was the data from the National Gallery of Denmark, it could be representation of male versus female artists, Danish versus international artists, or paintings versus non-paintings. 

3. **Systematic selection of single data-points for close reading** <br>In the Twitter data set, we systematically select the top 20 liked, retweeted and commented tweets, so that they can be found for close reading. If it was the data from the National Gallery of Denmark, it could be the top 20 most exhibited, borrowed or annotated items.  
The R code described below is written with the specific purpose of analyzing twitter data, but the three steps can hopefully be of inspiration to students and researchers in the social sciences and humanities who want to use distant reading to qualify and contextualize results in relation to their close readings. 
# Data
If you want to reproduce the analysis we present below, using not only the overall conceptual framework but also the exact code, we assume that you already have a dataset containing twitter data in a JSON format. It can have been acquired: 
1. Using Twitter’s APIs: Open, Academic, Premium (see more about APIs in this lesson)
2. Using this lesson from the Programming Historian (however, you need to choose a JSON rather than a CSV output).
In the project for which the workflow was originally developed, we had 200,000 tweets collected with the Premium API over two periods of 31 days each. However, for the purpose of this lesson, we made a new dataset using the Open API to make the test case as close to one that could easily be used in a classroom setting. 
