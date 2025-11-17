# Legeon Platform Architecture Diagram (Level 2)

```mermaid
flowchart LR
    %% Users
    subgraph Users
        UC[Consultants]
        UE[Enterprises]
        UP[Partners - MSPs, Training, SIs]
    end

    %% Frontend Layer
    subgraph FE[Frontend Layer]
        ONB[Onboarding and Profile dApp]
        ENTGW[Enterprise Gateway UI]
        GOVUI[Governance and Analytics UI]
        DISC[Discord Bot and Community Interfaces]
    end

    UC --> ONB
    UE --> ENTGW
    UP --> ENTGW
    UC --> DISC

    %% Backend and API Layer
    subgraph BE[Backend and API Layer]
        API[API Gateway]
        AUTH[Auth and Session Service]
        CV[Credential Verification Service]
        PG[Proof Generator Service]
        MATCH[Marketplace and Matching Service]
        ENG[Engagement and Payment Orchestrator]
        TLE[Talent Liquidity Engine]
        CGA[Client Growth Autopilot Service]
    end

    ONB --> API
    ENTGW --> API
    GOVUI --> API
    DISC --> AUTH

    API --> AUTH
    API --> CV
    API --> MATCH
    API --> ENG
    API --> PG
    API --> TLE
    API --> CGA

    %% Data and Storage Layer
    subgraph DATA[Data and Storage Layer]
        DB[(Postgres DB<br/>Profiles, Credentials, Reputation, Proof Metadata)]
        BLOB[(Encrypted Blob Storage<br/>Resumes, Cert PDFs, Contracts)]
        VEC[(Vector Store or pgvector<br/>Embeddings and Semantic Index)]
        LOGS[(Event and Audit Logs)]
    end

    CV --> DB
    CV --> BLOB
    PG --> DB
    PG --> LOGS
    MATCH --> DB
    MATCH --> VEC
    ENG --> DB
    ENG --> LOGS
    TLE --> DB
    TLE --> LOGS
    CGA --> LOGS

    DB <--> VEC

    %% AI Layer
    subgraph AI[AI and Copilot Layer]
        AIMATCH[AI Sourcing and Matching Engine]
        AIPARSE[AI Credential Parsing and Enrichment]
        AIGOV[AI Governance Insights and Policy Simulation]
        AIIP[AI Knowledge and IP Extractor]
        AIREP[AI Reputation and Scoring Engine]
        AIGROW[AI Demand and Opportunity Scoping]
    end

    CV --> AIPARSE
    AIPARSE --> DB
    AIPARSE --> VEC

    MATCH --> AIMATCH
    AIMATCH --> VEC
    AIMATCH --> DB
    AIMATCH --> TLE

    ENG --> AIGOV
    AIGOV --> LOGS

    AIIP --> BLOB
    AIIP --> DB

    ENG --> AIREP
    MATCH --> AIREP
    AIREP --> DB
    AIREP --> LOGS

    AIGROW --> CGA
    AIGROW --> MATCH

    %% Blockchain Layer
    subgraph BC[Blockchain Layer]
        subgraph MID[Midnight Confidential Layer]
            VCE[Verifiable Credential Engine]
            ESC[Private Escrow Contracts]
            CAR[Consent and Audit Registry]
        end

        subgraph CARD[Cardano Public Layer]
            PNFT[ProfileNFT Contract]
            LEGN[LEGN Token and Staking]
            GOV[DAO Governance Modules]
        end
    end

    PG --> VCE
    PG --> CAR
    ENG --> ESC

    DB --> VCE
    LOGS --> CAR

    VCE --> MIDTX[Midnight Transactions]
    ESC --> MIDTX
    CAR --> MIDTX

    MIDTX --> PNFT
    MIDTX --> GOV

    ENG --> PNFT
    GOVUI --> GOV
    GOV --> LEGN

    %% Enterprise and Partner Integrations
    subgraph EXT[Enterprise and Partner Integrations]
        SAPINT[SAP Integrations - Ariba, Fieldglass, S4HANA, BTP]
        EORINT[EOR and Compliance Providers]
        PARTSYS[Partner and VMS Systems]
    end

    ENTGW --> SAPINT
    ENG --> SAPINT
    CGA --> SAPINT
    ENTGW --> EORINT
    ENTGW --> PARTSYS
    CGA --> PARTSYS
