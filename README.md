# Distributed Systems Lab

This repository contains the laboratory experiments developed as part of the **Distributed Systems** practical course (Prof. Dr. Erwin Mayer). Java and Python were used as programming languages, based on network infrastructures with UDP, gRPC, and simulated distributed networks.

---

## Table of Contents
1. [Lab 1: Peer-to-Peer Computing (Multi-Player Game)](#lab-1-peer-to-peer-computing-multi-player-game)
2. [Lab 2: gRPC Programming (Chat Application & Service Chaining)](#lab-2-grpc-programming-chat-application--service-chaining)
3. [Lab 3: Distributed Algorithms (Echo Algorithm & Simulation)](#lab-3-distributed-algorithms-echo-algorithm--simulation)

---

## Lab 1: Peer-to-Peer Computing (Multi-Player Game)

In the first experiment, a simple distributed peer-to-peer application was implemented in the form of a multi-player game (inspired by the concept of "Tron" / trail structure).

- **Architecture:** Pure peer-to-peer architecture (friend approach via join command, no central routing required).
- **Communication:** Message-based via **UDP sockets** using **JSON** for data structures.
- **Features (Part 1):** 
  * Square playing field ($M \times M$) with configurable size and up to 4 players.
  * Control via arrow keys, permanently leaving a trail (collision detection with walls and trails leads to elimination).
  * Real-time synchronization of all game states between peers and visualization via GUI.
- **Extensions (Part 2):** 
  * Dynamic group composition with `Join` and `Leave` commands.
  * Management of active online peers and automatic message dispatch exclusively to reachable participants.

---

## Lab 2: gRPC Programming (Chat Application & Service Chaining)

The second experiment focused on a Service-Oriented Architecture (SOA) using **gRPC (Google RPC)** and Protocol Buffers (Proto3) as middleware.

- **Technologies:** Java, gRPC, Protocol Buffers (`.proto` specifications), IntelliJ.
- **Subtask 1 (Classic Chat):**
  * Registration of $N$ chat participants ($2 < N \le 10$) via nicknames with duplicate error handling.
  * Sending and receiving messages to all active participants as well as querying the current participant list.
  * Use of gRPC streaming mechanisms and `oneof` syntax elements.
  * Analysis of transmitted messages at the byte level (hex decoding of string and integer fields) using **Wireshark**.
- **Subtask 2 (Service Chaining / History Server):**
  * Implementation of an independent **History Server** as an additional service.
  * The main server mirrors all chat entries to the history server via a gRPC stream (*Service Chaining*).
  * New clients are provided with the existing chat history directly upon joining.

---

## Lab 3: Distributed Algorithms (Echo Algorithm & Simulation)

The third experiment examined distributed algorithms and principles for performing distributed computations (summing up memory locations in the network) without a central coordinator.

- **Core Concepts:** 
  * Application of the **Flooding** and **Echo algorithms** based on UDP messages.
  * Event/action design pattern for distributed algorithms.
- **Simulation Environment:**
  * A network specified in a JSON file (`mynet.json`) with $N \ge 10$ nodes and between $2N$ and $3N$ edges.
  * Each node is started as a separate process, controlled via a deployment component using command-line arguments.
  * Simulation of variable message latencies using artificial delays (`sleep`).
- **Controller Process & Logging:**
  * A central controller triggers the algorithm (START message) and continuously collects `LOG` messages (timestamp, sender, receiver, message type I/E).
  * At the end, the controller determines and visualizes the total available memory as well as the number of exchanged messages.
- **Extensions (Homework & Remote Deployment):**
  * Automated remote process startup via **SSH** (`Runtime.exec` / `ProcessBuilder`) on distributed machines.
  * Extension of the algorithm to determine the **top 5 memory consumers** in the network.

---
*Created as part of the Distributed Systems lab course.*
