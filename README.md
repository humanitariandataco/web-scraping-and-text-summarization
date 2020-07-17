# NLP_coronavirus_project
## Part II: AUTOMATIC WEB-SCRAPING AND TEXT SUMMARIZATION


1. INTRODUCTION


This part of the Deeplo AI-NLP project uses Natural Language Processing techniques and the Python Programming language as well as related libraries to accomplish an automatic web scraping and text summarization task. It collects and processes data about COVID-19 from publicly available sources in order to provide advice for public and enable them to easily follow news and important issues around the pandemic.  

The objectives of Part II are: (1) Creating a tool for people to follow the issues around the pandemic of COVID-19, (2) creating a summarization tool for longer texts to present important sentences best describe the topics, and (3) providing relevant links for further examination of users.

Thus the web-scraping part provides a list of headlines of related news and documents, their links, and their descriptions with short paragraphs when these descriptions are accessible from the original source or reasonable to understand the nature of topics.  Otherwise, the text summarization part of this study works to generate new summarizations, through use of its automatic text summary generator tool. The goal of the text summarization tool is to provide the most salient sentences of the reports as well as of transcriptions of speeches so that users, based on their understandings of the nature of the topics, can choose relevant links for further examination. 

Furthermore, the project updates the data and repeats its processes every day for timely monitoring of the COVID-19 issues and includes the following sections:

•	First, the Public Advice part provides the advice of an EU governing institution, which is European Commission, having included practical information to keep people healthy against Coronavirus. 

•	Myth Busters gives facts against myths and misconceptions in relation to Coronavirus, which is published by World Health Organization (WHO).

•	Risk Assessment Reports accesses reports of European Centre for Disease Prevention and Control (ECDC) from its official web-site and provides attributes of these reports, including their title, date, link, and descriptions as they are published in the original source. Additionally, the study generates short summaries of these reports, as the descriptions of the original source in relation to individual reports do not reasonably give a sense of summarization. 

•	Speeches of the WHO director, accessing WHO's website, provides a list of speeches of the general-director with their date, link and title, and then generates summarizations for transcriptions of these speeches, as the original source does not provide them. 

•	Global News provides a list of updates and their attributes about events and improvements in relation to the Coronavirus disease being published in official web sites of WHO and ECDC.

•	Finally, National News enables users to follow incidents in which the Coronavirus subject is mentioned in news being published by three web sites of the media, which are AD, NU and Telegraaf. These are chosen as they are known to be among the most visited news web sites in the Netherlands, according to top sites rankings, being found after a google search. Having accessed these news, through use of a translation tool, the section also provides an English version of the news titles and their short summaries as they are published in the original source in the Dutch language.


2. METHODS


2.1. Tools

The project uses Python as the primary language for all implementations, and uses the following libraries: 

•	Python’s threading module with the Timer() class for automatically updating the data and repeating all procedures

•	Requests, for accessing web-pages

•	BeautifulSoup, for web-scraping and accessing texts

•	Regular Expression Operations (RE), for data cleaning 

•	Natural Language Toolkit (NLTK), for processing texts, e.g., tokenization of words and sentences, for excluding stopwords, and for POS-tagging and stemming of words

•	Pandas, for handling data through use of data frames and saving them as csv files as well as reading data from HTML pages


2.2. Data Collection/Web-scraping: Overview

The qualitative data set of COVID-19, including all sections, has been collected through web-scraping with the BeautifulSoup library, HTML and XML parsers from several web-sites of trusted sources and the news media. These are: https://ec.europa.eu/, to access public advice of the European Commission; https://www.who.int/, for facts against myths and misconceptions, for public speeches of the general director of the World Health Organization (WHO), and for global news and events; https://www.ecdc.europa.eu/, for Rapid Risk Assessment Reports of the European Centre for Disease Prevention and Control (ECDC), and for global news and events; https://www.ad.nl/, https://www.nu.nl/, and https://www.telegraaf.nl, for national Dutch news.

The data has been stored with use of the 'pandas.to_csv()' function and google drive. Google Colab has been used for the processing of the data as well as for the creation of the text summary tool mentioned below. Details of the procedures have been described in each section.

2.3. Text-summarization 

This study follows the extractive approach and uses the term frequency technique as it seems more cost effective and adequate enough for the objective of the study. It achieves to produce topic relevant sentences for users, through extracting the most important sentences from single documents, namely: (1) a 1-2 pages long executive summaries of formal reports, and (2) opening remarks at media briefings. The process does not require human judgement or understanding of meanings of words, but relies on statistical significance. 

The importance of sentences in this study is determined based on word frequency scores. The score for each word is calculated by the number of its repetition in the given text. It means that the more frequently the word repeats itself in the text, the higher the score will be. Then, mathematically adding these frequency scores for words those appear in a sentence yields to sentence scores. As a result of this calculation, sentences which include words that have the highest word frequency scores in total also have the highest sentence scores, and therefore, are determined as the most important sentences of the text.

2.3.2. Creation of the text summarization tool

The text summarization tool of this study is created with the following procedures: 

Data pre-processing:

The algorithm begins with cleaning the text, which is received through web-scraping process of this project. Removing square brackets is necessary to get rid of references in the text. Cleaning also involves removing of extra spaces, special characters and digits. Regular Expression has been used to clean the data and the cleaned text is stored for the calculation of word frequencies.

Calculation of word frequencies:

The cleaned text, first of all, is tokenized through use of NLTK’s word tokenizer in order to assign integer values to the each word. Second, nouns and verbs are identified through use of ‘part-of-speech-tagging’ (POS-tagging) of NLTK, in order to count only these words for the frequency calculation, as these types of words are most likely to give concepts of the topic. The tagging process attaches each word a POS-tag automatically, indicating whether the word is noun, verb, adverb, adjective, preposition, conjunction, etc . For the purpose of this study, only the words tagged with the following ones are retained for further processing: "NN" (singular noun), "NNP" (proper noun), "NNS" (plural noun), "VB" (base form of verb), "VBD" (past tense of verb), "VBG" (gerund/present participle form of verb), "VBN" (past participle .of verb), "VBP" (singular present tense of verb), and "VBZ" (singular present tense of verb, 3rd person). These types of words are apparently sufficient to help identifying sentences that are more informative about the topic. 
Third, using NLTK’s stopword function, stop-words are filtered out, which are the most frequently used words, such as ‘what’, ‘for’, ‘is’, ‘at’ , ‘on’, ‘the’, and so on, but do not bear much meaning compared to the other types of words in English. Fourth, using NLTK’s PorterStemmer functions, the stem form of each word is obtained in order identify their exact repetition. 

Finally, the frequency of each word is calculated through counting it’s repetition in the whole text and the raw number of the sum is stored in a dictionary. This is named as the ‘frequency table’. In the dictionary, words are keys and raw numbers of the frequency are corresponding values of these keys.

Calculation of sentence scores:  

The algorithm, at this stage, using NLTK’s sent_tokenizer(), tokenizes the sentences of the original text, instead of the cleaned version of it, in order to be able to assign values for each sentence.  Then it splits each sentence into their words, using the word tokenizer.

The algorithm disregards very long sentences to avoid hardly readable sentences in the summaries. Similarly one or two words sentences do not make sense in a summary. Therefore, only sentences those having higher than 2 words and those having less than 70 words are processed for scoring. 
 
Then the algorithm, runs two subsequent ‘for loops’, first one is for sentences, and the second for the words of the frequency table created above. If it finds matches between the words of a sentence and words of the frequency table, it calculates the sum of word frequencies and divides the sum to the number of words in that sentence. So, the score, actually, represents the weighted score of the sentence. This is actually a normalization procedure to decrease the advantage of longer sentences over shorter ones. Eventually, the calculation produces scores for all sentences. Logically, sentences, having more of the words with the higher frequencies, gain higher scores.  

Thus the weighted score of the sentence is calculated through the formula of:

                    weighted sentence score = sum of word frequencies of the sentence / number of words

Obtaining the summary: 

As the length varies across texts of this study, putting a limit in ratio results in different lengths of summaries, which is determined by the number of sentences or words of the texts. In order to obtain an average length of summary, the algorithm chooses to give a summary with top ‘3’ sentences. To select these sentences, the algorithm orders the sentences according to their scores and selects the ones with the highest scores. Then it returns these sentences in their original order taken from the list of sentences of the original text. 
