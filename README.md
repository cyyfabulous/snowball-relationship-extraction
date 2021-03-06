# snowball-relationship-extraction
<p align="center"><img src="./snowball_flowchart.png" align="center"> </img> </p>

1. **Entity Resolution** - The most challenging and important step in explicit link extraction is of resolving the entities from mass media articles with the entities present in Neo4j graph database for which sophisticated ER algorithms have been used. 
1. **Creating Subset** - The mongoDb database contains around 4.9 million articles crawled from various news portals. With an aim to extract *located in* and *works in* relations from an article having ORGANIZATION as one of the entities we created a subset of articles mentioning at least one organization which is featured in our neo4j graph database. It significantly reduced the number of articles to around 50,000.
1. **Tagging** - The subset of articles are now tagged using the output of open-calais which provides the type, location, prefix, suffix etc of the entities present in the article.
1. **Snowball Execution** - After tagging, we created positive and negative seed sets for extracting works in and located in relations which were fed to the snowball classifier along with the set of tagged articles and other params. The output file contained the name of related entities along with the confidence.
1. **Verification** - These relations extracted from mass media articles are termed as explicit relations. We verified the extracted relations by reading the articles, the correct relations were pushed to the graph database thereby enriching the existing database.
1. **Database Augmentation** - Database is augmented with new links extracted from news articles.    

**Tag Articles Script**
1. tagArticles-1 contains the code for tagging the resolved entities mentioned in the articles. The resolved entities were stored in a mongo db collection.
1. tagArticles-2 this is another script to tag articles. In this script I tagged companies in graph database and the locations. Since, tagging the resolved locations only was not a good idea, as at a number of places locaion mentioned in the article could be a city in a district or the name of the location may contain some prefix like "North", "South" etc. So, I tagged all the locations mentioned in the article which were resolved later at the time of verification and data augmentation.
1. tagArticles-3 script to tag and store the articles in a collection instead of storing it in a dictionary. This is useful in case if the number of articles is more and can not be accomodated in a dictionary

**Seed set Folder** It contains positive and negative seed set which we have created for extracting different kind of relations along with the articles in which those entities were mentioned

### Snowball  
Snowball is a semi-supervised relation extraction approach and it requires only handful of examples as seed for learning and extracting structured data from plain text documents. These seed sets generate numerous patterns to include named-entity tags, for example \textit{<ORGANIZATION> in <LOCATION>}, this pattern will only match the pair of strings connected by "in" and connected strings are tagged by tagger as entity of types <ORGANIZATION> and <LOCATION>.  
**Command to run snowball**  
` python2 Snowball.py paramters.cfg sentences_file seeds_file_positive seeds_file_negative similarity_threshold confidance_threshold`  
In our experiments, setting similarity_threshold and confidence threshold to `0.7` gives the good result.
