graph TD
A[Test Matrix] --> B[reusable-linters.yml]
A --> C[reusable-tests.yml]
A --> D[reusable-cibuildwheel.yml]

    B --> E[flake8 + wemake]
    B --> F[isort + yamllint]
    B --> G[pre-commit hooks]

    C --> H[Unit tests]
    C --> I[Integration tests]
    C --> J[Coverage reports]

    D --> K[Multi-platform wheels]
    D --> L[Smoke tests]
