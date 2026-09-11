# Lab: Linux Files Management & Link Mechanics

## 📌 Objective
To demonstrate mastery over the Linux filesystem architecture by managing directories, creating files, and analyzing the structural differences between Hard Links and Symbolic (Soft) Links.

---

## 🔬 Hard Links vs. Soft Links Comparison

Hiring managers can easily see my understanding of underlying Linux storage nodes (Inodes) through this clear breakdown:

| Feature | Hard Link | Symbolic (Soft) Link |
| :--- | :--- | :--- |
| **Inode Value** | Shares the *exact same* inode as the source | Has a *different, unique* inode value |
| **If Source Deleted?** | File data is still fully accessible | The link breaks (becomes a dead/dangling link) |
| **Cross-Filesystem** | Cannot link across different filesystems | Can link across different filesystems/drives |
| **Directory Linking** | Not allowed for directories | Allowed to link to directories |

---

## 🛠️ Practical Exercises Executed

### 1. Creating a Symbolic (Soft) Link
I created a shortcut to a system configuration summary using the `-s` flag:
```bash
ln -s source_file.txt soft_link.txt
```

### 2. Creating a Hard Link
I created a direct physical link mirroring the original file data:
```bash
ln source_file.txt hard_link.txt
```

## 🔍 System Verification Output
Using the `ls -li` command, I verified the **inode numbers** (the first column) to confirm how the kernel tracks these files:

```bash
\$ ls -li
1343245 -rw-r--r-- 2 user user 24 Sep 11 22:30 hard_link.txt
1343245 -rw-r--r-- 2 user user 24 Sep 11 22:30 source_file.txt
1343299 lrwxrwxrwx 1 user user 15 Sep 11 22:31 soft_link.txt -> source_file.txt
```
*Observation: Notice that `source_file.txt` and `hard_link.txt` share the exact same inode number (`1343245`), while the soft link has its own unique layout (`1343299`).*
