---
title: Using Machine Learning to Extract Nuremberg Trials Transcript Document Citations
guest-author: Rosa Lin, Scott Jones, and Paul Deschner
---
In Harvard's [Nuremberg Trials Project](https://nuremberg.law.harvard.edu/), being able to link to cited documents in each trial's transcript is a key feature of site navigation. Each document submitted into evidence by prosecution and defense lawyers is introduced in the transcript and discussed, and the site user is offered the possibility at each document mention to click open the document and view its contents and attendant metadata. While document references generally follow various standard patterns, deviations from the pattern large and small are numerous, and correctly identifying the type of document reference -- is this a prosecution or defense exhibit, for example -- can be quite tricky, often requiring teasing out contextual clues.

While manual linkage is highly accurate, it becomes infeasible over a corpus of 153,000 transcript pages and more than 100,000 document references to manually tag and classify each mention of a document, whether it be a prosecution or defense trial exhibit, or a source document from which the former were often chosen. Automated approaches offer the most likely promise of a scalable solution, with strategic, manual, final-mile workflows responsible for cleanup and optimization.

Initial prototyping by Harvard of automated document reference capture focused on the use of pattern matching in regular expressions. Targeting only the most frequently found patterns in the corpus, Harvard was able to extract more than 50,000 highly reliable references. While continuing with this strategy could have found significantly more references, it was not clear that once identified, a document reference could be accurately typed without manual input.

At this point Harvard connected with Tolstoy, a natural language processing (NLP) AI startup, to ferret out the rest of the tags and identify them by type. Employing a combination of machine learning and rule-based pattern matching, Tolstoy was able to extract and classify the bulk of remaining document references.

## Background on Machine Learning

Machine learning is a comprehensive branch of artificial intelligence. It is, essentially, statistics on steroids. Working from a “training set” -- a set of human-labeled examples -- a machine learning algorithm identifies patterns in the data that allow it to make predictions. For example, a model that is supplied many labeled pictures of cats and dogs will eventually find features of the cat images that correlate with the label “cat,” and likewise, for “dog.” Broadly speaking, the same formula is used by self-driving cars learning how to respond to traffic signs, pedestrians, and other moving objects.

In Harvard’s case, a model was needed that could learn to extract and classify, using a labeled training set, document references in the court transcripts. To enable this, one of the main features used was surrounding context, including possible trigger words that can be used to determine whether a given trial exhibit was submitted by the prosecution or defense. To be most useful, the classifier needed to be very accurate (correctly labeled as either prosecution or defense), precise (minimal false positives), and have a high recall (few missing references).

## Feature Engineering

The first step in any machine learning project is to produce a thorough, unbiased training set.  Since Harvard staff had already identified 53,000 verified references, Tolstoy used that, along with an additional set generated using more precise heuristics, to train a baseline model.

The model is the predictive algorithm. There are many different families of models a data scientist can choose from. For example, one might use a support vector machine (SVM) if there are fewer examples than features, a convolutional neural net (CNN) for images, or a recurrent neural net (RNN) for processing long passages requiring memory. That said, the model is only a part of the entire data processing pipeline, which includes data pre-processing (cleaning), feature engineering, and post-processing.

Here, Tolstoy used a "random forest" algorithm. This method uses a series of decision-tree classifiers with nodes, or branches, representing points at which the training data is subdivided based on feature characteristics. The random forest classifier aggregates the final decisions of a suite of decision trees, predicting the class most often output by the trees. The entire process is randomized as each tree selects a random subset of the training data and random subset of features to use for each node.

Models work best when they are trained on the right features of the data. Feature engineering is the process by which one chooses the most predictive parts of available training data. For example, predicting the price of a house might take into account features such as the square footage, location, age, amenities, recent remodeling, etc.

In this case, we needed to predict the type of document reference involved: was it a prosecution or defense trial exhibit? The exact same sequence of characters, say "Exhibit 435," could be either defense or prosecution, depending on -- among other things -- the speaker and how they introduced it. Tolstoy used features such as the speaker, the presence or absence of prosecution or defense attorneys' names (or that of the defendant), and the presence or absence of country name abbreviations to classify the references. 

## Post-Processing

Machine learning is a great tool in a predictive pipeline, but in order to gain very high accuracy and recall rates, one often needs to combine it with heuristics-based methods as well. For example, in the transcripts, phrases like “submitted under” or “offered under” may precede a document reference. These phrases were used to catch references that had previously been missed. Other post-processing included catching and removing tags from false positives, such as years (e.g. January 1946) or descriptions (300 Germans). These techniques allowed us to preserve high precision while maximizing recall.

## Collaborative, Iterative Build-out

In the build-out of the data processing pipeline, it was important for both Tolstoy and Harvard to carefully review interim results, identify and discuss error patterns and suggest next-step solutions. Harvard, as a domain expert, was able to quickly spot areas where the model was making errors. These iterations allowed Tolstoy to fine-tune the features used in the model, and amend the patterns used in identifying document references. This involved a workflow of tweaking, testing and feedback, a cycle repeated numerous times until full process maturity was reached. Ultimately, Tolstoy was able to successfully capture more than 130,000 references throughout the 153,000 pages, with percentages in the high 90s for accuracy and low 90s for recall. After final data filtering and tuning at Harvard, these results will form the basis for the key feature enabling interlinkage between the two major data domains of the Nuremberg Trials Project: the transcripts and evidentiary documents. Working together with Tolstoy and machine learning has significantly reduced the resources and time otherwise required to do this work.
