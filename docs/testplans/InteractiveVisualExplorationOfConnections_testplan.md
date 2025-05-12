# Test Plan: Interactive Visual Exploration of Connections

## 1. Introduction

### 1.1 Purpose
This document outlines the test plan for the "Interactive Visual Exploration of Connections" feature of the NexusInsight project. The purpose of this plan is to detail the scope, approach, resources, and schedule of intended test activities. It aims to ensure that the feature meets the specified requirements and quality standards, providing users with an intuitive and insightful way to explore connections within Wikidata.

### 1.2 Scope of Testing
This test plan covers the functional and non-functional testing of the Interactive Visual Exploration of Connections feature as defined in the feature specification document: [`docs/specs/InteractiveVisualExplorationOfConnections_overview.md`](docs/specs/InteractiveVisualExplorationOfConnections_overview.md). This includes:
*   Entity search and initial graph display.
*   Node clustering by relationship types.
*   Hover-activated quick information pop-ups.
*   Node click functionality for expansion or refocusing.
*   Filtering by relationship type and item type.
*   Breadcrumb trail/history panel for navigation.
*   Data fetching and display integrity from the `wikidata-mcp` server.
*   UI/UX, performance, accessibility, and responsiveness.

Testing will focus on verifying the user stories and acceptance criteria outlined in the specification.

## 2. Test Strategy

### 2.1 Approach
A hybrid testing approach will be adopted, combining manual and automated testing where appropriate.
*   **Manual Testing:** Will be used for exploratory testing, usability testing, UI validation, and complex scenarios that are difficult to automate.
*   **Automated Testing:** Will be considered for repetitive functional tests, API integration tests with `wikidata-mcp`, and performance tests.

Testing will be conducted across different stages:
*   **Component Testing:** Individual UI components (search bar, graph canvas, filter panel, etc.) will be tested in isolation.
*   **Integration Testing:** Focus on the interaction between the frontend components and the `wikidata-mcp` server API.
*   **System Testing:** End-to-end testing of the feature to ensure all parts work together as expected.
*   **User Acceptance Testing (UAT):** To be conducted by stakeholders to validate the feature against user requirements.

### 2.2 Types of Testing
*   **Functional Testing:** Verifying that all features work as per the requirements (User Stories, Acceptance Criteria, Functional Requirements).
*   **Usability Testing:** Assessing the ease of use, intuitiveness, and overall user experience.
*   **Performance Testing:** Evaluating the responsiveness of graph interactions (hover, click, filter, pan, zoom), rendering speed, and API response times from `wikidata-mcp`.
*   **Accessibility Testing:** Ensuring compliance with WCAG 2.1 Level AA standards.
*   **UI Testing:** Verifying the visual design, layout, and responsiveness across different screen sizes (primarily desktop).
*   **Compatibility Testing:** Basic checks on modern web browsers (e.g., Chrome, Firefox, Safari, Edge).
*   **Negative Testing:** Testing how the system handles invalid inputs, unexpected user actions, and error conditions (e.g., API errors from `wikidata-mcp`).
*   **Data Integrity Testing:** Ensuring data fetched from `wikidata-mcp` is accurately displayed and attributed.

## 3. Test Scope

### 3.1 Features to be Tested
Based on [`docs/specs/InteractiveVisualExplorationOfConnections_overview.md`](docs/specs/InteractiveVisualExplorationOfConnections_overview.md):
*   **FR1, US1, AC1.1-AC1.3:** Entity search and initiation of graph exploration.
*   **FR2, US2, AC2.1-AC2.3:** Central node display for searched entity.
*   **FR3, US2, AC2.2-AC2.3:** Clustering of connected nodes by common relationship types.
*   **FR4, US3, AC3.1-AC3.3:** Hover-activated pop-ups with quick information.
*   **FR5, FR6, US4, AC4.1-AC4.3:** Click-to-expand/refocus functionality for nodes.
*   **FR7, FR8, US5, AC5.1-AC5.3:** Filtering by "Relationship Type" and "Item Type."
*   **FR9, FR10, US6, AC6.1-AC6.3:** Breadcrumb trail/history panel for navigation and backtracking.
*   **FR11 (Optional):** Graph layout change options.
*   **FR12, NFR4:** Data sourcing exclusively from `wikidata-mcp`.
*   **FR13, NFR5:** Clear data attribution to Wikidata with source links.
*   **NFR1:** Performance and responsiveness of interactions.
*   **NFR2:** Usability & UX (modern, clean, intuitive, insightful, engaging, discoverable, progressive disclosure).
*   **NFR3:** Accessibility (WCAG 2.1 Level AA).
*   **NFR6:** Responsiveness (device adaptation, primarily desktop).
*   API interactions with `wikidata-mcp` as per section 9 of the specification.

### 3.2 Features Not to be Tested
As per "Out of Scope" in [`docs/specs/InteractiveVisualExplorationOfConnections_overview.md`](docs/specs/InteractiveVisualExplorationOfConnections_overview.md):
*   User accounts, personalized profiles, or saving of graph states.
*   Real-time collaborative graph exploration.
*   Direct editing of Wikidata information.
*   Advanced graph analytics or data mining beyond visual exploration.
*   Support for data sources other than `wikidata-mcp`.
*   Offline mode or extensive local data caching beyond immediate performance needs.
*   Complex NLP for graph initiation.
*   Version control or history of changes to graph data itself.

## 4. Test Environment

### 4.1 Hardware
*   Desktop computers with modern specifications (sufficient RAM and CPU for browser-based graph rendering).
*   Various screen resolutions to test responsiveness (primarily desktop).

### 4.2 Software
*   **Operating Systems:** Windows 10/11, macOS (latest), Linux (e.g., Ubuntu latest).
*   **Web Browsers:** Latest stable versions of Google Chrome, Mozilla Firefox, Apple Safari, Microsoft Edge.
*   **Test Management Tool:** (To be determined, e.g., Jira, TestRail, or simple spreadsheets).
*   **Automation Tools:** (To be determined, e.g., Selenium, Cypress, Playwright for UI; Jest, Mocha for component/API tests).
*   **Accessibility Testing Tools:** (e.g., WAVE, Axe, browser developer tools).

### 4.3 Data Requirements
*   Access to a functional `wikidata-mcp` server instance populated with representative Wikidata.
*   A curated list of Wikidata entities (QIDs) for testing various scenarios:
    *   Entities with few connections.
    *   Entities with many connections (to test clustering and performance).
    *   Entities with diverse types of relationships and connected items.
    *   Entities with and without thumbnails/images.
    *   Specific entities mentioned in user stories (e.g., "Star Wars").
*   Sample API responses from `wikidata-mcp` for mocking during component/integration testing if direct access is intermittent.

## 5. Test Cases
Test cases will be designed to cover all functional requirements, user stories, and acceptance criteria. Each test case will include: Test Case ID, Title/Description, Preconditions, Test Steps, Expected Results, Actual Results, Pass/Fail Status, Tester, Date, and Notes.

### 5.1 Functional Test Cases (High-Level Examples)

**TC_FUNC_001: Entity Search and Initial Graph Display**
*   **Description:** Verify user can search for an entity and it's displayed as the central node.
*   **User Story:** US1, US2
*   **Acceptance Criteria:** AC1.1, AC2.1
*   **Steps:**
    1.  Navigate to the explorer page.
    2.  Enter a valid entity name (e.g., "Star Wars") in the search bar.
    3.  Initiate the search.
*   **Expected Result:** The graph displays "Star Wars" as the central node. Connected nodes are shown.

**TC_FUNC_002: Node Clustering**
*   **Description:** Verify connected nodes are clustered by common relationship types.
*   **User Story:** US2
*   **Acceptance Criteria:** AC2.2, AC2.3
*   **Steps:**
    1.  Perform TC_FUNC_001 with an entity having diverse connections.
    2.  Observe the initial graph display.
*   **Expected Result:** Nodes connected to the central entity are visually grouped by common relationship types (e.g., "Characters," "Planets"). User can easily identify these groups.

**TC_FUNC_003: Hover for Quick Info**
*   **Description:** Verify hovering over a node displays a tooltip with key details.
*   **User Story:** US3
*   **Acceptance Criteria:** AC3.1, AC3.2, AC3.3
*   **Steps:**
    1.  Display a graph with multiple nodes.
    2.  Hover the mouse cursor over a node.
*   **Expected Result:** A pop-up/tooltip appears displaying a thumbnail (if available) and key details (e.g., label, description). The main graph remains visible and interactive.

**TC_FUNC_004: Click to Expand/Refocus**
*   **Description:** Verify clicking a node expands its connections or refocuses the graph.
*   **User Story:** US4
*   **Acceptance Criteria:** AC4.1, AC4.2, AC4.3
*   **Steps:**
    1.  Display a graph.
    2.  Click on a non-central node.
*   **Expected Result:** The graph animates to re-center on the clicked node or expands its connections. New relevant nodes/relationships are fetched from `wikidata-mcp` and displayed.

**TC_FUNC_005: Filter by Relationship Type**
*   **Description:** Verify graph updates when filtering by relationship type.
*   **User Story:** US5
*   **Acceptance Criteria:** AC5.1, AC5.2, AC5.3
*   **Steps:**
    1.  Display a graph.
    2.  Open the filter panel.
    3.  Select a specific "Relationship Type" filter.
    4.  Apply the filter.
*   **Expected Result:** The graph updates to show only nodes and connections matching the selected relationship type. `wikidata-mcp` is queried with filter parameters.

**TC_FUNC_006: Filter by Item Type**
*   **Description:** Verify graph updates when filtering by item type.
*   **User Story:** US5
*   **Acceptance Criteria:** AC5.1, AC5.2, AC5.3
*   **Steps:**
    1.  Display a graph.
    2.  Open the filter panel.
    3.  Select a specific "Item Type" filter.
    4.  Apply the filter.
*   **Expected Result:** The graph updates to show only nodes and connections matching the selected item type. `wikidata-mcp` is queried with filter parameters.

**TC_FUNC_007: Breadcrumb Trail/History**
*   **Description:** Verify breadcrumb trail tracks navigation and allows backtracking.
*   **User Story:** US6
*   **Acceptance Criteria:** AC6.1, AC6.2, AC6.3
*   **Steps:**
    1.  Navigate through several nodes (e.g., Search A -> Click B -> Click C).
    2.  Observe the breadcrumb/history panel.
    3.  Click on a previous item in the breadcrumb (e.g., B).
*   **Expected Result:** Breadcrumb accurately shows A > B > C. Clicking B navigates the graph back to the state where B was the focus.

**TC_FUNC_008: Data Attribution**
*   **Description:** Verify clear data attribution to Wikidata with source links.
*   **Functional Requirement:** FR13
*   **Non-Functional Requirement:** NFR5
*   **Steps:**
    1.  Display a graph.
    2.  Inspect a node or its quick info pop-up.
*   **Expected Result:** Clear attribution to Wikidata is present, including a clickable link to the source Wikidata item for the entity.

**TC_FUNC_009: API Interaction - Initial Load**
*   **Description:** Verify correct API call to `wikidata-mcp` for initial graph load.
*   **API Design Note:** `GET /entity/{entity_id}/connections` with `depth=1`, `common_groups=true`.
*   **Steps:**
    1.  Initiate a search for an entity.
    2.  Monitor network requests.
*   **Expected Result:** A GET request is made to `/entity/{entity_id}/connections?depth=1&common_groups=true`. The response data is used to render the graph.

**TC_FUNC_010: API Interaction - Node Expansion**
*   **Description:** Verify correct API call to `wikidata-mcp` when a node is clicked to expand/refocus.
*   **API Design Note:** `GET /entity/{entity_id}/connections` with appropriate `depth`.
*   **Steps:**
    1.  Click on a node to expand/refocus.
    2.  Monitor network requests.
*   **Expected Result:** A GET request is made to `/entity/{clicked_node_id}/connections...`. The response data updates the graph.

**TC_FUNC_011: API Interaction - Filtering**
*   **Description:** Verify correct API call to `wikidata-mcp` when filters are applied.
*   **API Design Note:** `GET /entity/{entity_id}/connections` with `relationship_type` and/or `item_type` parameters.
*   **Steps:**
    1.  Apply a relationship type or item type filter.
    2.  Monitor network requests.
*   **Expected Result:** A GET request is made to `/entity/{current_central_node_id}/connections...` including the selected filter parameters. The graph updates based on the response.

### 5.2 Negative Test Cases (High-Level Examples)

**TC_NEG_001: Invalid Entity Search**
*   **Description:** Verify system behavior for non-existent entity search.
*   **Steps:**
    1.  Enter a gibberish string or non-existent entity name in the search bar.
    2.  Initiate search.
*   **Expected Result:** A user-friendly message is displayed (e.g., "Entity not found" or "No results"). The graph area remains empty or shows an appropriate placeholder.

**TC_NEG_002: API Error from `wikidata-mcp`**
*   **Description:** Verify graceful handling of API errors (e.g., server down, 500 error).
*   **Steps:**
    1.  Simulate an API error from `wikidata-mcp` (e.g., using browser dev tools to block request or mock error response).
    2.  Attempt an action that triggers an API call (search, expand, filter).
*   **Expected Result:** A user-friendly error message is displayed. The application remains stable and does not crash. Previously loaded data (if any) might persist or be cleared gracefully.

**TC_NEG_003: No Connections for an Entity**
*   **Description:** Verify behavior when a valid entity has no connections to display.
*   **Steps:**
    1.  Search for a valid Wikidata entity known to have no (or very few, filterable to none) connections of interest.
*   **Expected Result:** The entity is displayed as the central node. A message indicates "No connections found" or similar.

### 5.3 UI/UX Test Cases (High-Level Examples)

**TC_UI_001: Visual Appeal and Layout**
*   **Description:** Verify the interface is modern, clean, and intuitive.
*   **Non-Functional Requirement:** NFR2
*   **Steps:**
    1.  Explore various states of the graph (initial load, expanded, filtered).
*   **Expected Result:** UI elements are well-aligned, typography is clear, color scheme is consistent and appealing. Layout adheres to the description in Section 8 of the spec.

**TC_UI_002: Interaction Feedback**
*   **Description:** Verify smooth animations and clear visual cues for interactions.
*   **Non-Functional Requirement:** NFR2
*   **Steps:**
    1.  Hover over nodes.
    2.  Click nodes to expand/refocus.
    3.  Apply filters.
    4.  Pan and zoom the graph.
*   **Expected Result:** Interactions are accompanied by smooth animations. Hover states are clear. Pan/zoom is intuitive.

**TC_UI_003: Responsiveness (Desktop)**
*   **Description:** Verify the UI adapts gracefully to different desktop screen sizes.
*   **Non-Functional Requirement:** NFR6
*   **Steps:**
    1.  Open the application on different desktop screen resolutions.
    2.  Resize the browser window.
*   **Expected Result:** Layout adjusts without breaking. All elements remain accessible and usable.

### 5.4 Performance Test Cases (High-Level Examples)

**TC_PERF_001: Initial Graph Load Time**
*   **Description:** Measure time taken to load and render the initial graph for an entity with moderate connections.
*   **Non-Functional Requirement:** NFR1
*   **Steps:**
    1.  Select an entity with ~50-100 direct connections.
    2.  Initiate search and measure time until graph is fully rendered and interactive.
*   **Expected Result:** Load time within acceptable limits (e.g., < 3-5 seconds).

**TC_PERF_002: Interaction Latency (Click/Expand)**
*   **Description:** Measure time taken for graph to update after clicking a node to expand/refocus.
*   **Non-Functional Requirement:** NFR1
*   **Steps:**
    1.  Click a node to expand/refocus.
    2.  Measure time from click to when the graph update is complete.
*   **Expected Result:** Latency is minimal, providing a fluid experience (e.g., < 1-2 seconds).

**TC_PERF_003: Filtering Performance**
*   **Description:** Measure time taken for graph to update after applying filters.
*   **Non-Functional Requirement:** NFR1
*   **Steps:**
    1.  Apply a filter that significantly changes the displayed graph.
    2.  Measure time from filter application to graph update completion.
*   **Expected Result:** Update time is minimal (e.g., < 2-3 seconds).

**TC_PERF_004: Large Graph Handling**
*   **Description:** Test system behavior with an entity having a very large number of connections.
*   **Non-Functional Requirement:** NFR1, NFR2 (avoid overwhelming users)
*   **Steps:**
    1.  Search for an entity known to have thousands of direct connections.
*   **Expected Result:** System remains responsive. Clustering is effective. If performance degrades significantly, progressive disclosure or virtualization techniques are evident, or a user warning is provided. Pan/zoom remains smooth.

### 5.5 Accessibility Test Cases (High-Level Examples)

**TC_ACC_001: Keyboard Navigation**
*   **Description:** Verify all interactive elements are navigable and operable using only the keyboard.
*   **Non-Functional Requirement:** NFR3
*   **Steps:**
    1.  Use Tab, Shift+Tab, Enter, Spacebar to navigate and interact with search, nodes, filters, breadcrumbs.
*   **Expected Result:** All interactive elements are focusable and can be activated via keyboard. Focus indicator is clearly visible.

**TC_ACC_002: Screen Reader Compatibility**
*   **Description:** Verify content is understandable and navigable with a screen reader.
*   **Non-Functional Requirement:** NFR3
*   **Steps:**
    1.  Use a screen reader (e.g., NVDA, VoiceOver) to navigate the feature.
*   **Expected Result:** Nodes, relationships, tooltips, and controls are announced meaningfully. ARIA attributes are used correctly.

**TC_ACC_003: Color Contrast**
*   **Description:** Verify text and UI elements meet WCAG AA contrast ratios.
*   **Non-Functional Requirement:** NFR3
*   **Steps:**
    1.  Use color contrast analyzer tools on various UI elements.
*   **Expected Result:** All text and meaningful non-text elements meet minimum contrast requirements.

## 6. Requirements Traceability
A Requirements Traceability Matrix (RTM) will be created to map test cases back to User Stories, Acceptance Criteria, and Functional/Non-Functional Requirements from [`docs/specs/InteractiveVisualExplorationOfConnections_overview.md`](docs/specs/InteractiveVisualExplorationOfConnections_overview.md). This ensures comprehensive test coverage.

*(Example Snippet of RTM)*
| Requirement ID (US/AC/FR/NFR) | Requirement Description                                  | Test Case ID(s)                               |
|-------------------------------|----------------------------------------------------------|-----------------------------------------------|
| US1                           | Visually navigate connections...                         | TC_FUNC_001, ...                              |
| AC1.1                         | Entity shown as central node.                            | TC_FUNC_001                                   |
| FR3                           | Display connected nodes clustered...                     | TC_FUNC_002                                   |
| NFR1                          | Performance: Interactions must feel fast and fluid.      | TC_PERF_001, TC_PERF_002, TC_PERF_003, TC_PERF_004 |
| NFR3                          | Accessibility: WCAG 2.1 Level AA.                        | TC_ACC_001, TC_ACC_002, TC_ACC_003              |

## 7. Entry and Exit Criteria

### 7.1 Entry Criteria
*   Test plan approved.
*   Feature development for a testable build/release is complete.
*   `wikidata-mcp` server is stable and accessible with relevant test data.
*   Test environment is set up and verified.
*   Required test tools are available.

### 7.2 Exit Criteria
*   All planned test cases have been executed.
*   A defined percentage of test cases passed (e.g., 100% of critical, 95% of high priority).
*   No outstanding critical or high-severity defects.
*   All major known issues have fixes verified or have agreed-upon workarounds/deferrals.
*   Requirements traceability coverage is met.
*   Test summary report is generated and approved.
*   User Acceptance Testing (UAT) completed and signed off.

## 8. Test Deliverables
*   **Test Plan (this document):** [`docs/testplans/InteractiveVisualExplorationOfConnections_testplan.md`](docs/testplans/InteractiveVisualExplorationOfConnections_testplan.md)
*   **Test Cases:** Detailed test case specifications.
*   **Test Data:** List of Wikidata entities and specific data sets used for testing.
*   **Requirements Traceability Matrix (RTM).**
*   **Test Execution Logs:** Records of executed test cases and their outcomes.
*   **Defect Reports:** Detailed reports for all identified issues.
*   **Test Summary Report:** A comprehensive report summarizing testing activities, results, coverage, and outstanding issues upon completion of the test cycle.

## 9. Risks and Mitigations

| Risk ID | Risk Description                                                                 | Likelihood | Impact | Mitigation Strategy                                                                                                                               |
|---------|----------------------------------------------------------------------------------|------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| RISK_01 | `wikidata-mcp` server instability or unavailability during testing.                | Medium     | High   | Coordinate with `wikidata-mcp` team for stable builds. Use mocked API responses for frontend component testing where feasible. Schedule buffer time. |
| RISK_02 | Performance issues with large graph rendering or complex queries.                | Medium     | High   | Early performance testing with representative data. Optimize queries and rendering techniques. Implement progressive disclosure/virtualization if needed. |
| RISK_03 | Difficulty in achieving full WCAG 2.1 AA compliance for complex graph visuals.   | Medium     | Medium | Engage accessibility experts early. Use appropriate ARIA roles and properties. Iterative testing and refinement.                                    |
| RISK_04 | Test data setup for diverse scenarios is complex and time-consuming.             | Medium     | Medium | Identify and curate a core set of test entities early. Automate test data provisioning if possible.                                                |
| RISK_05 | Subjectivity in Usability (NFR2) and UI (NFR2) testing.                          | Low        | Medium | Define clear, measurable heuristics for usability. Involve multiple testers. Conduct formal usability testing sessions with target users if possible. |
| RISK_06 | Scope creep or late changes in requirements.                                     | Low        | Medium | Strict change control process. Assess impact of changes on test plan and schedule.                                                                  |
| RISK_07 | Delays in development affecting test schedule.                                   | Medium     | Medium | Regular communication with development team. Phased testing approach if possible.                                                                 |