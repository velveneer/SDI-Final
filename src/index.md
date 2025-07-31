# Software Defined Infrastructure

This repository contains an instruction to setup Hetzner Cloud for the class and all exercises completed in the Software Defined Infrastructure course by Johannes Rödel (jr125).
It includes:

- Source Code: All exercise implementations with configuration files, scripts, and infrastructure-as-code artifacts.

- Documentation: A detailed description of the exercises, workflows, and lessons learned, accessible through a browser-friendly GUI powered by MkDocs.

The purpose of this repository is to provide a reproducible learning environment and an easily navigable documentation platform.
`
The MkDocs documentation for this repository is hosted under :

`https://velveneer.github.io/SDI-Final/`

If you want to run it locally on your machine follow the instructions from the `README.md` in the Github repository.

## Project Structure 

```
├── src/                       # All work files
│   ├── cloud-setup/           # All exercises from the course
│   │   │   ├── docs/          # Hetzner Cloud Setup for the following exercises documentation (Markdown)
│   ├── exercises/             # All exercises from the course
│   │   ├── exercise-XY/       # One exercise
│   │   │   ├── docs/          # Exercise-specific documentation (Markdown)
│   │   │   └── src            # Exercise source code (scripts, configs, IaC, etc.)
│   │   ├── ...                # Additional exercises follow the same structure
│   └── index.md               # Main documentation home (rendered by MkDocs)                    
├── mkdocs.yml                 # MkDocs configuration (theme, navigation, settings)
└── README.md                  # Repository overview & setup instructions (this file)
```

## Getting Started

### Clone This Repository

`git clone https://github.com/velveneer/SDI-Final.git`

`cd SDI-Final`