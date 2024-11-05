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

_In this post I am going to explain some of the acute findings and knowledge I've gained while building a small practical text search app without using frameworks like langchain or external third-party APIs. Additionally, I've also extended the development to convert this app to infuse RAG(Retrieval Augmented Generation) capabilities using Gemini Flash 15B model._

---

## What is text retrival?

## Vectors and Embeddings

## What is Retrival Augmented Generation?

## App in a nutshell

![](/images/learnings_from_text_embeddings_RAG.jpg)

<div class="row no-gutters image-grid">
        <div class="col-6">
            <img src="/images/search.png" class="img-fluid border border-primary" alt="Image 1">
        </div>
        <div class="col-6">
            <img src="/images/search_results.png" class="img-fluid border border-primary" alt="Image 2">
        </div>
        <div class="col-6">
            <img src="/images/ask_results.png" class="img-fluid border border-primary" alt="Image 3">
        </div>
        <div class="col-6">
            <img src="/images/data_operations.png" class="img-fluid border border-primary" alt="Image 4">
        </div>
    </div>

![](/images/insomnia_output.png)

![](/images/insomnia_output_2.png)

![](/images/vector_database_structure.png)

## Text Retrieval algorithm: K-Nearest-Neighbor

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

## Knowledge Augmentation Prompting
