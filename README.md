# Riven [![Built with spaCy](https://img.shields.io/badge/made%20with%20❤%20and-spaCy-09a3d5.svg)](https://spacy.io)

Riven is an open source library built on [spaCy](https://spacy.io/) for processing and extracting information from unstructured financial documents such as annual reports, quarterly reports, prospectuses, proxies and news.

**NOTE!** Riven is not functional yet. This repository only contains boilerplate code.

## Why We Are Building Riven

Financial analysts spend a lot of time analyzing information in public disclosure and news. Much of this time is spent extracting information from the text in documents, so that they can gather insights and refine their investment thesis. 

We believe advances in natural language processing can greatly reduce the time it takes to analyze financial documents, freeing up time for higher level analysis. Riven is an experimental project that aims to provide the building blocks for such analysis. Examples of problems you can use Riven to solve include:

* Extracting operational data from financial documents (e.g. I want to see all the operational metrics Uber discloses)
* Comparing operational performance (e.g. I want to compare GMV, orders, MAUs, average basket size of e-commerce companies)
* Comparing accounting policies (e.g. I want to compare the revenue recognition policy of several companies)
* Finding publicly available information in news about a private company

## Why Riven?

* Riven is the first open source model trained for use on extracting metrics and their related values and dates from unstructured financial documents
* Note that Riven can be used to extract information from text only. It cannot be used to extract information from tables.
* Riven is built on [spaCy](https://spacy.io/), which makes it easy to pick up and apply to your own data
* Riven has been trained on US SEC financial disclosure and is highly suited to identifying and extracting finance related metrics
* It is free and open source

## About the Model

This is the very first release of Riven and the model is best viewed as a *prototype*. It is rough around the edges and represents the first step in open source research into NLP on financial texts. 

With that out of the way, here's a brief rundown of what's happening in the model:

...

## Named-Entity Recogniser

The NER component of the Blackstone model has been trained to detect the following entity types:

| Entity        | Name            | Examples                             |
| ------------- |---------------- | ------------------------------------:|
| COMPANY       | Company         | e.g. Snap, Amazon, Zoom              |
| METRIC        | Metric          | e.g. MAU; GMV; Customers             |
| VALUE         | Value           | e.g. 400 million; US$40 billion, 425 |
| DATE          | Date            | e.g. as of December 31, 2018; for fiscal 2018 |

## Usage

*Below are the key components in Riven, which we will expand on once built*

1. **Retrieve Filings.** Retrieves US SEC filings for tickers and saves them locally. Under the hood, Riven uses [Tika](https://tika.apache.org/)) to convert them into text and provides helper methods to clean the data for analysis.

```
# Download and save all annual reports for Amazon
scrapy crawl filing -a ticker=AMZN -a type="10-K"

# Filing will be downloaded in /data and converted to text format as well for analysis

...
```

2. **Named entity recognition.** Recognize company, metrics, values and dates within a document.

```
import spacy

# Load the model
nlp = spacy.load("riven_en")

# Text you want to analyze
text = "For the fiscal year ended January 31, 2019, we had 344 customers that contributed more than $100,000 of revenue, compared to just 143 customers one year before."

# Apply the model to the text
doc = nlp(text)

# Call displacy and pass `ner_displacy_options` into the option parameter`
displacy.serve(doc, style="ent", options=ner_displacy_options)
```

Which produces something that looks like this:

*Note example image only to be replaced by finance related one*

<img src="https://iclr.s3-eu-west-1.amazonaws.com/assets/iclrand/Blackstone/displacy.png">

3. **Information extraction.** Returns a list of company, metrics, values and dates identified in a document

```
import spacy

# Load the model
nlp = spacy.load("riven_en")

# Text you want to analyze
text = "For the fiscal year ended January 31, 2019, we had 344 customers that contributed more than $100,000 of revenue, compared to just 143 customers one year before."

# Apply the model to the text
doc = nlp(text)

# Get extracted relations
for relation in doc.relations:
    print(relation)

>>>
["customers that contributed more than $100,000 of revenue", 344, "fiscal year ended January 31, 2019"]
["customers that contributed more than $100,000 of revenue", 143, "fiscal year ended January 31, 2018"]

```
## References

* [Spacy / Prodigy - NLP library and annotation tool](https://spacy.io/)
* [Flair - NLP library](https://github.com/flairNLP/flair)
* [Doccano - open source annotation tool](https://github.com/doccano/doccano)
* [Magi - Chinese semantic search engine](https://magi.com/)
* [ReVerb - OpenIE library](http://reverb.cs.washington.edu/)
* [Open Information Extraction](https://openie.allenai.org/)
