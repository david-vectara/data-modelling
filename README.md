# Vectara Data Modelling Walkthrough

In this walk through we're going to look at the process of data modelling within Vectara
with the goal to answer a question.

## Dataset
For this example, we've chosen various product manuals for Pacemakers from Medtronic. This information
can be found here:

* Landing Page: https://www.medtronic.com/us-en/patients/treatments-therapies/pacemakers/our.html
* Library: https://manuals.medtronic.com/manuals/main/en_US/home/index

We have downloaded this data in the folder Medtronic and put the associated metadata in "pace_makers.csv".

## Goals
The key to performing successful data modelling is to understand the kinds of questions you want to ask of
the data. We need to ensure that answers are given from the specific model the user is interested in - a key
issue with LLMs and purely unstructured RAG architectures is that, lacking filter attributes, the responses
may be hallucinations or blurred from another product line.

We envisage this unstructured data and metadata will be used in the following flow:

Drill down path (iterative questions):
In this pathway, we get the user to "drill down" into their model from the manufacturer. This has a high number of interactions.
1. User Asks General Question, e.g. "How do we prepare for an implant"
2. The system, without a manufacturer runs a semantic query using the original question.
3. Using the metadata on the response, propose manufacturers. Take user answer.
4. Re-run the semantic query, and propose a list of products. Take user answer.
5. Re-run the semantic query and propose a list of models. Take user answer.
6. We re-run the original query:
    1. Using the full results, we provide facets on the UI showing the "type" and "date" released
    2. We show the summarized results across all documents for this model

As a last step, a UI could further restrict answers to a "type" of document or the "date" released which would create more focused answers from a targetted release.

Shortcut path (known model number):
1. User first inserts a model number
2. We lookup products using a metadata filter
3. Assuming we get a single matching result (case insensitive)
4. We "lock in" the manufacturer, product and model from this:

## Scope
In this tutorial, we will perform the work to ingest the data and build test harnesses. Out of scope for now, though maybe in another tutorial, we will build the user interface which implements these user paths using the filter attributes.