# Feature Overview: Interactive Visual Exploration of Connections

## 1. Introduction

This feature for the NexusInsight project allows users to intuitively understand the web of relationships around any Wikidata item. It aims to provide a visually engaging and insightful way to explore connections, moving beyond simple lists of facts. The core functionality relies heavily on the `wikidata-mcp` server for fast and structured graph data delivery, enabling users to navigate complex information visually, understand relationships, and discover new facts.

## 2. User Stories

*   US1: As a curious explorer, I want to visually navigate connections starting from an entity (e.g., "Star Wars"), so I can serendipitously discover related information like actors and their other works. (Source: [`docs/blueprint.md:204-207`](docs/blueprint.md:204))
*   US2: As a user, I want to see a central node for my searched item and its related nodes clustered by common relationship types, so I can quickly grasp the main connections. (Source: [`docs/blueprint.md:86`](docs/blueprint.md:86))
*   US3: As a user, I want to hover over a node to get quick info (thumbnail, key details), so I can get details without losing context of the overall graph. (Source: [`docs/blueprint.md:87`](docs/blueprint.md:87))
*   US4: As a user, I want to click on a node to expand its connections or refocus the graph on it, so I can delve deeper into specific relationships and entities. (Source: [`docs/blueprint.md:88`](docs/blueprint.md:88))
*   US5: As a user, I want to filter connections by relationship type (e.g., "influenced by," "student of") and item type (e.g., "Person," "Artwork"), so I can customize the graph display to my specific area of interest and reduce clutter. (Source: [`docs/blueprint.md:89-90`](docs/blueprint.md:89))
*   US6: As a user, I want to see a breadcrumb trail or history panel of my exploration path, so I can easily backtrack or understand how I navigated to the current view. (Source: [`docs/blueprint.md:91`](docs/blueprint.md:91))

## 3. Acceptance Criteria

**For US1: Visually navigate connections for serendipitous discovery**
*   AC1.1: Given a user searches for an entity, when the graph is displayed, then the entity is shown as the central node.
*   AC1.2: Given a user interacts with a connected node (e.g., an actor), when the graph updates, then related information for that node (e.g., other works by the actor) is visually presented.
*   AC1.3: Given a user explores the graph, then the navigation path allows for discovery of new, related entities and information.

**For US2: Central node with clustered related nodes**
*   AC2.1: Given a user searches for an entity, when the initial graph is displayed, then the searched entity is the central node.
*   AC2.2: Given the initial graph display, then nodes directly connected to the central entity are visually grouped or clustered based on common relationship types (e.g., "Artworks," "People Known").
*   AC2.3: Given the clustered display, then the user can easily identify the primary categories of connections for the central entity.

**For US3: Hover for quick info**
*   AC3.1: Given a displayed graph, when the user hovers the mouse cursor over any node, then a pop-up/tooltip appears.
*   AC3.2: Given the pop-up/tooltip appears, then it displays a thumbnail image (if available) and key details (e.g., creation date, location for an artwork) for the hovered node.
*   AC3.3: Given the pop-up/tooltip is displayed, then the main graph remains visible and interactive.

**For US4: Click to expand/refocus**
*   AC4.1: Given a displayed graph, when the user clicks on a node, then the graph animates to either re-center on the clicked node or expand to show its direct connections.
*   AC4.2: Given the graph re-centers or expands, then new relevant nodes and relationships are fetched from `wikidata-mcp` and displayed.
*   AC4.3: Given the graph updates, then the user can continue exploring from the newly focused or expanded node.

**For US5: Filter connections**
*   AC5.1: Given a displayed graph, then filter options for "Relationship Type" and "Item Type" are available in a sidebar or menu.
*   AC5.2: Given a user selects or modifies a filter, when the filter is applied, then the graph display updates dynamically to show only nodes and connections matching the filter criteria.
*   AC5.3: Given the graph updates based on filters, then the system re-queries `wikidata-mcp` with the appropriate filter parameters.

**For US6: Breadcrumb trail/history**
*   AC6.1: Given a user navigates through multiple nodes in the graph, then a breadcrumb trail or history panel is visible.
*   AC6.2: Given the breadcrumb trail/history panel, then it accurately reflects the sequence of nodes the user has focused on or expanded.
*   AC6.3: Given the breadcrumb trail/history panel, then the user can click on a previous item in the path to navigate back to that state of the graph.

## 4. Functional Requirements

*   FR1: The system shall allow users to initiate exploration by searching for a Wikidata entity.
*   FR2: The system shall display the searched entity as a central node in a visual graph.
*   FR3: The system shall display nodes connected to the central entity, clustered by common relationship types or groups, using data from `wikidata-mcp` (e.g., `/entity/Q762/connections?depth=1&common_groups=true`).
*   FR4: The system shall display a pop-up with quick information (e.g., thumbnail, creation date, location) when a user hovers over a node. This data is sourced from `wikidata-mcp`.
*   FR5: The system shall allow users to click on a node to either re-center the graph on the clicked node (showing its connections) or expand the connections of the clicked node within the current view.
*   FR6: The system shall fetch data for node expansion/refocus from `wikidata-mcp`.
*   FR7: The system shall provide filtering options for "Relationship Type" and "Item Type".
*   FR8: The system shall dynamically update the graph display based on applied filters, re-querying `wikidata-mcp`.
*   FR9: The system shall maintain and display a breadcrumb trail or history of the user's exploration path.
*   FR10: The system shall allow users to navigate back using the breadcrumb trail.
*   FR11: (Optional) The system may provide options to change the graph layout (e.g., hierarchical, force-directed).
*   FR12: Data displayed for nodes and connections must be sourced from the `wikidata-mcp` server.
*   FR13: The system must provide clear data attribution to Wikidata, including links to source Wikidata items for entities displayed. (Ref: [`docs/blueprint.md:169`](docs/blueprint.md:169))

## 5. Non-Functional Requirements

*   NFR1: **Performance:** Interactions (hover, click, filter, pan, zoom) must feel fast and fluid. Graph rendering and updates must be highly responsive. (Ref: [`docs/blueprint.md:172`](docs/blueprint.md:172))
*   NFR2: **Usability & UX:**
    *   The interface must be modern, clean, intuitive, and insightful. (Ref: [`docs/blueprint.md:138`](docs/blueprint.md:138))
    *   The experience should be engaging and promote discoverability. (Ref: [`docs/blueprint.md:138`](docs/blueprint.md:138))
    *   The system must avoid overwhelming users by prioritizing information, summarizing where appropriate, and using progressive disclosure. (Ref: [`docs/blueprint.md:180`](docs/blueprint.md:180))
*   NFR3: **Accessibility:** The feature must adhere to WCAG 2.1 Level AA standards. (Ref: [`docs/blueprint.md:174`](docs/blueprint.md:174))
*   NFR4: **Data Source Integrity:** The feature must heavily rely on the `wikidata-mcp` server for all graph data. (Ref: [`docs/blueprint.md:82`](docs/blueprint.md:82), [`docs/blueprint.md:86`](docs/blueprint.md:86), [`docs/blueprint.md:88`](docs/blueprint.md:88), [`docs/blueprint.md:90`](docs/blueprint.md:90), [`docs/blueprint.md:93`](docs/blueprint.md:93))
*   NFR5: **Trustworthiness:** Clear data attribution to Wikidata is mandatory, with links to source items. (Ref: [`docs/blueprint.md:169`](docs/blueprint.md:169))
*   NFR6: **Responsiveness (Device):** The interface should be responsive, adapting gracefully to different screen sizes, though the primary experience is optimized for desktop viewing due to the nature of complex graph visualizations. (Derived from "responsive" in [`docs/blueprint.md:138`](docs/blueprint.md:138))

## 6. Scope

**In Scope:**
*   Entity search functionality to initiate graph exploration.
*   Visual graph display featuring a central node for the searched entity and its connected nodes.
*   Clustering or grouping of connected nodes based on common relationship types.
*   Hover-activated pop-ups (tooltips) displaying quick information for nodes.
*   Click-to-expand functionality for nodes, revealing further connections, or click-to-refocus the graph on a selected node.
*   Filtering capabilities based on "Relationship Type" and "Item Type."
*   A breadcrumb trail or navigation history panel to track and revisit exploration steps.
*   Exclusive use of the `wikidata-mcp` server as the data source for all graph information.
*   Adherence to specified UI/UX design principles: modern, clean, intuitive, insightful, trustworthy, responsive, engaging, discoverable.
*   Compliance with WCAG 2.1 Level AA accessibility standards.
*   Mandatory data attribution to Wikidata, including links to the source items.
*   (Optional) Basic options for users to change the graph layout (e.g., force-directed, hierarchical).

**Out of Scope:**
*   User accounts, personalized profiles, or saving of graph states/explorations.
*   Real-time collaborative graph exploration features.
*   Direct editing or modification of Wikidata information through this interface.
*   Advanced graph analytics, statistical measures, or complex data mining features beyond visual exploration.
*   Support for any data sources other than the designated `wikidata-mcp` server.
*   Offline mode functionality or extensive local data caching beyond immediate performance optimization needs.
*   Complex Natural Language Processing (NLP) for graph initiation; a simple entity search mechanism is sufficient.
*   Version control or history of changes to the graph data itself (focus is on exploration of current data).

## 7. Dependencies

*   **Internal:**
    *   `wikidata-mcp` server: This is a critical dependency. The feature relies entirely on this server for:
        *   Fetching initial entity connections and clustered groups.
        *   Providing data for node expansion and refocusing.
        *   Supplying quick information for node hover pop-ups.
        *   Delivering filtered graph results.
        *   Specific API endpoints (e.g., `/entity/{id}/connections`) will be consumed.
*   **External:**
    *   Wikidata: The ultimate source of the data that `wikidata-mcp` processes and serves.
*   **Libraries/Frameworks (Anticipated):**
    *   A robust graph visualization library (e.g., D3.js, Vis.js, Cytoscape.js, React Flow) will be essential for rendering the interactive graph.
    *   A modern frontend JavaScript framework (e.g., React, Vue.js, Angular) will likely be used to build the user interface components, manage state, and handle interactions.

## 8. High-Level UI/UX Considerations

*   **Layout & Structure:**
    *   A prominent, easily accessible search bar for users to initiate their exploration by entering an entity name.
    *   The main viewport will be dedicated to the interactive graph visualization.
    *   A sidebar or a collapsible panel will house filtering options (e.g., by "Relationship Type," "Item Type").
    *   A clearly visible breadcrumb trail, likely positioned at the top of the graph area or in a dedicated history panel, to show the user's navigation path.
*   **Interactivity & Feedback:**
    *   Smooth, intuitive animations for graph transitions such as node expansion, refocusing, and updates post-filtering to provide a fluid user experience.
    *   Clear visual cues for all interactive elements, including hover states for nodes and edges, and distinct indicators for clickable nodes.
    *   Intuitive pan and zoom controls (e.g., mouse wheel, pinch-to-zoom on touch devices if applicable) for navigating large graphs.
*   **Visual Design:**
    *   Nodes should be clearly distinguishable, potentially using icons, images (as suggested in the blueprint), or color-coding to represent different types of entities or groups.
    *   Relationships (edges) should be rendered clearly, possibly with labels or styling to indicate the type of relationship.
    *   The overall visual aesthetic will adhere to the project's principles: modern, clean, and intuitive, ensuring a trustworthy and engaging experience.
    *   Tooltips for quick information on hover should be non-obtrusive, appearing quickly and formatted for easy readability.
*   **Information Management:**
    *   Careful consideration to balance information density with clarity, avoiding user overload. Progressive disclosure will be key.
    *   Node clustering is a primary strategy to manage complexity when an entity has numerous connections, helping users grasp main themes quickly.
    *   Text within nodes or on labels should be concise and legible.

## 9. API Design Notes (Interaction with `wikidata-mcp`)

The feature's data layer is entirely dependent on the `wikidata-mcp` server. Consistent and performant API responses are crucial.

*   **Primary Endpoint for Graph Data:**
    *   `GET /entity/{entity_id}/connections`
    *   Parameters:
        *   `depth={n}` (e.g., `depth=1` for initial load and expansions).
        *   `common_groups=true` (for initial load to get clustered view, as per blueprint).
        *   `relationship_type={type_qids}` (comma-separated Wikidata QIDs for relationship properties, e.g., `P31` for "instance of").
        *   `item_type={type_qids}` (comma-separated Wikidata QIDs for item types, e.g., `Q5` for "human").
    *   Expected Response: JSON containing lists of nodes and edges.
        *   **Nodes:** `id` (Wikidata QID), `label`, `description` (short), `thumbnail_url` (if available), `item_type_label`, `group_name` (if `common_groups=true`), `source_wikidata_url`, and other key properties for quick info display.
        *   **Edges:** `source_node_id`, `target_node_id`, `relationship_label` (property label), `relationship_id` (property QID).

*   **Endpoint for Entity Search/Autocomplete (if not handled by a separate search feature):**
    *   `GET /search/entities?q={search_term}`
    *   Expected Response: List of matching entities with `id` (QID) and `label`.

*   **Data for Hover Pop-ups:**
    *   To ensure responsiveness, data for hover pop-ups (thumbnail, key details like creation date, location) should ideally be included in the node objects returned by the `/entity/{entity_id}/connections` endpoint for all currently visible nodes. This avoids an extra API call per hover, which is critical for a fluid experience.

*   **Performance Considerations:**
    *   `wikidata-mcp` should be optimized to return these graph snippets quickly.
    *   Payloads should be as lean as possible while providing necessary information.
    *   Consider pagination or virtualized rendering on the client-side if an entity has an exceptionally large number of direct connections that cannot be effectively clustered.