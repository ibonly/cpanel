# cPanel Deployment via GitHub Actions

[![GitHub Actions Status](https://github.com/ibonly/cpanel/workflows/Deploy/badge.svg)](https://github.com/ibonly/cpanel/actions)  
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

Automate deployments to cPanel hosting using GitHub Actions with FTP/SFTP protocol support. This repository provides a seamless way to deploy your website files to a cPanel hosting environment directly from GitHub.

---

## 📝 Table of Contents
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Installation & Usage](#-installation--usage)
- [Workflow Overview](#-workflow-overview)
- [Customization](#-customization)
- [Security Best Practices](#-security-best-practices)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)

---

## 🚀 Features
- **Automatic Deployment**: Deploy on push to the `main` branch.
- **Manual Trigger**: Start deployments manually via GitHub Actions UI.
- **Secure Transfers**: Supports SFTP/FTPS for encrypted file transfers.
- **File Exclusions**: Exclude unnecessary files (e.g., `.github`, `.git`).
- **Customizable**: Easily adapt the workflow to your needs.
- **cPanel Integration**: Designed specifically for cPanel hosting environments.

---

## 📋 Prerequisites
Before using this repository, ensure you have:
1. A **GitHub account**.
2. A **cPanel hosting account** with FTP/SFTP access.
3. Basic knowledge of **GitHub Actions**.
4. A repository containing your website files.

---

## 🛠️ Installation & Usage

### 1. Fork/Copy this Repository
Clone this repository to your local machine or fork it to your GitHub account:
```bash
git clone https://github.com/ibonly/cpanel.git
```

### 2. Configure Secrets
Add the following secrets in your GitHub repository settings (Settings → Secrets → Actions):

- FTP_PASSWORD: Your cPanel account password.
- (Optional) SSH_PRIVATE_KEY: If using SSH key authentication.

You can also use the GitHub CLI to set secrets:
```bash
gh secret set FTP_PASSWORD --body "your_cpanel_password"
```
### 3. Configure the Workflow
Edit the .github/workflows/main.yml file to match your cPanel settings:
```yaml
- name: SFTP Deploy
  uses: SamKirkland/FTP-Deploy-Action@4.3.3
  with:
    server: ftp.yourdomain.com  # Replace with your server
    username: your_cpanel_user@yourdomain.com
    password: ${{ secrets.FTP_PASSWORD }}
    server-dir: /public_html/  # Deployment directory
  ```

### 4. Deploy!
Push to the main branch for automatic deployment.
Use the GitHub Actions UI to trigger a manual deployment.
---
🔧 Workflow Overview
The deployment workflow is defined in .github/workflows/main.yml. Here's a breakdown:
```yaml
name: Cpanel Deploy

on:
  push:
    branches: [main]  # Trigger on push to main branch
  workflow_dispatch:  # Allow manual triggers

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2  # Checkout repository code
      - uses: SamKirkland/FTP-Deploy-Action@4.3.3  # Deploy via SFTP
        with:
          protocol: sftp  # Use SFTP for secure transfers
          server: ${{ secrets.FTP_SERVER }}  # FTP server
          username: ${{ secrets.FTP_USERNAME }}  # cPanel username
          password: ${{ secrets.FTP_PASSWORD }}  # cPanel password
          server-dir: /public_html/  # Deployment directory
          exclude: |  # Exclude unnecessary files
            .github/**
            .git/**
```
---
🎛️ Customization
You can customize the workflow to suit your needs. Here are some common adjustments:

### 1. Change Deployment Directory
Deploy to a subdirectory within public_html:
```yaml
server-dir: /public_html/subdirectory/
```
### 2. Add Build Steps
For static sites or projects requiring a build process:
```yaml
- name: Build Project
  run: |
    npm install
    npm run build
```
### 3. Modify Exclusions
Exclude additional files or directories:
```yaml
timeout-minutes: 20
```
---
🤝 Contributing
Contributions are welcome! Follow these steps:
- Fork the repository.
- Create a new branch (git checkout -b feature/AmazingFeature).
- Commit your changes (git commit -m 'Add some AmazingFeature').
- Push to the branch (git push origin feature/AmazingFeature).
- Open a Pull Request.
---
📄 License
This project is licensed under the MIT License. See LICENSE for details.
