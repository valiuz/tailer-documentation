---
description: >-
  Before installing the Tailer SDK package, you need to prepare your local
  environment.
---

# Prepare your local environment for Tailer

Tailer SDK runs on Windows, Linux, and macOS. Tailer SDK requires Python. Supported versions are 3.7 or higher.

Before installing Tailer SDK, a number of tasks need to be performed depending if you're planning to use a Windows or macOS/Linux environment.

## 💻  Windows

### **Install Python**

To install Python:

1. Access this page: [https://www.python.org/downloads/release/python-382/](https://www.python.org/downloads/release/python-382/)
2. Select and download the installer for the release \(3.7 or higher\) that matches your operating system.
3. Launch the installer.
4. Select the **Customize installation** option, and make sure you check the **Add Python to environment variables** checkbox.
5. To check if Python installation was successful, open a terminal, and run the following command:

   ```text
   python --version
   ```

{% hint style="success" %}
The Python release number displays.
{% endhint %}

### **Create a working folder**

To create a working folder for Tailer SDK:

1. Access the working folder of your choice:

   ```text
   cd your_working_folder
   ```

2. Create a folder for Tailer SDK and access it:

   ```text
   mkdir tailer
   cd tailer
   ```

{% hint style="success" %}
Your Windows environment is now ready. You can proceed with the [installation](install-tailer-sdk.md).
{% endhint %}

## 🍏  macOS/Linux

### **Set Bash as default shell**

To change the default shell to Bash:

1. Run the following command:

   ```text
   chsh -s /bin/bash
   ```

2. Close your terminal and reopen it.
3. Run the following command to make sure the default shell is now bash:

   ```text
   env
   ```

{% hint style="success" %}
If you get the following result, Bash has been installed successfully:

```text
SHELL=/bin/bash
```
{% endhint %}

### **Create a working folder**

{% hint style="info" %}
We recommend that you create the Tailer ****SDK working folder in your home root directory.
{% endhint %}

To create and access the working folder, run the following commands:

```text
mkdir ~/tailer
cd ~/tailer
```

Examples in this documentation will assume that you're working in the **tailer** folder created at this step.

### **Upgrade Python**

While current versions of macOS and Linux include a version of Python 2, Tailer SDK only supports Python 3.7 or higher, so you need to upgrade to a newer version.

#### **Install Homebrew**

To install Python, you first need to install Homebrew, a package management system that simplifies software installation:

1. Run the following command:

   ```text
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```

2. Make sure the installation was successful by running the following command:

   ```text
   brew doctor
   ```

{% hint style="success" %}
Now that Homebrew is installed, you can upgrade to Python 3.
{% endhint %}

#### **Upgrade to Python 3**

To upgrade Python:

1. Check your current version of Python by running the following command:

   ```text
   python --version
   ```

2. If your version is inferior to 3.7, install the latest version of Python 3 by running the following command:

   ```text
   brew install python3
   ```

3. To confirm which version of Python 3 was installed, run the following command:

   ```text
   python3 --version
   ```

{% hint style="success" %}
Your macOS/Linux environment is now ready. You can proceed with the [installation](install-tailer-sdk.md).
{% endhint %}

