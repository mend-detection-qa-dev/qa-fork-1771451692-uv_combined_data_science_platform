# Phase 4 Combined Integration Test: Data Science Platform

## Test ID
T-P4-003

## Category
Combined Integration - Data Science with Platform-Specific Dependencies

## Priority
P1

## Description
This fixture simulates a data science/ML platform that combines platform-specific dependencies, heavy scientific packages, GPU support, and pre-release versions.

## Features Combined
1. **Platform Markers** - Different dependencies for Linux/macOS/Windows
2. **GPU Support** - CUDA-specific packages on Linux
3. **Heavy Dependencies** - NumPy, Pandas, PyTorch, TensorFlow
4. **Pre-release Versions** - Testing with alpha/beta packages
5. **Environment Markers** - Python version and platform conditionals
6. **Optional Dependencies** - GPU, visualization, notebook extras

## Real-World Inspiration
Based on patterns from:
- pytorch/pytorch installation requirements
- tensorflow/tensorflow platform-specific builds
- Data science projects with cross-platform support

## Expected Dependency Tree Structure
```
data-science-platform
├── Core Scientific Stack (all platforms)
│   ├── numpy>=1.24.0
│   ├── pandas>=2.0.0
│   ├── scikit-learn>=1.3.0
│   ├── matplotlib>=3.7.0
│   └── scipy>=1.11.0
├── Platform-Specific Dependencies
│   ├── Linux + CUDA
│   │   ├── torch==2.1.0+cu118
│   │   └── nvidia-cudnn-cu11
│   ├── macOS (ARM)
│   │   └── torch==2.1.0
│   └── Windows
│       └── torch==2.1.0+cpu
├── Optional Features
│   ├── gpu group: torch with CUDA support
│   ├── viz group: plotly, seaborn, bokeh
│   └── notebook group: jupyter, ipykernel
└── Development Dependencies
    ├── pytest>=8.0.0
    ├── black>=24.0.0
    ├── isort>=5.12.0
    └── pre-commit>=3.5.0
```

## Test Objectives
1. Verify platform marker evaluation (sys_platform, platform_machine)
2. Verify environment-specific dependencies are correctly filtered
3. Verify heavy dependencies with long transitive chains
4. Verify optional GPU support is properly grouped
5. Verify pre-release version handling
6. Verify cross-platform compatibility

## Success Criteria
- [ ] Platform markers correctly evaluated
- [ ] Linux+CUDA dependencies only on appropriate platform
- [ ] macOS ARM-specific packages identified
- [ ] Windows-specific packages identified
- [ ] Heavy dependencies (torch, tensorflow) with correct versions
- [ ] Optional dependency groups properly separated
- [ ] Total dependency count varies by platform (30-50 packages)

## UV Version Compatibility
- Minimum: UV 0.4.0+ (for enhanced platform support)
- Recommended: UV 0.7.0+

## Files in this Fixture
- `pyproject.toml` - Project configuration with platform markers
- `uv.lock` - Lock file with platform-specific resolutions
- `README.md` - This file

## Usage
```bash
# Generate lock file (platform-specific)
uv lock

# Sync with GPU support
uv sync --extra gpu

# Run dependency tree builder
dependency-tree-builder scan . > output.json
```

## Platform-Specific Testing
This fixture should be tested on multiple platforms:
- Linux x86_64 with CUDA
- Linux x86_64 without CUDA
- macOS ARM64 (Apple Silicon)
- macOS x86_64 (Intel)
- Windows x86_64

## Mend Integration Notes
This fixture tests Mend's ability to:
- Handle platform-specific dependencies
- Scan projects with conditional dependencies
- Report vulnerabilities in platform-specific packages
- Handle heavy scientific dependencies

## Related Tests
- T-P2-009: Platform Markers
- T-P2-010: Pre-release Versions
- T-P2-008: Optional Dependencies