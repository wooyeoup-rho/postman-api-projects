# Cafe & Wi-Fi API - Automated Testing

![Tests](https://github.com/wooyeoup-rho/postman-api-projects/actions/workflows/api-tests.yml/badge.svg)

A REST API for managing cafe data with automated testing using **Postman**, **Newman**, and CI/CD Integration with **Docker** and **GitHub Actions**.

## Architecture

The project demonstrates a **multi-repository testing architecture** using Docker & GitHub Actions.

- **API Repository**: https://github.com/wooyeoup-rho/python-projects/tree/main/flask/cafe-api
- **Test Repository**: https://github.com/wooyeoup-rho/postman-api-projects/tree/main/cafe-api-tests

## Features

- RESTful API for CRUD operations on cafe data.
- Comprehensive test suite with 13 test scenarios.
- Containerized testing environment with Docker Compose
- Automated CI/CD pipeline with GitHub Actions
- Cross-repository integration testing

## Running Tests Locally

```bash
# Clone both repositories
git clone https://github.com/wooyeoup-rho/python-projects.git
git clone https://github.com/wooyeoup-rho/postman-api-projects.git

# Navigate to test directory
cd postman-api-projects/cafe-api-tests

# Run tests
docker-compose up --build --abort-on-container-exit
```

## CI/CD Pipeline

### GitHub Actions

[Automated testing pipeline](../.github/workflows/api-tests.yml) that triggers on every push to `main` and performs:
1. **Multi-Repository Checkout** - Clones both API and test repos
2. **Dynamic Configuration** - Generates CI-specific docker-compose file with adjusted paths
3. **Container Build & Test** - Builds API image and runs Newman tests
4. **Result Reporting** - Captures logs and reports pass/fail status

```yaml
# .github/workflows/api-tests.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout test repo
        uses: actions/checkout@v3
        with:
          path: test-repo
      
      - name: Checkout API repo
        uses: actions/checkout@v3
        with:
          repository: wooyeoup-rho/python-projects
          path: api-repo
      
      # Dynamic path resolution for CI environment
      - name: Create docker-compose for CI
        run: |
          cat > docker-compose.ci.yml << 'EOF'
          services:
            api:
              build:
                context: ../../api-repo/flask/cafe-api
          EOF
```

## Test Coverage

- ✅ GET all cafes
- ✅ GET random cafe
- ✅ Search by location
- ✅ POST new cafe
- ✅ PATCH update cafe price
- ✅ DELETE cafe (with authentication)
- ✅ Error handling (404s, missing parameters)
- ✅ Data validation

## Technologies

- **API**: Python, Flask, SQLAlchemy, SQLite
- **Testing**: Postman, Newman
- **Containerization**: Docker, Docker Compose
- **CI/CD**: GitHub Actions
- **Version Control**: Git (multi-repository strategy)