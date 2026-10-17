# Users and Groups

> How Linux identifies users, who gets to use sudo, and how groups control access, tested on throwaway users.

## What it is
Every user has a number (uid) and a primary group (gid). My own `id` output:
```
uid=1000(ibz43) gid=1000(ibz43) groups=1000(ibz43),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users),1004(docker)
```
A group is a collection of users, so that they all get access together rather than one by one.

## Why it matters
sudo works because I'm in the sudo group. When I run it, it lets me run one
command as root. This keeps big mistakes limited, because I only get root's
power temporarily, for one command at a time.

## What I did
```bash
sudo whoami     # root
whoami          # ibz43
```
`sudo` ran that one command as root, then I was back to my own user.

**`adduser` vs `useradd`**

| | `sudo adduser testuser` | `sudo useradd testuser2` |
|---|---|---|
| Asks for a password | Yes | No |
| Home folder in `/home` | Yes | No |
| Groups | `testuser`, `users` | `testuser2` only |

`useradd` is better for scripts, because it doesn't ask questions, and a script can't answer prompts.

**Groups**
```bash
sudo groupadd devteam
sudo adduser testuser devteam
id testuser
# uid=1005(testuser) gid=1005(testuser) groups=1005(testuser),100(users),1006(devteam)
```

## What went wrong and how I fixed it
| Symptom | Cause | Fix |
|---------|-------|-----|
| `sudo usermod -G devteam testuser` removed `users` from testuser's groups | Without `-a`, `-G` replaces the extra groups. `-a` means append, which adds a group and keeps the existing ones | `sudo usermod -aG users testuser` put it back |
| `groupadd: group 'devteam' already exists` | I ran `groupadd devteam` twice | Harmless: the group was already there |

`usermod -aG` wasn't in my course notes. I worked it out by testing it.
