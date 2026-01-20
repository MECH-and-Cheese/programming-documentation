---
password: "V9!r@7zL$3qF&xP2#bM8w"
---

# Installing MKDocs on School PC's

*Last Updated November 18, 2025*

## Introduction

Many school computers are configured with strict security settings, which limits the ability of students to install software freely. Because of these restrictions, the standard installation process for MKDocs may not work on these machines.

This guide provides a step-by-step approach to installing and using MKDocs on school computers, ensuring that even with limited permissions, students can still set up a functional documentation environment. By following this section, you will learn how to work around common restrictions, safely configure MKDocs, and begin building and serving documentation without requiring administrative privileges.

Whether you are new to MKDocs or just need a solution for restricted school systems, this section will cover the necessary steps and best practices for getting started efficiently and securely.

---

## Installing Python

Most school computers do not come with Python pre-installed. However, it is still possible to install and use Python even when you have limited permissions. The following instructions will guide you through installing Python in a way that does not require administrative privileges.

### Step 1: Download the Python Installer

1. Open a web browser and go to the official Python website: [https://www.python.org/downloads/](https://www.python.org/downloads/)
2. Click the **Download Python Install Manager** button located on the top of the page.

> **Tip:** On school computers, avoid using the Microsoft Store version, as it may require admin privileges. Use the installer instead from python.org.

### Step 2: Run the Installer

1. Locate the download `.msix` installer file, usually in the downloads folder
2. Double-click the installer to run it
3. In the installer, simply click the Install Python Button and launch the instalelr
4. It will open command prompt, and when it prompts you to type either `y` or `n`, type `y` then click `enter`, except for the one that asks about getting help (should be the last)
5. Python should now be installed

### Step 3: Verify the Installation

1. Open *PowerShell*
2. Type the following command:

```bash
python --version
```

You should see the installed Python version, for example:

```nginx
Python 3.12.0
```

3. Additionally, check that `pip` (Python's package manager) is installed:

```bash
pip --version
```

If both commands return version numbers, Python has been installed successfully.

---

## Installing MKDocs

There are two possible ways to install MKDocs. Try option A first, and if option A does not work, try option B.

### Option A: Install a Local Version

Once Python is installed, you can install MKDocs without admin privileges using `pip`:

```bash
pip install --user mkdocs
```

The `--user` flag installs MKDocs into your personal user directory, bypassing the need for administrative permissions.

After installation, verify MKDocs is installed correctly:

```bash
mkdocs --version
```

You should see the installed version of MKDocs displayed.

### Option B: Installing MKDocs on a Virtual Environment

Using a virtual environment ensures that MKDocs and any related Python packages are isolated to your project folder. This avoids conflicts with system Python and works even on restricted school computers.

#### Step 1: Open PowerShell

* Navigate to your project folder where you want to store your MKDocs documentation. For example:

```bash
cd C:\Users\mechs\Documents\MKDocs\programming-documentation
```

#### Step 2: Create a Virtual Environment

**Before** completing this step, double check that there isn't already a virtual environment in the project. To check, simply see if there is a folder named "venv" in your project folder. If there is not, continue with this step.

Run the following command to create a virtual environment named `venv` in your project folder:

```bash
python -m venv venv
```

* This creates a folder called `venv` inside your project folder conatiniing a clean Python environment
* No admin privileges are needed because it installs locally in your project folder

#### Step 3: Activate the Virtual Environment

Activate the virtual environment so Python commands use the isolated environment

```bash
venv/Scripts/activate
```

After activating, your prompt should change to show `(venv)` at the beginning:

```objectivec
(venv) C:\Users\YourName\Documents\MKDocs\programming-documentation>
```

> **Tip:** Every time you want to work on this project, you need to activate the virtual environment first.

#### Step 4: Install MKDocs

With the virtual environment activated, install MKDocs using `pip`:

```bash
pip install mkdocs
```

* This installs MKDocs **only inside your virtual environment**
* No admin permissions are required

#### Step 5: Verify the Installation

Check that MKDocs is installed and working:

```bash
mkdocs --version
```

You should see the version number displayed, for example:

```pgsql
mkdocs, version 1.6.2
```

## Installing Material for MKDocs

The Material theme enhances MKDocs with a modern, professional look and extra features. You can install it in two ways: **inside your project's virtual environment** or **system-wide**.

> **Tip:** If you installed mkdocs via a virtual environment from the previous step, do the same here.

### Option 1: Installing Material in a Virtual Environment

1. Activate your virtual environment in your project folder:

```bash
cd C:\Users\YourName\Documents\MKDocs\programming-documentation
venv\Scripts\activate
```

2. Install the Material theme using `pip`:

```bash
pip install mkdocs-material
```

3. Verify the installation:

```bash
mkdocs --version
```

* You should see the MKDocs version installed
* The Material theme will now be available for your project.

4. Enable the theme in `mkdocs.yml`:

```yml
theme:
  name: material
```

Serve your project again, and it should show up with Material theme.

### Option 2: Installing Material System-Wide

1. Open **PowerShell**
2. Install Material globally:

```bash
pip install mkdocs-material
```

3. Verify installation:

```bash
mkdocs --version
```

4. Enable the theme in `mkdocs.yml`:

```yml
theme:
  name: material
```

> **Tip:** Install all plugins either only using the virtual environment or only using global download
> **Bonus Tip:** All of the plugins that we use will be located in the About page in the MKDocs section of this documentation. The install process will be the same