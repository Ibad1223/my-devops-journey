# File Permissions

> How Linux controls who can read, write or run a file, and how I fixed my first "Permission denied".

## What it is
rwx stands for read, write and execute. Every file has three sets of them:
user (the owner), group and others. Read lets you see what's in the file,
write lets you change it, and execute lets you run it.
On a directory, r lets you list it, w lets you add or delete files in it,
and x lets you enter it.

## Why it matters
Without permissions, any user or program could read, change or run any file.
A hacked program or a careless user could then damage or expose files they
should never have touched. Giving each file only the access it needs is
called the principle of least privilege.

## What I did
```bash
ls -l test.sh        # see permissions, owner and group
chmod u+x test.sh    # give the owner execute permission
```

My real output:
```
-rwxr----- 1 ibz43 ibz43 11 Oct  2 18:00 test.sh
```
That's 740: the owner has rwx, the group has r, and others have nothing.

| Mode | Symbolic  | Used for |
|------|-----------|----------|
| 755  | rwxr-xr-x | Scripts, programs and directories: owner can edit, everyone can run |
| 644  | rw-r--r-- | Regular files like configs and documents: owner edits, everyone else reads |
| 740  | rwxr----- | Files shared with a team but private from everyone else: owner full, group read, others nothing |

Each digit is a code, not a count: 4 = read, 2 = write, 1 = execute, added up.

## What went wrong and how I fixed it
| Symptom | Cause | Fix |
|---------|-------|-----|
| `./test.sh` gave "Permission denied" | File was missing execute permission | `chmod u+x test.sh` |
| `ls -1` showed no permissions | I typed the number 1, not lowercase l | Use `ls -l` |
