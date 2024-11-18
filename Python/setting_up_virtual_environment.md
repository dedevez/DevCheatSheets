# Setting Up a Virtual Environment

A virtual environment is an isolated Python environment that allows you to install project-specific dependencies without affecting the global Python installation on your system. This ensures that different projects can have different dependencies and versions, making your development process more manageable.

---

## Why Use a Virtual Environment?

- **Dependency Isolation**: Prevent conflicts between packages required by different projects.
- **Ease of Deployment**: Reproduce the environment easily using dependency files like `requirements.txt`.
- **Clean System**: Avoid cluttering your global Python environment.

This guide covers how to set up and activate a virtual environment on **Windows** and **Mac/Linux**.

---

## Step 1: Check Your Python Version

Make sure Python is installed on your system. You can check the version using the command below:

```bash
python --version
```

If Python is installed, you'll see the version number, e.g., `Python 3.10.0`. If not, download and install Python from [python.org](https://www.python.org/downloads/).

> **Note**: Use `python3` instead of `python` on Mac/Linux if `python` points to Python 2.x.

---

## Step 2: Install `virtualenv` (Optional)

The `virtualenv` tool is included with Python's `venv` module in versions 3.3 and above. However, you can install the standalone `virtualenv` package for more control if needed:

```bash
pip install virtualenv
```

---

## Step 3: Create a Virtual Environment

Navigate to your project directory and run the following commands based on your operating system.

### On Mac/Linux:

```bash
python3 -m venv venv
```

### On Windows:

```bash
python -m venv venv
```

- Replace `venv` with your desired virtual environment name, if needed.
- This creates a directory (`venv`) containing the isolated environment.

---

## Step 4: Activate the Virtual Environment

### On Mac/Linux:

```bash
source venv/bin/activate
```

### On Windows (Command Prompt):

```bash
venv\Scripts\activate
```

### On Windows (PowerShell):

```bash
.\venv\Scripts\Activate.ps1
```

Once activated, the environment's name (e.g., `venv`) will appear in your terminal prompt, indicating the virtual environment is active.

---

## Step 5: Install Dependencies

After activating the environment, you can install project-specific dependencies using `pip`:

```bash
pip install <package-name>
```

For example, to install Django:

```bash
pip install django
```

You can list installed packages using:

```bash
pip list
```

---

## Step 6: Save Dependencies to `requirements.txt`

To make your project reproducible, save the installed dependencies to a `requirements.txt` file:

```bash
pip freeze > requirements.txt
```

Other developers can install the same dependencies by running:

```bash
pip install -r requirements.txt
```

---

## Step 7: Deactivate the Virtual Environment

To exit the virtual environment and return to the global Python environment, use:

```bash
deactivate
```

This is important when switching between projects or working with different environments.

---

## Additional Tips

- Always activate your virtual environment before running Python scripts or installing packages for a project.
- If your virtual environment is no longer needed, you can delete the directory (e.g., `venv`).
- Consider using [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) for additional convenience when managing multiple environments.

---

## Troubleshooting

1. **`venv/bin/activate` Not Found**:
   Ensure you're in the correct directory where the virtual environment was created.

2. **Permission Denied on Mac/Linux**:
   If you encounter permission issues, try running the command with `sudo` or ensure the `venv/bin/activate` script has execute permissions:

   ```bash
   chmod +x venv/bin/activate
   ```

3. **PowerShell Execution Policy on Windows**:
   If the `Activate.ps1` script fails, you may need to change the execution policy:
   ```bash
   Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
   ```
