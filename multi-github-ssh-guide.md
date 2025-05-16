# 🚀 Multi-GitHub Account SSH Setup Guide

This guide helps you push/pull code to multiple GitHub accounts (e.g., personal + work) using SSH.

---

## ✅ Step 1: Generate a Second SSH Key

```bash
ssh-keygen -t rsa -C "you@example.com" -f ~/.ssh/id_rsa_second
```

---

## ✅ Step 2: Add the New Key to SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_second
```

---

## ✅ Step 3: Add Public Key to GitHub

```bash
cat ~/.ssh/id_rsa_second.pub
```

- Copy the above key output
- Go to https://github.com/settings/ssh/new
- Log into your **second GitHub account**
- Paste the key and save it

---

## ✅ Step 4: Configure SSH

```bash
nano ~/.ssh/config
```

Add this to the file:

```ssh
# Default GitHub account
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

# Second GitHub account
Host github-second
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_second
```

> You can change `github-second` to any alias you prefer (e.g., `github-work`, `github-somen`)

---

## ✅ Step 5: Clone or Add Remote Using Alias

Use the `github-second` alias in your Git remote:

```bash
git remote add origin git@github-second:username/repo-name.git
```

Or if cloning:

```bash
git clone git@github-second:username/repo-name.git
```

Example for a new repo:

```bash
git init
git remote add origin git@github-second:somen-chatterjee/my-project.git
git add .
git commit -m "initial commit"
git push -u origin main
```

---

## ✅ Step 6: Test the SSH Connection

```bash
ssh -T git@github-second
```

Expected response:

```bash
Hi somen-chatterjee! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 🧠 Notes

- `github-second` is just a nickname (alias)
- You must use the alias in your Git URLs
- No need to redo the key steps unless you reset the system

---

📁 Save this file as `multi-github-ssh-guide.md` in your project folder or DevDocs.

```config
Host github.com (primary)
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

Host github-second (secondry)
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_second
  IdentitiesOnly yes
```

