```mermaid
flowchart LR
    A[Recrutador] --> C

    subgraph API[AvaliadorGuia.API]
        CC[Controllers]
        SS[Services]
    end

    C[Candidato] --> API
    API --> DB[(SQLite Database)]
