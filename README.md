# Querying and Granularity: Exploring Sustainability within The MET's API 

## Nicolas Lord Info 664 Final Project 

## Description
My project looks closely at the different ways of requesting information in [The MET's API](https://metmuseum.github.io/) in order to look at objects in the collection that have information around the topic of sustainability. Also the project overall demonstrates the ways someone could query different topics in the MET and use stemming or smaller words to single out objects. Furthermore, once I came to the finding of a dataset of vases with trees on them, I used graphing techniques to point out overall what mediums are used mostly for a collection of objects under a specific topic. This can help inform of popular crafting techniques within the MET's collection. 

## Rationale 
I decided to explore this topic because it was broad enough to get started with the API and look for specific information. I wanted to show ways that a curator or researcher could single out objects in a large digital museum collection to look more specific at the medium and other subsequent information such as the dates, artist, culture, linked tags to ULAN, Getty AAT vocabularies, and wikidata pages. I hope that in doing a project like this it would demonstrate the research that can be done for exhibitions that explore environmental themes, eco-friendly materials, and raise awareness about ecological issues.

## Workflow
To complete this project I used components of an API request like root, path, and endpoint, and inspected the results using type() and dir() functions to understand the type of objects in the results of my request. 

### ex:
- root = 'https://collectionapi.metmuseum.org'
- path = '/public/collection/v1/search'
- parameter ='?medium=Vases&q=tree'

I then used .json() to parse response objects. 
Once I had a list of objectids, I then looped through the amount of matching objectids to the search (when searching sustainability it was 288). This allowed me to iterate through the objectids, slice through and create a subset of just the 288 objectids from the query result, construct a URL string, parse through the response with .json() to format into a python dictionary, and add the parsed dictionary to the first_results list: 

'''
{ 
first_results = []
for item in objectids[:288]:
    url = f'https://collectionapi.metmuseum.org/public/collection/v1/objects/{item}'
    response = requests.get(url)
    parsed = response.json()
    first_results.append(parsed)
}
'''
Once that was complete I was able to call for an objectid result to see the fields included in the collection information which helped inform the use of the **pandas library** in creating a new dataset: 


{ 
import pandas as pd 
titles = []
names = []
mediums = []
countries = []
accessionyears = []
urls = []
tags = []
objectwikidataurls = []

for item in first_results:
    title = item.get('title')
    titles.append(title)
    name = item.get('artistDisplayName')
    names.append(name)
    medium = item.get('medium')
    mediums.append(medium)
    country = item.get('country')
    countries.append(country)
    accessionyear = item.get('accessionYear')
    accessionyears.append(accessionyear)
    url = item.get('objectURL')
    urls.append(url)
    tag = item.get('tags')
    tags.append(tag)
    objectwikidataurl = item.get('objectWikidata_URL')
    objectwikidataurls.append(objectwikidataurl)
}

I then saved the results of the dataset as a csv. 

Once I had a csv table, I soon found out that when searching for "sustainability" or "environment" the results would include smaller words or variations such as "sustain" or "envision." This made it difficult to understand if the art was actually containing the topic I was looking for. I then tried to look up specific artists and mediums but ran into the same problem with other words populating the results or artists not being in the collection/ having work outside of the scope of the topic. I then decided to query a medium and subsequent descriptive word that would illustrate nature and allow for further investigation and came to "Vase" containing "Tree."

I then imported a new csv dataset containing very concrete matches of 207 Vases with trees on them or describing the object. Here is an example of one of the [results](https://www.metmuseum.org/art/collection/search/254190) This is the second item of the csv I've attached to the repository called "Met_vases_collection_info"

Once the new dataset was imported I used **pandas** and **matplotlib** libraries to work with the dataset and visualize differnt columns into a pie chart showing differnt mediums within the vase collection. 

I plotted the mediums that were mentioned in at least 10 objects into a bar and a pie chart, here is the bar chart code:  

{
df.value_counts('medium').nlargest(10).plot(kind='barh', xlabel='Frequency of Medium', title='Vases with Trees') 
}

The results showed that Terrecotta and Hard-paste porcelein were the most frequently used materials for vases depicting nature icons in The MET collection. 

## Futher Uses
I hope that demonstrating how to query in the above sections and visualize the information could encourage other researches to explore the MET API and pull out collection information that interested them under a specific topic. This could help establish trends and even compare information to other museum collections with a public API that can be easily queryed such as the Getty or the Art Institute of Chicago. The code above and in the attached python notebook files can be transformed for different searches and different data sets using the **response** areas of the MET API. Then you could use pandas to customize the desired fields to put into a dataset for further investigation. The python notebook containing graphing information could also serve as an example if a researcher wants to graph under a differnt field such as time-span, frequency of objects being from a particular country, etc. 

## Files
### Python Notebooks:
- met medium api vases tree search.ipynb
- Graphing vase collection content.ipynb
- Met API exploration around sustainability.ipyn

### CSV Files: 
- Met_vases_collection_info.csv
- Met_sustainable_collection_info.csv


