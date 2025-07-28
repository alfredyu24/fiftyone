# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

FiftyOne is an open-source computer vision dataset curation, analysis, and visualization platform. The project consists of:

- **Python Core**: Main library (`fiftyone/` package) providing dataset management, visualization, model evaluation, and integrations
- **React App**: TypeScript/React frontend (`app/` directory) for the interactive web application using yarn workspaces
- **Enterprise Features**: Additional functionality for team collaboration and cloud deployments

## Development Commands

### Python Core
```bash
# Install from source (required for development)
bash install.bash -d

# Run tests
pytest  # All tests
pytest tests/unittests/  # Unit tests only
pytest tests/intensive/  # Integration tests
pytest tests/isolated/   # Isolated tests

# Format Python code
black .

# Code quality checks
pylint fiftyone/
```

### Frontend App
```bash
cd app/

# Install dependencies
yarn

# Development server (with Python backend)
yarn dev:wpy  # Runs both frontend and Python backend concurrently
yarn dev      # Frontend only
yarn dev:py   # Python backend only

# Build for production
yarn build

# Run tests
yarn test
yarn test-ui  # With UI coverage

# Format/lint
yarn lint:prettify
yarn check

# Generate documentation
yarn doc

# Generate GraphQL schema
yarn gen:schema

# Compile Relay queries
yarn compile
```

### Build and Release
```bash
# Build everything (app + Python package)
make python

# Docker build
make docker

# Clean build artifacts
make clean
```

## High-Level Architecture

### Python Package Structure (`fiftyone/`)

- **core/**: Main dataset and sample management, views, aggregations, stages
  - `dataset.py`, `sample.py`, `view.py`: Core data abstractions
  - `session/`: App session management and client communication
  - `odm/`: Object Document Mapper for MongoDB integration
  - `aggregations.py`, `stages.py`: Data query and transformation pipeline
- **server/**: GraphQL API server, routes, and data loading
  - `main.py`: Server entry point
  - `routes/`: HTTP endpoints for media, events, aggregations
  - GraphQL schema for frontend communication
- **operators/**: Plugin system for custom operations and workflows
- **plugins/**: Plugin management and extensibility framework
- **utils/**: Dataset format parsers, integrations (COCO, CVAT, etc.)
- **zoo/**: Pre-built datasets and models

### Frontend App Structure (`app/`)

- **packages/**: Yarn workspace with modular React packages
  - `app/`: Main application shell and routing
  - `core/`: Dataset display, samples grid, and core UI
  - `looker/`: Media viewers (image, video, 3D) with overlay rendering
  - `looker-3d/`: 3D point cloud and mesh visualization
  - `embeddings/`: Interactive embeddings plots and visualization
  - `operators/`: Operator execution UI and plugin panels
  - `state/`: Recoil state management and hooks
  - `relay/`: GraphQL integration with Relay
  - `components/`: Shared UI components library

### Key Technologies

- **Backend**: Python, MongoDB, GraphQL (Strawberry), Hypercorn
- **Frontend**: React, TypeScript, Recoil, Relay, Vite, Three.js
- **Data Processing**: NumPy, PyTorch integrations, computer vision libraries
- **Database**: MongoDB for datasets, samples, and metadata storage

## Testing

### Python Tests
- Unit tests: `tests/unittests/`
- Integration tests: `tests/intensive/` 
- Isolated tests: `tests/isolated/`
- Benchmarking: `tests/benchmarking/`
- End-to-end: `e2e-pw/` (Playwright)

### Frontend Tests
- Component tests with Vitest
- End-to-end tests with Playwright in `e2e-pw/`

### Running Specific Tests
```bash
# Single test file
pytest tests/unittests/dataset_tests.py

# Specific test method
pytest tests/unittests/dataset_tests.py::test_method_name

# Run with coverage
pytest --cov=fiftyone

# Frontend component tests
cd app && yarn test

# E2E tests
cd e2e-pw && npm test
```

## Development Workflow

1. **Source Installation**: Always use `bash install.bash -d` for development
2. **Branch**: Work off `develop` branch (main development branch)
3. **Frontend Development**: Use `yarn dev:wpy` to run both frontend and backend
4. **Testing**: Run relevant test suites before submitting changes
5. **Code Quality**: Use Black for Python formatting, Prettier for TypeScript

## Key Entry Points

- **Python CLI**: `fiftyone/core/cli.py`
- **Python API**: `fiftyone/__init__.py` and `fiftyone/__public__.py`
- **Server**: `fiftyone/server/main.py`
- **Frontend**: `app/packages/app/src/index.tsx`
- **Dataset Loading**: `fiftyone/zoo/datasets/`
- **Model Integration**: `fiftyone/zoo/models/`

## Important Notes

- The app requires both Python backend and React frontend running
- MongoDB is required for dataset storage
- Many integrations require additional dependencies (install with extras)
- Use virtual environments for development
- GraphQL schema changes require running `yarn gen:schema`