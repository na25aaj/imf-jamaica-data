IMF Jamaica Report: PDF-to-Dashboard Pipeline (data files)

This repository holds the data files used by my Google Colab notebook for the Data Analysis with AI assignment. The notebook reads an IMF country report, uses LLMs to summarise it and pull out key indicators, checks the results with a second LLM acting as a judge, and builds a 4-panel dashboard.

Notebook: https://colab.research.google.com/drive/1yfFCKf05VGyIZmeWbO-rPg1KScsOwdu0

What's in this repository

File	What it is
null-002.2023.issue-083-en.pdf	The source report: Jamaica: 2022 Article IV Consultation, IMF Country Report No. 23/83
cache.zip	Saved responses from every LLM call the notebook makes
README.md	This file
LICENSE	MIT License for my own files in this repository

Why these files are here

	•	The PDF. The IMF website blocks downloads from cloud servers like Colab (it returns a 403 error), so the notebook downloads the report from this repository instead.
	•	The cache. Every Gemini response is saved the first time it is generated. When the notebook runs, it downloads cache.zip and reuses those saved answers, so it runs from start to finish without an API key and gives the same numbers as in my report.

How to run the notebook

	1.	Open the notebook using the Colab link above.
	2.	Click Runtime → Run all. If Colab warns that the notebook wasn't written by Google, click Run anyway.
	3.	The PDF and the cache download automatically, and every cell runs from the saved responses.

To make fresh API calls instead, add a Gemini API key to Colab secrets (key icon in the left sidebar) named GEMINI_API_KEY, switch on notebook access, and remove the cache_url from the config cell.

Pipeline overview

	1.	PDF cleaning: text extraction with pypdf, automatic header/footer removal, section detection (21 sections)
	2.	Chunking: about 400-token chunks with 50-token overlap, built from whole sentences (173 chunks)
	3.	RAG index: BAAI/bge-small-en-v1.5 embeddings in a FAISS IndexFlatL2 index
	4.	Extraction: section summaries, executive summary, theme counts, strengths and vulnerabilities, and indicator JSON with schema validation and fallbacks
	5.	Evaluation: gemini-3.5-flash-lite judges the outputs of gemini-3.1-flash-lite, alongside non-LLM checks (quote and number verification)
	6.	Extension: Multi-Query RAG compared with single-query RAG
	7.	Dashboard: tidy data with pd.melt and a 300 dpi 4-panel figure

Licence and source

My own files in this repository (this README and the cached responses) are released under the MIT License. See LICENSE.

The IMF report is not covered by that licence. It belongs to the International Monetary Fund and is included only so the notebook can run. Source:
IMF (2023) Jamaica: 2022 Article IV Consultation – Press Release; Staff Report; and Statement by the Staff Representative. IMF Country Report No. 23/83. Available at: https://www.imf.org/en/Publications/CR/Issues/2023/02/10/jamaica-2022-article-iv-consultation-press-release-staff-report-and-staff-statement-529664
