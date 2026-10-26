# Tagging-customer-reviews-with-an-LLM-no-BERT-allowed-
I recently built a pipeline that tags short skincare reviews with a two-level label scheme (a broad topic, plus a more specific factor nested under it). Each post can carry several labels.

How I approached it:

🔹 Taxonomy from data: I pulled the valid Level 1 → Level 2 pairs from the labelled examples, so the model only chooses from labels that exist.
🔹 Retrieval + few-shot prompting: for each post, TF-IDF finds the most similar labelled posts, and the LLM sees them as examples. That teaches it the quirks of how this dataset is labelled.
🔹 LLM on Groq (free tier): the model returns JSON, and several posts go in one request to stay inside rate limits.
🔹 Guardrails: every answer is checked against the taxonomy. Invented or mismatched labels are dropped, and a fallback covers any post the model fails on.
🔹 Honest evaluation: I held out part of the labelled data and measured precision, recall and F1 at both levels, next to a simple TF-IDF + Logistic Regression baseline. Results: [Level 1 F1 = __, Level 1 + 2 F1 = __ vs baseline __].
