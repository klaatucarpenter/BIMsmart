
# BIMsmart

Blockchain-based milestone payment system for construction projects.

Prototype developed during the Chainlink Fall 2021 Hackathon.

The project explores how smart contracts, BIM (Building Information Modeling), and oracle infrastructure can automate milestone-based payments between investors and contractors in construction projects.

This concept was inspired by real-world experience in construction project management and explores how blockchain infrastructure could automate milestone-based payments in complex project delivery.

Construction delivery often suffers from payment disputes, delayed cash flows and lack of transparent verification of project progress. BIMsmart proposes a trust‑minimized workflow where verified project progress automatically triggers payments via smart contracts.

---

# System Concept

## Idea overview

![Idea schema](img/idea-schema.jpg)

The system links project progress verification with automated payment release using blockchain smart contracts.

Typical workflow:

1. Construction progress is evaluated  
2. Progress data is verified through external services  
3. Chainlink oracle provides trusted input  
4. Smart contract releases milestone payment automatically  

This approach reduces disputes, improves transparency, and enables programmable financial flows in project delivery.

---

## System Architecture

![Project schema](img/project-schema.jpg)

The system consists of three main layers.

### Smart Contract Layer

Handles escrow logic and milestone-based payment release.

Responsibilities:

- storing milestone payment conditions  
- receiving verified progress data  
- executing automatic payment transfers  

### Oracle Layer (Chainlink)

Retrieves external progress evaluation data and feeds it into the blockchain through a secure oracle network.

Responsibilities:

- connecting blockchain with off-chain data  
- validating external inputs  
- triggering contract execution  

### Application Layer

Provides user interaction and system orchestration through frontend and external adapters.

Responsibilities:

- user interface  
- progress verification input  
- interaction with smart contracts  

---

## Role of IPFS

![IPFS schema](img/ipfs-role-schema.jpg)

IPFS is used for decentralized storage of project documentation and progress-related data.

Instead of storing large files directly on-chain, the system stores cryptographic references to files stored in IPFS. This ensures:

- transparency  
- immutability  
- reduced blockchain storage costs  

---

# Architecture Overview

The prototype architecture follows a hybrid **on-chain / off-chain system design**.

```
User / Project Manager
        |
        v
Frontend Application
        |
        v
Smart Contract (Escrow Logic)
        |
        v
Chainlink Oracle
        |
        v
External Adapter (Progress Evaluation)
        |
        v
Construction Data / Verification Source
```

Key design goals:

- trust minimization between investors and contractors
- automated milestone payments
- transparent verification of construction progress
- separation between blockchain logic and external data verification

---

# Architecture Decisions (ADR)

## ADR‑001: Smart contracts for milestone escrow

**Decision:**  
Payments are controlled by blockchain smart contracts instead of traditional escrow services.

**Reasoning:**

- eliminates reliance on centralized intermediaries
- ensures transparent payment rules
- guarantees deterministic execution of milestone payments

**Trade‑off:**

Smart contracts require reliable external data verification through oracles.

---

## ADR‑002: Use of Chainlink Oracles

**Decision:**  
External project progress data is delivered via Chainlink oracles.

**Reasoning:**

- provides secure bridge between off-chain and on-chain data
- widely used oracle infrastructure
- suitable for trusted data delivery in financial workflows

**Trade‑off:**

Oracle reliability becomes part of system trust model.

---

## ADR‑003: Off-chain storage with IPFS

**Decision:**  
Construction documentation and progress evidence are stored off-chain using IPFS.

**Reasoning:**

- blockchain storage is expensive
- project documentation may be large (images, scans, BIM files)
- IPFS provides decentralized storage with verifiable hashes

**Trade‑off:**

Requires maintaining availability of IPFS nodes.

---

# Research Inspiration

The project concept is inspired by the research work:

**Hesam Hamledari – Construction Payment Automation Using Blockchain-Enabled Smart Contracts and Robotic Reality Capture Technologies**

https://www.researchgate.net/publication/344972443_Construction_Payment_Automation_Using_Blockchain-Enabled_Smart_Contracts_and_Robotic_Reality_Capture_Technologies

---

# Repository Structure

The repository contains several components required to run the prototype.

```
BIMsmart
│
├── smart-contract
├── progress-evaluation-EA
├── frontend
├── backend-brownie
└── img
```

### Smart Contract

Implements milestone-based escrow logic.

Responsibilities:

- receiving oracle data
- verifying progress milestones
- triggering automatic payments

---

### External Adapter

The external adapter connects Chainlink with external progress evaluation logic.

Responsibilities:

- retrieving progress evaluation data
- formatting oracle responses
- providing results to Chainlink jobs

Location:

```
progress-evaluation-EA
```

---

### Frontend

Provides a user interface for interacting with the smart contract and monitoring project progress.

Responsibilities:

- contract interaction
- milestone visualization
- user workflow interface

Location:

```
frontend
```

---

# Running the System

## 1. Run Chainlink Node

Official documentation:

https://docs.chain.link/docs/running-a-chainlink-node/

Example Docker command:

```
cd ~/.chainlink-kovan

docker run -p 6688:6688 \
-v ~/.chainlink-kovan:/chainlink \
-it \
--env-file=.env \
smartcontract/chainlink:<version> local n
```

Latest Chainlink image versions are available here:

https://github.com/smartcontractkit/chainlink

---

## 2. Configure Oracle Infrastructure

For this project:

- Ethereum test network: **Kovan**
- External RPC provider: **Alchemy**
- Database: **PostgreSQL (pgAdmin4)**

Example gas configuration:

```
ETH_GAS_LIMIT_DEFAULT=10000000
```

---

## 3. Configure Chainlink Job

Job specification is located here:

```
progress-evaluation-EA/chainlink-job.toml
```

---

## 4. Configure Chainlink Bridge

Bridge parameters:

```
Name: bimsmart
URL: <progress-evaluation-EA_url>
```

---

## 5. Provide Schedule of Values

Example schema:

```
progress-evaluation-EA/tests/schedule_of_values_for_testing.xlsx
```

---

## 6. Environment Variables

Create `.env` files based on `.env.example` and provide:

- RPC endpoints
- API keys
- contract addresses

---

# Running External Adapter

```
python app.py
```

---

# Running the Frontend

Before launching the frontend, ABI files must be generated.

The following script updates the frontend automatically:

```
backend-brownie/scripts/update_frontend.py
```

Frontend documentation:

```
frontend/README.md
```

---

# Project Context

BIMsmart was created as an experimental prototype exploring how blockchain-based escrow mechanisms could improve trust, transparency and financial automation in construction project delivery.

The concept directly addresses common construction industry challenges:

- delayed contractor payments
- disputes over project progress
- lack of transparent verification mechanisms

The prototype demonstrates how programmable financial logic can be combined with real-world project data to enable automated milestone payments.
