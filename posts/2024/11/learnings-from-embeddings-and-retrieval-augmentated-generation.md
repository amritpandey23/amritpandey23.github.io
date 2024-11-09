<!--
.. title: Learnings from Embeddings and Retrieval Augmentated Generation
.. slug: learnings-from-embeddings-and-retrieval-augmentated-generation
.. date: 2024-11-30 12:53:55 UTC+05:30
.. tags:genai
.. category: Artificial Intelligence
.. link:
.. description: Here is the summary of insights I've gained after building a practical text search app using vector embedding and RAG without using frameworks or external APIs.
.. type: text
-->

_In this post, I’ll share insights and key findings from building a practical text search application without relying on frameworks like LangChain or external third-party APIs. I’ve also extended the app’s functionality to support Retrieval-Augmented Generation (RAG) capabilities using the Gemini Flash 15B model._

---

## What is Text Retrieval?

Text retrieval is the science of extracting relevant information from a dataset in response to a query. This challenge has a long history, predating modern information systems. Before digital systems, relevant data was retrieved by manually annotating documents with titles, summaries, and keywords.

After World War II, with the need to index large volumes of scientific publications, early solutions to text retrieval, or "ad-hoc retrieval," led to what we now call "search engines." Unlike today’s web-based search engines, early search systems were designed to solve retrieval problems for specific datasets using metadata, like human annotations.

It wasn’t until about a decade after the war that researchers developed methods for automated text retrieval, such as term-frequency and inverse-document-frequency (TF-IDF) algorithms. These algorithms match the frequency of terms in the query and corpus. This approach remains relevant, with the `BM25` algorithm as a widely used method in text retrieval today.

## Vectors and Embeddings

A vector is a mathematical representation, essentially a set of numbers. For example:

`[1, 2, 3]`

If we assign meaning to these numbers, they become useful. Say we represent the world in X, Y, and Z dimensions; then, `[1, 2, 3]` could indicate an object's location in this 3D space relative to a reference point.

In computer science, vectors often represent data numerically. For instance, a text string can be represented as a vector of 250 numbers. In text retrieval and natural language processing (NLP), vectors allow us to semantically compare meanings across sentences. Converting text into numbers enables computers to process language more efficiently.

Machine learning models trained on large datasets can convert text into these numerical representations, called _embeddings_. An embedding is essentially a vector produced when text data is transformed into a numerical format by a trained model.

## What is Retrieval-Augmented Generation (RAG)?

Retrieval-Augmented Generation (RAG) combines three key elements to solve the problem of generating relevant responses using Large Language Models (LLMs).

Let’s break down each component:

### Retrieval

As discussed in the [section above](#what-is-text-retrieval), retrieval involves extracting relevant information from a corpus. This is essential, as irrelevant data can lead to RAG failing in its objective.

In our application, data retrieval is designed to prioritize accuracy and relevance.

### Augmentation

Augmentation involves providing the model with context or information in the prompt to enhance its responses. A technique known as "Knowledge Augmentation" allows us to supplement LLMs with information they might lack. However, each LLM has a limit on the amount of context it can accept, known as the token limit. Therefore, the retrieval system must fetch not only accurate but also concise information from the knowledge base.

### Generation

Once retrieval and augmentation are complete, the LLM generates a response based on the relevant, augmented data provided in the prompt.

By now, readers should have a clearer understanding of RAG. RAG is crucial for generating responses on topics an LLM lacks knowledge in, serving as an alternative to fine-tuning, which can be time-consuming and costly.

## My Text Embedding and RAP Application

It's a common adage in engineering that one should never reinvent the wheel. I don’t subscribe to this. If we don’t understand how the wheel was originally invented, how can we hope to design something as complex as a car and we probably can never invent something like a car without wheels. Building from scratch is essential for understanding the challenges faced by existing systems, as it fosters insights and innovative solutions.

For this project, there are several frameworks that offer out-of-the-box utilities. The most popular is LangChain, which provides essential tools and utilities for building LLM applications. Additionally, free services like Cohere's search library and OpenAI’s text embedding model handle some of the core tasks I need. However, using them would only teach me how to work with their APIs, not the fundamental principles behind them.

### The story

To start, I designed a vector database from scratch. The design documents and diagrams are available in the GitHub repository. I used SQLite with the SQLite-Vec extension to transform it into a vector database.

I abstracted database operations using Python by creating wrappers around the SQLite database. This was crucial to encapsulate lower-level details and keep the implementation clean and manageable.

For text embeddings, I initially used TensorFlow’s open-source Wiki-Words model, which is trained on Wikipedia data. Later, I switched to Google’s Universal Sentence Encoder, which generates embeddings of size 512 (compared to 250 with Wiki-Words) and offers greater accuracy.

For the LLM, I used the Gemini Flash 1.5B model, my only external service dependency for generating responses. I am also exploring on-device models for LLM tasks, as this is a field I’m keen to dive deeper into.

For the frontend, I built a React application that communicates with the backend server using CORS. The UI is simple and sleek, with customizable client-side settings.

Overall, building this app was incredibly rewarding, giving me hands-on experience with core concepts like vector databases, embeddings, and model integration. I learned a lot along the way, and it’s been a valuable experience in practical engineering.

![](/images/learnings_from_text_embeddings_RAG.jpg)

#### Searching

The search functionality works as follows:

1. The user's query is converted into an embedding, and the system retrieves the nearest neighbors of this embedding from the stored text embeddings in the vector database.
2. The results are displayed in ascending order of distance from the query embedding, showing the closest matches first.

<!-- ![](/images/search.png) -->

![](/images/search_results.png)

#### Asking (RAG)

The process for asking questions and generating responses is as follows:

1. The user submits a question, which is converted into an embedding. Similar to the search process, the system retrieves a list of nearest embeddings from the vector database.
2. The retrieved search results are provided to the LLM in a knowledge-augmented prompt to generate a response.
3. The LLM generates a response based on the provided context.

![](/images/ask_results.png)

#### Data Operations

To expand the data available for search and response generation, the Data Operations page allows users to add new data to the database.

1. The user enters text data, which is then converted into embeddings and stored in the vector database.

![](/images/data_operations.png)

### Backend Server Application

![](/images/insomnia_output.png)

![](/images/insomnia_output_2.png)

### Vector Database internals

![](/images/vector_database_structure.png)

### Text Retrieval algorithm: K-Nearest-Neighbor

```sql
SELECT
    rowid,
    distance
FROM
    text_embeddings
WHERE
    sample_embedding
MATCH
    ?
AND
    k = {k}
ORDER BY
    distance
```

## Source Code
