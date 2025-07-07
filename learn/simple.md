```md
graph TD
A[GitHub Events] --> B[ci-cd.yml]
B --> C[pre-setup]
C --> D[build-changelog]
C --> E[build-src]
C --> F[build-wheels]

    E --> G[Source Distribution]
    F --> H[macOS Wheels]
    F --> I[Linux Wheels]

    D --> J[towncrier]
    J --> K[changelog patch]
    K --> E

    G --> L[PyPI Upload]
    H --> L
    I --> L
```
