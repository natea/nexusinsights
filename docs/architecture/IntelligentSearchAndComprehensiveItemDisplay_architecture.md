# High-Level Architecture: Intelligent Search & Comprehensive Item Display

**Version:** 1.0
**Date:** 2025-05-11
**Project:** NexusInsight
**Module:** Intelligent Search & Comprehensive Item Display

## 1. Overview

This document outlines the high-level architecture for the "Intelligent Search & Comprehensive Item Display" module of the NexusInsight project. This module is responsible for providing users with an intuitive interface to search for Wikidata items and view detailed, human-readable information about them. It relies heavily on the `wikidata-mcp` backend for data retrieval and search logic.

This architecture is derived from the [Feature Overview Specification](docs/specs/IntelligentSearchAndComprehensiveItemDisplay_overview.md), the [User Blueprint](docs/blueprint.md), and the [Research Executive Summary](research/05_final_report/01_executive_summary.md).

## 2. Key Components

The module will primarily consist of frontend components interacting with the `wikidata-mcp` backend.

### 2.1. Frontend Components

*   **`GlobalSearch` (Container Component):**
    *   Manages the overall search experience, likely positioned in the application header.
    *   Contains the `SearchBar` and `SuggestionsDropdown`.
*   **`SearchBar`:**
    *   **Responsibility:** Captures user search input (natural language, keywords, QIDs). Triggers API calls for suggestions and full search queries.
    *   **Interactions:** Sends input to `wikidata-mcp` for suggestions and search. Receives user input.
*   **`SuggestionsDropdown`:**
    *   **Responsibility:** Displays real-time search suggestions fetched from `wikidata-mcp` as the user types. Allows users to select a suggestion to initiate a search or navigate directly to an item.
    *   **Interactions:** Receives suggestion data from `GlobalSearch` (originating from `wikidata-mcp`).
*   **`SearchResultsPage` (Container Component):**
    *   **Responsibility:** Displays a list or grid of search results returned by `wikidata-mcp`. Handles pagination and potentially filtering/sorting controls.
    *   **Interactions:** Receives search result data. Renders `SearchResultItem` components.
*   **`SearchResultItem`:**
    *   **Responsibility:** Displays a single search result, typically including the item's label, description, and a thumbnail image. Provides a link/action to navigate to the `ItemDetailPage`.
    *   **Interactions:** Receives data for one item from `SearchResultsPage`.
*   **`ItemDetailPage` (Container Component):**
    *   **Responsibility:** Displays a comprehensive, human-readable view of a single Wikidata item. Fetches and organizes data for the selected item (identified by QID).
    *   **Interactions:** Fetches detailed item data from `wikidata-mcp` based on QID. Renders various sub-components to display this data.
    *   **Sub-Components:**
        *   **`ItemHeader`:** Displays the item's primary label, QID (copyable), and a concise description/alias.
        *   **`ItemImageDisplay`:** Displays the primary image associated with the item (e.g., from Wikimedia Commons via P18).
        *   **`KeyInformationBox` (Infobox):** Presents a structured summary of the most salient facts and properties (e.g., "instance of," "date of birth," "capital of").
        *   **`DescriptionsSection`:** Displays longer textual descriptions of the item in the preferred language.
        *   **`StatementsRelationshipsSection`:** Organizes and displays various properties, statements, and relationships of the item to other items. Values that are themselves Wikidata items should be linkable to their respective `ItemDetailPage`. This section might be further broken down by property types or semantic categories.
        *   **`MetadataLinksSection`:** Provides a direct link to the item on `www.wikidata.org`, language selection options for the current view, and potentially information about data freshness.

### 2.2. Backend Components

*   **`wikidata-mcp` Server (External Dependency):**
    *   **Responsibility:** Handles all core data operations:
        *   Processing natural language, keyword, and characteristic-based search queries.
        *   Generating real-time search suggestions.
        *   Retrieving, structuring, and providing comprehensive data for Wikidata items (labels, descriptions, aliases, statements, images, relationships) in multiple languages with fallbacks.
    *   **Interfaces:** Exposes HTTP API endpoints consumed by the frontend components.

## 3. Data Flow

1.  **Search Suggestions:**
    *   User types into `SearchBar`.
    *   `SearchBar` (via `GlobalSearch`) sends the current input prefix and preferred language to the `wikidata-mcp` `/api/v1/suggest` endpoint.
    *   `wikidata-mcp` returns a list of suggestions (QID, label, description).
    *   `SuggestionsDropdown` displays these suggestions.
2.  **Executing Search:**
    *   User submits the query from `SearchBar` (either by pressing Enter or selecting a suggestion).
    *   `SearchBar` (via `GlobalSearch`) sends the query, query type (optional), preferred language, and pagination parameters to the `wikidata-mcp` `/api/v1/search` endpoint.
    *   `wikidata-mcp` returns a paginated list of search results (QID, label, description, thumbnail URL).
    *   `SearchResultsPage` receives these results and renders `SearchResultItem` components.
3.  **Viewing Item Details:**
    *   User clicks on a `SearchResultItem` or navigates directly to an item's URL (e.g., `/item/{QID}`).
    *   `ItemDetailPage` extracts the QID and requests detailed item data from the `wikidata-mcp` `/api/v1/item/{qid}` endpoint, specifying the preferred language.
    *   `wikidata-mcp` returns a structured JSON object containing the item's label, description, aliases, image URL, `wikidata.org` URL, key facts, and detailed statements/relationships.
    *   `ItemDetailPage` and its sub-components render this information.

## 4. API Interaction (with `wikidata-mcp`)

The frontend will interact with the following conceptual API endpoints provided by `wikidata-mcp` (as outlined in the Feature Overview Specification):

*   **Search Endpoint:**
    *   **Request:** `GET /api/v1/search`
    *   **Parameters:** `q` (string, query), `type` (enum, optional: `natural_language`, `keyword`, `characteristic`), `lang` (string, language code), `limit` (int, optional), `offset` (int, optional).
    *   **Response (JSON):** Array of result objects, e.g., `{ "qid": "Q123", "label": "...", "description": "...", "thumbnail_url": "..." }`.
*   **Suggestions Endpoint:**
    *   **Request:** `GET /api/v1/suggest`
    *   **Parameters:** `prefix` (string, input prefix), `lang` (string, language code), `limit` (int, optional).
    *   **Response (JSON):** Array of suggestion objects, e.g., `{ "qid": "Q456", "label": "...", "description": "..." }`.
*   **Item Data Endpoint:**
    *   **Request:** `GET /api/v1/item/{qid}`
    *   **Parameters:** `lang` (string, preferred language code for response).
    *   **Response (JSON):** Structured object for the item, including `qid`, `label`, `description`, `aliases`, `language_fallbacks`, `image_url`, `wikidata_url`, `key_facts` (array of property-value pairs), and `statements` (object mapping PIDs to arrays of statement details). Example structure provided in the Feature Overview Specification.

## 5. State Management (Frontend)

A robust state management strategy is crucial for a responsive and consistent user experience.

*   **Global Application State:**
    *   User's preferred language (global setting, potentially from profile or browser).
    *   Current theme/UI settings.
*   **Search-Related State:**
    *   Current search query text (`searchQuery`).
    *   Live input value in `SearchBar` (`inputValue`).
    *   List of current suggestions (`suggestions`).
    *   Loading status for suggestions (`isLoadingSuggestions`).
    *   List of search results (`searchResults`).
    *   Loading status for search results (`isLoadingResults`).
    *   Pagination details (`currentPage`, `totalPages`, `itemsPerPage`).
    *   Any active search filters (`activeFilters`).
    *   Error states related to search/suggestions (`searchError`).
*   **Item Detail View State:**
    *   QID of the currently viewed item (`currentItemQID`).
    *   Fetched data for the current item (`currentItemData`).
    *   Loading status for item data (`isLoadingItemData`).
    *   Selected display language for the item page (if different from global).
    *   Error state for item loading (`itemError`).

**Approach:**
*   A centralized state management library (e.g., Redux, Zustand for React; Vuex for Vue; Svelte Stores for Svelte) is recommended for managing shared state like `searchQuery`, `searchResults`, `currentItemQID`, and `currentItemData`. This allows different components to react to changes and dispatch actions consistently.
*   Component-local state (e.g., visibility of a dropdown, input focus) can be managed by the frontend framework's native capabilities.
*   State should be designed to be serializable where appropriate to support potential features like sharing links to specific search results or item views.

## 6. Technology Considerations

*   **Frontend Framework:**
    *   The research summary suggests **React, Vue, or Svelte**. The choice should be based on team expertise, ecosystem support, and performance characteristics for data-intensive applications.
    *   All three support component-based architecture, which is well-suited for this module.
    *   The chosen framework will dictate component definition, state handling (e.g., React Hooks, Vue Composition/Options API, Svelte's reactive statements), and lifecycle management.
*   **UI Component Library:**
    *   A pre-built UI component library (e.g., Material UI, Ant Design, Chakra UI, or utility-first like Tailwind CSS with headless components) should be used to accelerate development, ensure accessibility (WCAG 2.1 AA target), and maintain visual consistency. This aligns with dependencies D003 and D005.
*   **Data Fetching:**
    *   A modern data fetching library (e.g., React Query, SWR, Apollo Client if GraphQL were an option, or built-in fetch with async/await) should be used to handle API requests, caching, and state synchronization related to `wikidata-mcp` interactions.
*   **Internationalization (i18n):**
    *   An i18n library (e.g., `react-i18next`, `vue-i18n`) will be needed for localizing UI chrome (buttons, static labels) as per D004. Item content localization is handled by `wikidata-mcp` based on the `lang` parameter.

## 7. Key Dependencies

As identified in the Feature Overview Specification and research:

*   **D001: `wikidata-mcp` Server:** The most critical dependency. All search and item data functionalities rely on its APIs, performance, and data accuracy.
*   **D002: UI/UX Design Specifications:** Detailed wireframes, mockups, and interaction guidelines are necessary for implementation.
*   **D003: Frontend Technology Stack:** The chosen JavaScript framework, state management solution, and UI component library.
*   **D004: Internationalization (i18n) Framework:** For localizing static UI elements.
*   **D005: Project-wide Style Guide:** To ensure visual consistency with the broader NexusInsight application.

## 8. Scalability and Performance Considerations

*   **Frontend Rendering:** Efficient rendering of potentially large lists (search results, statements) is key. Virtualization techniques (windowing) might be necessary for very long lists.
*   **API Call Optimization:** Minimize unnecessary API calls. Utilize caching strategies provided by data fetching libraries.
*   **Image Loading:** Lazy loading for images in search results and item pages to improve initial page load times.
*   **Code Splitting:** To reduce initial bundle size, ensuring faster load times for the application.
*   The performance of `wikidata-mcp` is paramount. The frontend architecture assumes `wikidata-mcp` meets the NFRs for API response times.

## 9. Future Considerations (Out of Scope for Initial Architecture)

*   Client-side caching beyond browser defaults (e.g., for offline viewing of recently accessed items).
*   Advanced filtering and sorting options on the `SearchResultsPage`.
*   User-specific search history or preferences.

This high-level architecture provides a foundation for the detailed design and development of the "Intelligent Search & Comprehensive Item Display" module.