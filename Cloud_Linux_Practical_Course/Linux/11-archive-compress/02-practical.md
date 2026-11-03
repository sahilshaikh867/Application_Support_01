# Archive & Compression — Practical Lab

## Goal

Create, inspect, extract and verify archives.

## Step 1 — Prepare Data

```bash
mkdir -p ~/linux-lab/archive/project/{config,logs}
echo "APP_PORT=8080" > ~/linux-lab/archive/project/config/app.conf
echo "Application started" > ~/linux-lab/archive/project/logs/app.log
```

## Step 2 — tar Archive

```bash
cd ~/linux-lab/archive
tar -cf project.tar project/
```

List:

```bash
tar -tf project.tar
```

## Step 3 — gzip Compression

```bash
tar -czf project.tar.gz project/
ls -lh project.tar project.tar.gz
```

Compare sizes.

## Step 4 — Extract

```bash
mkdir restored
tar -xzf project.tar.gz -C restored
```

Verify:

```bash
find restored -type f
cat restored/project/config/app.conf
```

## Step 5 — zip

```bash
zip -r project.zip project/
unzip -l project.zip
```

## Step 6 — Restore Test

Delete only the copied project:

```bash
rm -rf restored
```

Restore again and verify the files.

## Important Lesson

A backup is not complete until restoration has been tested.

## Cleanup

```bash
rm -rf ~/linux-lab/archive
```
