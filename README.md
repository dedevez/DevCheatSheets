
# DevCheatSheets

Welcome to **DevCheatSheets** – your one-stop repository for interactive, step-by-step cheat sheets and guides on common software development tasks. This repository is designed to help developers, from beginners to experts, quickly set up and explore projects, tools, and workflows with ease.

Each cheat sheet is a **Jupyter Notebook** that combines explanations, code snippets, and live executable cells. Whether you're starting a new project, managing your workflow, or exploring best practices in software development, these guides are here to make your life easier!

---

## Table of Contents

1. [Features](#features)  
2. [Getting Started](#getting-started)  
   - [Option 1: Jupyter Notebook Installation](#option-1-jupyter-notebook-installation)  
   - [Option 2: VS Code Setup](#option-2-vs-code-setup)  
   - [Option 3: IntelliJ IDEA Setup](#option-3-intellij-idea-setup)  
3. [Repository Structure](#repository-structure)  
4. [Using the Cheat Sheets](#using-the-cheat-sheets)  
5. [Contribution Guidelines](#contribution-guidelines)  
6. [Future Expansion](#future-expansion)  
7. [License](#license)  

---

## Features

- Interactive guides for popular software development activities.  
- Hands-on Jupyter Notebooks with runnable code examples.  
- Guides for setting up and using tools like:
  - Django
  - React
  - Docker
  - GitHub Actions CI/CD Pipelines
  - Git
  - [Additional topics coming soon!]  

- Easy-to-follow explanations in Markdown cells combined with executable Python and shell code.  
- Beginner-friendly setup instructions, with plans to grow into a resource for developers at all levels.

---

## Getting Started

To use this repository, you’ll need to set up a working environment that supports Jupyter Notebooks. Below are three options to choose from based on your preferred development setup.

### Option 1: Jupyter Notebook Installation

1. Install Jupyter using pip:
   ```bash
   pip install notebook
   ```
2. Start the Jupyter Notebook server:
   ```bash
   jupyter notebook
   ```
3. Open the desired `.ipynb` file from this repository.

---

### Option 2: VS Code Setup

1. Download and install [Visual Studio Code](https://code.visualstudio.com/).  
2. Install the **Jupyter** extension from the Extensions Marketplace.  
3. Open any `.ipynb` file in VS Code to edit and run the cheat sheets.

---

### Option 3: IntelliJ IDEA Setup

1. Download and install [IntelliJ IDEA](https://www.jetbrains.com/idea/).  
2. Install the **Jupyter Notebooks Plugin** from the Plugins Marketplace.  
3. Configure a Python interpreter in IntelliJ (if not already done).  
4. Open the `.ipynb` file and start editing interactively.

---

## Repository Structure

The repository is organized into topic-based directories. Each directory contains one or more Jupyter Notebooks.

```
├── README.md
├── django/
│   ├── 01_setting_up_django.ipynb
│   └── 02_django_advanced_models.ipynb
├── react/
│   ├── 01_setting_up_react.ipynb
│   └── 02_state_management_with_redux.ipynb
├── docker/
│   ├── 01_setting_up_docker.ipynb
│   └── 02_dockerizing_apps.ipynb
├── ci_cd/
│   ├── 01_github_actions_basics.ipynb
│   └── 02_deploying_with_docker.ipynb
├── git/
│   ├── 01_setting_up_git.ipynb
│   └── 02_git_branching_and_merging.ipynb
```

---

## Using the Cheat Sheets

1. Clone the repository:
   ```bash
   git clone https://github.com/dedevez/DevCheatSheets.git
   cd DevCheatSheets
   ```

2. Open the `.ipynb` file of interest in your preferred environment (Jupyter, VS Code, or IntelliJ IDEA).

3. Follow along with the instructions and execute the cells to see live results.

---

## Contribution Guidelines

We welcome contributions to this repository! If you have a cheat sheet idea or want to improve an existing guide, follow these steps:

### Naming Convention
- Use lowercase letters and underscores to separate words.  
- Prefix filenames with a number for ordering, if needed.  
- Include the topic and specific focus in the filename.

**Examples:**  
- `01_setting_up_django.ipynb`  
- `02_django_advanced_models.ipynb`

### Submission Standards
- **Python Version:** Use Python 3.9 or later.  
- **Markdown Cells:**  
  - Write clear and concise explanations.  
  - Use headers, bullet points, and code fences for clarity.  
- **Code Style:**  
  - Follow PEP 8 for Python code.  
  - Include comments and docstrings in code cells.  
- **Interactive Examples:** Include runnable code snippets with meaningful outputs.  
- **Metadata:** Strip unnecessary metadata from notebooks using tools like [nbstripout](https://github.com/kynan/nbstripout) to avoid conflicts in pull requests.

### Steps to Contribute
1. Fork this repository.  
2. Create a new branch:  
   ```bash
   git checkout -b feature/your-cheat-sheet-name
   ```
3. Add your cheat sheet (as a Jupyter Notebook) in the appropriate directory or create a new one.  
4. Submit a pull request explaining your contribution.

---

## Future Expansion

We plan to integrate auto-rendering tools like [Binder](https://mybinder.org/) to allow users to interact with the notebooks directly in their browsers. This feature will be added as the repository evolves.

---

## License

This repository is licensed under the MIT License. Feel free to use and share the content, but please provide proper attribution.

---

### Let me know if you’d like further tweaks or additions!
