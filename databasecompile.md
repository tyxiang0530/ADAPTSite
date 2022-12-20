---
title: Physicist Database
layout: default
filename: databasecompile.md
--- 
# Database Compilation

A key component of ADAPT is a continually
updated database of physicists and their demographic information, including binary gender, race or ethnicity, and birthplace. The database was initially constructed by
scraping information from websites of notable physicists, and is continuously updated by ADAPT as new names are identified in examined textbooks. The database is periodically checked by hand for accuracy. 

To initially construct the database, we compile lists of physicists and scientists from aggregator sites such as [the Wikipedia list of physicists](https://en.wikipedia.org/wiki/List_of_physicists). We place all of these into a txt file and then begin querying each names Wikipedia entry (if it exists) from the Wikimedia API. With the WikiMedia API, we can scrape the scientists name, birth country, birth city, race, gender. Sometimes, these fields are not available, but simply having a name in the database allows for faster and more accurate name detection from texts.

Our final compiled physicist database is an extensive list of around 3000 unique dictionaries containing the following information: The name of the person, the name returned when wikipedia is queried, whether the person is a human, the gender of the person, the birth city of the person, the birth country of the person, the race of the person, a 'MatchName' variable that is used for matching in the name grabbing phase, a 'FullName' variable that is used for checking in the wikipedia querying phase, the race of the person according to Wikipedia, the race of the person conducted through hand-tagging and a 'MatchingNames' variable that is leftover for sanity checking purposes. 

We manually supplement the race of the queried entry through hand-tagging. This hand-tagging is done through phenotypic observation by two researchers independently. Only races that have agreement from both taggers are retained.

# Database Headers

A sample entry in the database looks like the following:

| Name            | Wiki Name       | Human | Sex  | Birth Location  | Birth Country  | Race      | MatchName | FullName        | Wiki Race  | Matching Names |
|-----------------|-----------------|-------|------|-----------------|----------------|-----------|-----------|-----------------|------------|----------------|
| Michael Faraday | Michael_Faraday | human | male | Newington Butts | United Kingdom | caucasian | FARADAY   | MICHAEL FARADAY |            |                |

'Name' denotes the cleaned name of the scientist, 'Wiki Name' denotes the raw name returned from querying wikipedia, 'Human' denotes the attribute of the individual (if the queried output is not a human, it will have a different attribute tag. This is useful if last names such as 'Carnot' query 'Carnot Engine' instead of 'Sadi Carnot'). 'Sex' denotes the sex of the individual queried from Wikipedia, 'Birth Location' denotes the birth city, 'Birth Country' denotes the birth country, 'Race' denotes the hand-tagged race, 'MatchName' is a simplified last-name string from the individual. It contains only the last name of the physicist in the entry. We match last names instead of full names as we are iterating over tokens. This conveniently is also helpful to minimize the effect of OCR error, abbreviations, and odd formatting, as the simplicity reduces the chance of words being OCRed wrong. Additional information on OCR error can be found in the 'error sources' section of the website. 'FullName' is a string used for checking if the final returned physicist name matches fully with the database entry, 'Wiki Race' is the race as queried by Wikipedia, and 'Matching Names' is the boolean that informs whether the name tagged in the text matches the database entry or a queried Wikimedia API entry.

The database can be continually and automatically updated every time a new textbook is run through it. If an entry that is not in the database is encountered and tagged by the named entity recognition method, the entry will be queried through the Wikimedia API and then added into the database. This means that the more books that are run through the tool, the better the database gets.

# Extensions

If one wishes to create a similar tool for fields such as chemistry and biology, the brunt of the work will be in creating an additional database specific to those fields. To do so, a similar procedure can be followed.

1. Compile scientist names through aggregator sites and lists
2. Use the Wikimedia API to grab the necessary attributes from Wikipedia
3. Begin running textbooks through the tool. This process will not be used to generate results, rather they will allow the named entity recognition algorithm to grab out more names.
4. Use the Wikimedia API to grab attributes for your new names
5. Manually supplement any lacking headers. Race will most likely be the most sparse field, and some hand-tagging may be necessary to fill this field in. Alternatively, one can begin analysis on textbooks and fill in the necessary fields on outputted results. After completing this hand-tagging, update the database with these results.