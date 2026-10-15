# Lab 03 - Distributed Algorithms (Echo Algorithm & Simulation)

A simulation of distributed algorithms on a network of independent node processes. Each node runs its own process, communicates with its neighbors over **UDP** using **JSON** messages, and a central controller triggers the algorithm and collects logs. The implemented algorithm determines the **total available memory** and the **top 5 memory consumers** in the network (echo algorithm with a flooding variant).

## What the Project Does

- **Distributed topology:** A network is defined in `mynet.json` with N >= 10 nodes and between 2N and 3N edges.
- **Node processes:** Each node is started as a separate process with command-line arguments (own address, controller address, storage, neighbors).
- **Algorithm (flooding + echo):**
  - `S` (Start) - sent by the controller to the initiator node.
  - `I` (Initiate) - flooding message, broadcast to all neighbors.
  - `E` (Echo) - returned back up the spanning tree, carrying the top-5 storage list.
  - `R` (Result) - the initiator sends the final result to the controller.
  - `L` (Log) - every node sends a log entry (timestamp, sender, receiver, type) to the controller.
- **Latency simulation:** Artificial random delays (0-99 ms) are added when handling incoming messages.
- **Controller:** Triggers the algorithm with an `S` message via a port entered on the console, counts `I` and `E` messages, and prints the final result.
- **Top-5 consumers:** The `E` messages carry `StorageEntry` lists that are merged and trimmed to the highest 5 storages along the way.

## Project Structure

```text
project3/
├── .gitignore                  # Ignores build/, .gradle/, .idea/, out/
├── .idea/                      # IntelliJ IDEA project configuration
├── assets/                     # Screenshots / diagrams
├── build.gradle.kts            # java + application plugin, Jackson, fat-jar task
├── settings.gradle.kts
├── gradle/wrapper/             # Gradle wrapper jar + properties
├── gradlew / gradlew.bat       # Gradle wrapper (8.4)
├── mynet.json                  # Network topology (10 nodes, addresses, neighbors, storage)
├── README.md
└── src/main/
    ├── java/
    │   ├── META-INF/MANIFEST.MF
    │   └── org/example/
    │       ├── Deployment.java   # Entry point: reads mynet.json, starts controller + all nodes
    │       ├── Controller.java   # Central controller: triggers algorithm, collects logs/results
    │       ├── Main.java         # Node entry point: parses args, creates and starts a Node
    │       ├── Node.java         # Node logic: flooding, echo, top-5 merge, logging
    │       ├── Message.java      # Base message + I/S/E/R/L subtypes (Jackson polymorphic)
    │       ├── StorageEntry.java # address + storage value, sorted descending
    │       └── startnode         # bash helper script to launch a node
```

> `build/`, `out/` and `.gradle/` (build output) are omitted.

> Note: `mynet.json` is expected in the working directory (the `Deployment` reads `mynet.json` by default, or a path passed as the first argument).

## Requirements

- Java 17 (project targets `JavaVersion.VERSION_17`)
- Gradle wrapper is included (`gradlew`)
- Dependencies are fetched automatically (Jackson databind)

## Build & Run

### 1. Build (also builds the fat jar `build/libs/vs-lab3.jar`)

```powershell
.\gradlew.bat build
```

### 2. Run the deployment

The `Deployment` class is the main entry point (also set as `mainClass`). It:

1. reads `mynet.json` (must contain >= 10 nodes),
2. opens the controller in a **separate terminal window** (port `9999`),
3. starts every node as its own process,
4. terminates all processes when the main window is closed.

Run it from 3/lab3 so `mynet.json` is found:

```powershell
.\gradlew.bat run
```

Alternatively, run the fat jar directly:

```powershell
java -jar build\libs\vs-lab3.jar
```

> On Linux/macOS the deployment uses `x-terminal-emulator`; on Windows it uses a new `cmd` window.

### 3. Trigger the algorithm

- The controller prints a network summary (number of nodes and edges) and then asks for a port:
  ```text
  Insert the Initiator node port or 'exit':
  ```
- Enter the port of an initiator node (e.g. `8001`) to send the `S` (start) message.
- Watch the controller output for `I`/`E` log counts and the final `R` result with the top-5 memory consumers.
- Enter `exit` to stop the controller.
