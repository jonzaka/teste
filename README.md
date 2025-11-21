flowchart LR
    A[Recrutador] --> C
    B[Candidato] --> C

    subgraph API[AvaliadorGuia.API]
        subgraph Controllers
            CC1[CandidateController]
            CC2[JobController]
            CC3[ChallengeController]
            CC4[SessionController]
            CC5[HintController]
        end

        subgraph Services
            S1[CandidateService]
            S2[JobService]
            S3[ChallengeService]
            S4[SessionService]
            S5[HintService]
        end
    end

    C --> API
    API --> DB[(SQLite Database<br/>Candidates, Jobs, Challenges, Sessions, Hints)]
