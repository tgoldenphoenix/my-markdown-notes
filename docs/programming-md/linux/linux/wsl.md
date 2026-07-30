# WSL Notes

## Access the Windows file system through WSL

```bash
cd /mnt/c/Users
**OR**
cd /mnt/d/your_folder/your_folder
```

- `/mnt/c` == `C:\`
- `/mnt/c/Users/` = `C:\Users`

## Git Bash vs WSL

Git Bash on Windows comes pre-packaged with full Windows ports of standard GNU command-line utilities, including grep, awk, sed, `find`, `curl`, and `tar`.

A PostgreSQL server happily listening on Windows localhost:5432 was not automatically reachable from inside WSL

they solve two very different problems. Git Bash is a POSIX-emulation layer running on Windows, while WSL 2 runs a real Linux kernel inside a lightweight managed VM. Those architectural differences explain almost every strange behavior you will see.

## MINGW

kk

