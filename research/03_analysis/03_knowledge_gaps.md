# Knowledge Gaps Identified for NexusInsight Research

This document outlines identified knowledge gaps, unanswered questions, areas requiring deeper exploration, and assumptions needing validation based on the initial research phase for the NexusInsight project. The primary findings are documented in [`research/02_data_collection/01_primary_findings_part1.md`](research/02_data_collection/01_primary_findings_part1.md) and [`research/02_data_collection/01_primary_findings_part2.md`](research/02_data_collection/01_primary_findings_part2.md), and initial patterns in [`research/03_analysis/01_patterns_identified.md`](research/03_analysis/01_patterns_identified.md).

These gaps will inform targeted research queries in subsequent cycles.

## 1. `wikidata-mcp` Server - Specific Capabilities & Limitations

*   **Gap:** While the research confirms the *conceptual* reliance on `wikidata-mcp` and outlines *desired* API functionalities (Section 5 of primary findings), the *current, concrete capabilities, performance benchmarks, and development status/roadmap* of the actual `wikidata-mcp` server (referenced as `github.com/zzaebok/mcp-wikidata` in citations) are not detailed. The `wikidata-mcp` repository https://github.com/ebaenamar/wikidata-mcp is an alternative to the one mentioned above.
*   **Questions:**
    *   What is the current implementation status of the API endpoints listed in Section 5.2 of the primary findings (e.g., `get_direct_connections`, `get_path_between`, `translate_visual_query`) in the `zzaebok/mcp-wikidata` or `ebaenamar/wikidata-mcp` projects?
    *   What are the performance characteristics (latency, throughput) of the existing `wikidata-mcp` for typical queries envisioned by NexusInsight?
    *   What are the known limitations or planned development priorities for `wikidata-mcp` that might impact NexusInsight's feature set or timeline?
    *   How does `wikidata-mcp` handle data freshness from Wikidata, and what are the typical sync latencies?
*   **Assumption to Validate:** The `wikidata-mcp` server can/will be developed to efficiently support all critical API interactions required by NexusInsight.

## 2. Interactive Visual Exploration - Specific Techniques for Wikidata Scale

*   **Gap:** General strategies for large-scale graph visualization (progressive loading, aggregation, WebGL) are identified (Section 1.1, 3.1). However, specific, proven examples or best practices for applying these techniques *specifically to the scale and structure of Wikidata data* via a client-server model like NexusInsight/`wikidata-mcp` need more detail.
*   **Questions:**
    *   Are there case studies or examples of web-based clients successfully visualizing and interacting with Wikidata-scale subgraphs (e.g., thousands of nodes/edges dynamically loaded) using a backend for pre-processing? What specific libraries/approaches were most effective?
    *   What are the most effective data abstraction/aggregation techniques for Wikidata to present meaningful summaries in an interactive visual explorer, considering its rich but sometimes inconsistent schema?
    *   How can `wikidata-mcp` best support dynamic Level-of-Detail (LOD) rendering and progressive loading for the client? What data structures or API calls are optimal?
*   **Assumption to Validate:** Existing frontend visualization libraries can be effectively adapted to provide a smooth interactive experience with data chunks served by `wikidata-mcp` without overwhelming the client.

## 3. Visual Query Building - UX and Technical Implementation Details

*   **Gap:** Paradigms for visual query builders are known (Section 1.2), but detailed UX patterns for building complex, Wikidata-specific queries (including qualifiers, ranks, multi-hop paths) visually, and the precise translation logic to SPARQL (or other `wikidata-mcp` query language) need further exploration.
*   **Questions:**
    *   What are best-practice UI/UX patterns for visually constructing queries that involve Wikidata-specific features like item types, properties, qualifiers, and ranks in an intuitive way for non-SPARQL experts?
    *   How can the visual query builder provide effective real-time feedback and validation, leveraging `wikidata-mcp`'s schema awareness, without excessive API calls?
    *   What are the complexities and edge cases in translating a rich visual query representation into optimized SPARQL queries suitable for `wikidata-mcp`? Are there existing libraries or algorithms that can be adapted?
*   **Assumption to Validate:** A visual query language can be designed that is both expressive enough for meaningful Wikidata exploration and simple enough for users unfamiliar with SPARQL.

## 4. Frontend Technology - Optimal Choices for NexusInsight's Specific Needs

*   **Gap:** Pros and cons of various frontend frameworks (React, Vue, Svelte) and visualization libraries (D3, Cytoscape.js, Sigma.js, React Flow) are listed (Sections 2.1, 2.2). However, a more targeted recommendation based on the *specific combination* of `wikidata-mcp` interaction, large-scale graph visualization, and visual query building needs further synthesis.
*   **Questions:**
    *   Given the reliance on `wikidata-mcp` for heavy lifting, which frontend framework offers the best balance of rendering performance for dynamic graph updates, state management for complex UI, and developer ecosystem for visualization library integration?
    *   Which specific visualization library (or combination) is best suited for rendering potentially large, interactive subgraphs received from `wikidata-mcp`, supporting features like dynamic loading, clustering, and smooth interaction, while integrating well with the chosen frontend framework?
    *   Are there examples of projects with similar client-server architecture and visualization goals that have successfully used these technologies?
*   **Assumption to Validate:** A suitable combination of modern frontend framework and visualization library exists that can meet all performance, interactivity, and development requirements.

## 5. User-Specific Data - Scalability and Integration

*   **Gap:** Options for user data storage (PostgreSQL, MongoDB) are discussed (Section 2.4). The extent and complexity of user-specific data (saved queries, visualization states, user preferences, annotations) and how this integrates with the core Wikidata exploration flow needs clarification.
*   **Questions:**
    *   What is the anticipated volume and complexity of user-specific data?
    *   How will user-specific data (e.g., saved visual queries, custom graph layouts) interact with or overlay the data retrieved from `wikidata-mcp`?
    *   What are the security and privacy implications for storing user-specific data related to their Wikidata explorations?
*   **Assumption to Validate:** The chosen user data storage solution can scale and integrate seamlessly with the primary application flow.

## 6. Benchmarking Existing Tools - Quantitative Insights

*   **Gap:** Strengths and weaknesses of existing tools are qualitatively assessed (Section 4). Quantitative benchmarks or deeper usability studies of these tools, especially concerning performance with large datasets or ease of complex query formulation, could provide more concrete targets for NexusInsight.
*   **Questions:**
    *   Are there published benchmarks on the performance (query speed, rendering speed) of tools like WDQS, Scholia, or Kumu.io when handling complex queries or large graph segments from Wikidata?
    *   Are there usability studies that identify specific pain points in existing Wikidata exploration tools that NexusInsight should prioritize addressing?
*   **Assumption to Validate:** NexusInsight can demonstrably improve upon specific, measurable aspects of existing tools.

*(This list will be refined and updated as research progresses and new information comes to light.)*