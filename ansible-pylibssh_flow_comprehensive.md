```mermaid
graph LR
    %% Core Components Flow
    subgraph "Core SSH Components"
        A[Session] --> B[Channel]
        A --> C[SFTP]
        A --> D[SCP]
        E[Errors] --> A
        E --> B
        E --> C
        E --> D
    end

    subgraph "libssh C Library Interface"
        F[libssh.pxd] --> A
        F --> B
        F --> C
        F --> D
        G[sftp.pxd] --> C
        H[callbacks.pxd] --> A
        H --> B
    end

    subgraph "Test Coverage"
        I[session_test.py] --> A
        J[channel_test.py] --> B
        K[sftp_test.py] --> C
        L[scp_test.py] --> D
        M[version_test.py] --> N[_libssh_version]
        O[sshd_test.py] --> A
        O --> B
    end

    subgraph "Build Process"
        P[pyproject.toml] --> Q[PEP517 Backend]
        Q --> R[Cython Compilation]
        R --> S[.pyx → .c → .so]
        T[tox.ini] --> U[Test Execution]
        V[pytest.ini] --> U
    end

    %% Data Flow
    W[User Code] --> A
    A --> X[SSH Connection]
    X --> Y[Execute Commands]
    Y --> B
    B --> Z[Command Output]

    A --> AA[File Transfer]
    AA --> C
    AA --> D
    C --> BB[SFTP Operations]
    D --> CC[SCP Operations]

    %% Error Handling
    X -.-> E
    Y -.-> E
    AA -.-> E
    BB -.-> E
    CC -.-> E

    %% Styling
    classDef core fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef clib fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef test fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef build fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef flow fill:#fce4ec,stroke:#c2185b,stroke-width:2px

    class A,B,C,D,E core
    class F,G,H clib
    class I,J,K,L,M,O test
    class P,Q,R,S,T,U,V build
    class W,X,Y,Z,AA,BB,CC flow
```

# 🗂️ ARCHITECTURE.md — Codebase Structure & Design Insights

## 📌 1️⃣ Main Codebase Structure

**Overview:**

- **Core Cython Extensions:**

  - Modules: `session`, `channel`, `sftp`, `scp`, `errors`
  - Provides performant SSH functionality via Cython.

- **Build System:**

  - Custom **PEP517 backend** for Cython compilation and libssh integration.

- **Test Organization:**

  - **Unit tests** mirroring source structure.
  - **Integration tests** for end-to-end SSH interactions.

- **Configuration:**
  - Extensive linting, formatting, and build configuration for reproducibility and style consistency.

---

## 🔗 2️⃣ Component Relationships & Data Flow

**How it works:**

- `Session` is the **central component** managing SSH connections.
- `Channel`, `SFTP`, and `SCP` depend on `Session` for all SSH operations.
- `Errors` module provides **exception handling** across all components.
- `libssh` C library interfaces through `.pxd` header files to expose C functions to Cython.

**Diagram (conceptual):**

```
Session
 ├─> Channel
 ├─> SFTP
 ├─> SCP
 └─> Errors
```

---

## ✅ 3️⃣ Testing Strategy

**Test Layers:**

- **Unit Tests:**

  - Test individual components in isolation with **mocked dependencies**.

- **Integration Tests:**

  - Full interactions with a **live SSH server** or containerized service.

- **Smoke Tests:**

  - Quick post-build validation to catch obvious breakage.

- **Test Infrastructure:**
  - Shared fixtures, SSH service utilities, test configuration files.

---

## 🔑 Key Architecture Insights

- **Cython-Heavy:**

  - Core functionality lives in `.pyx` files compiled to C extensions for performance.

- **Modular Design:**

  - Clear separation of concerns between SSH operations: session, channel, sftp, scp.

- **Custom Build:**

  - Custom **PEP517 backend** handles the multi-stage Cython build with libssh dependencies.

- **Comprehensive Testing:**

  - Each component is covered by unit tests plus real-world integration tests.

- **Quality Assurance:**
  - **Linting:** `flake8`, `wemake-python-styleguide`
  - **Formatting:** `isort`, `black`
  - **Automation:** `pre-commit` hooks, coverage reports for reliability.

---

**Designed for maintainability, performance, and cross-platform builds.** 🚀
