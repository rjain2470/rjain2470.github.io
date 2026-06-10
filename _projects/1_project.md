---
layout: page
title: conductor
description: "A natural-language interface to the L-functions and Modular Forms Database (LMFDB)."
img: assets/img/conductor_flowchart.png
importance: 1
category: work
related_publications: false
---

GitHub: [https://github.com/rjain2470/conductor](https://github.com/rjain2470/conductor). Website: [conductormath.org](https://conductormath.org).

Conductor is a natural language interface to the [LMFDB](https://www.lmfdb.org) (L-functions and Modular Forms Database). It translates mathematical questions into SQL, executes them against the LMFDB PostgreSQL database, and returns structured data with optional exploratory analysis and plots. It is designed to make the rich collection of mathematical data in the LMFDB more accessible for exploration, research, and teaching.

Given a query in plain English, Conductor identifies and retrieves the relevant mathematical data from the database, and can optionally summarize, analyze, or visualize the results at the user's convenience. This allows users to explore the LMFDB without needing to learn its internal structure or write database queries by hand.

For instance, a mathematician can directly ask Conductor the following questions:

> **Can you plot the regulator against the conductor for the rank-1 elliptic curves over Q with conductor under 10,000 on a log-log scale?**

> **Plot the real period vs the analytic order of Sha for elliptic curves of rank 2 with conductor under 5,000.**

> **Which semistable elliptic curves have prime conductor under 500 and non-trivial torsion? Show me the distribution of torsion subgroup structures.**

> **I'm interested in the relationship between regulator and discriminant for totally real cubic fields of class number 1 — can you pull those and plot them on a log-log scale?**

> **Give me a table of the weight-2 newforms with CM at squarefree levels under 500.**

By utilizing modern methods in prompt engineering, text-to-SQL, and AI-powered data analysis, Conductor allows users to spend less time navigating databases and more time exploring mathematical questions.

## Architecture 🏛️

<p align="center">
  <img src="/assets/img/conductor_flowchart.png"
       alt="Conductor's backend pipeline."
       width="90%">
</p>

The backend of Conductor consists of a seven-stage FastAPI pipeline with error handling. We utilize Claude Haiku 4.5 for classification, else we use Claude Sonnet 4.6, which handles user interactions and more complicated tasks. It works as follows:

0. An intent classifier determines whether the incoming message is a mathematical query or a conversational message. Conversational messages receive a natural response and skip all subsequent stages.
1. An LLM-as-judge assesses query precision before any database interaction. If the query is ambiguous, it asks a followup question. If clear, it returns a refined restatement passed to all subsequent stages.
2. A lightweight object resolution stage fires when the query references a concrete mathematical object and resolves it to a database identifier. If no concrete object is found, the query passes through unchanged.
3. Our LLM maps the query to a list of relevant LMFDB table names using a two-layer hierarchical schema index (16 domains, 86 tables).
4. Our LLM produces a validated SQL query using the tables identified in Stage 3. Correctness is enforced by using our preloaded schema as a ground truth.
5. We run the SQL over a read-only SQLAlchemy connection with a 15-second timeout, returning a pandas DataFrame.
6. *(optional)* We translate a follow-up natural language instruction into Python. Plots and subsequent data analysis are captured in-memory and returned as base64-encoded PNGs alongside the generated code.

## Future Work 💠

As of now, Conductor operates via a layered pipeline of LLM calls which handle various tasks, including user interactions, SQL generation, data analysis, and more. While this pipeline is functional and works well, its reasoning capabilities are limited by the fact that each stage is executed independently with a small context window.

A more robust and modern architecture would replace the current layered approach with a single agentic LLM equipped with tools designed for it to execute each aspect of the pipeline. Here, rather than passing through a fixed pipeline with a different LLM at each stage, the model would plan its own approach, iterating and correcting errors until it has an optimal final response.

Another related direction is developing a Model Context Protocol (MCP) server for the LMFDB. Such tooling would allow any MCP-compatible client, such as Claude, or Cursor, to query the database directly, bypassing the need for a dedicated tool. This framework would not only make the LMFDB more accessible to human mathematicians, but would also open the door for agentic AI systems to explore mathematical data autonomously. As agentic systems grow increasingly capable, we believe that such substrates may provide exciting opportunities for mathematical discovery.

## Limitations 🟥

- The server connects to devmirror.lmfdb.xyz, which may only have partial coverage of the full LMFDB. Moreover, since the LMFDB itself is not fully comprehensive, some data may be unavailable.
- Queries are subject to API rate limits. Therefore, responses may slow under heavy load.
- Conductor is under active development, and thus you may encounter occasional errors or unexpected behaviour. If you do, please open a GitHub issue to report it.

## Acknowledgements 🌲

This work would be impossible without the collective work of hundreds of mathematicians in computing and curating the data which is available on the LMFDB. See [lmfdb.org/acknowledgment](https://www.lmfdb.org/acknowledgment) for a list of contributors.
