# Lab 03 - Reading Files in Linux

## Objective

Practice reading and editing files using cat, less, more, head, tail, and nano.

---

## Environment

- Operating System: Kali Linux
- Terminal: Zsh
- User: hassan

---

## Commands Executed

```bash
cd ~/CyberLab

nano linux_notes.txt
```

Content typed into the file:

```
Linux Fundamentals
Cybersecurity
Networking
Blue Team
SOC Analyst
```

Save and exit with `CTRL + O`, `Enter`, `CTRL + X`.

```bash
cat linux_notes.txt

head linux_notes.txt

tail linux_notes.txt

less linux_notes.txt
```

Inside `less`, scroll using the arrow keys, then press `q` to quit.

---

## Skills Gained

- Read small files with `cat`
- View the start of a file with `head`
- View the end of a file with `tail`
- Scroll through large files with `less`
- Create and edit files with `nano`

---

## Conclusion

This lab demonstrated the core Linux commands used to read and monitor files. These are essential daily tools for a SOC Analyst, especially `tail -f` for live log monitoring during investigations.
