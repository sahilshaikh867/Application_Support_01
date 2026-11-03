# Linux Editors — Notes

Linux administrators frequently edit configuration files.

Common editors:
- nano
- vim
- vi

## Nano

Open:

```bash
nano file.txt
```

Important shortcuts:
- `Ctrl+O` save
- `Ctrl+X` exit
- `Ctrl+W` search

## Vim Modes

Vim has modes.

### Normal mode
Navigation and commands.

### Insert mode
Typing text.

Enter insert mode:

```text
i
```

Return to normal:

```text
Esc
```

Save and exit:

```text
:wq
```

Exit without saving:

```text
:q!
```

## Safe Configuration Editing

Before changing an important configuration file:

```bash
sudo cp /etc/example.conf /etc/example.conf.bak
```

Then edit.

Validate the configuration when the application provides a validation command.
