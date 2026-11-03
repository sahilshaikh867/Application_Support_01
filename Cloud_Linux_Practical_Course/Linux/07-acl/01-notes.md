# Linux ACL — Notes

Traditional Linux permissions provide owner, group and others. ACLs provide more detailed access rules.

## Commands

```bash
getfacl
setfacl
```

## Check ACL

```bash
getfacl file.txt
```

## Grant User Access

```bash
setfacl -m u:alice:r file.txt
```

## Grant Group Access

```bash
setfacl -m g:developers:rwx project/
```

## Remove ACL Entry

```bash
setfacl -x u:alice file.txt
```

## Default ACL

A default ACL on a directory can influence permissions inherited by newly created files and directories.

Example:

```bash
setfacl -d -m g:developers:rwx project/
```

ACLs are powerful but should be documented because they can make access harder to understand if used carelessly.
