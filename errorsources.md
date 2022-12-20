---
title: Error Sources
layout: default
filename: errorsources.md
--- 

# Error and Bias Sources

There are a handful of error and bias sources that exist in this tool. This section will cover some of them and explain the effects they may have.

## OCR Error

We typically have to rely on scans of textbooks to remain under fair-use policy. However, in order to prepare scans of textbooks into a machine-readable format, we need to utilize optical character recognition (OCR). OCR is a method by which machine learning or algorithmic methods convert image pixel data into text strings that can then be read into the program. However, if scans are poor quality, then OCR can return erroneous results. Furthermore, certain characters are difficult to differentiate, such as a capital 'i' and a lowercase 'L'. Though modern techniques apply machine learning and use context to avoid such mix-ups, errors can still occur. These errors can deter database matching from occuring. For a future extension, it may be advantageous to utilize a database matching algorithm that is tolerant towards error (for instance, if enough letters match, then we still register a database hit). 

The program works the best with source textbooks, and thus if buy-in from authors and publishers occurs, we can return more accurate results.

It should be noted that OCR error is not a deal-breaker for the algorithm. An example we have seen is the name 'Daniel Bernoulli' being read in as 'Dniel Bernouli'. Though this is not directly matched by the database, it is still tagged by named entity recognition. When queried through the Wikimedia API, 'Dniel Bernouli' still returns the appropriate information for 'Daniel Bernoulli'. However, the 'MatchingNames' boolean for the returned information is False since the input name and the queried output do not match. This is why some human post-processing may still be required after the tool has run.

## Wikipedia Gender and Race Bias

The predominant source from which we query demographic information on physicists from is Wikipedia. If information on a specific physicist does not exist within the Wikipedia database, then we will have incomplete information. However, Wikipedia articles generally favor an over-representation of white men. These biases are acknowledged by Wikipedia itself ([racial bias](https://en.wikipedia.org/wiki/Racial_bias_on_Wikipedia), [gender bias](https://en.wikipedia.org/wiki/Gender_bias_on_Wikipedia)) and have been covered in papers such as ["Can History Be Open Source? Wikipedia and the Future of the Past"](https://www.jstor.org/stable/4486062), by Roy Rosenzweig. An numerical example of such biases is that in a 2021 study conducted by the site, only 17% of published biographies are on women.

This bias reveals that the heavy use of Wikipedia as a database can yield overrepresentations of white men in our database. However, we have attempted to remedy this by aggregating names from historical websites and texts that focus on minority physicists. We supplement the database with hand-tagged information of these entries.

## Named Entity Recognition Bias

There are a wide array of papers covering biases in named entity recognition methods. Mishra et al., in their 2020 paper "Assessing demographic bias in named entity recognition" note that “We observe significant performance gap between White names and names from other demographics”. Due to the training sets used by named entity recognition methods overrepresenting white names, these algorithms tend to be better and parsing out names such as "John" and "Bob" rather than names of minorities. This further contributes to our tool being better at finding White names than minority names.

## Bias of the Majority

White men tend to be overrepresented in physics textbooks. As we run more textbooks through our database, our named entity recognition will end up tagging more White men, and as a result, more White men will be added to the database. Since database matching is the most accurate form of name grabbing, our tool will be better at grabbing white men out of textbooks than minorities.

## Binary Gender and Singular Race

For computational simplicity, we treat gender as binary in our output. We also only tag a singular race. However, we acknowledge that gender and race are far more complex areas and can extend beyond the outputs we yield.