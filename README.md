# 🔐 Bitwarden Vault Stats Workflow

This repository contains a [GitHub Actions workflow](./.github/workflows/bitwarden-vault-stats.yaml) that securely retrieves secrets, generates statistics from a Bitwarden vault, and sends the results via [Bitwarden Send](https://bitwarden.com/products/send/). 

[![🔐 Generating Bitwarden Vault Stats ...](https://github.com/bitwarden-labs/vault-stats-workflow/actions/workflows/bitwarden-vault-stats.yaml/badge.svg)](https://github.com/bitwarden-labs/vault-stats-workflow/actions/workflows/bitwarden-vault-stats.yaml)

The workflow leverages the [Bitwarden CLI tool](https://github.com/bitwarden/cli) for seamless interaction with the vault.

![Screenshot 2025-01-08 at 22 46 45](https://github.com/user-attachments/assets/11729c77-195c-47e8-b428-63e5f546e7d4)


## 🛡 How Credentials Are Handled

Credentials such as `EMAIL`, `MASTER_PASSWORD`, and `ORGANIZATION_ID` are stored in Bitwarden and retrieved securely during the workflow using the [bitwarden/sm-action@v2](https://github.com/bitwarden/sm-action). 

![image](https://github.com/user-attachments/assets/e20df77c-033b-4c47-8278-d491c9ec0ad2)

Bitwarden provides [an integration with GitHub Actions](https://bitwarden.com/help/github-actions-integration/) to retrieve secrets from [Bitwarden Secrets Manager](https://bitwarden.com/products/secrets-manager/) and inject them into GitHub Actions workflows. The integration will inject retrieved secrets as masked environment variables inside an action. 

![image](https://github.com/user-attachments/assets/d03df5d7-037a-4ac7-b753-3369d9da783c)

![image](https://github.com/user-attachments/assets/ce4df7a2-be19-4650-8a0e-b8c645f063be)

The `BW_ACCESS_TOKEN` is stored securely in the repository’s GitHub Secrets.

![Screenshot 2025-01-08 at 22 35 49](https://github.com/user-attachments/assets/1dd6493c-09af-43d3-9675-85905b582b72)

![image](https://github.com/user-attachments/assets/08a971d2-8f18-4aa9-a5f5-c356b8fe7db9)

### Runtime Handling

- Secrets are never exposed in plaintext in the logs.
- The workflow uses environment variables to pass sensitive data (e.g., `BW_SESSION`) between steps securely.
- All vault interactions (e.g., logging in, unlocking, listing items, creating Sends) are handled via the [Bitwarden CLI tool](https://github.com/bitwarden/cli), which operates securely using the retrieved session token.

![Screenshot 2025-01-08 at 22 54 08](https://github.com/user-attachments/assets/b260ea69-9fb4-4604-9fe4-a793fffa4362)

## 📝 Example Output

In **GitHub Actions Logs**:

```bash
🔗 Bitwarden Send URL: https://vault.bitwarden.com/#/send/7R9Qv76scUWDarJfAT5kNg/mrFiqYnLGx0X3w-Xrv_JA
```

In **Bitwarden Send**:

```bash
Your vault has access to: 15 logins, 1 cards, 0 identities, and 2 secure notes.
```
![image](https://github.com/user-attachments/assets/0187c12e-1aba-4793-82b8-e4c58a46ce33)


## 📂 Repository Structure

```bash
.
├── .github/
│   └── workflows/
│       └── bitwarden-vault-stats.yml  # GitHub Actions workflow file
└── README.md                          # Documentation (this file)
```

## 📋 Workflow Steps

1.	🔑 **Get Bitwarden Secrets**: Retrieves `EMAIL`, `MASTER_PASSWORD`, and `ORGANIZATION_ID` from [Bitwarden Secrets Manager](https://bitwarden.com/products/secrets-manager/) for a valid US account.
2.	⚙️ **Install Bitwarden CLI**: Installs the [Bitwarden CLI tool](https://github.com/bitwarden/cli) to interact with the vault.
3.	🔐 **Log in and Unlock Vault**: Logs into the vault and unlocks it using the [Bitwarden CLI](https://github.com/bitwarden/cli).
4.	📊 **Generate Vault Stats**: Counts the total number of items for each type: Logins, Cards, Identities, and Secure Notes.
5.	📤 **Send Stats via Bitwarden Send**: Sends the stats securely via [Bitwarden Send](https://bitwarden.com/products/send/), and outputs a shareable URL in the logs.

## 🖥️ Bitwarden CLI Tool

The [Bitwarden CLI tool](https://github.com/bitwarden/cli) (`bw`) is the core component used in this workflow to:

- Log in to Bitwarden.
- Unlock the vault.
- List and count items (Logins, Cards, Identities, Secure Notes).
- Create secure, shareable Bitwarden Sends.

For more information about the Bitwarden CLI, [visit the official documentation](https://bitwarden.com/help/cli/).

## ⚠️ Security Considerations

- Keep `BW_ACCESS_TOKEN` secure: Store it in GitHub Secrets to prevent unauthorized access.
- Sensitive data is passed securely through environment variables and never exposed in plaintext logs.
- [Bitwarden Send](https://bitwarden.com/products/send/): Provides an additional layer of security by sharing the stats through a secure, time-limited URL.
