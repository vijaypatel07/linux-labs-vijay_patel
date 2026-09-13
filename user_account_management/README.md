# user account management operations in Linux

Start by creating a new user account named "joker"

```sudo useradd joker```

**To verify that the user was created, we'll examine the /etc/passwd file:**
```bash
sudo grep -w 'joker' /etc/passwd   
or
cat /etc/passwd
or
cat /etc/passwd | grep joker
```
> -w means match word 'joker' in /etc/passwd  location 

> **joker:x:5001:5001::/home/joker:/bin/sh** <br>
The /etc/passwd file is like a phonebook for user accounts. Each line represents one user account, with different pieces of information separated by colons (:). <br>

This line shows: <br>
<i>Username: joker <br>
Password: x (the actual password is stored securely elsewhere) <br>
User ID: 5001 <br>
Group ID: 5001 <br>
Home Directory: /home/joker, but it hasn't been created yet <br>
Default Shell: /bin/sh</i>

## Creating a User with a Home Directory
Now, let's create another user named "bob" and give them a home directory. <br>
```sudo useradd -m bob```
```
The -m option tells the system to create a home directory for the user.
A home directory is like a personal folder where a user can store their files and settings.
```

**Let's verify that the home directory was created:** <br>
``sudo ls -ld /home/bob``
> **drwxr-x--- 2 bob bob 57 Jan 19 13:33 /home/bob** <br>
This output shows:<br>
**d** at the start means it's a directory <br>
**rwxr-x---** shows who can read, write, or execute in this directory <br>
The two bob entries show that both the user and group owner of this directory is bob <br>
**57** is the size of the directory in bytes <br>
**Jan 19 13:33** is when the directory was created <br>
**/home/bob** is the location of the directory


## Setting a User Password
``` sudo passwd joker ```

> Behind the scenes, Linux stores encrypted passwords in a secure file called /etc/shadow. <br>
This is more secure than storing them in the /etc/passwd file where anyone could see them. <br>

## Modifying User Properties
Let's change joker's home directory <br>
```sudo usermod -d /home/wayne joker```
> **usermod** is the command to modify user account settings <br>
**-d /home/wayne** specifies the new home directory <br>
**joker** is the user we're modifying

Let's verify the change: `sudo grep -w 'joker' /etc/passwd` <br>
joker:x:1001:1001::/home/wayne:/bin/sh


## Changing User Shell 
Another important setting we can modify is the user's default shell. The shell is the program that interprets and runs the commands you type in the terminal. <br>
* By default, the user 'joker' is using /bin/sh as their shell. 
* While *sh* (Bourne Shell) is a basic shell that's present on most Unix-like systems, 
* *bash* (Bourne Again Shell) offers more features and is generally more user-friendly.

**Change joker's default shell to bash:** <br>
`sudo usermod -s /bin/bash joker` <br>
Verify the change: `sudo grep -w 'joker' /etc/passwd` <br>

## Adding a User to a Group
 let's add joker to the sudo group: <br>
 `sudo usermod -aG sudo joker`
* usermod is the command to modify user accounts 
* -aG means "append to Group" (add to a group without removing from other groups) <br>
* sudo is the group we're adding the user to <br>
* joker is the user we're modifying

 **Verify the change:**`groups joker`
 >joker : joker sudo

 >now you can switch to the joker user to sudo (root privileges) using: `su - joker`

Once logged in as joker, let's try to view a file that normally requires root privileges: <br>
`sudo cat /etc/shadow`
```
$ sudo cat /etc/shadow
[sudo] password for joker:
root:*:20494:0:99999:7:::
daemon:*:20494:0:99999:7:::
bin:*:20494:0:99999:7:::
sys:*:20494:0:99999:7:::
sync:*:20494:0:99999:7:::
games:*:20494:0:99999:7:::
man:*:20494:0:99999:7:::
lp:*:20494:0:99999:7:::
mail:*:20494:0:99999:7:::
news:*:20494:0:99999:7:::
uucp:*:20494:0:99999:7:::
proxy:*:20494:0:99999:7:::
www-data:*:20494:0:99999:7:::
backup:*:20494:0:99999:7:::
list:*:20494:0:99999:7:::
irc:*:20494:0:99999:7:::
_apt:*:20494:0:99999:7:::
nobody:*:20494:0:99999:7:::
systemd-network:!*:20494::::::
systemd-timesync:!*:20494::::::
dhcpcd:!:20494::::::
messagebus:!:20494::::::
syslog:!:20494::::::
systemd-resolve:!*:20494::::::
uuidd:!:20494::::::
landscape:!:20494::::::
polkitd:!*:20494::::::
vijay:$y$j9T$zIxAVmr3R8G6ZPjhsRBuM0$BXWrJRnYpqk7d7zHFXBBMpqnAcF.iwYj8tdrIsBND96:20577:0:99999:7:::
joker:$y$j9T$mIf22RbEuJIuk49KFZr7I1$BJNtCwXWgIAWsODp/RQn1rMRAA8/R3w3D4CB3XsJaX8:20709:0:99999:7:::
bob:!:20709:0:99999:7:::` 
```

### To remove joker from the sudo group, use:
`sudo gpasswd -d joker sudo`
> What each part means: <br>
**sudo**: Runs the command with administrator privileges.<br>
**gpasswd**: Manages group membership.<br>
**-d**: Deletes a user from a group.<br>
**joker**: The user to remove.<br>
**sudo**: The group from which the user is removed.

> Verify the change with: `groups joker`


## Locking and Unlocking User Accounts
Sometimes, you might need to temporarily disable a user account without deleting it.
### Lock the joker account: `sudo passwd -l joker` <br>
The -l option locks the password. <br>
Try to switch to the joker user:  
* Enter the password you set
* You should see an "authentication failure" message. This means the account is successfully locked.

### let's unlock the account: `sudo passwd -u joker`
The -u option unlocks the password.

## Deleting a User
We'll delete the "bob" user we created earlier. <br>
Delete bob and their home directory:
``sudo userdel -r bob``
> The **userdel** command deletes user accounts. <br>
The -r option removes the user's home directory and mail spool.


Verify that the user has been deleted:
```bash 
sudo grep -w 'bob' /etc/passwd
sudo ls -ld /home/bob
```


## Summary
>useradd - to create new users <br>
passwd - to change user passwords, Acc lock/unlock, <br>
usermod - to modify user accounts <br>
userdel - to delete user accounts <br>

```text
Congratulations! You've completed the Linux User Account Management lab. You've learned how to:

Create new user accounts
Set user passwords
Modify user properties like home directory and default shell
Add users to groups
Lock and unlock user accounts
Delete user accounts
You've also been introduced to important Linux concepts like the /etc/passwd file, home directories, shells, and user groups. These are fundamental skills for Linux system administration. Remember, in real-world scenarios, always follow your organization's security policies when managing user accounts.
```