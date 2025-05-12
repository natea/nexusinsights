# Potential Information Sources for NexusInsight Research

This document outlines potential sources of information to answer the key research questions for the NexusInsight project. The primary method of information gathering will be targeted queries to Perplexity AI, supplemented by reviews of specific documentation and community resources where appropriate.

## 1. Primary Information Source:

*   **Perplexity AI (via MCP Tool):**
    *   **Method:** Formulate specific queries based on the [Key Research Questions](02_key_questions.md) document.
    *   **Focus:** Leverage Perplexity AI's ability to synthesize information from a wide range of web sources, academic papers, and technical documentation.
    *   **Data to Extract:** Answers to key questions, summaries of technologies, best practices, potential challenges, existing tool analyses, and API design considerations. Citations will be requested and recorded.
    *   **System Prompts for Perplexity AI will emphasize:**
        *   The context of building a "superior Wikidata client (NexusInsight)" that relies on a `wikidata-mcp` backend.
        *   The need for actionable insights, comparisons, and practical recommendations.
        *   Focus on modern, scalable, and user-centric solutions.

## 2. Secondary & Supporting Sources (to be consulted directly if Perplexity AI results are insufficient or require deeper dives):

*   **Technology Documentation:**
    *   **Frontend Frameworks:** Official documentation for React, Vue.js, Svelte (and others identified).
    *   **Visualization Libraries:** Official documentation and examples for D3.js, Cytoscape.js, Vis.js, Sigma.js, React Flow, Chart.js, Apache ECharts, etc.
    *   **Backend Technologies (if client-side backend is explored):** Documentation for Node.js (with Express/Fastify), Python (with Flask/FastAPI).
    *   **Databases (for user data):** Documentation for PostgreSQL, MongoDB.

*   **Wikidata & Related Tools:**
    *   **Wikidata Query Service (WDQS):** Documentation and examples (primarily to understand its capabilities and limitations, which `wikidata-mcp` aims to abstract).
    *   **Existing Wikidata Client Tools:**
        *   Reasonator: Explore its interface and information presentation.
        *   SQID (Wikidata Item Quality Evaluator): Analyze its approach to item summaries.
        *   Scholia: Review its scholarly profile generation and visualization techniques.
    *   **Wikidata API Documentation:** To understand the underlying data structures that `wikidata-mcp` will likely interact with.

*   **Visualization Platforms & Communities:**
    *   **Kumu.io:** Explore its features, gallery, and any available documentation on its graph visualization approach.
    *   **Flourish.studio:** Analyze its range of visualizations and ease of use.
    *   Communities and blogs related to data visualization (e.g., Nightingale - Journal of the Data Visualization Society, FlowingData).

*   **Academic & Research Papers:**
    *   Search (via Perplexity AI or Google Scholar) for papers on:
        *   Knowledge graph visualization and exploration.
        *   User interfaces for query builders.
        *   Performance optimization for large-scale graph rendering.
        *   Human-computer interaction with complex datasets.

*   **Technical Blogs & Articles:**
    *   Articles on web development best practices, frontend architecture, API design, and data visualization techniques from sources like:
        *   Smashing Magazine
        *   CSS-Tricks
        *   Medium (relevant tech publications)
        *   Developer blogs from companies working with large-scale data.

*   **Open Source Projects:**
    *   Explore GitHub repositories of similar open-source data exploration tools or visualization components to understand implementation details and common patterns.

## Information Management:

*   Findings from Perplexity AI will be documented in the `02_data_collection` folder, primarily in `01_primary_findings.md` (and its parts).
*   Information from secondary sources will supplement these findings or be noted in `02_secondary_findings.md` (and its parts).
*   All significant sources used will be compiled in `research/05_final_report/06_references.md`.

This list is not exhaustive and may be expanded as the research progresses and new, relevant sources are identified.