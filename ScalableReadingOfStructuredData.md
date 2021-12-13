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
- Helle Strandgaard Jensen
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
In this lesson, we introduce a workflow for scalable readings of structured data. The lesson is structured in two parallel tracks: 
1. A general track suggesting a way to work analytically with structured where the distant reading of a large dataset is used as context for a close reading of symastically of distinctive datapoints. 
2. An example trac which uses we use simple functions in the programming language R to analyze Twitter data. 
Combining these two tracks we show how scalable reading can be used to work with a wide variety of structured data. Our suggested scalable reading workflow includes two types of distant readings that will help explore and analyze overall features in large data sets (timely dimensions and binary relationships), plus a way of using distant reading to select individual data points for close reading in a systematic and reproducible manner.
* Setting up a workflow where exploratory, distant reading is used as a context that guides the selection of data for close reading 
* Employ exploratory analyses to find patterns in well-structured data
* Apply and chain basic filtering and arranging functions in R (if you have no or little knowledge of R, we recommend looking at this lesson before you being)

# Gateway for newcomers: bridging traditional and computational methods 
The combination of close and distant reading we suggest in this lesson is meant as a way for newcomers to adjust to the kind of computational thinking embedded in digital methods. When connecting distant reading of large datasets (an approach usually new to humanities and social science scholars) to close reading of single data points, we bridge between unfamiliar and familiar methods. In our experience, the scalable reading where the analysis of the entire data sets represents a set of contexts for the close reading eases the difficulties newcomers might experience in asking questions of their material which can be solved using computational thinking. The reproducible way of selecting individual cases for closer inspection speaks, for instance, directly to central questions within the discipline of history and sociology regarding the representativity of case studies. 

# The Scalable Reading 
We originally used the workflow presented below to analyze the remembrance of the American children’s television program Sesame Street on Twitter. We used the combined close and distant reading to find out which events generated discussion of Sesame Street’s history, which twitter-users dominated the discourse about Sesame Street’s history, and how they remembered the show. However, the same analytical framework can also be used to analyze something as different as the digitized collections and metadata from the National Gallery of Denmark. Though a very different content, the scalable reading works equally well to establish an analytical model that combines a distant reading of the distribution of the Gallery’s collections in space and over time; explore its focus on e.g., portraits or places; and then zoom in on, for instance, the top exhibited works by acquisition year, period, content or artist. 
The workflow has three steps: 
1. Exploration of a dataset’s timely dimension. 
In the Twitter data, we explore how a phenomenon gains traction on the platform during a certain period of time. If it was data from the National Gallery of Denmark, it could be the timely distribution of their collection based on the year a work was done or acquired.
 
2. Exploration of binary relations in a dataset
In the case of the Twitter data we explore the use of hash-tags (versus non-use), the distribution of tweets on verified versus non-verified accounts, and the interaction level of these two account types. If it was the data from the National Gallery of Denmark, it could be representation of male versus female artists, Danish versus international artists, or paintings versus non-paintings. 

3. Systematic selection of single data-points for close reading
In the Twitter data set, we systematically select the top 20 liked, retweeted and commented tweets, so that they can be found for close reading. If it was the data from the National Gallery of Denmark, it could be the top 20 most exhibited, borrowed or annotated items. 
The R code described below is written with the specific purpose of analyzing twitter data, but the three steps can hopefully be of inspiration to students and researchers in the social sciences and humanities who want to use distant reading to qualify and contextualize results in relation to their close readings. 
Data
If you want to reproduce the analysis we present below, using not only the overall conceptual framework but also the exact code, we assume that you already have a dataset containing twitter data in a JSON format. It can have been acquired: 
a)	Using Twitter’s APIs: Open, Academic, Premium (see more about APIs in this lesson)
b)	Using this lesson from the Programming Historian (however, you need to choose a JSON rather than a CSV output).
In the project for which the workflow was originally developed, we had 200,000 tweets collected with the Premium API over two periods of 31 days each. However, for the purpose of this lesson, we made a new dataset using the Open API to make the test case as close to one that could easily be used in a classroom setting. 
