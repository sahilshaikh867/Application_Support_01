# Linux Editors — Practical Lab

## Goal

Edit files using nano and vim.

## Step 1 — Nano

```bash
mkdir -p ~/linux-lab/editors
cd ~/linux-lab/editors
nano app.conf
```

Add:

```text
APP_NAME=TrainingApp
APP_ENV=lab
APP_PORT=8080
```

Save and exit.

Verify:

```bash
cat app.conf
```

## Step 2 — Vim

```bash
vim nginx-training.conf
```

Press `i`, type:

```text
server {
    listen 8080;
    server_name training.local;
}
```

Press `Esc`.

Save and exit:

```text
:wq
```

## Step 3 — Backup

```bash
cp app.conf app.conf.bak
```

Modify `app.conf`, then compare:

```bash
diff app.conf.bak app.conf
```

## Cleanup

```bash
rm -rf ~/linux-lab/editors
```
