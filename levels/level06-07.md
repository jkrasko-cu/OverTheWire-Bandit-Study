# Level 6 -> Level 7: Server-Wide Specific File Searching 

## The Challenge
In the previous two levels, we got an overview understanding of the `file` and `find` commands. Now, we similarly want to go through a file system to find a specific text found on a specific file.

In this challenge, we're thrown several specifications:
- The password file is stored on the server
- It is owned by a specific user (bandit7)
- It is owned by a specific group (bandit6)
- The file size is 33 bytes

## The Approach
The first instinct is recognizing that this level is quite similar to the previous one. Let's start by modifying our previous command; 

`find . -type f -size 1033c ! -executable -exec file {} +`

Well, we already know how to find filesizes. We'll change `-size 1033c` to `33c`. Note from the previous level that "c" represents bytes, which is what the problem asks for.

Also, let's remove the `! -executable` flag. Nothing is said about executable files here. 

At this point, we're left with new concepts; let's tackle user and group ownerships next. We look onto the man page for the find function, and we find flags for `-user` and `-group`. With this information, we can shape our command to be as such:

`find . -type f -size 33c -user bandit7 -group bandit6 -exec file {} +`

But something strange happens when we use it right now; we're not getting any results. This is where it's important to review the problem; we're looking for a file on the server, not just files within the current directory. Since we know that `find .` refers to finding files within the CURRENT DIRECTORY, we know this must be changed. As such, we'll change it to be `find /` -- looking from the root directory. We're sure to encapsulate the server within this, as the server's root directory will be found somewhere within it.

But there's another issue! Now, we're getting a bunch of junk lines with error messages. We're searching such a large breadth of content that we're trying to read things we shouldn't be able to read, thus giving us errors.

We need to filter out that content. This holds the final piece of the puzzle; output redirection.
Errors in linux go to the standard error stream, characterized by the file descriptor `2`.

Then, to filter out all the error messages, we'd use `2>/dev/null`. 
The `>` tells the program to take the standard error stream and output it to wherever we specify.
Since we have no use for it, we can put it in `/dev/null` -- essentially a blackhole where all the data is discarded when put into it.

Great! We can find the file through the use of `find / -type f -size 33c -user bandit7 -group bandit6 -exec file {} + 2>/dev/null`.

Although, we don't want to navigate through all these directories, so let's just make one last change; replace `-exec file` with `-exec cat` and directly get the contents of the file (and display the password!)

## The Solution 
- `find / -type f -size 33c -user bandit7 -group bandit6 -exec cat {} + 2>/dev/null`
    - `find /` specifies to search from the root directory, which the server is stored on
    - `-type f` only matches regular files
    - `-size 33c` specifies to search for files of 33 byte (c) size
    - `-user bandit7` specifies files owned by the user bandit7
    - `-group bandit6` specifies files owned by the group bandit6
    - `-exec cat {} +` displays file contents directly
    - `2>/dev/null` discards all the permission denied errors we don't need to see

## Key Takeaways of Problem
- Learning how to manipulate standard output and standard error streams to filter out unwanted text
- Using man pages to figure out important flags used in file searching
- Knowing how and when to increase the breadth of files being searched

## What I Personally Learned
- **Initial Mistake:** I was searching from the wrong directory instead of the root, which didn't give me results.
- **Cybersecurity Patterns:** `find` and error redirection will be helpful as I move on to larger file breadths.

## Real-World Applications
- Privilege Escalation - This allows us to find bad SUID binaries (files that run with file owner permissions and not the user's permissions) that can be manipulated by hackers. If they can manipulate a SUID binary owned by the root, they'll be able to use root privileges to execute harmful code.

**Example:** finding SUID binaries owned by root `find / -perm -4000 -user root 2>/dev/null` -- if one of these binaries are manipulated, hackers can abuse this.

- Incident Response - This has increased our repertoire which we can use to identify suspicious files, in regards to suspicious file owners or file sizes.

**Example:** finding files modified recently by suspicious individuals `find / -user hacker -mtime -1 2>/dev/null` -- helps to understand the actions of an attack and find what files were compromised

- Security Auditing - Being able to proficiently navigate file systems is important in finding vulnerabilities.

**Example:** Finding files writable by anyone: `find / -type f -perm -002 2>/dev/null` -- files modifiable by anyone have potential vulnerabilities involved

## Password
<details><summary>Password</summary>
    <pre>
    morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
    </pre>
   </details>