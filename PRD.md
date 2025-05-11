## Product Requirements Document: Escalator Passenger Flow Simulator

**1. Introduction**

*   **1.1. Purpose:** This document outlines the requirements for the Escalator Passenger Flow Simulator, a web-based tool designed to visualize and analyze the dynamics of passenger movement on escalators under various policies and conditions. It serves as a guide for development, testing, and future enhancements.
*   **1.2. Scope:** The simulator will model passenger arrivals, queueing, boarding, movement on a two-lane escalator, and exiting. Users will be able to adjust key parameters and observe the impact on throughput and passenger experience. The current version focuses on a deterministic, slot-based model with options for "Walk Left, Stand Right" and "Stand Both Lanes" policies.
*   **1.3. Target Users:**
    *   Urban planners and transit system designers.
    *   Operations researchers and systems analysts.
    *   Students and educators in fields related to queueing theory, simulation, and transportation.
    *   Curious individuals interested in understanding public space dynamics.
*   **1.4. Goals:**
    *   Provide a visual and interactive way to understand escalator efficiency.
    *   Allow users to explore the tradeoffs between different escalator usage policies.
    *   Serve as an educational tool for demonstrating concepts from queueing theory and systems analysis.
    *   Offer insights into how parameters like arrival rate, walker percentage, and escalator design affect flow.
*   **1.5. Definitions & Acronyms:**
    *   **WS Policy:** Walk Left, Stand Right policy.
    *   **SS Policy:** Stand Both Lanes policy.
    *   **Slot:** A discrete unit of space on the escalator a passenger can occupy.
    *   **Walker:** A passenger who prefers to walk on the escalator.
    *   **Stander:** A passenger who prefers to stand on the escalator.
    *   **Throughput:** Number of passengers exited per unit of time (e.g., passengers/sec).
    *   **UI:** User Interface.

**2. Overall Description**

*   **2.1. Product Perspective:** The Escalator Passenger Flow Simulator is a standalone, client-side web application. It does not require server-side processing beyond serving the initial HTML, CSS, and JavaScript files.
*   **2.2. Product Features (High-Level):**
    *   Interactive parameter adjustment via UI controls.
    *   Real-time visual simulation of passenger movement on a two-lane escalator.
    *   Dynamic display of key performance metrics.
    *   Ability to reset and re-run simulations with new parameters.
*   **2.3. User Characteristics:** Users are expected to have a basic understanding of web interfaces. Users interested in in-depth analysis may have backgrounds in engineering, planning, or mathematics.
*   **2.4. Operating Environment:** The simulator must run in modern web browsers (e.g., Chrome, Firefox, Safari, Edge) that support HTML5 Canvas and JavaScript ES6+.
*   **2.5. Design and Implementation Constraints:**
    *   Must be implemented as a single HTML file embedding CSS and JavaScript for ease of distribution and use.
    *   The simulation model should be computationally efficient enough to run smoothly in real-time on typical user hardware.
    *   Visual clarity is paramount for understanding the simulation.
*   **2.6. Assumptions and Dependencies:**
    *   Users have JavaScript enabled in their browsers.
    *   The simulation model uses discrete time steps and discrete slots on the escalator.

**3. Specific Requirements**

*   **3.1. Functional Requirements:**
    *   **FR-1: Parameter Configuration:**
        *   **FR-1.1:** The user MUST be able to adjust the "Arrival Rate" (passengers/sec) via a slider. The current value MUST be displayed.
        *   **FR-1.2:** The user MUST be able to adjust the "Walker Percentage" (%) via a slider. The current value MUST be displayed.
        *   **FR-1.3:** The user MUST be able to adjust the "Escalator Length" (slots) via a slider. The current value MUST be displayed. Changes to this parameter SHOULD reset the simulation.
        *   **FR-1.4:** The user MUST be able to adjust the "Escalator Speed" (slots/sec) via a slider. The current value MUST be displayed.
        *   **FR-1.5:** The user MUST be able to adjust the "Walker Speed Boost" (additional slots/sec for walkers) via a slider. The current value MUST be displayed.
        *   **FR-1.6:** The user MUST be able to adjust the "Walker Min Spacing" (slots walkers prefer ahead) via a slider. The current value MUST be displayed.
        *   **FR-1.7:** The user MUST be able to select the "Escalator Policy" (Walk Left, Stand Right; Stand Both Lanes) via a dropdown menu.
    *   **FR-2: Simulation Control:**
        *   **FR-2.1:** The user MUST be able to reset the simulation to its initial state with current parameter settings using a "Reset Simulation" button.
    *   **FR-3: Visual Simulation Display:**
        *   **FR-3.1:** The simulation MUST be rendered on an HTML5 Canvas.
        *   **FR-3.2:** A queueing area MUST be visually distinct from the escalator.
        *   **FR-3.3:** The escalator MUST be depicted with two distinct lanes.
        *   **FR-3.4:** Passengers MUST be visually represented (e.g., as circles).
        *   **FR-3.5:** Walkers and Standers MUST be visually distinguishable (e.g., by color based on their original preference).
        *   **FR-3.6:** Passengers in the queue MUST be visualized.
        *   **FR-3.7:** Passengers on the escalator lanes MUST be visualized moving upwards.
        *   **FR-3.8:** An "Exit" area MUST be visually distinct.
    *   **FR-4: Passenger Behavior Modeling:**
        *   **FR-4.1: Arrival:** Passengers MUST arrive at the queue based on the specified "Arrival Rate" using a probabilistic model for fractional arrivals.
        *   **FR-4.2: Type Assignment:** Arriving passengers MUST be assigned as "Walker" or "Stander" based on "Walker Percentage."
        *   **FR-4.3: Queueing:** Passengers arriving when the escalator boarding slots are occupied MUST join a queue.
        *   **FR-4.4: Boarding (WS Policy - Walk Left, Stand Right):**
            *   FR-4.4.1: Walkers (original preference) SHOULD attempt to board the left lane if space (considering `walkerMinSpacing`) is available.
            *   FR-4.4.2: If the left lane is not available, walkers MAY attempt to board the right lane if space (1 slot) is available.
            *   FR-4.4.3: Standers (original preference) SHOULD attempt to board the right lane if space (1 slot) is available.
        *   **FR-4.5: Boarding (SS Policy - Stand Both Lanes):**
            *   FR-4.5.1: Any passenger (walker or stander) MUST attempt to board any available lane (1 slot space).
            *   FR-4.5.2: Boarding SHOULD attempt to balance load between lanes if both are free.
            *   FR-4.5.3: All passengers in SS policy effectively behave as standers for speed calculation.
        *   **FR-4.6: Movement on Escalator (General):**
            *   FR-4.6.1: Passengers MUST move at a base speed defined by "Escalator Speed."
        *   **FR-4.7: Movement on Escalator (WS Policy):**
            *   FR-4.7.1: Walkers in the left lane (Lane 0) SHOULD move at "Escalator Speed" + "Walker Speed Boost."
            *   FR-4.7.2: Walkers in the right lane (Lane 1) SHOULD move at "Escalator Speed" + "Walker Speed Boost" UNLESS a non-walker (original stander) is closely ahead, in which case they move at "Escalator Speed."
            *   FR-4.7.3: Standers in any lane move at "Escalator Speed."
            *   FR-4.7.4: Walkers (in either lane, if attempting to walk) MUST attempt to maintain "Walker Min Spacing" from the passenger ahead, slowing down if necessary.
            *   FR-4.7.5: Standers (or walkers behaving as standers) MUST attempt to maintain a 1-slot spacing, slowing down if necessary.
        *   **FR-4.8: Lane Shuffling (WS Policy for original Walkers):**
            *   FR-4.8.1: Walkers in the left lane significantly slowed down MAY attempt to shuffle to the right lane if a suitable gap exists.
            *   FR-4.8.2: Walkers in the right lane MAY attempt to shuffle to the left lane if a suitable gap (respecting `walkerMinSpacing`) exists and the left lane offers a speed advantage or they are currently forced to stand.
            *   FR-4.8.3: A cooldown period MUST be implemented after a shuffle to prevent rapid flickering.
        *   **FR-4.9: Exiting:** Passengers reaching the end of the "Escalator Length" MUST be removed from the simulation and counted as "Exited."
    *   **FR-5: Statistics Display:**
        *   **FR-5.1:** The simulation MUST display the current "Time" elapsed (seconds).
        *   **FR-5.2:** The simulation MUST display the current number of "Queued" passengers.
        *   **FR-5.3:** The simulation MUST display the current number of passengers "On Escalator."
        *   **FR-5.4:** The simulation MUST display the total number of "Exited" passengers.
        *   **FR-5.5:** The simulation MUST display the current "Throughput" (Exited passengers / Time).
        *   **FR-5.6:** The simulation MUST display the "Average Walker Speed" (slots/sec) for passengers (original preference) currently on the escalator.
        *   **FR-5.7:** The simulation MUST display the "Average Stander Speed" (slots/sec) for passengers (original preference) currently on the escalator.
        *   **FR-5.8:** The simulation MUST display the "Average Overall Speed" (slots/sec) for all passengers currently on the escalator.
*   **3.2. User Interface (UI) Requirements:**
    *   **UI-1:** The layout MUST be clean and intuitive, with controls clearly separated from the visualization and statistics.
    *   **UI-2:** Parameter adjustment controls (sliders, dropdowns) MUST be responsive and provide immediate feedback on their current value.
    *   **UI-3:** The visualization canvas MUST be adequately sized to clearly show passenger movement.
    *   **UI-4:** Statistics MUST be clearly labeled and updated in real-time.
*   **3.3. Performance Requirements:**
    *   **PERF-1:** The simulation animation SHOULD run smoothly (target >= 30 FPS) on typical modern desktop hardware.
    *   **PERF-2:** UI controls MUST respond to user input without noticeable lag.
*   **3.4. Usability Requirements:**
    *   **USE-1:** The simulator MUST be easy to understand and operate without requiring extensive documentation for basic use.
    *   **USE-2:** Visual cues (colors, movement) MUST effectively communicate the state of the simulation.

**4. Non-Functional Requirements**

*   **NFR-1: Maintainability:** The JavaScript code SHOULD be well-structured, commented, and organized into logical functions/classes to facilitate understanding and future modifications.
*   **NFR-2: Portability:** The simulator MUST function correctly across major modern web browsers (Chrome, Firefox, Safari, Edge) without browser-specific code where avoidable.
*   **NFR-3: Reliability:** The simulation SHOULD run stably without crashing or producing obviously incorrect visual or statistical output under normal parameter ranges.
*   **NFR-4: Scalability (Conceptual):** While the current model is deterministic, the design should not overtly preclude future extensions to more complex or stochastic models.

**5. Future Considerations (Out of Scope for Current Version but Potential Enhancements)**

*   Stochastic arrival patterns and passenger type determination.
*   Boarding time delays.
*   More complex passenger decision-making (e.g., balking, reneging from queue).
*   Modeling of escalator breakdowns or variable speeds.
*   Ability to save/load simulation parameters.
*   Data logging and export for offline analysis.
*   Different escalator configurations (e.g., number of lanes, spiral escalators).
*   Agent-based modeling for more nuanced individual behaviors.
*   Accessibility improvements (e.g., keyboard navigation for controls, ARIA attributes).
