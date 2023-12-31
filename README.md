# Rock is not dead: NLP experiments on rock news articles
# Overview
This repository contains multiple NLP experiments on web rock-based news articles. The text corpus is comprised by 20K rock news headlines and descriptions with unlabelled data (for demonstration purposes, a random subset of 1K articles is available in this repository). The data was retrieved between January 2022 and September 2023 from 6 specialized rock websites: Loudersound, loudwire, Ultimate Classic Rock, Kerrang!, Planet Rock and The New York Times.

<details>
<summary> Table of Contents </summary>

1. [Dictionary-based Named Entity Recognition](#dictionary-based-named-entity-recognition)
2. [Rule-based text classification](#rule-based-text-classification)
3. [Topic modelling experiments](#topic-modelling-experiments)
    + [LDA model using Scikit-learn](#1-lda-model-using-scikit-learn)
    + [LDA model using Gensim](#2-lda-model-using-gensim)
4. [Rule-based text classification Vs. Machine Learning classification: final thoughts and further research](#rule-based-text-classification-vs-machine-learning-classification-final-thoughts-and-further-research)
5. [Visualization portfolio](#visualization-portfolio)
6. [References](#references)

</details>

**Data architecture diagram**

![](https://github.com/IvoDSBarros/Rock-is-not-dead_NLP-experiments-on-rock-news-articles/blob/e5519627ca185c86533a61edd8c57618d31d94e7/output/visuals/data_architecture_diagram.png)


## Dictionary-based Named Entity Recognition
### Goals
The purpose of this model is identifying and extracting rock artist/rock artist member names from the headlines and descriptions of the above-mentioned text corpus. A custom dictionary-based named entity recognition (NER) approach is tested. The pre-built dictionary is based on data extracted from Wikipedia lists on rock, metal and punk bands gathered by a web scraper (the rock artist master data available in this repository is restricted to artists starting with the letter "A").

### Challenges
+ Single/multiple rock artist name(s) and/or single/multiple rock artist member name(s) might be mentioned in a news headline and/or news description. Hence, the text of the headline and the text of the description were combined to perform the search of the rock artist and the rock artist member. Additionally, only whole/compound words are matched to avoid inaccurate labelling. The pre-built dictionaries of rock artists/artists member names contain 38,663 records. Given the ultimate purpose is assigning lists of identified rock artist/artist member names per every single text of the corpus, performance is a critical issue. Several methods were assessed including Vectorization, Flashtext, Regex and a whole word search approach proposed on Stack Overflow (question 5319922, user200783). The latter, in conjunction with a text preprocessing approach which removes special characters and a set of rock artists/artists member names, demonstrated to be the fastest and most effective.

+ Acronyms are used to mention some rock artists (A7X, RHCP, RATM, GN'R) and the definite article "The" is sometimes excluded to mention artists whose name starts with "The" (a stop word removal approach would perfectly work out for bands like "The Beatles" or "The Rolling Stones" but it is not the case for bands such as "The Who" - it would be completely removed as "The" and "Who" are both stop words - or "The Doors" - "Doors" would be matching both "The Doors" and "Three Doors Down" news articles afterwards). Moreover, popular songs or albums are often mentioned without reference to the rock artist in the headlines and/or descriptions. Misspellings were identified on the rock artist names in the headlines.

+ The words of the news headlines from the websites Loudwire and Ultimate Classic Rock start with capital letter.

+ Bands like "Yes", "HIM", "Sweet" or "The Band" lead to misleading labelling. Additional text preprocessing actions were required to ensure accurate outcomes.
<br>

**Top 50 rock artists and rock artist members** 

![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/79f97e3178c6c49564fc5677b22d771efe11d92a/output/visuals/dict_based_ner_viz.png)

**A closer look at the identified rock artist and rock artist members** 
<br>All tables presented in this repository are based on stratified random samples of 10 news articles (website is the stratifying field).
<br>The headline and the description were combined to perform the extraction of the named entities.

**Headline**
![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/c6979f1778cee7f11e7b0b91642c6a70625b2099/output/visuals/dict_based_ner_table_headline_random_sample.png)

**Description**
![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/ac9abff682ffb2410b6e73d5a3be7df75eaf5229/output/visuals/dict_based_ner_table_desc_random_sample.png)

<div align = "right">    
  <a href="#overview">(back to top)</a>
</div>

## Rule-based text classification
### Goals
This rule-based text classification model is intended to identify keywords and assign both topic labels and publication type categories across the unlabelled rock news headlines. A set of pre-defined rules was manually created for this purpose. The core keywords of the rock news headlines' semantic landscape comprise the following: "album", "single", "song', "show", "tour" and "video". The keywords are the basis to derive the classification rules and to assign human-readable contextualized tags.

### Challenges
+ Ensure all semantically relevant keywords, in which the set of classification rules is based on, are integrated in the cleaned text corpus when performing the extraction of common nouns and verbs. A function was designed in this respect by combining the selection of the mentioned part-of-speech (POS) tags and a list of all relevant keywords.

+ Considering the target POS tags, the previously identified rock artists names were initially replaced by a unique word, "Bandname", to mitigate inaccuracies of the POS tagging activities performed at a later stage. The word "Bandname" was later removed from the text corpus.

+ Stemming was the most effective text normalization technique to prepare the text corpus for further processing. This was particularly significant when dealing with verb tenses. E.g., as "think" and "say" are relevant keywords and irregular verbs, its past simple form was replaced by the present simple.

+ A dictionary was created to ensure synonyms of relevant keywords are accurately standardized, considering the specific semantic field these keywords show in the context of rock news. Actually, the verbs "drop", "unleash", "share", "premier" and "launch" are generally related to music releases, whilst "unveil" and "reveal" tend to be associated to announcements in most cases.
<br>

**Alluvial diagram on the rock news articles** 

![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/9bcac112f107897702938baf1c492eaa6a415e86/output/visuals/rule_based_text_%20class_viz.png)

<br>

**Generated keywords, topic labels and publication type categories by headline**

![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/688f332b4bdc57869a5054f0fbfbcdbcc60f7691/output/visuals/rule_based_text_%20class_table_random_sample.png)

<div align = "right">    
  <a href="#overview">(back to top)</a>
</div>

## Topic modelling experiments
### Goals
Along with the rule-based text classification model, an unsupervised machine learning method for topic modelling was conducted – the Latent Dirichlet Allocation (LDA). Two models were developed using the Python's libraries **(1) Scikit-learn** and **(2) Gensim**. A stratified 80/20 split-sample validation was implemented using the website as the stratifying field. The text preprocessing methodological choices have already been detailed in the Rule-based text classification chapter.

#### Challenges
+ As the LDA algorithm is stochastic and the output is different every run, the random state parameter was set to 0 to ensure the reproducibility of the Scikit-learn and Gensim LDA models. 

+ The results obtained through the inverse document frequency (TF–IDF) were not aligned with expectations across both models. Despite its main purpose of scaling down the impact of predominant tokens, the interpretability of topics was not as coherent and as comprehensible as frequencies of events.

+ The hyperparameter optimization of the Scikit-learn LDA model, namely the parameters "n_components" and "learning_decay", was performed using the grid search method. The parameter "n_components" search was set as higher than 4 to ensure reliable interpretability of topics.

+ A Gensim Ensemble LDA was implemented to overcome the instability and non-reproducibility of the sklearn and "gensim lda"/"ldamulticore" approaches.

<br>

#### 1. LDA model using Scikit-learn 
#### Results
**LDA evaluation model metrics in Scikit-learn** <br>
Perplexity and likelihood score are conventional performance metrics available in the Scikit-learn library to diagnose a LDA model. These statistical measures evaluate the predictive accuracy of a model on unseen texts. According to the available literature, the lower the perplexity, the more accurate is the model. On the contrary, a higher likelihood score is indicative of a better fit. However, there is no pre-defined threshold that defines a lower perplexity score or a higher likelihood score. Based on the work of Blei, D. et al. (2003), the perplexity goes in the opposite direction of the number of topics. This means that the former tends to decrease when the latter increases. On the other hand, a study conducted by Chang J. et al. (2009) suggested there is no relationship between perplexity and human interpretation. In conformity with the chart below, the 5 topics model ranks #2 when it comes to the statistical assumption of lower perplexity and higher likelihood score. This model was the most consonant with human interpretation as shown by a manual random topic assignment validation, independently of the recurrent inaccurate tagging. Surprisingly, the perplexity score only decreased in the models with 7, 9 and 12 topics. 

<br>

**Perplexity and Likelihood score over number of topics in the test set**
![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/ece861c4ba221b268ae79178f4c2c1a931c45197/output/visuals/lda_sklearn_assessment.png)

+ **Perplexity** =  901.4
+ **Likelihood score** = -108,604.3

<br>

**Interactive topic model visualization with pyLDAvis** <br>
To get a visual overview of the LDA model, the Python library “pyLDAvis” based on the R package “LDAvis”, developed by Sivert C. & Shirley K. (2014), was used. The left panel of the visualization is intended to clarify both the prevalence of each topic of the model and the interconnection between topics. In fact, the left-hand side chart shows 5 big bubbles distributed along the quadrants and distant between each other ([click here to access the interactive topic model visualization](https://raw.githack.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/main/output/visuals/sklearn_train_lda_model_viz.html)). The obtained visual representation is symptomatic of a good model. In spite of this theoretical assumption, as stated above, incorrect labelling was critical mainly related to news articles assigned with the category "diverse topics" from the manual rule-based classifier. To a certain extent, the topics generated by the LDA model portray the previously mentioned semantic landscape of the rock news headlines:

+ Topic 0: album and single releases;
+ Topic 1: tour announcement;
+ Topic 2: live performance and song related;
+ Topic 3: miscellaneous;
+ Topic 4: video and festival. <br><br>

**Topic relevance by headline in the test set**
![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/6c30984f56efb720c592076a061e8d602610c927/output/visuals/lda_sklearn_topics_by_text_random_sample.png)

<br>

**Labelling accuracy of the topic "album and single releases"**
![](https://github.com/IvoDSBarros/Rock-is-not-dead_NLP-experiments-on-rock-news-articles/blob/51e7c119dd079256fdc97f82be3fe9482e4b308b/output/visuals/lda_sklearn_labelling_accuracy_album_single.png)

<br>

#### 2. LDA model using Gensim
Replicability and instability are two major issues of topic modelling. The Ensemble LDA method aims to mitigate these issues by *"finding and generating stable topics from the results of multiple topic models"* and remove topics *"that are noise and are not reproducible"* (Řehůřek, 2022b). Additionally, there is no *"need to know the exact number of topics ahead of time"* (Řehůřek, 2022a).

#### Results
The Ensemble LDA returned 7 topics which represent the semantic landscape of the rock news headlines more effectively:
+ Topic 0: tour announcement; 
+ Topic 1: live performance related;
+ Topic 2: single and video releases;
+ Topic 3: album announcement;
+ Topic 4: song related;
+ Topic 5: video and movie related;
+ Topic 6: artist death related;

<br>

**Top 10 words by topic**

![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/42a6ef8d36cbf1a3f92267ffa2ba4c4f40e91316/output/visuals/gensim_topics_words_top_10.png)

<br>

**Topic relevance by headline in the test set**
![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/dbf4f756fc1b6188b3ee99876191b8b1a2b07f7f/output/visuals/lda_gensim_topics_by_text_random_sample.png)

<br>

**LDA evaluation model metrics in Gensim** <br>
A UMass Coherence score and Perplexity were used to evaluate the Ensemble LDA model. Within the topic modelling context, *"a set of statements or facts is said to be coherent, if they support each other"* (Röder et al., 2015). In simple terms, coherence is the *"humans’ semantic appreciation of a topic represented by its N top words"* (Trenquier, 2018). The UMass coherence score relies on document frequency and considers order among the top words of a topic (Röder et al., 2015). It reaches its peak at 0 meaning that topics are perfectly coherent. The UMass was chosen over C_V metric as the latter is not recommended *"when it is used for randomly generated word sets"* (Roeder, 2018). A UMass score of -14.9 was obtained. This indicates topics are not fully coherent. However, the 7 topics model was the most consistent (significantly better than the LDA Scikit-learn model) with human interpretation. Once again, a manual random topic assignment validation was conducted to assess the accuracy of the models. Incorrect labelling was detected mostly referring to news articles tagged with the manual rule-based category "diverse topics". The Perplexity (formula: 2^(-bound)) value is considered acceptable as it is the lower of the 3 models returned by LDA Ensemble. In contrast to the LDA Scikit-learn model, the perplexity value consistently decreased while the number of topics increased.

<br>

**Perplexity and Coherence score over number of topics in the test set**
![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/2c2fe431a98634554e179355e56412e2990cbf32/output/visuals/lda_gensim_assessment.png)

+ **Perplexity** = 163.2 
+ **UMass coherence score** = -14.9

<br>

**Labelling accuracy of the topic "album announcement"**
![](https://github.com/IvoDSBarros/rock-is-not-dead_nlp-experiments-on-rock-news-articles/blob/9723231925d2d4ba00f3781d5e7dab8997c094cf/output/visuals/lda_gensim_labelling_accuracy_album.png)

<div align = "right">    
  <a href="#overview">(back to top)</a>
</div>

## Rule-based text classification Vs. Machine Learning classification: final thoughts and further research

+ Natural language is intrinsically ambiguous in its lexical, semantic or syntactic form (Yadav et al., 2021). The text corpus of this study, comprised of unlabelled rock news headlines and descriptions, is very revealing of this ambiguity. Additional subjectivity and complexity come along with ambiguity as a news article can be assigned to multiple categories making this process a multi-label text classification challenge.

+ The rule-based text classification model is more reliable as a whole, more flexible, accurate and consistent with  human interpretation. However, the required analysis to set up and maintain a manual rule-based text classifier is demanding and time-consuming. Furthermore, the developed manual rule-based text classifier also generates inaccurate assignments.

+ Instability and non-reproducibility are two well-known issues of the LDA algorithm. In respect to reliability, flexibility and accuracy, the rule-based text classifier outperformed both the unsupervised machine learning models tested. The diversity of the rock news headlines' semantic landscape was better captured by the LDA Gensim Ensemble model when compared to the LDA Scikit-learn model. Inaccurate assignments were recurring in both LDA models but more critical in the Scikit-learn.

+ A hybrid text classification method will be further developed from the generated labelled data of the rule-based text classification model.

+ Following the approach of M. Kelechava (2019), the LDA Gensim Ensemble model will be used as a basis for a machine learning supervised model.    

<br>

**Manual rule-based text classification Vs. Unsupervised Machine Learning classification**
<br>The alluvial diagram below is based on the test set.
<br>Sklearn and Gensim LDA main topics below 40% were categorized as "multi-category".

![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/07fa5db4494b21f3468d70390d497fa3e8b1dff3/output/visuals/rule_based_vs_%20ml_lda.png)

<div align = "right">    
  <a href="#overview">(back to top)</a>
</div>

## Visualization portfolio
**Metallica: the monster still lives**
<br>Infographic based on the text corpus

![](https://github.com/IvoDSBarros/Experimenting-NLP-on-rock-news-articles/blob/bbd96d23028682b430b1a392bc1e343436d3deff/output/visuals/metallica_the_monster_still_lives_infographic.png)

<br>

**Tableau Public**
<br>The layout of the vizzes below have been set for desktop. When using phone/tablet, the viewing of the dashboard is not optimal.
<br>

+ [Rock Is Not Dead: 2022 Year In Review](https://public.tableau.com/app/profile/ivo.barros/viz/Rock_Is_Not_Dead_Year_In_Review/Dashboard)

+ [It's Only Rock N' Roll But I Like It: Infographic](https://public.tableau.com/app/profile/ivo.barros/viz/Its_Only_Rock_N_Roll_But_I_Like_It_Infographic/Infographic)

+ [It's Only Rock N' Roll But I Like It: Dashboard](https://public.tableau.com/app/profile/ivo.barros/viz/Its_Only_Rock_N_Roll_But_I_Like_It_Dashboard/Dashboard)

<div align = "right">    
  <a href="#overview">(back to top)</a>
</div>

## References
+ [Blei, D., Ng, A., Jordan, M. (2003) Latent Dirichlet Allocation. Journal of Machine Learning Research, 3, 993-1022.](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)
+ [Chang, J., Boyd-Graber, J., Gerrish, S., Wang, C., Blei, D. (2009, December) Reading Tea Leaves: How Humans Interpret Topic Models. NIPS'09: Proceedings of the 22nd International Conference on Neural Information Processing Systems, 288–296.](https://proceedings.neurips.cc/paper/2009/file/f92586a25bb3145facd64ab20fd554ff-Paper.pdf)
+ [Kelechava, M. (2019) Using LDA Topic Models as a Classification Model Input. Predicting Future Yelp Review Sentiment. Towards Data Science.](https://towardsdatascience.com/unsupervised-nlp-topic-models-as-a-supervised-learning-input-cf8ee9e5cf28)
+ [Řehůřek, R. (2022a) models.ensembelda – Ensemble Latent Dirichlet Allocation. https://radimrehurek.com/gensim/models/ensemblelda.html](https://radimrehurek.com/gensim/models/ensemblelda.html)
+ [Řehůřek, R. (2022b) Ensemble LDA. https://radimrehurek.com/gensim/auto_examples/tutorials/run_ensemblelda.html](https://radimrehurek.com/gensim/auto_examples/tutorials/run_ensemblelda.html)
+ [Roeder, M., Both, A., Hinneburg, A. (2015, February). Exploring the Space of Topic Coherence Measures. WSDM '15: Proceedings of the Eighth ACM International Conference on Web Search and Data Mining, 399–408. https://doi.org/10.1145/2684822.2685324](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf)
+ [Roeder, M. (2018) Not being able to replicate coherence scores from paper #13](https://github.com/dice-group/Palmetto/issues/13)
+ [Sievert, C., Shirley, K. (2014, June) LDAvis: A method for visualizing and interpreting topic. Proceedings of the Workshop on Interactive Language Learning, Visualization, and Interfaces, 63–70.](https://nlp.stanford.edu/events/illvi2014/papers/sievert-illvi2014.pdf) 
+ [Trenquier, H. (2018) Improving Semantic Quality of Topic Models for Forensic Investigations. University of Amsterdam, MSc System and Network Engineering Research Project 2.](https://rp.os3.nl/2017-2018/p76/report.pdf)
+ [Yadav, A., Patel, A., Shah, M. (2021) A comprehensive review on resolving ambiguities in natural language processing. AI Open, Volume 2, Pages 85-92](https://www.sciencedirect.com/science/article/pii/S2666651021000127)

<div align = "right">    
  <a href="#overview">(back to top)</a>
</div>
