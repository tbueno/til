# Find out the process using an specific port

Quite often I find myself running several application in my terminal windows, and usually each of them representing a different service or API I want to use. Since they obviously all run in localhost, I usually assign them common port numbers that I am familiar with:

- 8080
- 3000
- 5000
- 4567
- etc...

Eventually some tabs are closed by mistake, keeping the processes running in background and, when I try to run the same service again, I ended up receiving errors due the fact that the specific port I want to use is already bound, like this example from go:

````bash
 ➜ go run server.go
2017/08/27 20:10:31 listen tcp :8080: bind: address already in use
````

And it is not always easy to figure out by checking via the process name like

````bash
 ➜ ps aux | grep server.go                                                                        
bueno            11504   0.0  0.0  2423384    236 s000  R+    9:00PM   0:00.00 

````

Luckily, since I use always the same ports, it is easier to find whether the specific port my service try to use is been user by other process or not.

## `lsof` to the rescue

When we read the `man` page from `lsof`, it is not so clear about how it can help me in this case


````bash

LSOF(8)                                                                                                                                                                    LSOF(8)

NAME
       lsof - list open files

SYNOPSIS
       lsof  [ -?abChKlnNOPRtUvVX ] [ -A A ] [ -c c ] [ +c c ] [ +|-d d ] [ +|-D D ] [ +|-e s ] [ +|-E ] [ +|-f [cfgGn] ] [ -F [f] ] [ -g [s] ] [ -i [i] ] [ -k k ] [ +|-L [l] ] [
       +|-m m ] [ +|-M ] [ -o [o] ] [ -p s ] [ +|-r [t[m<fmt>]] ] [ -s [p:s] ] [ -S [t] ] [ -T [t] ] [ -u s ] [ +|-w ] [ -x [fl] ] [ -z [z] ] [ -Z [Z] ] [ -- ] [names]

DESCRIPTION
       Lsof revision 4.89 lists on its standard output file information about files opened by processes for the following UNIX dialects:

            Apple Darwin 9 and Mac OS X 10.[567]
            FreeBSD 8.[234], 9.0, 10.0 and 11.0 for AMD64-based systems
            Linux 2.1.72 and above for x86-based systems
            Solaris 9, 10 and 11

       An open file may be a regular file, a directory, a block special file, a character special file, an executing text reference, a library, a stream or a network file (Inter-
       net socket, NFS file or UNIX domain socket.)  A specific file or all the files in a file system may be selected by path.

       Instead  of  a formatted display, lsof will produce output that can be parsed by other programs.  See the -F, option description, and the OUTPUT FOR OTHER PROGRAMS section
       for more information.

````


But if we look closely, we will notice that `stream or a network file (Internet socket, NFS file or UNIX domain socket.)` are also listened in the command. If we understand the basics of how unix systems use files for socket management, we will notice that a command for listing open files should also bring socket information. Now, the question that remains is: which process holds the port that i'm looking for?


### `-i` for internet files

Reading the man we quickly find a flag that filters only internet files:

````bash

i [i]   selects the listing of files any of whose Internet address matches the address specified in i.  If no address is specified, this option selects the listing of all Internet and x.25  (HP-UX)  network files.

````
Combining this with the protocol and the port I am looking for will be enough to figure out the process information

````bash

 ➜ lsof -i tcp:8080

COMMAND   PID  USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
gateway 12034 bueno    3u  IPv6 0xaec2037784fd67d9      0t0  TCP *:http-alt (LISTEN)

````
