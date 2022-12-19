---
title: How the database is compiled
layout: template
filename: codemethod.md
--- 
# Database Compilation

A key component of ADAPT is a continually
updated database of physicists and their demographic information, including binary gender, race or ethnicity, and birthplace. The database was initially constructed by
scraping information from websites of notable physicists, \cite{Note2} and is continuously updated by ADAPT as new names are identified in examined textbooks. The database is periodically checked by hand for accuracy. 

The physicist database is an extensive list of around 3000 unique dictionaries containing the following information: The name of the person, the name returned when wikipedia is queried, whether the person is a human, the gender of the person, the birth city of the person, the birth country of the person, the race of the person, a 'MatchName' variable that is used for matching in the name grabbing phase, a 'FullName' variable that is used for checking in the wikipedia querying phase, the race of the person according to Wikipedia, the race of the person conducted through hand-tagging and a 'MatchingNames' variable that is leftover for sanity checking purposes. 



The 'MatchName' variable is used for name matching. It contains only the last name of the physicist in the entry. We match last names instead of full names as we are iterating over tokens. This conveniently is also helpful to minimize the effect of OCR error, abbreviations, and odd formatting, as the simplicity reduces the chance of words being OCRed wrong.