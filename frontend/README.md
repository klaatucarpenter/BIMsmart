# BIMsmart Frontend

Frontend interface for interacting with the BIMsmart blockchain payment
system.

The application allows users to interact with the smart contract, submit
project progress data and monitor milestone-based payments.

This frontend is part of the BIMsmart prototype developed during the
Chainlink Fall 2021 Hackathon.

------------------------------------------------------------------------

# Role in the System

The frontend acts as the user interface for the BIMsmart architecture.

Responsibilities:

• interacting with the smart contract\
• submitting project progress data\
• visualizing project milestones\
• uploading project documentation to IPFS

The frontend communicates with:

-   the deployed smart contract
-   the Chainlink oracle workflow
-   decentralized storage (IPFS)

------------------------------------------------------------------------

# Architecture Position

User\
↓\
React Frontend\
↓\
Smart Contract (Ethereum)\
↓\
Chainlink Oracle\
↓\
External Adapter (progress evaluation)

The frontend itself does not evaluate project progress.\
It only provides the interface for submitting data and interacting with
the smart contract.

------------------------------------------------------------------------

# Smart Contract ABI

Before running the frontend, the **ABI of the deployed smart contract
must be available**.

The ABI is automatically copied to the frontend by the following script:

backend-brownie/scripts/update_frontend.py

Run this script after deploying the smart contract.

------------------------------------------------------------------------

# Environment Configuration

To upload project documentation to **IPFS**, the application requires
API credentials.

Create a `.env` file based on `.env.example` and provide the required
keys.

This prototype uses **Fleek Storage** as the IPFS provider.

Documentation: https://fleek.co/storage/

Example `.env` variables:

REACT_APP_FLEEK_API_KEY= REACT_APP_FLEEK_API_SECRET=

------------------------------------------------------------------------

# Installation

Install dependencies:

yarn install

------------------------------------------------------------------------

# Running the Frontend

Start the development server:

yarn start

The application will be available at:

http://localhost:3000

------------------------------------------------------------------------

# Build for Production

yarn build

This creates an optimized production build in the `build/` directory.

------------------------------------------------------------------------

# Project Context

This frontend is a prototype interface designed to demonstrate how
blockchain-based escrow logic could automate milestone payments in
construction projects.

The concept was inspired by real-world experience in construction
project management and explores how blockchain infrastructure could
support transparent financial workflows in complex project delivery.
