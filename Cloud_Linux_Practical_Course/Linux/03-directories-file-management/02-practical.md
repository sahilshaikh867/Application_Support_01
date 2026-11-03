# Directories & File Management — Practical Lab

## Goal

Build a controlled directory tree and practice file operations.

## Step 1

```bash
mkdir -p ~/linux-lab/files/{app,backup,logs}
cd ~/linux-lab/files
```

## Step 2 — Create Files

```bash
touch app/app.conf app/app.log
echo "application started" > logs/app.log
```

## Step 3 — Copy

```bash
cp app/app.conf backup/app.conf.bak
```

## Step 4 — Move

```bash
mv app/app.log logs/application.log
```

## Step 5 — Find

```bash
find ~/linux-lab/files -type f
find ~/linux-lab/files -name "*.log"
```

## Step 6 — Metadata

```bash
stat logs/application.log
file logs/application.log
```

## Step 7 — Backup Exercise

```bash
cp -r app backup/app-copy
```

## Verification

Create this final tree:

```text
files/
├── app/
│   └── app.conf
├── backup/
│   ├── app.conf.bak
│   └── app-copy/
└── logs/
    └── application.log
```

## Cleanup

```bash
rm -rf ~/linux-lab/files
```
