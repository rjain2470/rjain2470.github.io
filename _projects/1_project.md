---
layout: page
title: conductor
description: "A natural-language interface to the L-functions and Modular Forms Database (LMFDB)."
img: assets/img/conductor_flowchart.png
importance: 1
category: work
related_publications: false
---
Website: [conductormath.org](https://conductormath.org).
GitHub: [https://github.com/rjain2470/conductor](https://github.com/rjain2470/conductor).

<img src="/assets/img/conductor_flowchart.png" alt="Diagram of Conductor’s workflow." style="float: right; width: 35%; max-width: 300px; margin: 0 0 1rem 1.5rem;">

The L-functions and Modular Forms Database ([LMFDB](https://www.lmfdb.org)) is a comprehensive dataset of mathematical objects arising in number theory and arithmetic geometry, cataloguing hundreds of thousands of elliptic curves, modular forms, number fields, and more. With the increasing usage of machine learning methods in mathematics, it has become an important source for exploratory data analysis and mathematical discovery, with one notable example being the discovery of the [murmurations phenomenon](https://arxiv.org/abs/2204.10140).

While the LMFDB has a web interface, querying and analyzing data often requires going through its underlying SQL server, which is often difficult even for technical users. To solve this problem, we introduce Conductor, a natural language interface to the LMFDB. It translates mathematical questions into SQL, executes them against the LMFDB PostgreSQL database, and returns structured data with optional exploratory analysis and plots. It is designed to make the rich collection of mathematical data in the LMFDB more accessible for exploration, research, and teaching.

Given a query in plain English, Conductor identifies and retrieves the relevant mathematical data from the database, and can optionally summarize, analyze, or visualize the results at the user's convenience. This allows users to explore the LMFDB without needing to learn its internal structure or write database queries by hand.

For instance, a mathematician can directly ask Conductor the following questions:

<p style="color: gray;"><em>"Plot the regulator against the conductor for the rank-1 elliptic curves over Q with conductor under 10,000 on a log-log scale."</em></p>
<p style="color: gray;"><em>"What is the average rank of the elliptic curves with conductor less than 5000?"</em></p>
<p style="color: gray;"><em>"Give me a table of the weight-2 newforms with CM at squarefree levels under 500."</em></p>

Under the hood, Conductor operates via a multi-stage pipeline. First, queries are classified and scanned for keywords. Then, a separate LLM later routes to the relevant parts of the SQL schema, pulls the relevant data, then optiomally passes the results to a data analysis stage. Finally, the output is returned to the user together with all of the generated code. In sum, Conductor is an attempt to reduce friction for mathematicians using the LMFDB and accelerate progress in data-driven mathematical research, while ensuring a maximal amount of transparency.

## Limitations 

- The server connects to devmirror.lmfdb.xyz, which may only have partial coverage of the full LMFDB. Moreover, since the LMFDB itself is not fully comprehensive, some data may be unavailable.
- Queries are subject to API rate limits. Therefore, responses may slow under heavy load.
- Conductor is under active development, and thus you may encounter occasional errors or unexpected behaviour. If you do, please open a GitHub issue to report it.

## Future Work

As of now, Conductor operates via a layered pipeline of LLM calls which handle various tasks, including user interactions, SQL generation, data analysis, and more. While this pipeline is functional and works well, its reasoning capabilities are limited by the fact that each stage is executed independently with a small context window.

A more robust and modern architecture would replace the current layered approach with a single agentic LLM equipped with tools designed for it to execute each aspect of the pipeline. Here, rather than passing through a fixed pipeline with a different LLM at each stage, the model would plan its own approach, iterating and correcting errors until it has an optimal final response.

Another related direction is developing a Model Context Protocol (MCP) server for the LMFDB. Such tooling would allow any MCP-compatible client, such as Claude, or Cursor, to query the database directly, bypassing the need for a dedicated tool. This framework would not only make the LMFDB more accessible to human mathematicians, but would also open the door for agentic AI systems to explore mathematical data autonomously. As agentic systems grow increasingly capable, we believe that such substrates may provide exciting opportunities for mathematical discovery.
