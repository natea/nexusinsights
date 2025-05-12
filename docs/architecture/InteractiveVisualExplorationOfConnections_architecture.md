# High-Level Architecture: Interactive Visual Exploration of Connections

## 1. Introduction

This document outlines the high-level architecture for the "Interactive Visual Exploration of Connections" feature within the NexusInsight project. This feature enables users to visually navigate and understand the complex web of relationships surrounding Wikidata entities, leveraging the `wikidata-mcp` server for data acquisition and processing. The goal is to provide an intuitive, performant, and insightful graph exploration experience.

## 2. Architectural Goals

The architecture is designed to meet the following key goals:

*   **Intuitive User Experience:** Provide a visually engaging and easy-to-understand interface for exploring complex graph data.
*   **High Performance:** Ensure fast loading times, fluid interactions (pan, zoom, hover, click), and responsive graph updates.
*   **Seamless `wikidata-mcp` Integration:** Efficiently consume and display data from the `wikidata-mcp` server, which is the sole source of graph information.
*   **Modularity & Maintainability:** Design components that are well-encapsulated, promoting reusability and ease of maintenance.
*   **Accessibility:** Adhere to WCAG 2.1 Level AA standards to ensure the feature is usable by people with diverse abilities.
*   **Scalability (Client-Side):** Handle potentially large graph datasets gracefully on the client-side through efficient rendering and data management.

## 3. System Architecture Overview

The feature follows a **client-server model**:

*   **Client (Web Browser):** A Single Page Application (SPA) built with a modern JavaScript framework. It is responsible for:
    *   Rendering the user interface (graph, filters, search, tooltips).
    *   Managing user interactions.
    *   Fetching data from the `wikidata-mcp` server via API calls.
    *   Managing client-side state.
*   **Server (`wikidata-mcp`):** An external, critical dependency that serves as the backend. It provides:
    *   Pre-processed and structured graph data (nodes, edges, clusters).
    *   Entity search functionality.
    *   Data for node details and tooltips.

No additional backend components are envisioned for this specific feature beyond the `wikidata-mcp` server.

**Conceptual Diagram:**

```
+---------------------+      HTTP/S API Calls      +---------------------+
| User (Web Browser)  |-------------------------->|   wikidata-mcp      |
|                     |      (JSON Payloads)       |   Server            |
| +-----------------+ |                            |                     |
| | NexusInsight    | |                            | (Graph Data, Search)|
| | Frontend App    | |                            +---------------------+
| | (SPA)           | |
| |                 | |
| | +-------------+ | |
| | | Graph Vis.  | | |
| | +-------------+ | |
| +-----------------+ | |
+---------------------+
```

## 4. Component Breakdown

The frontend application will be composed of several key components:

### Frontend Application (SPA)

*   **`GraphContainer` Component:**
    *   **Responsibilities:** Orchestrates the overall graph visualization area. Manages the primary graph state (current central node, visible nodes/edges, layout settings). Fetches initial graph data and handles data updates for expansion/refocus.
    *   **Interactions:** Communicates with `ApiConnector` for data, `StateManagement` for global state, and `GraphVisualizationEngine` for rendering.

*   **`GraphVisualizationEngine` Component:**
    *   **Responsibilities:** A wrapper around the chosen graph visualization library (e.g., Cytoscape.js). Renders nodes and edges, applies layout algorithms (e.g., force-directed, clustered), handles low-level graph interactions (pan, zoom, node drag if enabled), and emits events for higher-level interactions (node click, hover).
    *   **Interactions:** Receives graph data and configuration from `GraphContainer`.

*   **`SearchInput` Component:**
    *   **Responsibilities:** Provides an input field for users to search for Wikidata entities. May include autocomplete suggestions fetched from `wikidata-mcp`. Initiates graph exploration or changes the central node.
    *   **Interactions:** Triggers data fetching via `ApiConnector` or updates `StateManagement` with the selected entity.

*   **`NodeInteractionManager` Component/Logic:**
    *   **Responsibilities:** Handles user interactions directly on graph nodes, such as hover events (to trigger `InfoTooltip`) and click events (to trigger node expansion, refocus, or display detailed information).
    *   **Interactions:** Listens to events from `GraphVisualizationEngine`. Updates `StateManagement` or triggers `ApiConnector` for new data.

*   **`InfoTooltip` Component:**
    *   **Responsibilities:** Displays a small pop-up with concise information (thumbnail, key details) when a user hovers over a node. Data is typically pre-fetched with the main graph data to ensure responsiveness.
    *   **Interactions:** Shows/hides based on events from `NodeInteractionManager` and data from `StateManagement`.

*   **`FilterPanel` Component:**
    *   **Responsibilities:** Provides UI elements (e.g., dropdowns, checkboxes) for users to filter connections by "Relationship Type" and "Item Type."
    *   **Interactions:** Updates filter criteria in `StateManagement`, which triggers `ApiConnector` to re-fetch filtered graph data from `wikidata-mcp`.

*   **`HistoryBreadcrumb` Component:**
    *   **Responsibilities:** Displays the user's navigation path (sequence of focused entities). Allows users to click on previous items to backtrack to a prior graph state.
    *   **Interactions:** Reads history from `StateManagement` and updates it upon navigation.

*   **`ApiConnector` Service/Module:**
    *   **Responsibilities:** Centralizes all HTTP communication with the `wikidata-mcp` API. Handles request formatting, sending requests, parsing responses, and basic error handling.
    *   **Interactions:** Used by various components (`GraphContainer`, `SearchInput`, `FilterPanel`) to fetch data.

*   **`StateManagement` Store (e.g., Zustand, Redux Toolkit, Vuex, React Context):**
    *   **Responsibilities:** Manages the global client-side state of the feature. This includes:
        *   Current graph data (nodes, edges).
        *   Selected filters.
        *   User exploration history.
        *   Loading states.
        *   Current central entity.
        *   Error states.
    *   **Interactions:** Accessed and updated by most components to ensure a consistent application state.

### Backend Components

*   **`wikidata-mcp` Server (External Dependency):**
    *   **Responsibilities:** Provides all necessary data via a well-defined API. This includes:
        *   Initial entity connections and clustered groups (e.g., via `/entity/{id}/connections?depth=1&common_groups=true`).
        *   Data for node expansion and refocusing.
        *   Quick information for node hover pop-ups (ideally included in the main graph data payload).
        *   Filtered graph results based on specified criteria.
        *   Entity search/autocomplete functionality (e.g., via `/search/entities`).

## 5. Data Flow

1.  **Initiation:**
    *   User types an entity name into `SearchInput`. Autocomplete suggestions may be fetched from `wikidata-mcp` (`/search/entities`).
    *   User selects an entity.
2.  **Initial Graph Load:**
    *   `GraphContainer` (triggered by `SearchInput` or initial load) requests `ApiConnector` to fetch connections for the selected entity from `wikidata-mcp` (`GET /entity/{entity_id}/connections` with `depth=1`, `common_groups=true`).
    *   `ApiConnector` receives JSON data (nodes, edges, group info, hover details).
    *   Data is processed and stored in `StateManagement`.
    *   `GraphContainer` passes data to `GraphVisualizationEngine` for rendering.
    *   `HistoryBreadcrumb` is updated.
3.  **Node Hover:**
    *   User hovers over a node in `GraphVisualizationEngine`.
    *   `NodeInteractionManager` detects hover, retrieves pre-fetched node details from `StateManagement`.
    *   `InfoTooltip` is displayed with the details.
4.  **Node Click (Expand/Refocus):**
    *   User clicks a node in `GraphVisualizationEngine`.
    *   `NodeInteractionManager` detects click.
    *   `GraphContainer` requests `ApiConnector` to fetch connections for the clicked node from `wikidata-mcp` (`GET /entity/{clicked_node_id}/connections`).
    *   New data updates `StateManagement`, and the graph is re-rendered by `GraphVisualizationEngine`, potentially re-centering or showing new connections.
    *   `HistoryBreadcrumb` is updated.
5.  **Filtering:**
    *   User interacts with `FilterPanel`, selecting/deselecting relationship or item types.
    *   Filter criteria in `StateManagement` are updated.
    *   `GraphContainer` requests `ApiConnector` to re-fetch graph data from `wikidata-mcp` with new filter parameters appended to the `/entity/{entity_id}/connections` call.
    *   Graph updates with filtered data.
6.  **Backtracking via History:**
    *   User clicks an item in `HistoryBreadcrumb`.
    *   `StateManagement` reverts to the selected historical state (or triggers a re-fetch for that state if not fully cached).
    *   Graph updates accordingly.

## 6. Technology Stack (Recommendations)

*   **Frontend Framework:** **React**
    *   **Reasoning:** Component-based architecture, large ecosystem, strong community support, good performance characteristics, and suitability for complex, interactive UIs. TypeScript is highly recommended for use with React.
    *   *Alternatives: Vue.js, Svelte.*
*   **Graph Visualization Library:** **Cytoscape.js**
    *   **Reasoning:** Specifically designed for network/graph visualization, highly performant with larger datasets, extensive customization options, built-in layout algorithms, good event handling, and framework agnostic (integrates well with React).
    *   *Alternatives: D3.js (more low-level), Vis.js, React Flow (React-specific).*
*   **State Management:** **Zustand**
    *   **Reasoning:** Simple, scalable, and minimal boilerplate state management solution for React. Offers a hook-based API that is easy to integrate.
    *   *Alternatives: Redux Toolkit, React Context API (for simpler state).*
*   **Styling:** **CSS Modules** or **Styled-components**
    *   **Reasoning:** Provide scoped styling to avoid conflicts and improve maintainability.
*   **Build Tool:** **Vite**
    *   **Reasoning:** Fast development server and optimized builds.
    *   *Alternative: Webpack.*
*   **Programming Language:** **TypeScript**
    *   **Reasoning:** Adds static typing to JavaScript, improving code quality, maintainability, and developer experience, especially in larger projects.

## 7. API Interaction with `wikidata-mcp`

The feature relies entirely on the `wikidata-mcp` server. Key interactions include:

*   **Fetching Graph Data:**
    *   `GET /entity/{entity_id}/connections`
    *   **Parameters:**
        *   `depth={n}`: Controls the depth of connections to fetch.
        *   `common_groups=true`: For initial load to get clustered views.
        *   `relationship_type={type_qids}`: Comma-separated QIDs for filtering by relationship.
        *   `item_type={type_qids}`: Comma-separated QIDs for filtering by item type.
    *   **Response:** JSON containing nodes (with ID, label, description, thumbnail, type, group, source URL) and edges (source, target, label, ID). Data for hover tooltips should be included in node objects to minimize API calls.
*   **Searching Entities:**
    *   `GET /search/entities?q={search_term}`
    *   **Response:** JSON list of matching entities (ID, label).

Efficient and well-structured responses from `wikidata-mcp` are critical for performance.

## 8. Performance and Scalability Considerations

*   **Client-Side Rendering:** The SPA architecture handles rendering efficiently.
*   **Efficient Data Fetching:**
    *   Only request necessary data from `wikidata-mcp` using appropriate `depth` and filters.
    *   Ensure `wikidata-mcp` payloads are lean but complete for the current view (including hover info).
*   **Client-Side Caching:** Implement caching for `wikidata-mcp` API responses (e.g., using libraries like React Query or SWR, or a custom solution within `StateManagement`) to reduce redundant calls for recently viewed entities or graph states.
*   **Graph Library Optimization:** Leverage Cytoscape.js's performance features. For extremely large numbers of connections not manageable by clustering, consider:
    *   **Virtualization:** Rendering only visible nodes/edges if the library supports it or can be augmented.
    *   **Progressive Loading/Rendering:** Load and display core elements first, then add more detail.
*   **Debouncing/Throttling:** Apply to frequent UI events like search input, pan/zoom interactions if they trigger expensive operations.
*   **Code Splitting:** Use dynamic imports to load components or libraries only when needed, reducing initial bundle size.
*   **Memoization:** Use React.memo and `useMemo`/`useCallback` hooks to prevent unnecessary re-renders of components.

## 9. Accessibility Considerations (WCAG 2.1 Level AA)

*   **Keyboard Navigation:** All interactive elements, including graph nodes, filters, and history, must be navigable and operable via keyboard.
*   **ARIA Attributes:** Use appropriate ARIA roles, states, and properties to make graph elements and controls understandable to assistive technologies. For example, nodes can be `role="button"` or `role="link"` with descriptive `aria-label`.
*   **Focus Management:** Ensure logical focus order and visible focus indicators.
*   **Color Contrast:** Maintain sufficient contrast between text, UI elements, and backgrounds, including graph nodes and edges.
*   **Text Alternatives:** Provide text alternatives for non-text content where appropriate (e.g., icons).
*   **Screen Reader Compatibility:** Test with screen readers to ensure information is conveyed effectively.
*   **Alternative Representations:** Consider if a non-visual representation or summary of graph connections could be offered for complex visualizations.

## 10. Modularity and Reusability

*   **Component-Based Design:** React's component model inherently promotes modularity.
*   **Service Abstraction:** The `ApiConnector` service encapsulates data fetching logic and can be reused.
*   **Generic Components:** `GraphVisualizationEngine` (if designed as a flexible wrapper), `FilterPanel`, and `InfoTooltip` could potentially be adapted for other uses within NexusInsight if similar functionality is needed.
*   **State Management Decoupling:** Separating state logic allows components to be more focused on presentation and interaction.

## 11. Security Considerations

*   **Data Source:** All data is sourced from `wikidata-mcp`. The security of this data relies on the `wikidata-mcp` server.
*   **Frontend Security:** Standard web application security practices must be followed:
    *   **Cross-Site Scripting (XSS) Prevention:** Sanitize any data from `wikidata-mcp` before rendering it as HTML if it's not inherently safe (e.g., labels, descriptions). React helps mitigate some XSS risks by default.
    *   **Cross-Site Request Forgery (CSRF):** Not directly applicable as this feature is primarily read-only and doesn't involve sensitive state-changing user actions submitted via forms.
*   **API Interaction:** Use HTTPS for all communication with `wikidata-mcp`.
*   **No Sensitive User Data:** This feature does not store sensitive user-specific data beyond client-side caching for performance, which is confined to the user's browser.

## 12. Risks and Mitigation

*   **Performance with Extremely Large/Dense Graphs:**
    *   **Risk:** UI becomes slow or unresponsive when an entity has thousands of direct connections.
    *   **Mitigation:** Heavy reliance on `wikidata-mcp` to provide effective `common_groups` clustering. Client-side, implement robust virtualization/progressive rendering if needed. Optimize Cytoscape.js settings. Provide user feedback during long operations.
*   **Complexity of Graph Interactions and UI:**
    *   **Risk:** Users find the graph difficult to navigate or understand.
    *   **Mitigation:** Iterative UI/UX design with user testing. Clear visual cues. Comprehensive `InfoTooltip`. Effective default layouts.
*   **Dependency on `wikidata-mcp` API:**
    *   **Risk:** Changes in `wikidata-mcp` API break the feature; `wikidata-mcp` performance issues impact user experience.
    *   **Mitigation:** Maintain a clear API contract. Implement robust error handling and user feedback for API failures. Client-side caching can alleviate some load on `wikidata-mcp` and provide resilience against temporary network issues.
*   **Accessibility Challenges with Complex Visualizations:**
    *   **Risk:** Graph visualization is inherently difficult to make fully accessible.
    *   **Mitigation:** Diligent application of WCAG guidelines, keyboard navigation, ARIA attributes. Consider alternative ways to present connection information (e.g., a structured list alongside the graph).