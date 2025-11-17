# Legeon DAO, Treasury, and LEGN Token Architecture

```mermaid
flowchart LR
    %% Actors
    subgraph ACTORS[Participants]
        C[Consultants]
        ENT[Enterprises]
        PART[Partners - MSPs, Training, SIs]
        GEN[Genesis Innovators]
    end

    %% On-Chain Components
    subgraph ONCHAIN[On-Chain Components]
        subgraph CARD[Cardano Public Layer]
            LEGN[LEGN Token Contract]
            TRES[DAO Treasury]
            GOV[Governance Modules - Proposals and Voting]
            STAKE[Reputation and Staking Logic]
            REWARD[Reward Distributor]
        end

        subgraph MID[Midnight Links]
            ESC[Private Escrow Contracts]
            METRICS[Private Performance and Delivery Metrics]
        end
    end

    %% Off-Chain and Platform Services
    subgraph OFFCHAIN[Off-Chain and Platform Services]
        MKT[Marketplace and Engagement Engine]
        TLE[Talent Liquidity Engine]
        CGA[Client Growth Autopilot & Demand Engine]
        REPUTE_SVC[Reputation and Scoring Service]
        GOVPORT[Governance and Voting UI]
        DIST[Reward Calculation and Distribution Service]
    end

    %% Enterprise Flows
    ENT -->|Direct Project Spend in Fiat or Stable| MKT
    CGA -->|Qualified Opportunities & Proposals| MKT
    CGA -->|Outbound Offers & Proposals| ENT

    MKT --> ESC
    ESC -->|Milestone Approved| MKT

    %% Payment and Fee Flow
    MKT -->|Consultant Payment (Fiat or Stable)| C
    MKT -->|Platform Fee 2 to 5 Percent| TRES

    %% Metrics and Reputation (Dynamic Reputation Graph v2)
    MKT --> REPUTE_SVC
    TLE --> REPUTE_SVC
    REPUTE_SVC --> METRICS
    REPUTE_SVC --> STAKE

    %% Staking and Reputation
    C -->|Stake LEGN for Reputation and Visibility| STAKE
    GEN -->|Genesis Staking and Participation| STAKE
    PART -->|Partner Staking for Access and Slashing| STAKE

    STAKE --> GOV
    STAKE --> REWARD

    %% Governance Flow
    C -->|Submit Proposals and Vote via UI| GOVPORT
    PART -->|Submit Proposals and Vote via UI| GOVPORT
    GEN -->|Genesis Governance Participation| GOVPORT

    GOVPORT --> GOV
    GOV --> TRES
    GOV --> REWARD

    %% Treasury and Rewards
    TRES -->|Grants, Bounties, Training Subsidies| C
    TRES -->|Ecosystem and Integration Grants| PART
    REWARD -->|LEGN Rewards for Delivery and Governance| C
    REWARD -->|LEGN Rewards for Genesis and Long Term Contribution| GEN
    REWARD -->|LEGN Rewards for Talent Liquidity and Sourcing| TLE
    REWARD -->|LEGN Rewards for Growth and Demand Generation| CGA

    %% Token Utility
    ENT -->|Stake or Hold LEGN for Premium Analytics and Access| LEGN
    PART -->|Stake LEGN for Deeper Integration and Priority| LEGN

    LEGN --> STAKE
    LEGN --> TRES
