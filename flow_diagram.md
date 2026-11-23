flowchart LR
    %% Actores
    Dev(["👨‍💻 Desarrollador<br/>(Local / Terminal)"])
    Users(["🌐 Usuarios Finales<br/>(Navegador Web)"])

    %% Control de Versiones y CI/CD
    subgraph GitHub_Ecosystem ["GitHub"]
        Repo[("📦 Repositorio<br/>(Rama main)")]
        Actions["⚙️ GitHub Actions<br/>1. CI: npm test & npm run build<br/>2. CD: Trigger Deploy Hook"]
    end

    %% Nube / Producción
    subgraph Cloud_Prod ["Infraestructura Cloud"]
        Render["🚀 Render Web Service<br/>(Node.js + Express + TS)<br/>Endpoints: /health, /ready, /api/*"]
        Atlas[("🍃 MongoDB Atlas<br/>(Cluster M0 - Base 'tvhub')<br/>Colecciones: channels, users, favorites")]
    end

    %% Flujos de Trabajo
    Dev -- "1. git push origin main" --> Repo
    Repo -- "2. Dispara Workflow" --> Actions
    Actions -- "3. HTTP POST Deploy Hook<br/>(si CI pasa en verde)" --> Render
    Render -. "4. Clona código y compila" .-> Repo

    %% Flujo de Datos y Usuarios
    Dev -. "Seed inicial (import:all-channels)<br/>vía MONGO_URI" .-> Atlas
    Render <-- "5. Lectura/Escritura TLS<br/>(MONGO_URI)" --> Atlas
    Users <-- "6. HTTPS (UI + API REST<br/>+ Cookies HttpOnly)" --> Render

    %% Estilos
    style Dev fill:#1e293b,stroke:#38bdf8,stroke-width:2px,color:#fff
    style Users fill:#1e293b,stroke:#34d399,stroke-width:2px,color:#fff
    style Repo fill:#0f172a,stroke:#94a3b8,stroke-width:2px,color:#fff
    style Actions fill:#1d4ed8,stroke:#60a5fa,stroke-width:2px,color:#fff
    style Render fill:#047857,stroke:#34d399,stroke-width:2px,color:#fff
    style Atlas fill:#065f46,stroke:#10b981,stroke-width:2px,color:#fff
