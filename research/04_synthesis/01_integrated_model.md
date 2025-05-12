# Integrated Model for NexusInsight Development

This document presents an integrated model for the NexusInsight project, synthesizing findings from the initial research phase. It outlines a conceptual architecture and key operational flows based on primary findings ([`research/02_data_collection/01_primary_findings_part1.md`](research/02_data_collection/01_primary_findings_part1.md), [`research/02_data_collection/01_primary_findings_part2.md`](research/02_data_collection/01_primary_findings_part2.md)) and identified patterns ([`research/03_analysis/01_patterns_identified.md`](research/03_analysis/01_patterns_identified.md)).

## I. Core Architectural Pillars

NexusInsight is envisioned as a sophisticated client application heavily reliant on a specialized backend (`wikidata-mcp`) for accessing and processing Wikidata.

1.  **NexusInsight Client (Frontend):**
    *   **Responsibilities:** User Interface (UI), User Experience (UX) management, interactive visualization rendering, visual query construction interface, client-side state management, and communication with `wikidata-mcp` and potentially a minimal client-side backend.
    *   **Key Characteristics:**
        *   **Rich Interactivity:** Enables dynamic exploration of graph data (pan, zoom, node expansion, filtering).
        *   **Intuitive Visual Query Builder:** Allows users to construct complex Wikidata queries without writing SPARQL.
        *   **Optimized Rendering:** Employs efficient techniques (e.g., WebGL, virtualization, incremental updates) to handle potentially large subgraphs served by `wikidata-mcp`.
        *   **Responsive Design:** Adapts to various screen sizes and provides timely feedback.
        *   **User-Centric:** Incorporates UX strategies like progressive disclosure and guided workflows.

2.  **`wikidata-mcp` Server (Primary Backend):**
    *   **Responsibilities:** Interface with raw Wikidata (SPARQL endpoint, data dumps), query execution and optimization, data preprocessing (subgraph extraction, aggregation, formatting for visualization), schema provision, autocomplete services, translation of visual queries into executable queries (e.g., SPARQL), and caching.
    *   **Key Characteristics:**
        *   **Data Abstraction Layer:** Shields NexusInsight client from the complexities of direct Wikidata interaction.
        *   **Performance-Oriented:** Designed for low-latency responses and efficient handling of large data requests.
        *   **Schema-Aware:** Provides necessary metadata to support intelligent query building and data interpretation on the client.
        *   **Extensible API:** Exposes well-defined endpoints (as detailed in Section 5 of primary findings) for entity search, connection exploration, query execution, etc., ideally following Model Context Protocol (MCP) principles for discoverability.

3.  **Optional Client-Side Backend/Orchestration Layer:**
    *   **Responsibilities (if deemed necessary):** User authentication, user session management, storage and retrieval of user-specific data (preferences, saved queries, annotations), and potentially complex client-side state orchestration or API aggregation not suitable for the primary `wikidata-mcp`.
    *   **Key Characteristics:**
        *   **Lightweight:** Focused on user-specific concerns, not core Wikidata processing.
        *   **Secure:** Manages sensitive user information.

## II. Key Operational Flows

### A. Interactive Visual Exploration of Connections:

1.  **Initiation:** User searches for an entity or starts from a bookmarked/predefined entry point in the NexusInsight client.
2.  **Initial Data Fetch:** Client requests initial data for the target entity and its immediate neighborhood from `wikidata-mcp` via an API call (e.g., `get_entity_details`, `get_direct_connections`).
3.  **`wikidata-mcp` Processing:** `wikidata-mcp` queries Wikidata, preprocesses the results (e.g., extracts relevant subgraph, formats for visualization), and returns an optimized data payload to the client.
4.  **Client Rendering:** NexusInsight client renders the initial subgraph using its chosen visualization library.
5.  **User Interaction:** User interacts with the graph (e.g., clicks to expand a node, applies a filter).
6.  **Incremental Data Fetch:** Client sends new requests to `wikidata-mcp` for additional data based on user interaction (e.g., `explore_neighborhood` for an expanded node).
7.  **`wikidata-mcp` Responds:** Backend processes the request and sends back the new data chunk.
8.  **Client Updates:** Client incrementally updates the visualization, integrating the new data smoothly.
    *   *Underlying Mechanisms:* Progressive loading, Level-of-Detail (LOD) rendering, client-side and server-side caching.

### B. Visual Query Building:

1.  **Interface Presentation:** NexusInsight client presents a visual canvas or structured interface for query construction.
2.  **User Constructs Query:** User drags and drops visual elements representing entity types, properties, relationships, and applies filters.
    *   **Schema Assistance:** Client leverages `wikidata-mcp` (e.g., via `search_entity`, `search_property` endpoints, or a dedicated schema endpoint) to provide autocomplete suggestions and validate choices in real-time.
3.  **Visual Query Representation:** Client maintains an internal representation of the visual query.
4.  **Translation Request (Optional/Iterative):** Client might send partial visual query structures to `wikidata-mcp` for validation or preview (e.g., `validate_query_fragment`).
5.  **Final Query Submission:** Once the user finalizes the visual query, the client sends its representation (e.g., JSON structure) to `wikidata-mcp` (e.g., via `translate_visual_query` or directly to an endpoint that accepts visual queries).
6.  **`wikidata-mcp` Translation & Execution:** `wikidata-mcp` translates the visual query into an executable query (e.g., SPARQL), optimizes it, and executes it against Wikidata.
7.  **Results Processing & Return:** `wikidata-mcp` processes the results, potentially formatting them for display or further exploration, and returns them to the client.
8.  **Client Displays Results:** NexusInsight client displays the results, which could be a list, a table, or a new/updated visualization.

## III. Foundational Technologies & Strategies

*   **Frontend Stack:** Likely a modern JavaScript/TypeScript framework (React, Vue, or Svelte) combined with a powerful graph visualization library (Cytoscape.js, Sigma.js, or a D3.js-based custom solution) capable of WebGL rendering for performance.
*   **Backend (`wikidata-mcp`):** Technology stack as per its existing/planned implementation (e.g., Python/Node.js).
*   **API:** RESTful or GraphQL API for `wikidata-mcp`, adhering to MCP principles where possible.
*   **Data Handling:** Emphasis on incremental loading, pagination, efficient data structures, and caching at both client and server levels.
*   **UX Principles:** Progressive disclosure, guided workflows, clear visual hierarchy, and responsiveness.

## IV. Integration with User Blueprint

This model directly supports the core concepts from the [`docs/blueprint.md`](docs/blueprint.md):
*   **Interactive Visual Exploration:** Addressed by Flow II.A and the client/`wikidata-mcp` responsibilities.
*   **Visual Query Building:** Addressed by Flow II.B.
*   **Technological Suggestions:** Aligns with blueprint's suggestions for JS/TS frontend, choice of frameworks, and backend considerations.
*   **`wikidata-mcp` Reliance:** Central to this integrated model.

This integrated model provides a high-level architectural and operational framework. Specific technology choices and detailed API specifications will depend on further investigation into the knowledge gaps identified in [`research/03_analysis/03_knowledge_gaps.md`](research/03_analysis/03_knowledge_gaps.md), particularly concerning the concrete capabilities of `wikidata-mcp` and optimal visualization techniques for Wikidata's scale.