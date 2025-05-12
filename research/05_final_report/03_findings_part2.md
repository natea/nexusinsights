# NexusInsight Strategic Research - Detailed Findings (Part 2)

This document (Part 2) continues the detailed compilation of primary research findings for the NexusInsight project, focusing on existing tools, inspirations, and `wikidata-mcp` server interaction.

*Original primary findings documents: [`research/02_data_collection/01_primary_findings_part1.md`](../02_data_collection/01_primary_findings_part1.md) and [`research/02_data_collection/01_primary_findings_part2.md`](../02_data_collection/01_primary_findings_part2.md).*

---
## 3.4 Existing Tools & Inspirations
*(Derived from [`research/02_data_collection/01_primary_findings_part2.md#section-4-existing-tools--inspirations-questions-q41-q42`](../02_data_collection/01_primary_findings_part2.md#section-4-existing-tools--inspirations-questions-q41-q42))*

**Query 4 Summary:** Reviewed existing Wikidata exploration tools (Reasonator, SQID, Scholia, WDQS) and visualization platforms (Kumu.io, Flourish) to identify strengths and weaknesses relevant to NexusInsight.

### 3.4.1 Review of Wikidata Exploration Tools (Addressing Q4.1)

*   **Wikidata Query Service (WDQS):**
    *   Strengths: Direct SPARQL access, powerful, official. (Citation [P2S4C1])
    *   Weaknesses: Steep learning curve for SPARQL, UI can be intimidating, limited built-in visualization. (Citation [P2S4C1])
    *   NexusInsight: Aim to provide WDQS power via visual queries, better UX.
*   **Reasonator:**
    *   Strengths: Human-readable summaries, good for quick entity overview. (Citation [P2S4C2])
    *   Weaknesses: Limited query capability, not primarily a graph exploration tool.
    *   NexusInsight: Could integrate Reasonator-like summaries.
*   **SQID (Wikidata Item Quality Dashboard):**
    *   Strengths: Focus on data quality, useful for data curation insights.
    *   Weaknesses: Not a general exploration tool.
    *   NexusInsight: Could incorporate quality indicators if relevant.
*   **Scholia:**
    *   Strengths: Excellent for scholarly data (publications, researchers), specific views. (Citation [P2S4C3])
    *   Weaknesses: Domain-specific, less general-purpose exploration.
    *   NexusInsight: Could offer specialized views inspired by Scholia for certain domains.

### 3.4.2 Review of Visualization Platforms (Addressing Q4.2)

*   **Kumu.io:**
    *   Strengths: Highly interactive, good for network mapping, storytelling features, easy to use for non-coders. (Citation [P2S4C4])
    *   Weaknesses: Not directly integrated with Wikidata, data import needed, can be slow with very large networks.
    *   NexusInsight: Emulate Kumu's interactivity and ease of visual manipulation.
*   **Flourish:**
    *   Strengths: Wide range of chart types, good for storytelling, embeddable.
    *   Weaknesses: Not primarily a graph/network tool, data import needed.
    *   NexusInsight: Less direct inspiration for core graph features, but good for general data presentation principles.
*   **Gephi:**
    *   Strengths: Powerful desktop tool for graph analysis and visualization, many layout algorithms. (Citation [P2S4C5])
    *   Weaknesses: Desktop-based, not web-native, steep learning curve.
    *   NexusInsight: Aim for Gephi's analytical power in a web-based, user-friendly client.
*   **Cytoscape (Desktop & .js):**
    *   Strengths: Strong in bioinformatics, robust, .js version for web. (Citation [P2S4C6])
    *   Weaknesses: Desktop version not web; .js version requires development effort.
    *   NexusInsight: Cytoscape.js is a strong candidate library.

**Key Strengths to Emulate:**
*   WDQS: Query power.
*   Reasonator: Readability.
*   Scholia: Domain-specific views.
*   Kumu.io: Interactivity, ease of visual manipulation.
*   Gephi/Cytoscape: Analytical depth, layout variety.

**Weaknesses to Avoid:**
*   WDQS: Steep learning curve, intimidating UI.
*   Standalone Tools (Kumu, Flourish): Lack of direct Wikidata integration.
*   Desktop Tools: Not web-accessible for broad use.

---
**Citations from Primary Findings - Section 4 (Existing Tools & Inspirations):**
*   [P2S4C1] wikidata.org - "Wikidata Query Service"
*   [P2S4C2] wikidata.org - "Help:Reasonator"
*   [P2S4C3] scholia.toolforge.org - Scholia Main Page
*   [P2S4C4] kumu.io - Kumu Main Page
*   [P2S4C5] gephi.org - Gephi Main Page
*   [P2S4C6] cytoscape.org - Cytoscape Main Page

---
## 3.5 `wikidata-mcp` Server Interaction
*(Derived from [`research/02_data_collection/01_primary_findings_part2.md#section-5-wikidata-mcp-server-interaction-question-q51`](../02_data_collection/01_primary_findings_part2.md#section-5-wikidata-mcp-server-interaction-question-q51))*

**Query 5 Summary:** Elaborated on expected interaction patterns and API design considerations between NexusInsight (client) and `wikidata-mcp` (server).

### 3.5.1 Expected Interaction Patterns (Addressing Q5.1)

*   **Client-Initiated Requests:** NexusInsight initiates most interactions.
*   **Asynchronous Communication:** Non-blocking calls to keep UI responsive.
*   **Incremental Data Loading:** Fetch data in chunks for large results/graph exploration.
*   **Real-time Feedback (Optional):** For query validation or autocomplete.
*   **State Management:** Client manages UI state; `wikidata-mcp` manages data state/cache.
*   **Error Handling:** Robust mechanisms for network/server errors.

### 3.5.2 Crucial API Endpoint Categories (Addressing Q5.1)

*   **Entity Data Retrieval:**
    *   `get_entity_details(entity_id)`: Fetch core data for an entity.
    *   `search_entities(query_string, type_filter)`: Autocomplete/search for entities.
*   **Connection Exploration:**
    *   `get_direct_connections(entity_id, direction, property_filter)`: Get immediate neighbors.
    *   `get_path_between(entity_id_1, entity_id_2, max_depth)`: Find paths.
    *   `explore_neighborhood(entity_id, depth, filters)`: Fetch subgraph around an entity.
*   **Visual Query Support:**
    *   `translate_visual_query(visual_query_json)`: Convert visual model to SPARQL/executable query.
    *   `execute_translated_query(query_id_or_sparql)`: Run the query.
    *   `get_query_results(execution_id, page, page_size)`: Paginated results.
    *   `get_schema_info(element_type)`: Fetch property/item type details for query builder.
    *   `validate_query_fragment(fragment_json)`: (Optional) Early validation.
*   **Utility/Metadata:**
    *   `get_server_status()`
    *   `get_property_suggestions(query_string)`
    *   `get_item_type_suggestions(query_string)`

### 3.5.3 API Design Considerations (Addressing Q5.1)

*   **Stateless vs. Stateful:** Prefer stateless API calls from `wikidata-mcp` if possible, client manages session.
*   **Data Formats:** Optimized JSON (e.g., compact formats for graphs like Cytoscape JSON).
*   **Pagination and Rate Limiting:** Essential for large result sets and server stability.
*   **Caching:** `wikidata-mcp` should implement caching; client can also cache.
*   **Error Codes and Messages:** Clear, standardized error responses.
*   **Versioning:** Plan for API evolution.
*   **Security:** Authentication/authorization if user-specific data or write operations are involved (though `wikidata-mcp` is primarily read-focused on Wikidata).
*   **MCP Principles:** If `wikidata-mcp` is a true Model Context Protocol server, interactions might be more dynamic (discoverable tools/resources) than a fixed REST API. NexusInsight would use `use_mcp_tool` with appropriate arguments.

**Interaction Flow Example (Interactive Exploration):**
1.  Client: User searches "Q123". Client calls `search_entities("Q123")`.
2.  Server (`wikidata-mcp`): Returns entity details.
3.  Client: User clicks "Expand connections". Client calls `get_direct_connections("Q123")`.
4.  Server: Returns connected entities/properties.
5.  Client: Renders new nodes/edges.

**Conclusion for `wikidata-mcp` Interaction:** A well-defined, efficient, and potentially MCP-aligned API is crucial. The listed endpoints cover core needs for interactive exploration and visual query building. (Citations [P2S5C1], [P2S5C2], [P2S5C3])

---
**Citations from Primary Findings - Section 5 (`wikidata-mcp` Interaction):**
*   [P2S5C1] apifriends.com - "What is API Design?"
*   [P2S5C2] restfulapi.net - "REST API Design"
*   [P2S5C3] graphql.org - GraphQL Main Page (as an alternative API paradigm)

---
*(End of Detailed Findings Part 2)*