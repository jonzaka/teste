flowchart LR
    %% ==== Atores ====
    R[Recrutador]:::actor
    U[Candidato]:::actor

    %% ==== API ====
    subgraph API[AvaliadorGuia.API (ASP.NET Core)]
        
        %% --- Segurança / Autenticação ---
        subgraph SEC[Security]
            AuthCtrl[AuthController]
            AuthSvc[AuthService (JWT)]
        end

        %% --- Camada de Controllers ---
        subgraph Ctrls[Controllers]
            CCand[CandidateController]
            CJob[JobController]
            CChal[ChallengeController]
            CSess[SessionController]
            CHint[HintController]
        end

        %% --- Camada de Services ---
        subgraph Svcs[Services]
            SCand[CandidateService]
            SJob[JobService]
            SChal[ChallengeService]
            SSess[SessionService]
            SHint[HintService]
        end
    end

    %% ==== Banco de Dados ====
    DB[(SQLite Database<br/>Candidates, Jobs, Challenges, Sessions, Hints)]:::db

    %% ==== Fluxo de Autenticação ====
    R -->|Login / Token| AuthCtrl
    U -->|Login / Token| AuthCtrl
    AuthCtrl --> AuthSvc
    AuthSvc --> DB

    %% ==== Fluxo Recrutador ====
    R -->|Gerenciar vagas e desafios| CJob
    R -->|Criar desafios técnicos| CChal
    R -->|Consultar candidatos| CCand
    R -->|Consultar sessões| CSess

    %% ==== Fluxo Candidato ====
    U -->|Cadastro / Atualização| CCand
    U -->|Iniciar sessão de entrevista| CSess
    U -->|Pedir dicas ao Tutor| CHint

    %% ==== Controllers -> Services ====
    CCand --> SCand
    CJob --> SJob
    CChal --> SChal
    CSess --> SSess
    CHint --> SHint

    %% ==== Services -> DB ====
    SCand --> DB
    SJob --> DB
    SChal --> DB
    SSess --> DB
    SHint --> DB

    classDef actor fill:#dae8fc,stroke:#6c8ebf,stroke-width:1px;
    classDef db fill:#e1d5e7,stroke:#9673a6,stroke-width:1px;
