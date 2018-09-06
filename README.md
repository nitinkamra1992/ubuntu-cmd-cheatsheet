# Cheatsheet of useful Ubuntu Commands

## Linux

**chage**

```bash
chage -l {username}
```
**passwd** and **usermod**

```bash
passwd -l {username}
usermod -L -e {YYYY-mm-dd} {username}

passwd -u {username}
usermod -U -e {YYYY-mm-dd} {username}
```

## Killing processes

1. Safest: interactive, matches REGEX to process name
```bash
killall -9 -ir REGEX_PATTERN
```

2. Pretty safe, matches username, then process name
```bash
kill -9 `ps aux | grep USERNAME | grep PROCESSNAME | awk '{print $2}'`
```

3. Less safe
```bash
kill -9 $(pidof PROCESSNAME)
killall -9 PROCESSNAME
pkill -9 PROCESSNAME
pkill -9 -f PROCESSNAME
```