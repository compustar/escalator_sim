# Escalator Passenger Flow Simulator

This project is a simple, interactive web-based simulator demonstrating passenger flow dynamics on escalators under different policies and conditions. Developed using pure HTML, CSS, and JavaScript, it's designed as an educational tool to visualize concepts from queueing theory and systems analysis, particularly the "stand vs. walk" debate on multi-lane escalators.

## Features

*   **Interactive Parameters:** Adjust key simulation variables like arrival rate, walker percentage, escalator length and speed, walker speed boost, walker spacing preference, and the escalator policy.
*   **Real-time Visualization:** Watch passengers move in real-time on a two-lane escalator, observing queue buildup, boarding, movement, and exiting.
*   **Policy Comparison:** Directly compare the impact of "Walk Left, Stand Right" vs. "Stand Both Lanes" policies on overall system performance and passenger experience.
*   **Performance Metrics:** Track important statistics dynamically, including simulation time, queue length, passengers on the escalator, total exited passengers, and throughput.
*   **Pure Web Technology:** Runs entirely in your browser from a single HTML file, requiring no server setup or dependencies beyond a modern web browser.

## Getting Started

The simulator is contained within a single HTML file. To run it:

1.  Clone or download this repository to your local machine.
    ```bash
    git clone <repository_url>
    ```
    (Replace `<repository_url>` with the actual URL of your Git repository)
2.  Navigate to the project directory.
3.  Open the `index.html` (or `escalator_sim.html` if you named it differently) file in any modern web browser (Chrome, Firefox, Safari, Edge).

That's it! The simulation will start automatically.

## How to Use

*   **Controls:** The left panel provides sliders and a dropdown to adjust simulation parameters. Changing these values will immediately affect how new passengers arrive and behave.
*   **Simulation Area:** The central canvas visualizes the queue, the two escalator lanes, and the exit area. Blue circles represent original walkers, and green circles represent original standers.
*   **Statistics:** The right panel displays real-time performance metrics of the simulation.
*   **Reset:** Click the "Reset Simulation" button to clear all passengers and restart the simulation with the current parameter settings.

## Parameters Explained

*   **Arrival Rate:** How many passengers arrive at the queue per second.
*   **Walker Percentage:** The percentage of arriving passengers who prefer to walk on the escalator (blue circles).
*   **Escalator Length:** The length of the escalator in abstract "slots."
*   **Escalator Speed:** The base speed of the escalator in "slots per second."
*   **Walker Speed Boost:** How much faster walkers can move on the escalator compared to the base speed (in slots per second).
*   **Walker Min Spacing:** The minimum number of slots a walker tries to maintain between themselves and the passenger directly ahead on the escalator.
*   **Escalator Policy:**
    *   **Walk Left, Stand Right (WS):** Walkers primarily use the left lane and attempt to walk. Standers use the right lane. Walkers may use the right lane if the left is blocked but might be forced to stand if a slow passenger is ahead. Walkers attempt to maintain `Walker Min Spacing` when boarding either lane.
    *   **Stand Both Lanes (SS):** All passengers stand on either lane, maintaining 1-slot spacing and moving at the `Escalator Speed`. Load balancing is attempted between lanes upon boarding.

## Simulation Logic (Simplified)

*   The simulation operates in discrete time steps.
*   Passengers arrive probabilistically based on the arrival rate.
*   Passengers join a single queue before boarding.
*   Boarding logic depends on the selected policy and availability of space at the start of each lane, respecting spacing rules.
*   Passengers on the escalator move based on their effective speed (base speed + boost for walking walkers) and the speed of the passenger directly ahead (maintaining spacing).
*   Passengers reaching the end exit the system.

**Note:** This is a simplified model for illustration. Real-world escalator dynamics involve more complex factors like individual variability, dynamic decision-making, and physical constraints.

## Based on Original Research

This simulator was inspired by the research presented in the paper:

**Escalator Etiquette: Stand or Walk? A Systems Analysis**
by Michael C. Fu
Systems 2024, 12(6), 203; https://doi.org/10.3390/systems12060203

The paper provides a quantitative analysis using deterministic queueing models to explore the tradeoffs between different escalator usage policies, particularly focusing on clearing congestion during peak periods. This simulator aims to provide a visual and interactive complement to the theoretical insights presented in the paper.

## Development and Contribution

This project is a self-contained example. If you wish to contribute or enhance it, feel free to fork the repository and submit pull requests. Areas for improvement could include:

*   Adding more sophisticated passenger behaviors (e.g., dynamic lane changing based on observed speed).
*   Implementing stochastic elements more rigorously (e.g., random service times).
*   Improving visual fidelity and performance.
*   Adding more complex scenarios or escalator configurations.
*   Refactoring the JavaScript for better modularity and testability.

## License

This project is open-source and available under the Apache 2.0 License.
