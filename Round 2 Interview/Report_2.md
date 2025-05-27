## Exploring a Data-Driven Chat: A Look at the Pizza RAG Project

This project dives into an interesting approach for making data, specifically from an Excel spreadsheet about pizza orders, "talkable." The idea is to build a system where you can ask questions in plain English and get answers based on the data, a technique often called Retrieval Augmented Generation (RAG).

**Setting the Stage: Tools and Data**

The first steps involved getting the right tools for the job. This meant installing and setting up several specialized Python libraries. Key players here are `llama-index` (a framework for building these kinds of applications), `llama-parse` (for understanding the document structure), and libraries for connecting to Google's Gemini models (for creating data "embeddings" – numerical representations – and for the language understanding part) and Pinecone (a cloud-based vector database to store these embeddings efficiently).

API keys, the digital keys to these services (Google, Llama Cloud, Pinecone), were configured. The system was then told to use specific Gemini models: "models/text-embedding-004" for turning text into these numerical fingerprints and "models/gemini-2.5-flash-preview-04-17" as the "brain" to process questions and generate answers.

**Getting the Data Ready**

The core data, initially in an Excel file named `pizza_data.xlsx`, was first converted into a more universally friendly CSV (Comma Separated Values) format. This `pizza_data.csv` then became the focus.

A tool called `LlamaParse` was brought in to read this CSV. An interesting detail here was the "system prompt" given to `LlamaParse`. This prompt was quite ambitious, outlining a series of data transformations: converting order and delivery times, extracting features like the hour or day of the week, encoding categorical information (like restaurant names or pizza types), normalizing numbers, and even creating new insights like "is weekend" or "is rush hour."

However, what `LlamaParse` primarily did in this setup was to take the raw CSV data and structure it as a Markdown table. This Markdown version, while well-formatted, didn't seem to incorporate the advanced feature engineering described in that initial prompt. It essentially presented the original data in a new textual skin.

This Markdown output was then further processed. Each line, representing a row from the original pizza order data (including the header row), was treated as a separate piece of information or "node." This resulted in 125 distinct nodes.

**Teaching the System: Embedding and Storing**

With the data broken down into these 125 row-based nodes, the next step was to make them understandable for the AI. Each node was fed to the Gemini embedding model, which converted the text of each row into a dense numerical vector. Think of it as giving each row a unique, rich fingerprint.

These fingerprints, along with their original text, needed a home. This is where Pinecone came in. A new vector index (a specialized database for these embeddings) named "excel-rag" was set up in the cloud (specifically, on AWS in the us-east-1 region). The system then carefully "upserted" – uploaded and indexed – all 125 of these vectorized data rows into Pinecone. Now, the data was not just stored, but organized in a way that similar rows (semantically speaking) would be "close" to each other in this vector space.

**Putting It to the Test: Asking Questions**

The final stage was to query this newly built system. A query engine was created, acting as an interface to ask questions. When a question is posed:

1.  The query engine converts the question into an embedding (using the same Gemini model).
2.  It searches the Pinecone index for data rows whose embeddings are most similar to the question's embedding.
3.  These retrieved rows (the most relevant context from the dataset) are then passed to the Gemini language model along with the original question.
4.  The language model uses this context to formulate an answer.

Let's see how it did with a few test questions:

*   **"How many orders were delivered by Domino's in total?"**
    The system responded: `"There were 2 orders delivered by Domino's."`
    Looking at the raw data preview, Domino's appears much more frequently. This suggests the RAG system might have retrieved only a couple of Domino's entries and based its answer on that limited sample, rather than aggregating across the entire dataset effectively.

*   **"Where are the locations of the deliveries?"**
    The response was a list:
    `The locations where deliveries take place include: New York, NY, Chicago, IL, Los Angeles, CA, Houston, TX, Phoenix, AZ, Miami, FL, Omaha, NE, Louisville, KY, Milwaukee, WI, Albuquerque, NM, Atlanta, GA.`
    This seems quite reasonable. The system likely pulled various rows mentioning these cities and synthesized the list.

*   **"ORD016 order details"**
    The answer here was: `"The details for order ORD016 are not available."`
    This was a bit of a surprise. Given that each row, including the one for ORD016, was individually embedded and stored, one might expect it to be retrievable. The data for ORD016 clearly shows details like `Domino's, Denver, CO, 2024-01-16 19:45:00`, etc. This could point to a few things: perhaps the retrieval didn't pinpoint this specific row effectively, or the LLM, even if provided with the row, couldn't confidently extract and present it as "details."

**Some Parting Thoughts**

This exploration demonstrates a modern way to interact with structured data. The setup is quite sophisticated, leveraging powerful AI models and vector databases.

The discrepancy between the ambitious LlamaParse prompt (aiming for feature engineering) and its actual use here (more like CSV-to-Markdown conversion) is noteworthy. It highlights that the tool was used for basic parsing, and the data itself remained in its raw tabular form within the Markdown, rather than being pre-transformed into a more analytical structure.

The performance on queries shows promise, especially for factual lookups spread across different entries (like locations). However, it also reveals challenges, particularly with aggregate questions (like counting all Domino's orders) or very specific row lookups that might get lost in the semantic shuffle. The strategy of treating each CSV row as an independent document is a straightforward first step for RAG on tabular data, but it might not always be the most effective for all types of questions one might ask of a dataset.

Overall, it’s a fascinating peek into building a conversational interface for data, showcasing both the potential and the current areas where refinement could lead to even more accurate and comprehensive answers.