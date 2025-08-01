# Task
Answer questions about a document and provide references

## Inputs
- **Document:** `{$DOCUMENT}`
- **Question:** `{$QUESTION}`

## Instructions
I'm going to give you a document. Then I'm going to ask you a question about it.

1. First, write down exact quotes from the document that help answer the question, numbered in order. Quotes should be relatively short.
2. If there are no relevant quotes, write "No relevant quotes" instead.
3. Then, answer the question, starting with "Answer:". Do not include or reference quoted content verbatim in the answer. Don't say "According to Quote [1]" when answering. Instead, make references to quotes relevant to each section of the answer solely by adding their bracketed numbers at the end of relevant sentences.

Here is the document:

```
{$DOCUMENT}
```

Here is the question:

```
{$QUESTION}
```

**Format your response as follows:**

### Relevant Quotes
1. "Company X reported revenue of $12 million in 2021."
2. "Almost 90% of revenue came from widget sales, with gadget sales making up the remaining 10%."

### Answer
[1] Company X earned $12 million. [2] Almost 90% of it was from widget sales.

If the question cannot be answered by the document, say so.

Answer the question immediately without preamble.