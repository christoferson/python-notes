# Python Guide for Windows - Common Commands

## Table of Contents
- [Python Installation Management](#python-installation-management)
- [Virtual Environments](#virtual-environments)
- [Package Management with pip](#package-management-with-pip)
- [Complete Workflow Examples](#complete-workflow-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Python Installation Management

### List all Python installations
```bash
py -0
```
**Output example:**
```
Installed Pythons found by py Launcher for Windows
 -3.8-64        C:\Python38\python.exe *
 -3.9-64        C:\Python39\python.exe
 -3.12-64       C:\Users\XXX\AppData\Local\Programs\Python\Python312\python.exe
```

The `*` indicates the default Python version.

### Check current Python version
```bash
python --version
```

### Check Python path
```bash
where python
```

### Use a specific Python version

```bash
# Run a script with Python 3.12
py -3.12 script.py

# Run a command with Python 3.12
py -3.12 -c "print('Hello, World!')"

# Open Python 3.12 interactive shell
py -3.12

# Use pip with specific Python version
py -3.12 -m pip install package-name
```

### Check Python path and version inside Python
```python
import sys
print(sys.executable)  # prints the path to the Python executable
print(sys.version)     # prints the Python version
```

---

## Virtual Environments

### What is a Virtual Environment?
A virtual environment is an isolated Python environment that allows you to:
- Install packages without affecting other projects
- Use different package versions for different projects
- Keep your system Python clean
- Easily share project dependencies with others

### Why Use Virtual Environments?
❌ **Without virtual environment:**
- All packages installed globally
- Package conflicts between projects
- Hard to reproduce project setup
- Risk of breaking system Python

✅ **With virtual environment:**
- Each project has its own packages
- No conflicts between projects
- Easy to share requirements
- Clean and organized

---

## Creating and Using Virtual Environments

### Step 1: Create a virtual environment

```bash
# Using default Python
python -m venv .venv

# Using specific Python version (recommended)
py -3.12 -m venv .venv
```

**What this does:**
- Creates a folder named `.venv` in your current directory
- Copies Python executable into `.venv`
- Creates a space for installing packages

**Common folder names:**
- `.venv` (recommended - hidden folder)
- `venv`
- `env`

### Step 2: Activate the virtual environment

```bash
.venv\Scripts\activate
```

**After activation, your prompt will change:**
```bash
C:\codes\my-project> .venv\Scripts\activate
(.venv) C:\codes\my-project>
```

The `(.venv)` prefix shows that the virtual environment is active.

### Step 3: Work in the virtual environment

Once activated, any `python` or `pip` commands will use the virtual environment:

```bash
# Check which Python you're using (should point to .venv)
where python

# Install packages (will install only in this virtual environment)
pip install requests

# Run your scripts
python script.py
```

### Step 4: Deactivate the virtual environment

```bash
deactivate
```

Your prompt will return to normal:
```bash
(.venv) C:\codes\my-project> deactivate
C:\codes\my-project>
```

### Delete a virtual environment

Simply delete the `.venv` folder:

```bash
rmdir /s .venv
```

Or delete it manually in File Explorer.

---

## Package Management with pip

### Install a package
```bash
pip install package-name

# Examples:
pip install requests
pip install pandas
pip install numpy
```

### Install a specific version
```bash
pip install package-name==1.2.3

# Example:
pip install requests==2.28.0
```

### Install multiple packages
```bash
pip install requests pandas numpy matplotlib
```

### Upgrade a package
```bash
pip install --upgrade package-name

# Example:
pip install --upgrade requests
```

### Uninstall a package
```bash
pip uninstall package-name

# Example:
pip uninstall requests
```

### List all installed packages
```bash
pip list
```

**Output example:**
```
Package    Version
---------- -------
pip        23.0.1
requests   2.28.2
numpy      1.24.2
```

### Show package information
```bash
pip show package-name

# Example:
pip show requests
```

**Output example:**
```
Name: requests
Version: 2.28.2
Summary: Python HTTP for Humans.
Home-page: https://requests.readthedocs.io
Author: Kenneth Reitz
License: Apache 2.0
Location: C:\codes\my-project\.venv\Lib\site-packages
Requires: charset-normalizer, idna, urllib3, certifi
Required-by:
```

### Save installed packages to a file
```bash
pip freeze > requirements.txt
```

This creates a `requirements.txt` file with all installed packages and their versions:
```
requests==2.28.2
numpy==1.24.2
pandas==1.5.3
```

### Install packages from requirements.txt
```bash
pip install -r requirements.txt
```

This installs all packages listed in `requirements.txt`.

---

## Complete Workflow Examples

### Example 1: Starting a New Project

```bash
# 1. Create project directory
mkdir my-project
cd my-project

# 2. Create virtual environment with Python 3.12
py -3.12 -m venv .venv

# 3. Activate virtual environment
.venv\Scripts\activate

# 4. Verify you're using the correct Python
where python
python --version

# 5. Install packages you need
pip install requests pandas numpy

# 6. Create your Python script
# (create script.py with your code)

# 7. Run your script
python script.py

# 8. Save your dependencies
pip freeze > requirements.txt

# 9. Deactivate when done
deactivate
```

### Example 2: Working on an Existing Project

```bash
# 1. Navigate to project directory
cd existing-project

# 2. Create virtual environment
py -3.12 -m venv .venv

# 3. Activate virtual environment
.venv\Scripts\activate

# 4. Install dependencies from requirements.txt
pip install -r requirements.txt

# 5. Run the project
python main.py

# 6. Deactivate when done
deactivate
```

### Example 3: Daily Workflow

```bash
# Morning: Start working
cd my-project
.venv\Scripts\activate

# Work on your code...
python script.py

# Install new package if needed
pip install new-package

# Update requirements.txt
pip freeze > requirements.txt

# Evening: Done for the day
deactivate
```

---

## Best Practices

### ✅ DO:

1. **Always use virtual environments**
   ```bash
   # Create venv for every project
   py -3.12 -m venv .venv
   ```

2. **Use specific Python versions**
   ```bash
   # Good
   py -3.12 -m venv .venv

   # Avoid
   python -m venv .venv
   ```

3. **Keep requirements.txt updated**
   ```bash
   pip freeze > requirements.txt
   ```

4. **Name your virtual environment `.venv`**
   - Standard convention
   - Hidden folder (starts with `.`)
   - Easy to add to `.gitignore`

5. **Activate virtual environment before working**
   ```bash
   .venv\Scripts\activate
   ```

### ❌ DON'T:

1. **Don't install packages globally**
   ```bash
   # Bad - installs globally
   pip install requests

   # Good - activate venv first
   .venv\Scripts\activate
   pip install requests
   ```

2. **Don't commit virtual environments to Git**
   - Add `.venv/` to `.gitignore`
   - Only commit `requirements.txt`

3. **Don't mix Python versions in same project**
   - Choose one Python version per project
   - Stick with it

---

## Project Structure

### Recommended folder structure:

```
my-project/
│
├── .venv/                  # Virtual environment (don't commit)
├── .gitignore             # Git ignore file
├── requirements.txt       # Package dependencies
├── README.md             # Project documentation
├── main.py               # Main script
└── src/                  # Source code folder
    ├── __init__.py
    └── module.py
```

### Example `.gitignore` for Python projects:

```gitignore
# Virtual environments
.venv/
venv/
env/

# Python cache
__pycache__/
*.py[cod]
*$py.class

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

---

## Quick Reference Cheat Sheet

### Python Launcher
```bash
py -0                          # List all Python installations
py -3.12                       # Use Python 3.12
py -3.12 script.py             # Run script with Python 3.12
py -3.12 -m pip install pkg    # Install package with Python 3.12
```

### Virtual Environment
```bash
# Create
py -3.12 -m venv .venv

# Activate
.venv\Scripts\activate

# Deactivate
deactivate

# Delete
rmdir /s .venv
```

### Package Management
```bash
pip install package-name              # Install package
pip install package-name==1.2.3       # Install specific version
pip install --upgrade package-name    # Upgrade package
pip uninstall package-name            # Uninstall package
pip list                              # List installed packages
pip show package-name                 # Show package info
pip freeze > requirements.txt         # Save dependencies
pip install -r requirements.txt       # Install from requirements.txt
```

### Check Environment
```bash
python --version         # Check Python version
where python            # Check Python path
pip list                # List installed packages
pip show package-name   # Show package details
```

---

## Troubleshooting

### Issue: "python is not recognized as an internal or external command"

**Solution:**
- Use `py` launcher instead: `py -3.12`
- Or add Python to PATH during installation

### Issue: Virtual environment not activating

**Symptoms:**
```bash
C:\codes\my-project> .venv\Scripts\activate
# No (.venv) prefix appears
```

**Solution:**
Enable script execution in PowerShell:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Then try activating again.

### Issue: "Access is denied" when creating virtual environment

**Solution:**
- Run Command Prompt or PowerShell as Administrator
- Or create the virtual environment in a folder where you have write permissions

### Issue: Wrong Python version in virtual environment

**Problem:**
```bash
(.venv) C:\codes> python --version
Python 3.8.10  # But you wanted 3.12!
```

**Solution:**
Delete the virtual environment and recreate with specific version:
```bash
deactivate
rmdir /s .venv
py -3.12 -m venv .venv
.venv\Scripts\activate
python --version  # Should now show 3.12
```

### Issue: Package not found after installation

**Problem:**
```bash
pip install requests
# Later...
python script.py
# ModuleNotFoundError: No module named 'requests'
```

**Solution:**
Make sure virtual environment is activated:
```bash
.venv\Scripts\activate
pip list  # Check if package is installed
pip install requests  # Install again if needed
```

### Issue: Multiple Python versions causing confusion

**Solution:**
Always use `py -3.12` to specify exact version:
```bash
# Check which Python versions you have
py -0

# Use specific version
py -3.12 -m venv .venv
```

---

## Common Scenarios

### Scenario 1: Sharing your project with a teammate

**You:**
```bash
# Save your dependencies
pip freeze > requirements.txt

# Commit to Git (don't commit .venv folder!)
git add requirements.txt
git commit -m "Add requirements"
git push
```

**Your teammate:**
```bash
# Clone the project
git clone <repository-url>
cd project

# Create virtual environment
py -3.12 -m venv .venv

# Activate
.venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Start working!
python main.py
```

### Scenario 2: Upgrading Python version for a project

```bash
# 1. Note your current packages
pip freeze > old_requirements.txt

# 2. Deactivate and delete old virtual environment
deactivate
rmdir /s .venv

# 3. Create new virtual environment with new Python version
py -3.12 -m venv .venv

# 4. Activate new virtual environment
.venv\Scripts\activate

# 5. Install packages
pip install -r old_requirements.txt

# 6. Test your project
python main.py

# 7. Update requirements.txt if needed
pip freeze > requirements.txt
```

### Scenario 3: Working on multiple projects

```bash
# Project 1
cd C:\codes\project1
.venv\Scripts\activate
python main.py
deactivate

# Project 2
cd C:\codes\project2
.venv\Scripts\activate
python app.py
deactivate
```

Each project has its own isolated environment!

---

## Additional Resources

- [Python Official Documentation](https://docs.python.org/)
- [pip Documentation](https://pip.pypa.io/)
- [Virtual Environments Guide](https://docs.python.org/3/tutorial/venv.html)
- [Python Windows FAQ](https://docs.python.org/3/faq/windows.html)

---
