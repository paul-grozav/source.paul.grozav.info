---
layout: page
ptitle: Others - migrated from old site
---

### 1. Ubuntu remove guest session

Edit `/etc/lightdm/lightdm.conf` so that it looks like this:
```bash
greeter-session=unity-greeter
user-session=ubuntu
allow-guest=false
```
Thanks to: http://askubuntu.com/questions/62564/how-do-i-disable-the-guest-session

You can also remove the remote login by adding:
```bash
greeter-show-remote-login=false
```

---
---
---

### 3. F10 problem in gnome terminal

**Problem**:

Whenever you press F10 in terminal, you get a right click / context menu.

**Solution**:
- Create the directory `~/.config/gtk-3.0`using the command:`mkdir -p ~/.config/gtk-3.0`
- In that directory, create the file `gtk.css` and put this content inside:

```css
@binding-set NoKeyboardNavigation {
unbind "<shift>F10";
}

* {
gtk-key-bindings: NoKeyboardNavigation
}
```
- Restart X server

Thanks to: <a href="https://bugs.launchpad.net/ubuntu/+source/unity/+bug/726639">https://bugs.launchpad.net/ubuntu/+source/unity/+bug/726639</a>, comment #18

---
---
---

### 4. Mount remote filesystem with sshfs
1. Make sure you have <em>sshfs</em>installed on your computer and on your server. If not, install it using: `apt-get install sshfs`
2. Switch to super user using: `sudo su`
3. Create the mount point using: `mkdir /mnt/remoteFS` and change it's owner using: `chown [user-name]:[group-name] /mnt/remoteFS`
4. Add your user to <em>fuse</em>group using: `adduser [your-user] fuse`
5. Switch back to your user using:`exit`
6. Log out and then back in</u> or run <i>newgrp fuse</i> to apply the group changes. Now mount the remote filesystem using:`sshfs remote-user@remote.server:/remote/directory /mnt/remoteFS`
Thanks to: <a href="http://www.debianadmin.com/mount-a-remote-file-system-through-ssh-using-sshfs.html">http://www.debianadmin.com/mount-a-remote-file-system-through-ssh-using-sshfs.html</a>

---
---
---

### 5. install Oracle Java with mozilla plugin

<strong>Problem</strong>:

Some apps require Oracle's Java. This is a how to install and use java from oracle(sun) instead of icedtea on linux(tested on debian and ubuntu).

<strong>Solution</strong>:
1. Download Java from <a href="http://www.java.com/en/">www.java.com</a>. Download the <em>.tar.gz</em> version. Here you have the <a href="http://javadl.sun.com/webapps/download/AutoDL?BundleId=65685">32 bit version</a> and the <a href="http://javadl.sun.com/webapps/download/AutoDL?BundleId=65687">64 bit version</a>.
2. Extract the archive to <span class="lang:sh decode:true  crayon-inline ">/opt/java/64</span>. You should get this folder:  <span class="lang:sh decode:true  crayon-inline ">/opt/java/64/jre1.7.0_05</span> .
3. Run as super user the following:

```bash
update-alternatives --install "/usr/bin/java" "java" "/opt/java/64/jre1.7.0_05/bin/java" 1
update-alternatives --set java /opt/java/64/jre1.7.0_05/bin/java
```

1. To install the firefox and chrome plugin, run as your user:
```bash
mkdir -v ~/.mozilla/plugins
sudo apt-get remove icedtea-6-plugin && sudo apt-get remove icedtea-7-plugin
rm -v ~/.mozilla/plugins/libnpjp2.so
ln -s /opt/java/64/jre1.7.0_05/lib/amd64/libnpjp2.so ~/.mozilla/plugins/
```
2. Restart the browser
You can also add the java web start if you need it:

```bash
update-alternatives --install "/usr/bin/javaws" "javaws" "/opt/java/64/jre1.7.0_05/bin/javaws" 1
update-alternatives --set javaws /opt/java/64/jre1.7.0_05/bin/javaws
```

---
---
---

### 6. Programs I often use(d) on windows:
1. <a href="http://www.microsoft.com/en-us/download/details.aspx?id=8483">Windows Installer 3.1</a> - <a href="http://download.microsoft.com/download/2/6/1/261fca42-22c0-4f91-9451-0e0f2e08356d/WindowsXP-KB942288-v3-x86.exe">for Windows XP</a>
2. <a href="http://www.microsoft.com/en-us/download/details.aspx?id=21">Microsoft .NET framework 3.5</a>&nbsp;&nbsp;(requires internet connection)
3. <a href="http://www.microsoft.com/en-us/download/details.aspx?id=17718">Microsoft .NET framework 4</a>&nbsp;(offline installer)

---
---
---

### 7. Checking and waiting for a PID to finish

Knowing the PID of a process, you can create a script that waits for the process to finish, and then do something.
```bash
#!/bin/bash
emacs &
# Get the PID of the last process
emacspid=$!
# Wait for the process to finish it's job
echo Waiting for emacs to finish
while true; do
sleep 1
# This variable holds the number of running processes with that PID
appIsRunning=$(ps -p $emacspid | grep -c $!)
if [ "$appIsRunning" == "1" ]; then
echo Emacs is running
else
echo Emacs has finished
break
fi
done
```

The following code shows how you can start a number of feeds and wait for all of them to finish, before you do something:
```bash
#!/bin/bash
NUMBER_OF_PROCESSES=3

pids=""
for((i=0; i&lt;$NUMBER_OF_PROCESSES; i++)) do
emacs &
pids="$pids $!" # Add to pids	a space	and the	pid of the last	process
done

function pidIsRunning(){
echo $(ps -p $1 | grep -c $1)
}

# Wait for the processes to finish their job
echo Waiting for processes to finish
while true; do
# This makes the script sleep for 1 second, and then recheck the processes
sleep 1
r=""
for pid in $pids; do
r="$r $(pidIsRunning $pid)"
done
echo "$r"
cr=""
for((i=0;	i&lt;$NUMBER_OF_PROCESSES;	i++)) do
cr="$cr 0"
done
if [ "$r" == "$cr" ]; then
echo "All processes are finished"
break
fi
done
```

---
---
---

### 8. Sum files in folder - linux bash

If you need to sum all files in a folder, you can use the following command: `find . -name "*" -ls | awk '{total += $7} END {print total}'`.
Instead of `.` you can specify any directory, and instead of `*` you could sum just pdf files by using `*.pdf`

If you need a recursive version that sums all files, you can use:
```bash
#!/bin/bash
O=$IFS
IFS=$(echo -en "nb")
for file in $(find $1 -type f)
do
echo "$sum:$file"
sum=$(( $sum + $(ls -l $file | cut -f 5 -d " ") ))
done
IFS=$O
echo "$sum"
```

---
---
---

### 9. Determine the number of hours between your computer and UTC/GMT

You can use:
```cpp
time_t when = time(NULL);
struct tm utc = *gmtime(&when);
struct tm lcl = *localtime(&when);
int delta_h = utc.tm_hour - lcl.tm_hour;
```
Remember, that calling `gmtime()` will destroy your current time structure.

---
---
---

### 10. How to download a website for offline usage

You can use wget for that:
```bash
wget -U Mozilla --recursive --no-clobber --page-requisites --html-extension --convert-links -- restrict-file-names=windows --domains www.cplusplus.com --no-parent www.cplusplus.com/reference/
```
That command will download cplusplus.com/reference to your computer, and you will be able to access the site without an internet connection.
P.S.: All the assets(.css, .js) will be downloaded for offline usage, and the link are converted so that you can surf from one page to the other. The pages will look just as if you were on the internet.

---
---
---

### 11. C++ substring on char array

```cpp
//! Returns y characters from char* str, starting with character from position x
//! Note: it adds '\0' at the end of the array
char* subchars(char* str, short x, short y);

char* subchars(char* str, short x, short y)
{
    char* ret = new char[y + 1];
    for (short i = x; i < x + y; i++)
        ret[i - x] = str[i];
    ret[y] = '\0';
    return ret;
}
```

---
---
---

### 12. C++ TCP Sockets in Linux

### C++ Server
ServerSocket.h file content:
```cpp
/*!
* ServerSocket.h
*
* Created on: Mar 12, 2013
* Author: Tancredi-Paul Grozav (paul@grozav.info)
*/

#ifndef SERVERSOCKET_H_
#define SERVERSOCKET_H_

#include <stdio.h> //perror, stderr, printf, fprintf
#include <cstdlib> // exit
#include <unistd.h> // fork, close
#include <string.h> // memset
#include <netdb.h> // AI_PASSIVE, getaddrinfo, gai_strerror
#include <arpa/inet.h> // inet_ntop
#include <sys/wait.h> // waitpid

/*! This class simplifies the usage of sockets */
class ServerSocket {
public:
    struct ClientSocket {
        //! Connector's address information
        struct sockaddr_storage clientAddr;
        //! Client socket file descriptor
        int connectionFileDescriptor;
    };

    typedef int (*PointerToClientHandlerFunction)(ClientSocket);

    ServerSocket(int port);
    virtual ~ServerSocket();
    /*! This method must take as an argument, the pointer to a function that takes as a parameter a ServerSocket::ClientSocket and returns an int. The int is the PID exit status */
    void acceptClient(PointerToClientHandlerFunction clientHandlerFunction);

private:
    char* portToCharArray(int port);
    int socketFileDescriptor; // listen on socketFileDescriptor,
};

#endif /* SERVERSOCKET_H_ */
```

ServerSocket.cpp file content:
```cpp
/*
 * ServerSocket.cpp
 *
 * Created on: Mar 12, 2013
 * Author: Tancredi-Paul Grozav (paul@grozav.info)
 */

#include "ServerSocket.h"

void ServerSocketSignalChildHandler(int s)
{
    while (waitpid(-1, NULL, WNOHANG) > 0)
        ;
}

ServerSocket::ServerSocket(int port)
{
    //Creating and filling the hints structure, used to create the servinfo structure
    struct addrinfo hints; // filled out with relevant information
    memset(&hints, 0, sizeof hints); // make sure the struct is empty
    hints.ai_family = AF_UNSPEC; // don't care IPv4 or IPv6
    hints.ai_socktype = SOCK_STREAM; // TCP stream sockets
    hints.ai_flags = AI_PASSIVE; // use my IP

    // make servinfo point to a linked list of 1 or more struct addrinfos
    struct addrinfo *servinfo; // will point to the results (holds the interfaces)
    int rv;
    if ((rv = getaddrinfo(NULL, portToCharArray(port), &hints, &servinfo)) != 0)
    {
        fprintf(stderr, "getaddrinfo: %s\n", gai_strerror(rv));
        exit(1);
    }

    // loop through all the results and bind to the first we can
    struct addrinfo *p;
    for (p = servinfo; p != NULL; p = p->ai_next)
    {
        if ((socketFileDescriptor = socket(p->ai_family, p->ai_socktype,
            p->ai_protocol)) == -1)
        {
            perror("server: Can not call socket() on that interface");
            continue;
        }

        int yes = 1;
        if (setsockopt(socketFileDescriptor, SOL_SOCKET, SO_REUSEADDR, &yes,
            sizeof(int)) == -1)
        {
            perror("Can not call setsockopt() on that interface");
            exit(1);
        }

        if (bind(socketFileDescriptor, p->ai_addr, p->ai_addrlen) == -1)
        {
            close (socketFileDescriptor);
            perror("server: Can not call bind() on that interface");
            continue;
        }

        break;
    }

    // If we reached the end of the list
    if (p == NULL)
    {
        fprintf(stderr, "server: failed to bind\n");
        exit(2);
    }
    // free the servinfo variable
    freeaddrinfo(servinfo); // all done with this structure
    // we now have the sockfd structure

    int backLog = 10; // how many pending connections queue will hold

    if (listen(socketFileDescriptor, backLog) == -1)
    {
        perror("Can not listen() on that socket");
        exit(1);
    }

    // reap all dead processes
    // This code is responsible for reaping zombie processes
    // that appear as the fork()ed child processes exit.
    // If you make lots of zombies and don't reap them,
    // your system administrator will become agitated.
    struct sigaction sa;
    sa.sa_handler = &ServerSocketSignalChildHandler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = SA_RESTART;
    if (sigaction(SIGCHLD, &sa, NULL) == -1)
    {
        perror("sigaction");
        exit(1);
    }
}

ServerSocket::~ServerSocket()
{
}

void ServerSocket::acceptClient(
    PointerToClientHandlerFunction clientHandlerFunction)
{
    ServerSocket::ClientSocket clientSocket;
    char s[INET6_ADDRSTRLEN];
    socklen_t sin_size = sizeof clientSocket.clientAddr;
    clientSocket.connectionFileDescriptor = accept(socketFileDescriptor,
        (struct sockaddr *) &clientSocket.clientAddr, &sin_size);
    if (clientSocket.connectionFileDescriptor == -1)
    {
        perror("Can not accept() the connection");
        // continue;
    }
    printf("Connection accepted\n");

    if (((struct sockaddr *) &clientSocket.clientAddr)->sa_family == AF_INET)
    { //IPv4 address
        inet_ntop(clientSocket.clientAddr.ss_family,
            &(((struct sockaddr_in*) ((struct sockaddr *) &clientSocket
                .clientAddr))->sin_addr), s, sizeof s);
    }
    else
    { //IPv6 address
        inet_ntop(clientSocket.clientAddr.ss_family,
            &(((struct sockaddr_in6*) ((struct sockaddr *) &clientSocket
                .clientAddr))->sin6_addr), s, sizeof s);
    }
    printf("Got connection from %s\n", s);

    pid_t pid = fork();
    if (pid == -1)
    {
        perror("Can not fork()");
    }
    else if (pid == 0)
    { // this is the child process
        exit(clientHandlerFunction(clientSocket));
    }
    else
    {
        printf("pid = %d > 0", pid);
    }
    close(clientSocket.connectionFileDescriptor); // parent doesn't need this
}

char* ServerSocket::portToCharArray(int port)
{
    int base = 10;
    static char buf[32] =
    { 0 };
    int i = 30;
    for (; port && i; --i, port /= base)
        buf[i] = "0123456789abcdef"[port % base];
    return &buf[i + 1];
}
```
main.cpp file content:
```cpp
/*!
 * main.cpp
 *
 * Created on: Mar 12, 2013
 * Author: Tancredi-Paul Grozav (paul@grozav.info)
 */
#include "ServerSocket.h"

/*! Handles a client. The int returned is the exit status of the PID created for that client. */
int clientHandler(ServerSocket::ClientSocket clientSocket)
{
// Receiving message
    char buffer[1024];
    printf("Waiting for message from client ...");
    if (recv(clientSocket.connectionFileDescriptor, &buffer, sizeof(buffer), 0)
        == -1)
        perror("Can not read from the socket");
    printf(" Client said: "%s"\n", buffer);

// Sending message
    char message[1034];
    strcpy(message, "You said: ");
    strcat(message, buffer);
    printf("char* message = "%s"\n", message);
    printf("Sending a message to the client ...");
    if (send(clientSocket.connectionFileDescriptor, message, strlen(message), 0)
        == -1)
        perror("Can not write the message to the socket");
    printf(" Message sent\n");

// Closing the socket
    printf("Disconnecting the client ...");
    close(clientSocket.connectionFileDescriptor);
    printf(" Socket closed!\n");
    return 0;
}

int main(int argc, char* argv[])
{
    ServerSocket s(1234);
    printf("server: waiting for connections...\n");
    while (1)
        s.acceptClient(&clientHandler);
    return 0;
}
```

### C / C++ Client
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netdb.h>

void die(const char *msg)
{
// Print the message to standart error output stream
    perror(msg);
// Exit the program with the errorCode 1
    exit(1);
}

typedef void (*PointerToHandlerFunction)(int socketFileDescriptor);

void clientSocket_connect(const char* hostAddress, int port,
    PointerToHandlerFunction handler)
{
// Creating the socketFileDescriptor
    int socketFileDescriptor = socket(AF_INET, SOCK_STREAM, 0);
// Die if the socketFileDescriptor could not be created
    if (socketFileDescriptor < 0)
        die("ERROR opening socket");

// Get the server host
    struct hostent* server = gethostbyname(hostAddress);
// Die if the server info could not be loaded
    if (server == NULL)
        die("ERROR, no such host\n");

// Create the serverAddress structure
// Define the serverAddress structure
    struct sockaddr_in serverAddress;
// Zero-fill the structure
    bzero((char*) &serverAddress, sizeof(serverAddress));
// Set the sin_family member
    serverAddress.sin_family = AF_INET;
// Set the HOST ADDRESS
    bcopy((char*) server->h_addr, (char*)&serverAddress.sin_addr.s_addr, server->h_length);
// Set the PORT NUMBER
    serverAddress.sin_port = htons(port);

// Connect to the server using the serverAddress structure or die
    if (connect(socketFileDescriptor, (struct sockaddr*) &serverAddress,
        sizeof(serverAddress)) < 0)
        die("ERROR connecting");

// Call the handler function
    handler(socketFileDescriptor);

// Close the socket
    close(socketFileDescriptor);
}

void connectionHandler(int socketFileDescriptor)
{
// Send data
// Define the buffer
    char buffer[256];
// Set the message in that buffer (bytes to be sent)
    strcpy(buffer,
        "GET /maya/ HTTP/1.1nhost: debian.server.paul.grozav.infon\n");
// Write the bytes to the socket
    int n = write(socketFileDescriptor, buffer, strlen(buffer));
// Die if you couldn't write those bytes
    if (n < 0)
        die("ERROR writing to socket");

// Clear the buffer - we'll read data to the same buffer
    bzero(buffer, 256);

// Read data to buffer
// Read from socket and store to buffer
    n = read(socketFileDescriptor, buffer, 255);
// Die if you couldn't read
    if (n < 0)
        die("ERROR reading from socket");

// Print the received bytes
    printf("%s\n", buffer);
}

int main(int argc, char* argv[])
{
    clientSocket_connect("debian.server.paul.grozav.info", 80,
        &connectionHandler);
    return 0;
}
```

---
---
---

### 13. Writing a structure to binary buffer and then reading it back

```cpp
#include <iostream> // for cout and endl
#include <cstring> // for memcpy
struct header {
    char a;
    unsigned int b;
};
int main(int argc, char* argv[]){
    // Creating a structure and filling it with data
    header h;
    h.a = 51;
    h.b = 3147483647;// greater than MAX_VALUE for signed int (2147483647)

    // Create a buffer, and write the structure
    char buffer[ sizeof(h.a) + sizeof(h.b) ];// 1 for char + 4 for int = 5 bytes
    buffer[0]=h.a;
    memcpy( buffer+1, &h.b, sizeof(h.b) );

    // Printing the buffer
    std::cout.write( buffer, 5 );
    std::cout << std::endl;

    // Create a new structure using the data from the buffer
    header n;
    n.a = buffer[0];
    // unsigned int* pointerToUnsignedInt = (unsigned int*)(buffer + 1);
    // n.b = *pointerToUnsignedInt;
    // Or simply:
    n.b = *((unsigned int*)(buffer + 1));//value of (pointer to (unsigned int) )

    // Print the new structure
    std::cout << "n.a = " << n.a << " (as char) = " << (int)n.a << " (as int)" << std::endl;
    std::cout << "n.b=" << n.b << std::endl;

    return 0;
}
```
Or even shorter :
```cpp
#include <iostream> // for cout and endl
#include <cstring> // for memcpy
struct header {
    char a;
    unsigned int b;
};
int main(int argc, char* argv[]){
    // Creating a structure and filling it with data
    header h;
    h.a = 51;
    h.b = 3147483647;// greater than MAX_VALUE for signed int (2147483647)

    // Create a buffer, and write the structure
    char* buffer = (char*)&h;

    // Printing the buffer
    std::cout.write( buffer, sizeof(h) );
    std::cout << std::endl;

    // Create a new structure using the data from the buffer
    header n = *((header*)buffer);//value of (pointer to (header) )

    // Print the new structure
    std::cout << "n.a = " << n.a << " (as char) = " << (int)n.a << " (as int)" << std::endl;
    std::cout << "n.b = " << n.b << std::endl;

    return 0;
}
```

---
---
---

### 14. C++ time measuring

This measures the <a title="wall-clock time" href="http://en.wikipedia.org/wiki/Wall-clock_time">wall-clock time</a>(Real time from start to finish - with interruptions) :
```cpp
#include <iostream>
#include <sys/time.h>
using namespace std;
int main(int argc, char* argv[]){
    timeval t1;
    gettimeofday( &t1, NULL );
    cout << "Program running "; cout.flush();
    for(int a=0; a<3; a++){
        for(int b=0; b<10000; b++)
            for(int c=0; c<100000; c++){}
                cout << "."; cout.flush();
    }
    cout << "nProgram END !n";
    timeval t2;
    gettimeofday( &t2, NULL );
    timeval delta;
    delta.tv_sec = t2.tv_sec - t1.tv_sec;
    delta.tv_usec = t2.tv_usec - t1.tv_usec;
    if(delta.tv_usec < 0){
        delta.tv_usec += 1000000;
        delta.tv_sec -= 1;
    }
    cout << "Runtime: " << delta.tv_sec << " seconds and " << delta.tv_usec << " microseconds (that is 1/(10^6) seconds)" << endl;
    return 0;
}
```
For measuring the <a title="CPU time" href="http://en.wikipedia.org/wiki/CPU_time">CPU Time</a>(Time the CPU uses to do the job - without being interrupted) use the clock_t type and the clock() function:
`clock_t timeStart = clock();`

---

"The time returned by gettimeofday() is affected by discontinuous jumps in the system time (e.g., if the system administrator manually changes the system time). If you need a monotonically increasing clock, see clock_gettime(2). " According to <a href="http://linux.die.net/man/2/gettimeofday">http://linux.die.net/man/2/gettimeofday</a>
So, here's an example with clock_gettime() for both wall-clock time and CPU time:
```cpp
#include <iostream>
#include <time.h> // for clock_gettime()
#include <unistd.h> // for sleep()

using namespace std;

int main(int argc, char* argv[])
{
    struct timespec start, end;
    unsigned long long int diff;

    // Measure monotonic time
    clock_gettime(CLOCK_MONOTONIC, &start); // start
    sleep(1); // do stuff
    clock_gettime(CLOCK_MONOTONIC, &end); // end
    // print Delta
    diff = 1000000000L * (end.tv_sec - start.tv_sec) + end.tv_nsec - start.tv_nsec;
    cout << "elapsed time = " << diff << " nanoseconds" << endl;

    // Measure CPU time
    clock_gettime(CLOCK_PROCESS_CPUTIME_ID, &start); // start
    sleep(1); // do stuff
    clock_gettime(CLOCK_PROCESS_CPUTIME_ID, &end); // end
    // print Delta
    diff = 1000000000L * (end.tv_sec - start.tv_sec) + end.tv_nsec - start.tv_nsec;
    cout << "elapsed process CPU time = " << diff << " nanoseconds" << endl;

    return 0;
}
```

---
---
---

### 15. Bytes to ASCII and binary

The following program writes the ASCII value and the binary value for each byte read from the inputFile.

```cpp
//
// bytesToAsciiAndBinary.cpp
//
// Created on: Apr 3, 2013
// Author: Tancredi-Paul Grozav (paul@grozav.info)
//
#include <iostream> // for cout
#include <fstream> // for ifstream, ofstream
#include <bitset> // for converting to binary
#include <limits.h> // for converting to binary (CHAR_BIT)
#include <cstring> // for strcmp
using namespace std;
/*!
Parameters:
./bytesToAsciiAndBinary inputFile [outputFile]
For each byte in the inputFile, it writes it's ASCII value and it's binary value in the outputFile, separated by a space (" ")
If the outputFile parameter is missing then it prints the values to stdout.
*/
int main(int argc, char* argv[]){
    if(argc == 1){
        cerr << "Please specify an inputFile as the first parameter" << endl;
    }else if(argc >= 2){
        if( strcmp(argv[1], "-h") == 0 ){
            cout << "./bytesToAsciiAndBinary inputFile [outputFile]nFor each byte in the inputFile, it writes it's ASCII value and it's binary value in the outputFile, separated by a space (" ")nIf the outputFile parameter is missing then it prints the values to stdout." << endl;
            return 0;
        }
        char* inputFileName = argv[1];
        ifstream inputFile( inputFileName );
        ofstream outputFile;
        if(argc == 3) outputFile.open( argv[2] );
        char c;//character read from the file
        while( inputFile.good() ){//loop while extraction from file is possible
            c = inputFile.get();//read the caracter from the file
            if(argc == 2)
                cout << (short int)((unsigned char)c) << " " << std::bitset<CHAR_BIT>( c ) << endl;
            if(argc == 3)
                outputFile <<(short int)((unsigned char)c) << " " << std::bitset<CHAR_BIT>( c ) << endl;
        }
        inputFile.close();
        if(argc == 3) outputFile.close();
    }
}
```
Here is the first ten lines of output you would get if you run the program on it’s source code:

```bash
paul@grozav:~/Desktop$ ./bytesToAsciiAndBinary bytesToAsciiAndBinary.cpp
47 00101111
47 00101111
10 00001010
47 00101111
47 00101111
32 00100000
98 01100010
121 01111001
116 01110100
101 01100101
```

You can do something similar with BASH:
The following command prints the ASCII characters 65 (A) and 66 (B) and 67(C) on stdout – without a n (new line) at the end
```bash
paul@devel:~$ for c in 65 66 67; do printf $(printf '%03ot' "$c"); done
ABCpaul@devel:~$
```


---
---
---

### 16. C++ Utils

Useful functions when working in C++

Contents of **Utils.h**:
```cpp
//
// Utils.h
//
// Created on: Aug 29, 2012
// Author: Tancredi-Paul Grozav (paul@grozav.info)
//

#ifndef UTILS_H_
#define UTILS_H_

#include <string>
#include <vector>

using namespace std;

namespace gtp{
//! Other functions needed through the program (Functions created by Tancredi-Paul Grozav - paul@grozav.info )
class Utils {
public:
    Utils();
    virtual ~Utils();

    //! Returns the position of the first digit, based on isdigit()
    static int firstDigitPosition(string s);
    //! Right trim
    static string rtrim(string s);
    //! Left trim
    static string ltrim(string s);
    //! Left and right trim
    static string trim(string s);

    //! Converts a given int to string
    static string intToStr(int v);
    //! Converts a given double to string
    static string doubleToStr(double v);
    //! Converts a given long to string
    static string longToStr(long v);
    //! Converts a given string to char*
    static char* stringToCharArr(string s);
    //! Executes the given command on the current operating system
    static int run(string command);
    //! Checks if the specified file exists or not
    static bool fileExists(string filename);
    //! Checks if the specified directory exists or not
    static bool directoryExists(string dirName);
    //! Converts a given month to it's code value
    static string monthToCode(string month);
    //! Explodes a string by a given character. Ex: eplodeStringByCharacter("ana,are,mere", ',')={"ana", "are", "mere"}
    static vector<string> explodeStringByCharacter(string str, char ch);
    //! Reads the content of a given file, into a string variable, and returns it's value
    static string readFileToString(string file);
    //! Adds as many "0"es as needed in order to represent the given on n digits
    static string nDigits(int nr, unsigned long n);
    //! Removes all occurences of character c in string s
    static string removeCharacter(string s, char c);
    //! Returns all characters from string s that come after the first character left and the first character right
    static string betweenChars(string s, char left, char right);
    //! Returns a string of length n, filled with character c
    static string returnNTimesChar(char c, unsigned long n);
};

}//! Ending namespace gtp
#endif//! UTILS_H_
```

Contents of **Utils.cpp**:
```cpp
//
// Utils.cpp
//
// Created on: Aug 29, 2012
// Author: Tancredi-Paul Grozav <paul@grozav.info>
//

#include "Utils.h"
#include <algorithm>
#include <iostream>
#include <string>
#include <string.h>//for strcpy
#include <vector>
#include <sstream>
#include <fstream>
#include <cstdlib>
#include <sys/stat.h>

using namespace std;

namespace gtp {

Utils::Utils(){}
Utils::~Utils(){}

string Utils::intToStr(int v) {
    // Create a stringstream
    stringstream ss;
    // Add number to the stream
    ss << v;
    // Return a string with the contents of the stream
    return ss.str();
}

string Utils::doubleToStr(double v) {
    // Create a stringstream
    stringstream ss;
    // Add number to the stream
    ss << v;
    // Return a string with the contents of the stream
    return ss.str();
}

string Utils::longToStr(long v) {
    // Create a stringstream
    stringstream ss;
    // Add number to the stream
    ss << v;
    // Return a string with the contents of the stream
    return ss.str();
}

char* Utils::stringToCharArr(string s){
    char* buf = new char[s.size()];
    strcpy(buf, s.c_str());
    return buf;
}

int Utils::firstDigitPosition(string s){
    for(unsigned int i = 0; i < s.size(); i++)
        if( isdigit( s.at(i) ) )
            return i;
    return -1;
}

// TRIM FUNCTIONS
string Utils::rtrim(string s){
    while(s.at(s.size()-1) == ' ')
        s = s.substr(0, s.size()-1);
    return s;
}
string Utils::ltrim(string s){
    while(s.at(0) == ' ')
        s = s.substr(1, s.size()-1);
    return s;
}
string Utils::trim(string s){return ltrim(rtrim(s)); }

int Utils::run(string command) {
    return system(command.c_str());
}

bool Utils::fileExists(string fileName){
    ifstream f(fileName.c_str());
    return f;
}
bool Utils::directoryExists(string dirName){
    struct stat myStat;
    return (stat(dirName.c_str(), &myStat) == 0) && (((myStat.st_mode) & S_IFMT) == S_IFDIR);
}

string Utils::monthToCode(string month){
    if(month == "01") return "F";
    else if(month == "02") return "G";
    else if(month == "03") return "H";
    else if(month == "04") return "J";
    else if(month == "05") return "K";
    else if(month == "06") return "M";
    else if(month == "07") return "N";
    else if(month == "08") return "Q";
    else if(month == "09") return "U";
    else if(month == "10") return "V";
    else if(month == "11") return "X";
    else if(month == "12") return "Z";
    else return "?";
}

vector<string> Utils::explodeStringByCharacter(string str, char ch) {
    std::string next = "";
    std::vector<std::string> result;
    // For each character in the string
    for (unsigned int i = 0; i < str.size(); i++) {
        // If we've hit the terminal character
        if (str.at(i) == ch) {
            // If we have some characters accumulated
                if (next.length() > 0) {
                    // Add them to the result vector
                    result.push_back(next);
                    next = "";
                }
        } else {
            // Accumulate the next character into the sequence
            next += str.at(i);
        }
    }
    result.push_back(next);
    return result;
}

string Utils::readFileToString(string file){
    ifstream f(file.c_str());
    // Buffer
    stringstream b;
    b << f.rdbuf();
    return b.str();
}

string Utils::nDigits(int nr, unsigned long n){
    string nrStr = intToStr(nr);
    unsigned long nrLen = nrStr.size();
    if(nrLen > n) return "";
    if(nrLen == n) return nrStr;
    string prefix = "";
    for(unsigned long i = 0; i<n-nrLen; i++)
        prefix += "0";
    return prefix+nrStr;
}

string Utils::removeCharacter(string s, char c){
    s.erase(remove(s.begin(), s.end(), c), s.end());
    return s;
}

string Utils::betweenChars(string s, char left, char right){
    unsigned int pos = s.find_first_of(left);
    if (pos == string::npos) return s;
    string st = s.substr(pos+1, s.size()-pos-1);
    pos = st.find_first_of(right);
    if (pos == string::npos) return s;
    st = st.substr(0, pos);
    return st;
}

string Utils::returnNTimesChar(char c, unsigned long n){
    if(n > 10){
        std::cout << "You asked to return " << n << " times the caracter '" << c << "'. That is weird!" <<std::endl;
    }
    string r = "";
    for(; n >= 1; n--){
        r += c;
    }
    return r;
}

}// Ending Name space

```

---
---
---

### 17. Archives under linux

#### Creating archives
Create a .tgz file containing files from the current directory:
```bash
tar -zcf archive.tgz file1 directory1 file2 directory2
```
The options `cf` tells the program to `c`reate an archive and write it to a `f`ile. This does no compression to your files, it just writes all the files to one archive file. If you want to compress the files, you can add the `z` option, `z` stands for ZIP compression algorithm.

#### Inspecting archives
See the files that are inside an archive:
```bash
tar -tf archive.tgz
```
The options `tf` tells the program to lis`t` the files from the archive `f`ile. If you want to view more details about the files inside the archive you can add the `v` option, `v` stands for verbose.

#### Extracting archives
1. Extract a .tgz file from the current directory, to the current directory:
```bash
tar -xf archive.tgz
```
The options `xf` tells the program to e`x`tract from a `f`ile. If you want to see the extracted files, you can add the `v` option, `v` stands for verbose.

2. Extract all .tgz files from the current directory, to the current directory and then remove the archives:
```bash
for f in $(ls -1 *.tgz); do tar -xvf $f; rm -f $f; done
```

---
---
---

### 18. Function "returning multiple types"

O.K., so even though this function is not returning multiple types ... we could say it does. It actually returns a `void*`, that is a void pointer. In other words, you can return a pointer to anything, any kind of object / structure / type.

```cpp
#include <iostream>
using namespace std;

class Number{
public:
    Number(int a){ x=a; }
    friend ostream& operator<<(ostream& os, const Number& s);
    int x;
};
ostream& operator<<(ostream& os, const Number& s){
    os << "Number(" << s.x << ")"; return os;
}

class String{
public:
    String(string a){ x=a; }
    friend ostream& operator<<(ostream& os, const String& s);
    string x;
};
ostream& operator<<(ostream& os, const String& s){
    os << s.x; return os;
}

void* giveMeA(string what){
    if(what == "string"){
        return new String("This is a String sample");
    }else if(what == "number"){
        return new Number(230001);
    }
}

int main(int argc, char* argv[]){
    String s = *((String*)giveMeA("string"));
    cout << s << endl;
    Number n = *((Number*)giveMeA("number"));
    cout << n << endl;
    return 0;
}
```

Outputs:

```bash
This is a String sample
Number(230001)
```

---
---
---

### 19. C++ - Make a POST HTTP request over a socket

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netdb.h>

void die(const char *msg){
// Print the message to standard error output stream
perror(msg);
// Exit the program with the errorCode 1
exit(1);
}

typedef void (*PointerToHandlerFunction)(int socketFileDescriptor);

void clientSocket_connect(const char* hostAddress, int port, PointerToHandlerFunction handler){
// Creating the socketFileDescriptor
int socketFileDescriptor = socket(AF_INET, SOCK_STREAM, 0);
// Die if the socketFileDescriptor could not be created
if(socketFileDescriptor < 0) die("ERROR opening socket");

// Get the server host
struct hostent* server = gethostbyname(hostAddress);
// Die if the server info could not be loaded
if(server == NULL) die("ERROR, no such host\n");

// Create the serverAddress structure
// Define the serverAddress structure
struct sockaddr_in serverAddress;
// Zero-fill the structure
bzero((char*)&serverAddress, sizeof(serverAddress));
// Set the sin_family member
serverAddress.sin_family = AF_INET;
// Set the HOST ADDRESS
bcopy((char*)server->h_addr, (char*)&serverAddress.sin_addr.s_addr, server->h_length);
// Set the PORT NUMBER
serverAddress.sin_port = htons(port);

// Connect to the server using the serverAddress structure or die
if (connect(socketFileDescriptor, (struct sockaddr*)&serverAddress, sizeof(serverAddress)) < 0) die("ERROR connecting");

// Call the handler function
handler(socketFileDescriptor);

// Close the socket
close(socketFileDescriptor);
}

void connectionHandler(int socketFileDescriptor){
// Send data
// Define the buffer
char buffer[2048];
// Set the message in that buffer (bytes to be sent)
// strcpy(buffer, "GET /maya/ HTTP/1.1\r\nhost: debian.server.paul.grozav.info\r\n\r\n"); // GET request

// POST Request
// strcpy(buffer, "POST /php/post.php HTTP/1.1\r\nContent-Type: application/x-www-form-urlencoded\r\nHost: debian.server.paul.grozav.info\r\nContent-Length: 32\r\n\r\nusername=grozav1&password=paul12");
strcpy(buffer, "POST /php/post.php HTTP/1.1\r\n");
strcat(buffer, "Content-Type: application/x-www-form-urlencoded\r\n");
strcat(buffer, "Host: debian.server.paul.grozav.info\r\n");
strcat(buffer, "Content-Length: 32\r\n");
strcat(buffer, "\r\n");
strcat(buffer, "username=grozav1&password=paul12");
// Write the bytes to the socket
int n = write(socketFileDescriptor, buffer, strlen(buffer));
// Die if you couldn’t write those bytes
if(n < 0) die("ERROR writing to socket");

// Clear the buffer – we’ll read data to the same buffer
bzero(buffer,256);

// Read data to buffer
// Read from socket and store to buffer
n = read(socketFileDescriptor, buffer, 255);
// Die if you couldn’t read
if(n < 0) die("ERROR reading from socket");

// Print the received bytes
printf("%s\n", buffer);
}

int main(int argc, char* argv[]){
// clientSocket_connect("debian.server.paul.grozav.info", 80, &connectionHandler);
clientSocket_connect("debian.server.paul.grozav.info", 80, &connectionHandler);
return 0;
}
```

---
---
---

### 20. Get CSV from BhavCopy Commodity Wise

Did you ever want to get data from http://www.mcxindia.com/SitePages/BhavCopyCommoditywiseArchive.aspx ? I did! And I made this code to help you all. The script downloads all CSV files from that URL and saves them to the local disk. To do that it takes about 3.5 hours (on my PC). You must have a folder named output in the current directory, then the script will create there a folder where it will put all the .CSV files.

```php
#!/usr/bin/php5
<?php
/*
Author: Tancredi-Paul Grozav (paul@grozav.info)
This script should help download data from mcxindia
*/
define('SPEAK', true);
define('WEB_APP_URL', 'http://www.mcxindia.com/SitePages/BhavCopyCommoditywiseArchive.aspx');
define('OUTPUT_FILE_PATH', 'output/' . date('YmdHis')); //Whenever you run the script, it creates a folder containing all the files
mkdir(OUTPUT_FILE_PATH); //create the folder to hold all files

// ! Check for requirements

if (!in_array('curl', get_loaded_extensions())) die("n" . 'CURL is not installed. Fatal error: Can not check for weather image');

// Utils functions

function gzdecode($data, &$filename = '', &$error = '', $maxlength = null)
    {
    $len = strlen($data);
    if ($len < 18 || strcmp(substr($data, 0, 2) , "x1fx8b"))
        {
        $error = "Not in GZIP format.";
        return null; // Not GZIP format (See RFC 1952)
        }

    $method = ord(substr($data, 2, 1)); // Compression method
    $flags = ord(substr($data, 3, 1)); // Flags
    if ($flags & 31 != $flags)
        {
        $error = "Reserved bits not allowed.";
        return null;
        }

    // NOTE: $mtime may be negative (PHP integer limitations)

    $mtime = unpack("V", substr($data, 4, 4));
    $mtime = $mtime[1];
    $xfl = substr($data, 8, 1);
    $os = substr($data, 8, 1);
    $headerlen = 10;
    $extralen = 0;
    $extra = "";
    if ($flags & 4)
        {

        // 2-byte length prefixed EXTRA data in header

        if ($len - $headerlen - 2 < 8)
            {
            return false; // invalid
            }

        $extralen = unpack("v", substr($data, 8, 2));
        $extralen = $extralen[1];
        if ($len - $headerlen - 2 - $extralen < 8)
            {
            return false; // invalid
            }

        $extra = substr($data, 10, $extralen);
        $headerlen+= 2 + $extralen;
        }

    $filenamelen = 0;
    $filename = "";
    if ($flags & 8)
        {

        // C-style string

        if ($len - $headerlen - 1 < 8)
            {
            return false; // invalid
            }

        $filenamelen = strpos(substr($data, $headerlen) , chr(0));
        if ($filenamelen === false || $len - $headerlen - $filenamelen - 1 < 8)
            {
            return false; // invalid
            }

        $filename = substr($data, $headerlen, $filenamelen);
        $headerlen+= $filenamelen + 1;
        }

    $commentlen = 0;
    $comment = "";
    if ($flags & 16)
        {

        // C-style string COMMENT data in header

        if ($len - $headerlen - 1 < 8)
            {
            return false; // invalid
            }

        $commentlen = strpos(substr($data, $headerlen) , chr(0));
        if ($commentlen === false || $len - $headerlen - $commentlen - 1 < 8)
            {
            return false; // Invalid header format
            }

        $comment = substr($data, $headerlen, $commentlen);
        $headerlen+= $commentlen + 1;
        }

    $headercrc = "";
    if ($flags & 2)
        {

        // 2-bytes (lowest order) of CRC32 on header present

        if ($len - $headerlen - 2 < 8)
            {
            return false; // invalid
            }

        $calccrc = crc32(substr($data, 0, $headerlen)) & 0xffff;
        $headercrc = unpack("v", substr($data, $headerlen, 2));
        $headercrc = $headercrc[1];
        if ($headercrc != $calccrc)
            {
            $error = "Header checksum failed.";
            return false; // Bad header CRC
            }

        $headerlen+= 2;
        }

    // GZIP FOOTER

    $datacrc = unpack("V", substr($data, -8, 4));
    $datacrc = sprintf('%u', $datacrc[1] & 0xFFFFFFFF);
    $isize = unpack("V", substr($data, -4));
    $isize = $isize[1];

    // decompression:

    $bodylen = $len - $headerlen - 8;
    if ($bodylen < 1)
        {

        // IMPLEMENTATION BUG!

        return null;
        }

    $body = substr($data, $headerlen, $bodylen);
    $data = "";
    if ($bodylen > 0)
        {
        switch ($method)
            {
        case 8:

            // Currently the only supported compression method:

            $data = gzinflate($body, $maxlength);
            break;

        default:
            $error = "Unknown compression method.";
            return false;
            }
        } // zero-byte body content is allowed

    // Verifiy CRC32

    $crc = sprintf("%u", crc32($data));
    $crcOK = $crc == $datacrc;
    $lenOK = $isize == strlen($data);
    if (!$lenOK || !$crcOK)
        {
        $error = ($lenOK ? '' : 'Length check FAILED. ') . ($crcOK ? '' : 'Checksum FAILED.');
        return false;
        }

    return $data;
    }

function string_get_before($string, $before)
    {
    return substr($string, 0, strpos($string, $before));
    }

function string_get_after($string, $after)
    {
    $pos = strpos($string, $after) + strlen($after);
    return substr($string, $pos, strlen($string) - $pos);
    }

function string_starts_with($haystack, $needle)
    {
    return !strncmp($haystack, $needle, strlen($needle));
    }

function toPostParameters($pp)
    {
    for ($i = 0; $i < count($pp); $i++)
    foreach($pp[$i] as $key => $value) $pp[$i] = $key . '=' . $value;
    $pp = implode('&', $pp);
    return $pp;
    }

function getPage($header = NULL, $post = '')
    {
    $curlObject = curl_init();
    curl_setopt($curlObject, CURLOPT_URL, WEB_APP_URL);
    curl_setopt($curlObject, CURLOPT_RETURNTRANSFER, 1);
    if ($post !== '')
        {
        curl_setopt($curlObject, CURLOPT_POST, 1);
        curl_setopt($curlObject, CURLOPT_POSTFIELDS, $post);

        // print('POST:'.$post);

        }

    curl_setopt($curlObject, CURLOPT_VERBOSE, true);
    curl_setopt($curlObject, CURLOPT_HEADER, true);
    curl_setopt($curlObject, CURLINFO_HEADER_OUT, true);
    if (!is_null($header)) curl_setopt($curlObject, CURLOPT_HTTPHEADER, $header);
    $r['requestHeader'] = '';
    $r['responseHeader'] = '';
    $r['response'] = curl_exec($curlObject);
    $r['responseHeader'] = substr($r['response'], 0, curl_getinfo($curlObject, CURLINFO_HEADER_SIZE));
    $r['response'] = substr($r['response'], curl_getinfo($curlObject, CURLINFO_HEADER_SIZE));
    $r['requestHeader'] = curl_getinfo($curlObject, CURLINFO_HEADER_OUT);
    return $r;
    }

function saveEventValidationAndViewState($r)
    {
    global $prevRequest;
    $prevRequest['eventValidation'] = rawurlencode(valueExtractor2($r['response'], '__EVENTVALIDATION'));
    $prevRequest['viewState'] = rawurlencode(valueExtractor2($r['response'], '__VIEWSTATE'));
    $prevRequest['cookie'] = implode('; ', getCookieValues($r['responseHeader']));
    $prevRequest['firstExpiryDate'] = getSelectOptionValuesAsArray(string_get_before(string_get_after($r['response'], '<select name="mDdlExpDate" id="mDdlExpDate" class="dd" style="text-transform: uppercase">') , "rnrn"));
    $prevRequest['firstExpiryDate'] = rawurlencode($prevRequest['firstExpiryDate'][0]);
    }

function valueExtractor($from, $tag)
    {
    $r = string_get_after($from, 'id="' . $tag . '" value="');
    $r = string_get_before($r, '" />');
    return $r;
    }

function valueExtractor2($tags, $tag)
    {
    $tags = explode('|', $tags);
    for ($i = 0; $i < count($tags); $i++)
    if ($tags[$i] === $tag) return $tags[$i + 1];
    }

function getCookieValues($headers)
    {
    $headers = explode("n", $headers);
    $r = array();

    // ! For all headers

    foreach($headers as $key => $header)
        {

        // ! If it's a 'Set-Cookie'

        if (string_starts_with($header, 'Set-Cookie: '))
            {

            // ! Set cookie values

            $cookieValues = substr($header, strlen('Set-Cookie: '));
            $cookieValues = explode('; ', $cookieValues);
            array_push($r, $cookieValues[0]);
            }
        }

    return $r;
    }

// downloads and saves the CSV file

function getCSVForSymbolAndDate($symbol, $expiryDate)
    {
    global $prevRequest;
    if (SPEAK) echo 'Getting ' . $symbol . ' for ' . $expiryDate . ' ...';

    // Third request - Clicking OK to get the values in HTML format

    $headers = array();
    array_push($headers, 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8');
    array_push($headers, 'Accept-Encoding: gzip, deflate');
    array_push($headers, 'Accept-Language: en-US,en;q=0.5');
    array_push($headers, 'Cache-Control: no-cache');
    array_push($headers, 'Connection: keep-alive');
    array_push($headers, 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8');
    array_push($headers, 'Cookie: __utma=169759165.1477622451.1368783172.1368783172.1368783172.1; __utmb=169759165.1.10.1368783172; __utmc=169759165; __utmz=169759165.1368783172.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); ' . $prevRequest['cookie'] . '');
    array_push($headers, 'DNT: 1');
    array_push($headers, 'Host: www.mcxindia.com');
    array_push($headers, 'Pragma: no-cache');
    array_push($headers, 'Referer: http://www.mcxindia.com/SitePages/BhavCopyCommoditywiseArchive.aspx');
    array_push($headers, 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:21.0) Gecko/20100101 Firefox/21.0');
    array_push($headers, 'X-MicrosoftAjax: Delta=true');
    $pp = array();
    array_push($pp, array(
        'ScriptManager1' => 'MupdPnl|mBtnGo'
    ));
    array_push($pp, array(
        '__EVENTARGUMENT' => ''
    ));
    array_push($pp, array(
        '__EVENTTARGET' => ''
    ));
    array_push($pp, array(
        '__EVENTVALIDATION' => $prevRequest['eventValidation']
    ));
    array_push($pp, array(
        '__LASTFOCUS' => ''
    ));
    array_push($pp, array(
        '__VIEWSTATE' => $prevRequest['viewState']
    ));
    array_push($pp, array(
        'mBtnGo.x' => '9'
    ));
    array_push($pp, array(
        'mBtnGo.y' => '14'
    ));
    array_push($pp, array(
        'mChkAll' => 'on'
    ));
    array_push($pp, array(
        'mDdlExpDate' => rawurlencode($expiryDate)
    ));
    array_push($pp, array(
        'mDdlSymbol' => $symbol
    ));
    array_push($pp, array(
        'mTbFromDate' => ''
    ));
    array_push($pp, array(
        'mTbToDate' => ''
    ));
    $r = getPage($headers, toPostParameters($pp));
    $r['response'] = gzdecode($r['response']);

    // print_r($r);die();

    saveEventValidationAndViewState($r);

    // Fourth request - get the prices in CSV format

    $headers = array();
    array_push($headers, 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8');
    array_push($headers, 'Accept-Encoding: gzip, deflate');
    array_push($headers, 'Accept-Language: en-US,en;q=0.5');
    array_push($headers, 'Cache-Control: no-cache');
    array_push($headers, 'Connection: keep-alive');
    array_push($headers, 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8');
    array_push($headers, 'Cookie: __utma=169759165.1477622451.1368783172.1368783172.1368783172.1; __utmb=169759165.1.10.1368783172; __utmc=169759165; __utmz=169759165.1368783172.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); ' . $prevRequest['cookie'] . '');
    array_push($headers, 'DNT: 1');
    array_push($headers, 'Host: www.mcxindia.com');
    array_push($headers, 'Pragma: no-cache');
    array_push($headers, 'Referer: http://www.mcxindia.com/SitePages/BhavCopyCommoditywiseArchive.aspx');
    array_push($headers, 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:21.0) Gecko/20100101 Firefox/21.0');
    array_push($headers, 'X-MicrosoftAjax: Delta=true');
    $pp = array();
    array_push($pp, array(
        '__EVENTTARGET' => 'linkButton'
    ));
    array_push($pp, array(
        '__EVENTARGUMENT' => ''
    ));
    array_push($pp, array(
        '__LASTFOCUS' => ''
    ));
    array_push($pp, array(
        '__VIEWSTATE' => $prevRequest['viewState']
    ));
    array_push($pp, array(
        'mDdlSymbol' => $symbol
    ));
    array_push($pp, array(
        'mDdlExpDate' => rawurlencode($expiryDate)
    ));
    array_push($pp, array(
        'mChkAll' => 'on'
    ));
    array_push($pp, array(
        '__EVENTVALIDATION' => $prevRequest['eventValidation']
    ));
    $r = getPage($headers, toPostParameters($pp));

    // print_r($r);die(toPostParameters($pp));

    $CSVFile = $r['response'];
    file_put_contents(OUTPUT_FILE_PATH . '/' . $symbol . '_' . date('Ymd', strtotime($expiryDate)) , $CSVFile);

    // echo "n".$CSVFile;

    if (SPEAK) echo " DONEn";
    }

function getSelectOptionValuesAsArray($options)
    {
    $options = explode("rn", trim($options));
    foreach($options as & $o)
        {
        $o = string_get_after($o, 'value="');
        $o = string_get_before($o, '"');
        }

    return $options;
    }

function getExpiryDatesForSymbol($symbol)
    {
    global $prevRequest;

    // Second request - selecting the symbol - this responds with the expiryDates for that symbol

    $headers = array();
    array_push($headers, 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8');
    array_push($headers, 'Accept-Encoding: gzip, deflate');
    array_push($headers, 'Accept-Language: en-US,en;q=0.5');
    array_push($headers, 'Cache-Control: no-cache');
    array_push($headers, 'Connection: keep-alive');
    array_push($headers, 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8');
    array_push($headers, 'Cookie: __utma=169759165.1477622451.1368783172.1368783172.1368783172.1; __utmb=169759165.1.10.1368783172; __utmc=169759165; __utmz=169759165.1368783172.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); ' . $prevRequest['cookie'] . '');
    array_push($headers, 'DNT: 1');
    array_push($headers, 'Host: www.mcxindia.com');
    array_push($headers, 'Pragma: no-cache');
    array_push($headers, 'Referer: http://www.mcxindia.com/SitePages/BhavCopyCommoditywiseArchive.aspx');
    array_push($headers, 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:21.0) Gecko/20100101 Firefox/21.0');
    array_push($headers, 'X-MicrosoftAjax: Delta=true');
    $pp = array();
    array_push($pp, array(
        'ScriptManager1' => 'MupdPnl|mDdlSymbol'
    ));
    array_push($pp, array(
        '__EVENTARGUMENT' => ''
    ));
    array_push($pp, array(
        '__EVENTTARGET' => 'mDdlSymbol'
    ));
    array_push($pp, array(
        '__EVENTVALIDATION' => $prevRequest['eventValidation']
    ));
    array_push($pp, array(
        '__LASTFOCUS' => ''
    ));
    array_push($pp, array(
        '__VIEWSTATE' => $prevRequest['viewState']
    ));
    array_push($pp, array(
        'mDdlExpDate' => $prevRequest['firstExpiryDate']
    )); //first expiryDate from previous response
    array_push($pp, array(
        'mDdlSymbol' => $symbol
    ));
    array_push($pp, array(
        'mTbFromDate' => ''
    ));
    array_push($pp, array(
        'mTbToDate' => ''
    ));
    $r = getPage($headers, toPostParameters($pp));
    $r['response'] = gzdecode($r['response']);
    saveEventValidationAndViewState($r);

    // Extract expiryDates for that symbol

    $r['response'] = string_get_after($r['response'], '<select name="mDdlExpDate" id="mDdlExpDate" class="dd" style="text-transform: uppercase">');
    return getSelectOptionValuesAsArray(string_get_before($r['response'], "rnrn"));
    }

// these are used whenever you need to make a request

$prevRequest['eventValidation'] = '';
$prevRequest['viewState'] = '';
$prevRequest['cookie'] = '';
$prevRequest['firstExpiryDate'] = ''; //

// Loads the page, in order to get the symbols

$headers = array();
array_push($headers, 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8');
array_push($headers, 'Accept-Encoding: gzip, deflate');
array_push($headers, 'Accept-Language: en-US,en;q=0.5');
array_push($headers, 'Cache-Control: no-cache');
array_push($headers, 'Connection: keep-alive');
array_push($headers, 'DNT: 1');
array_push($headers, 'Host: www.mcxindia.com');
array_push($headers, 'Pragma: no-cache');
array_push($headers, 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:21.0) Gecko/20100101 Firefox/21.0');
$r = getPage($headers);
$r['response'] = gzdecode($r['response']);

// print_r($r);die();
// Save the eventValidation and viewState

$prevRequest['eventValidation'] = rawurlencode(valueExtractor($r['response'], '__EVENTVALIDATION'));
$prevRequest['viewState'] = rawurlencode(valueExtractor($r['response'], '__VIEWSTATE'));
$prevRequest['firstExpiryDate'] = getSelectOptionValuesAsArray(string_get_before(string_get_after($r['response'], '<select name="mDdlExpDate" id="mDdlExpDate" class="dd" style="text-transform: uppercase">') , "rnrn"));
$prevRequest['firstExpiryDate'] = rawurlencode($prevRequest['firstExpiryDate'][0]);

// Extract symbols

$d = $r['response'];
$d = string_get_after($d, '<select name="mDdlSymbol" onchange="javascript:setTimeout('__doPostBack('mDdlSymbol', '') ', 0)" id="mDdlSymbol" class="dd">');
$symbols = getSelectOptionValuesAsArray(string_get_before($d, "rnrn"));

// for($i=0; $i<=136; $i++) if($symbols[$i] != 'TURDESI') unset($symbols[$i]);// RUN ONLY FOR ONE SYMBOL
// for($i=0; $i<=136-3; $i++) unset($symbols[$i]);// RUN ONLY FOR LAST X SYMBOLS
// print_r($symbols);die();// View symbols
// Get all CSV files for all symbols*expiryDates

foreach($symbols as $symbol)
    {
    if (SPEAK) echo 'Getting expiryDates for symbol:' . $symbol . " ...";
    $expiryDates = getExpiryDatesForSymbol($symbol);
    if (SPEAK) echo ' DONE' . "n";
    foreach($expiryDates as $expiryDate) getCSVForSymbolAndDate($symbol, $expiryDate);
    }
```

A .CSV file will look like:

```csv
Date,Commodity Symbol,Contract/Expiry Month,Open(Rs),High(Rs),Low(Rs),Close(Rs),PCP(Rs),Volume(In Lots),Volume(In 000's),Value(In Lakhs),OI(In Lots)
06 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,475.25,490.00,0,0.000 KGS,0.00,0
07 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,461.00,475.25,0,0.000 KGS,0.00,0
08 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,461.00,0,0.000 KGS,0.00,0
09 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
10 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
12 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
13 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
14 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
15 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
16 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
17 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
19 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
20 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
21 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
22 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,447.25,447.25,0,0.000 KGS,0.00,0
23 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,447.25,0,0.000 KGS,0.00,0
24 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
26 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
27 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
28 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
29 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
30 Nov 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
01 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
03 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
04 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
05 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
06 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
07 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
08 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
10 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
11 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
12 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
13 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
14 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
15 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
17 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
18 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
19 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
20 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
21 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
22 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
24 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
26 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
27 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,460.75,460.75,0,0.000 KGS,0.00,0
28 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,474.50,460.75,0,0.000 KGS,0.00,0
29 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,474.50,474.50,0,0.000 KGS,0.00,0
31 Dec 2012,ALMOND,28 Feb 2013,0.00,0.00,0.00,474.50,474.50,0,0.000 KGS,0.00,0
```

---
---
---

### 21. Last entries from firefox history

The firefox history is saved in a sqlite database file.

File Locations:
1. Windows XP - `C:\Documents and Settings\<username>\Application Data\MozillaFirefox\Profiles\<profile folder>\places.sqlite
2. Windows Vista - `C:\Users\<user>\App\DataRoaming\MozillaFirefox\Profiles\<profile folder>\places.sqlite
3. GNU/Linux - `/home/<user>/.mozilla/firefox/<profile folder>/places.sqlite`
4. Mac OS X - `/Users/<user>/Library/Application Support/Firefox/Profiles/default.lov/places.sqlite`

You should go to that directory and execute the following command(linux command) in order to see the last 10 visited web pages:
```bash
sqlite3 places.sqlite "SELECT datetime(moz_historyvisits.visit_date/1000000,’unixepoch’), moz_places.title, moz_places.url FROM moz_places, moz_historyvisits WHERE moz_places.id = moz_historyvisits.place_id ORDER BY moz_historyvisits.visit_date DESC LIMIT 10"
```

To see a list of open tabs for the current session, go to ~/.mozilla/firefox/<profile_folder> and run:
```bash
python2 <<< $’import jsonnf = open(“sessionstore-backups/recovery.js”, “r”)njdata = json.loads(f.read())nf.close()nfor win in jdata.get(“windows”):ntfor tab in win.get(“tabs”):ntti = tab.get(“index”) – 1nttprint tab.get(“entries”)[i].get(“url”)’
```

Tested and works with firefox 35.0.

---
---
---

### 22. Linux date

```bash
# Find the date for today + some interval:
date -d "43 days" +"%Y-%m-%d"

# Find the date for some day + some interval:
date -d "2012-05-22 + 23 days" +"%Y-%m-%d"

# Find the first next sunday, after a given date. In this example, the next sunday after 2013-05-10 is 2013-05-12:
date -d "20130510 + $(( 7 - $(date -d 20130510 "+%u") )) days" +"%Y %m %d"
# Or, this: echo '2015-02-02 +'{1..7}' days' | xargs -n1 date -d | grep Sun
```

---
---
---

### 23. SSH simplified authentication

Add structures like this one to `~/.ssh/config`:
```bash
Host server1
HostName server1.mydomain.com
Port 8131
User horg
```

Now when you’ll say: `ssh server1`, the ssh program will know you mean: `ssh -p 8131 horg@server1.mydomain.com`. That means that you can provide an alias for your hostname and it will know the default user, and port.

---
---
---

### 24. bits/atomicity.h: No such file or directory

If you get this error while compiling a .cpp file:
```bash
fatal error: bits/atomicity.h: No such file or directory
```
You can solve it by making a link from ext to bits.
```bash
paul@grozav:/usr/include/c++/4.6/bits$ sudo ln -s ../ext/atomicity.h .
```
Short solution:
```bash
( cd /usr/include/c++/$(g++ -v 2>&1 | grep version | awk '{print $3}')/bits ; ln -s ../ext/atomicity.h . )
```

---
---
---

### 25. C++ output operator

Ever wanted to print a class?

```cpp
#include <iostream>
using namespace std;

class Car{
public:
    Car(string m, int ns);
    friend ostream& operator<<(ostream &out, const Car& c);
private:
    int numberOfSeats;
    string manufacturer;
};

Car::Car(string m, int ns){
    numberOfSeats = ns;
    manufacturer = m;
}

ostream& operator<<(ostream &out, const Car& c)
{
    out << "Car{" << endl;
    out << "tmanufacturer: " << c.manufacturer << endl;
    out << "tnumberOfSeats: " << c.numberOfSeats << endl;
    out << "}" << endl;
    return out;
}

int main(int argc, char* argv[]){
    Car t("Dacia", 29);
    cout << t << endl;
    return 0;
}
```
Would print:
```cpp
paul@grozav:~/Desktop$ g++ -o oo oo.cpp && ./oo
Car{
manufacturer: Dacia
numberOfSeats: 29
}

paul@grozav:~/Desktop$
```

---
---
---

### 26. C++ singleton class

Here is an example of a singleton class. That is a class that only allows you to create one object from it. So that you have only one instance in your program.
```cpp
#include <iostream>
using namespace std;

class Processor
{
public:
static Processor& getInstance();
void process();
private:
Processor();
~Processor();
Processor(const Processor&);// Prevent copy-construction
Processor& operator=(const Processor&);// Prevent assignment
};

Processor& Processor::getInstance()
{
static Processor p;
return p;
}

void Processor::process()
{
cout << "Processing ..." << endl;
}

Processor::Processor()
{
cout << "Process constructor" << endl;
}

Processor::~Processor()
{
cout << "Process destructor" << endl;
}

int main(int argc, char* argv[])
{
Processor& p = Processor::getInstance();
p.process();

Processor& y = Processor::getInstance();
y.process();
return 0;
}
```
This program prints:
```bash
paul@grozav:~/Desktop$ g++ Singleton.cpp -o Singleton && ./Singleton
Process constructor
Processing ...
Processing ...
Process destructor
```

---
---
---

### 27. PHP CLI arguments
Ever wanted to group parameters that you receive? Here’s a method:

```php
<?php
function processArguments($groupDelimiter = '-'){
    global $argv;
    array_shift($argv);//remove the script name
    $delimiterLength = strlen($groupDelimiter);
    $args = array();
    $lastGroup = null;
    foreach($argv as $arg){
        if(substr($arg, 0, $delimiterLength) == $groupDelimiter){
            $lastGroup = substr($arg, $delimiterLength, strlen($arg)-$delimiterLength);
            $args[$lastGroup] = array();
        }else{
            if($lastGroup === null)
                $args[$arg] = $arg;
            else
                array_push($args[$lastGroup], $arg);
        }
    }
    return $args;
}
```
Result:
```bash
paul@samsung:/home/paul$ ./run.php free element -list item1 item2 ... -empty_list
Array
(
    [free] => free
    [element] => element
    [list] => Array
    (
        [0] => item1
        [1] => item2
        [2] => ...
    )

    [empty_list] => Array
    (
    )

)
```

---
---
---

### 28. GIT CLI branch history viewer

```bash
git log --graph --abbrev-commit --decorate --format=format:’%C(bold blue)%h%C(reset) -- %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n” %C(white)%s%C(reset) %C(dim white)- %an%C(reset)’ --all
```
Thanks to: http://stackoverflow.com/a/9074343

---
---
---

### 29. diff two command outputs

Ever wanted to diff two command outputs? like ls -l . Here’s how you can do it:
```bash
diff <(ls -l NEW/.tgz) <(ls -l OLD/.tgz)
```

---
---
---

### 30. C++ UDP Socket in linux

Write to multicast
```cpp
#include <stdio.h> // perror
#include <arpa/inet.h> // inet_addr
#include <stdlib.h> // exit, atoi
#include <string.h> // strlen, memset
#include <unistd.h> // close

int main(int argc, char *argv[]) {
    if (argc != 4) {
        fprintf(stderr, "USAGE: %s <server_ip> <word> <port>n", argv[0]);
        exit(1);
    }

    // Create the UDP socket
    int sock;
    if ((sock = socket(PF_INET, SOCK_DGRAM, IPPROTO_UDP)) < 0) fprintf(stdout, "Failed to create socket");

    // Construct the server sockaddr_in structure
    struct sockaddr_in echoserver;
    memset(&echoserver, 0, sizeof(echoserver)); // Clear struct
    echoserver.sin_family = AF_INET; // Internet/IP socket - not bluetooth or InfraRed
    echoserver.sin_addr.s_addr = inet_addr(argv[1]); // IP address
    echoserver.sin_port = htons(atoi(argv[3])); // server port

    // Send the word
    unsigned int echolen = strlen(argv[2]);
    if(sendto(sock, argv[2], echolen, 0, (struct sockaddr *) &echoserver, sizeof(echoserver)) != echolen)
    fprintf(stdout, "Could not send all bytes");

    close(sock);
    exit(0);
}
```

---
---
---

### 31. VirtualBox

Each operating system user has a list of virtual machines (VMs).

The nice thing about VirtualBox is that you can run the machines without having any GUI installed on your host.

1. Print the list of all VMs :`VBoxManage list vms`
2. See a list of running VMs :`VBoxManage list runningvms`
3. Start VM - standard GUI :`VBoxManage startvm VM_NAME`
4. Start VM - simple GUI :`VBoxSDL --startvm VM_NAME`
5. Start VM - no GUI :`VBoxHeadless --startvm VM_NAME`
6. Start VM - no GUI in the background :`VBoxManage startvm VM_NAME --type headless`
7. Start VM - no GUI with VNC :`VBoxHeadless --startvm VM_NAME --vnc --vncport 5901 --vncpass YOUR_PASS`. You can then open a VNC to it with:`vncviewer -autopass localhost::5901`, and then type your password
8. Pause VM :`VBoxManage controlvm VM_NAME pause`
9. Resume paused VM :`VBoxManage controlvm VM_NAME resume`
10. Reset VM :`VBoxManage controlvm VM_NAME reset`
11. Forced Power off VM :`VBoxManage controlvm VM_NAME poweroff`
12. Nicely Power off VM :`VBoxManage controlvm VM_NAME acpipowerbutton`
13. Save VM state :`VBoxManage controlvm VM_NAME savestate`
14. Import VM from ovf :`VBoxManage import SRV.sampleServer.ovf` you might need to add `--vsys 0 --eula accept` if there is a license to accept
15. Generate a new MAC for one VM :`VBoxManage modifyvm SRV.sampleServer --macaddress1 auto` will generate a random MAC address for the first network adapter
16. Rename the VM : `VBoxManage modifyvm CURRENT_VM_NAME --name NEW_VM_NAME`
17. Reset VM : VBoxManage controlvm VM_NAME reset

---
---
---

### 32. Debian - Change linux hostname

In order to change the host name of your linux(tested on debian) machine you can simply: `nano /etc/hostname`
Change the host name, Ctrl+O , Enter , Ctrl + X. Then please update the following file too:`nano /etc/hosts`
Change the host name, Ctrl+O , Enter , Ctrl + X. You can now restart the hostname service by running:
```bash
/etc/init.d/hostname.sh stop
/etc/init.d/hostname.sh start
```
Log out and back in.

Or you could simply do:
```bash
echo sampleName > /etc/hostname && /etc/init.d/hostname.sh stop && /etc/init.d/hostname.sh start && exit
```

---
---
---

### 33. Debian - bash colors

```bash
PS1='e[0m($?)e[1;31mue[0m@e[1;32mhe[0m:e[1;34mwe[0m>'
alias ls='ls --color=auto'
```

---
---
---

### 34. C++ 1970-01-01 00:00:00 + some time

You want to add something to a date? or make one manually?

Create a struct tm variable and set it’s properties, then call the mktime() function on it to compute the new date. Then simply extract the properties you’d like

```cpp
struct tm t;
t.tm_year = 70;// This is year-1900, so year = 1970
t.tm_mon = 0;
t.tm_mday = 1;
t.tm_hour = 0;
t.tm_min = 20321;
t.tm_sec = 12992;
time_t timeSinceEpoch = mktime(&t);//compute the date

long int date = (t.tm_year + 1900)*10000//we add 4 zeros to the right for MM DD (month and day)
+(t.tm_mon + 1)*100//we add 2 zeros to the right for DD (days)
+t.tm_mday;
```

---
---
---

### 35. Install LaTeX on linux

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML" async="" type="text/javascript">// <![CDATA[
// ]]></script>
In order to install $$\LaTeX{}$$ on your linux system you could install <strong>texlive</strong> for a basic subset of $$\TeX{}$$ Live's functionality. Or you could install <strong>texlive-full</strong> for the complete $$\TeX{}$$ Live distribution.

So, simply run:
<pre class="lang:sh decode:true ">apt-get install texlive
</pre>
or
<pre class="lang:sh decode:true ">apt-get install texlive-full
</pre>
You could do a:
<pre class="lang:sh decode:true ">apt-cache search texlive
</pre>
to see a complete list of texlive packages available for your operating system.

You should also install the recomended fonts:
<pre class="lang:sh decode:true ">apt-get install texlive-fonts-recommended
</pre>
I use the $$\TeX{}$$ <strong>Maker</strong> editor. You can install it by running:
<pre class="lang:sh decode:true ">apt-get install texmaker
</pre>

---
---
---

### 36. Search for file containing string

This will search all `*.h` and `*.html` files from `../src` and will print the file name, line number and line content for the lines that contain "recv(" or "send(":
```bash
egrep -nir –include=*.{h,html} "recv(|send(" ../src
```
OutputFormat:
```bash
(FILE:LINE:CONTENTn)*
```
Sample output:
```bash
../src/Socket.h:62: result recv( char * buf,int len,int flags = 0 );
../src/html/functions_func_0x73.html:100:<li>send()
../src/Connection.h:88: virtual bool wait_to_send( unsigned int timeout ) = 0;
```

---
---
---

### 37. C++ char array length problem

Say you have `char line[4*1024] = "ana";`. How many bytes of memory does it use?
```cpp
#include <iostream> // std::cout
#include <cstring> // std::getline()
int main(int argc, char* argv[])
{
    char line[4*1024] = "ana";
    std::cout << line << std::endl;
    std::cout << "strlen(line)=" << strlen(line) << std::endl;
    std::cout << "sizeof(line)=" << sizeof(line) << std::endl;
    return 0;
}
```
outputs:
```bash
ana
strlen(line)=3
sizeof(line)=4096
```
Why?

Well, you reserved `4KiO = 41024` bytes of memory, and `sizeof()` gives you that value.
You set the first three bytes of that memory with `'a'`, `'n'` and `'a'`, that uses 3 bytes from 41024 bytes available.
And that's what `strlen()` gives you back. Note that `strlen()` does not count the final `'\0'` character that exists in the array.
Therefore `strlen()` will always be `<= sizeof()-1`. That's why you can't compile the following:
```cpp
#include <iostream> // std::cout
#include <cstring> // std::getline()
int main(int argc, char* argv[])
{
    char line[4] = "abcd";
    std::cout << line << std::endl;
    std::cout << "strlen(line)=" << strlen(line) << std::endl;
    std::cout << "sizeof(line)=" << sizeof(line) << std::endl;
    return 0;
}
```
`"abcd"` actually means `{'a', 'b', 'c', 'd', '\0'}`. When you use strings they are char arrays with a `'\0'` at the end. That means they are one character longer than what you see.

With g++ you get the following error when initializing the line variable:
```bash
error: initializer-string for array of chars is too long [-fpermissive]
```
Because you must use at most 3 characters, and let one free char for the `'\0'` character. However, you can solve the problem like this:
```cpp
#include <iostream> // std::cout
#include <cstring> // std::getline()
int main(int argc, char* argv[])
{
    char line[4] = {'a', 'b', 'c', 'd'};
    std::cout << line << std::endl;
    std::cout << "strlen(line)=" << strlen(line) << std::endl;
    std::cout << "sizeof(line)=" << sizeof(line) << std::endl;
    return 0;
}
```
The following example shows how by assigning a string to a char array adds the `'\0'` character at the end:
```cpp
#include <iostream> // std::cout
#include <cstring> // strlen()
int main(int argc, char* argv[])
{
    char line[4] = "abc";
    for(size_t i=0; i<sizeof(line); i++)
        std::cout << "line[" << i << "] as int = " << (int)line[i] << std::endl;
    return 0;
}
```
outputs:
```bash
line[0] as int = 97
line[1] as int = 98
line[2] as int = 99
line[3] as int = 0
```
While:
```cpp
#include <iostream> // std::cout
#include <cstring> // strlen()
int main(int argc, char* argv[])
{
    char line[4] = {'a', 'b', 'c', 'd'};
    for(size_t i=0; i<sizeof(line); i++)
        std::cout << "line[" << i << "] as int = " << (int)line[i] << std::endl;
    return 0;
}
```
outputs:
```bash
line[0] as int = 97
line[1] as int = 98
line[2] as int = 99
line[3] as int = 100
```
But watch what happens with this one:
```cpp
#include <iostream> // std::cout
#include <cstring> // strlen()
int main(int argc, char* argv[])
{
    char line[10];// = {'a', 'b', 'c', 'd'};
    line[0] = 'a';
    line[1] = 'b';
    line[2] = 'c';
    line[3] = 'd';
    for(size_t i=0; i<sizeof(line); i++)
        std::cout << "line[" << i << "] as int = " << (int)*(line + i) << std::endl;
    return 0;
}
```
outputs:
```bash
line[0] as int = 97
line[1] as int = 98
line[2] as int = 99
line[3] as int = 100
line[4] as int = -61
line[5] as int = 127
line[6] as int = 0
line[7] as int = 0
line[8] as int = -48
line[9] as int = 9
```
It reserves 10 characters but I only assign values to the first 4, the rest (from index 4 to 9) have random values, values that were in memory before.

---
---
---

### 38. Watch RAM memory usage

Prints RAM usage in percentage:
```bash
watch -n 1 "free -m ; echo ; echo Memory usage: $((100*$(free -m | grep Mem | awk ‘{print $3}’)/$(free -m | grep Mem | awk ‘{print $2}’)))%"
```

---
---
---

### 39. Run commands from a URL

#### Bash

If you have a script you need to run, and you want to share it with others you can put it on a web server and run it from there with:

```bash
bash <(wget -qO- http://paul.grozav.info/work/sh/simpleExample)
```

Actually, wget will load and print the file to stdout (standard output) and bash will read it from the standard input. the less (<) sign takes the stdout of wget and sends it to bash's stdin. http://paul.grozav.info/work/sh/simpleExample is a simple text file, take a look!

#### PHP
```php
php5 -r "$(wget -qO- http://paul.grozav.info/work/sh/simplePHPExample)"
```

#### Python
```python
wget -qO- http://paul.grozav.info/work/sh/simplePythonExample | python
```
Same thing, the output of wget(the script - lines of code) are passed to python to interpret/run them.

---
---
---

### 40. GNU plot ping time with python
```python
#!/usr/bin/python
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# All rights reserved
import sys

maxPointsPerScreen = 20
points = []
for line in iter(sys.stdin.readline, ""):
    if "bytes from" in line:
        words = line.split()
        command = ""
        if len(points) > maxPointsPerScreen:
            command += "set xrange ["+str(len(points)-maxPointsPerScreen)+":"+str(len(points))+"]\n"
        command += "plot '-' w lines;\n"
        points.append( [ len(points), words[7][5:] ] )
        for point in points:
            command += str(point[0])+" "+str(point[1])+"\n"
        command += "e\n"
        sys.stdout.write(command)
        sys.stdout.flush()
```
You must install the package `gnuplot` with your package manager, and then you can run the program like this:
```bash
ping debian.org | ./parse.py | gnuplot
```

---
---
---

### 41. Bash script update config file

Say you have a config file like this one in `/home/myuser/config.cfg` :
```txt
protocol = ymsg
username = tedi1010
password = abcdefg
username = meetoo
```
Say you'd like to change the value of protocol from ymsg to http. You can do this from a script by running the following command:
```bash
sed -i "s/($TARGET_KEY *= *).*/1$REPLACEMENT_VALUE/" $CONFIG_FILE
```
Note: In some environments you might have to add the `-c` flag to sed
Once you replace `$TARGET_KEY` with `protocol` and `$REPLACEMENT_VALUE` with `http` and `$CONFIG_FILE` with `/home/myuser/config.cfg` the command will make that change. So what you need to run is:
```bash
sed -i "s/(protocol *= *).*/1http/" /home/myuser/config.cfg
```

---
---
---

### 42. Scan a list of files for given words
```bash
#!/bin/bash
# Author: Tancredi-Paul Grozav (paul@grozav.info)
serverPrefix="" # ssh myremoteserver" # Use this if you want to scan files from another computer
needles=( "K" "k" ) # list of words that you are searching for
for ARG in $* # for each file given as an argument
do
for i in "${needles[@]}" # for each needle
do
echo -n Searching for $i in $ARG ...
noOccurences=`$serverPrefix grep $i $ARG | wc -l`
if [ $noOccurences -eq 0 ] ; then
echo Not found.
else
echo Found $noOccurences times :
$serverPrefix grep $i $ARG
fi
done
done
```

---
---
---

### 43. Qt gl.h no such file

After installing Qt 5.2 on Debian 7.3 (wheezy) (amd64 O.S.) from http://download.qt-project.org/official_releases/online_installers/1.5/qt-linux-opensource-1.5.0-x64-online.run I opened Qt Creator and compiled their “Application Example” application example :) and I got this error:
```bash
fatal error: GL/gl.h: No such file or directory
```
To fix it I had to run as root:
```bash
apt-get install x11proto-gl-dev mesa-common-dev
```
But then I got this error:
```bash
/usr/bin/ld: cannot find -lGL
```
To solve this one I had to(run as root):
```bash
apt-get install freeglut3 freeglut3-dev
```

---
---
---

### 44. gedit terminal plugin can not see text

Lots of users are complaining about not beeing able to see any text in the terminal plugin.
That is because the text color is white and the background color is white.
To fix this please open dconf and go to org -> gnome -> gedit -> plugins -> terminal .
There please uncheck the use-theme-colors property. You can also make sure that
the property background-color and foreground-color are not the same (or close).
Then simply close dconf and restart gedit to see the changes.

---
---
---

### 45. SSH long delay because of GSSAPIAuthentication

Connecting to a server using ssh might take a long time. This was my case (output of ssh -v user@server.com):
```bash
...
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: Roaming not allowed by server
debug1: SSH2_MSG_SERVICE_REQUEST sent
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic,password
debug1: Next authentication method: gssapi-keyex
debug1: No valid Key exchange context
debug1: Next authentication method: gssapi-with-mic
debug1: Unspecified GSS failure. Minor code may provide more information
Credentials cache file '/tmp/krb5cc_1000' not found

debug1: Unspecified GSS failure. Minor code may provide more information
Credentials cache file '/tmp/krb5cc_1000' not found

debug1: Unspecified GSS failure. Minor code may provide more information

debug1: Unspecified GSS failure. Minor code may provide more information
Credentials cache file '/tmp/krb5cc_1000' not found

debug1: Next authentication method: publickey
debug1: Offering RSA public key: /home/paul/.ssh/id_rsa
debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic,password
debug1: Trying private key: /home/paul/.ssh/id_dsa
debug1: Trying private key: /home/paul/.ssh/id_ecdsa
debug1: Next authentication method: password
user@server.com's password:
```
It seems the server accepts the following authentication methods: "publickey,gssapi-keyex,gssapi-with-mic,password"
and the client tries each one until it manages to connect. In my case, the
authentication was delayed because of this method: "gssapi-with-mic". So, I
configured my client to not use this method(to skip it). You can make this
configuration in two ways:

1. Use `ssh -o GSSAPIAuthentication=no user@host -p port` This way at the
authentication step, the client will skip the "gssapi-with-mic" method.
2. Edit `/etc/ssh/ssh_config` and uncomment or add the following line: `GSSAPIAuthentication no`

---
---
---

### 46. C++ int division does not produce a float number

Ever wondered why dividing two simple integers does not return a floating point number?

When you divide two integers the operation used is:
$$
/ : \mathbb{Z} \times \mathbb{Z} \to \mathbb{Z} ~~ where ~~ /(x,y) = abs(x/y)
$$

Where $$\mathbb{Z}$$ is the set of integer numbers and the `abs()` function returns the absolute value of it's parameter (the absolute value being an integer).

If you need to get the actual result from dividing the two numbers you should "help" the compiler see the two numbers as being float, and then the result of the operation will be a float as well:
$$
/ : \mathbb{Q} \times \mathbb{Q} \to \mathbb{Q} ~~ where ~~ /(x,y) = x/y
$$

Where $$\mathbb{Q}$$ is the set of rational numbers. Also, dividing two numbers will always return a rational number.
```cpp
#include <iostream>

using namespace std;

int main(){
    int a = 7;
    int b = 3;

    int r1 = a/b; // 2.33333 expected
    cout << r1 << endl; // 2 printed
    // REASON: The result of dividing two integers will always be an integer / : ZxZ -> Z

    float r2 = a/b; // 2.33333 expected
    cout << r2 << endl; // 2 printed -.-
    // REASON: Just putting the result in a float variable will not change the result, that is still an integer

    float a1 = a;
    float b1 = b;
    float r3 = a1/b1; // 2.33333 expected
    cout << r3 << endl; // 2.33333 printed
    // REASON: Now if you divide two float numbers you will get a float value (the result you'd expect) / : QxQ -> Q

    float r4 = (float)a / (float)b; // 2.33333 expected
    cout << r4 << endl; // 2.33333 printed
    // Or simply cast values to float if you expect a float result

    return 0;
}
```

---
---
---

### 47. C++ Socket Buffer

**SocketBuffer.h**:
```cpp
/*
* SocketBuffer.h
*
* Created on: Feb 4, 2014
* Author: Tancredi-Paul Grozav <paul@grozav.info>
*/

#ifndef SOCKETBUFFER_H_
#define SOCKETBUFFER_H_

#include "net/Socket.h" // Socket
#include <stdexcept> // std::runtime_error

/**
* This class contains a socket and a buffer. The class allows you to read bytes from the socket and append them to the buffer or store them in buffer from position 0.
* @author Tancredi-Paul Grozav <paul@grozav.info>
*/
class SocketBuffer
{
public:
/**
* Creates a SocketBuffer object using the Socket pointer.
* @param socket A pointer to a Socket object.
*/
SocketBuffer(Socket* socket);

/**
* Destroy the object.
*/
virtual ~SocketBuffer();

public:
/**
* Frees memory by allocating (bufferBytesInUse -numberOfBytes) bytes, copying bytes from i to bufferBytesInUse from buffer to the new memory zone, frees the old buffer and makes the buffer_ pointer point to the new allocated memory.
* @param numberOfBytes The number of bytes to remove from the start of the buffer.
* @throws A std::runtime_error containing a descriptive message.
*/
void freeFirstNBytes(size_t numberOfBytes) throw (std::runtime_error);

/**
* Frees unused memory by freeing (bufferSize – bufferBytesInUse) bytes. It allocates bufferBytesInUse bytes,
* moves the bytes in use from the current buffer to the new buffer, frees the old buffer,
* sets the buffer pointer to point to new location.
* @throws A std::runtime_error containing a descriptive message.
*/
void freeUnusedMemory() throw (std::runtime_error);

/**
* Returns the pointer to the buffer.
* @return A pointer to the buffer — the first character in the buffer.
*/
char* getBuffer();

/**
* Returns the number of bytes used in the buffer.
* @return number of bytes used in the buffer.
*/
size_t getBufferBytesInUse();

/**
* Returns the number of bytes allocated for the buffer.
* @return number of bytes allocated for the buffer.
*/
size_t getBufferSize();

/**
* Receives next N bytes from the socket and appends them to the buffer.
* @param numberOfBytes The number of bytes to read and append to the current buffer.
* @throws A std::runtime_error containing a descriptive message.
* @return The number of bytes appended(read from the socket) or -1 if there was an exception while reading from socket.
*/
size_t receiveBytesAndAppend(size_t numberOfBytes)
throw (std::runtime_error);

/**
* Make sure we have a buffer of numberOfBytes bytes. If the current buffer is smaller, then free it and allocate a new zone. Receive from the socket N bytes (N<=numberOfBytes) and put them in the buffer.
* The number of bytes received from the socket is bufferBytesInUse.
* @param numberOfBytes The number of bytes to read and put into the current buffer.
* @throws A std::runtime_error containing a descriptive message.
*/
void receiveBytesAndReplace(size_t numberOfBytes) throw (std::runtime_error);

private:
/**
* The buffer that keeps the data read from socket.
*/
char* buffer_;

/**
* The number of bytes used in the buffer.
*/
size_t bufferBytesInUse_;

/**
* The number of bytes allocated for the buffer.
*/
size_t bufferSize_;

/**
* Socket used read from it and store in the buffer.
*/
Socket* socket_;

};

#endif /* SOCKETBUFFER_H_ */
```

**SocketBuffer.cpp**:
```cpp
/*
* SocketBuffer.cpp
*
* Created on: Jan 31, 2014
* Author: Tancredi-Paul Grozav <paul@grozav.info>
*/

#include "SocketBuffer.h"
#include "net/Socket.h" // Socket
#include <cstdlib> // malloc
#include <cstring> // strlen()

SocketBuffer::SocketBuffer(Socket* socket) :
buffer_(0), bufferBytesInUse_(0), // 0 bytes used
bufferSize_(0), // 0 bytes allocated
socket_(socket) // NULL pointing at memory addres 0
{
}

SocketBuffer::~SocketBuffer()
{
}

size_t SocketBuffer::getBufferSize()
{
return bufferSize_;
}

size_t SocketBuffer::getBufferBytesInUse()
{
return bufferBytesInUse_;
}

char* SocketBuffer::getBuffer()
{
return buffer_;
}

size_t SocketBuffer::receiveBytesAndAppend(size_t numberOfBytes)
throw (std::runtime_error)
{
// Make sure we have room for numberOfBytes bytes in the current buffer, if not, then allocate a new buffer.
if (bufferSize_ – bufferBytesInUse_ < numberOfBytes)
{ // If we have less than numberOfBytes bytes free in the current buffer
void* address = malloc(bufferBytesInUse_ + numberOfBytes); // Allocate (bufferBytesInUse_ + numberOfBytes) bytes to hold the current used bytes and the new bytes to come.
if (address == 0)
{ // If the allocation was impossible …
throw std::runtime_error(
"Exception in SocketBuffer::receiveBytesAndAppend(): Unable to allocate memory."); // Throw an runtime_error
}
else
{ // If the allocation was possible …
if (buffer_ != 0)
{ // If the buffer does not point to memory address 0
memcpy(address, buffer_, bufferBytesInUse_); // Copy bufferBytesInUse_ used bytes from the old buffer (buffer_) to the new buffer(address)
free (buffer_); // Free the bytes reserved for buffer_
} // If the buffer_ points to memory address 0 there is nothing to copy and nothing to free, therefore we can make it point to address
buffer_ = (char*) address; // Make the buffer_ pointer point to the new address (new buffer)
bufferSize_ = bufferBytesInUse_ + numberOfBytes; // The new size of the buffer is the number of bytes in use
// std::cout << "Made room for extra " << numberOfBytes << " bytes" << std::endl;
}
}

try
{
Socket::result result = socket_->recv(buffer_ + bufferBytesInUse_,
numberOfBytes); // Receive up to numberOfBytes bytes from the socket and store them in buffer(in memory starting from buffer_ + bufferBytesInUse_)
bufferBytesInUse_ += result.count; // Increase the number of bytes used in the buffer with result.count (the number of bytes received — might be less than numberOfBytes)
std::cout << result.count << " bytes appended to buffer" << std::endl;
// std::cout << "used/allocated = " << bufferBytesInUse_ << "/" << bufferSize_ << " buffer=""; for(size_t i=0; i<bufferBytesInUse_; i++) std::cout << (short int)((unsigned char)*(buffer_+i)) << ",";std::cout << """ << std::endl;
return result.count; // Return the number of bytes received from the socket and appended to the buffer
}
catch (std::exception& e)
{
throw std::runtime_error(
"Exception in SocketBuffer::receiveBytesAndAppend(): Unable to read from socket."); // Throw an runtime_error
}
return -1; // If exception while reading from socket return -1
}

void SocketBuffer::receiveBytesAndReplace(size_t numberOfBytes)
throw (std::runtime_error)
{
if (bufferSize_ < numberOfBytes) // Next bytes can not fit in the current buffer
{
if (buffer_ != 0) // If the buffer was initialized before
{
// std::cout << "Freeing memory" << std::endl;
free (buffer_); // Free memory used for buffer_
}

// std::cout << "Allocate new memory" << std::endl;
void* address = malloc(numberOfBytes); // Allocate numberOfBytes for our buffer
if (address == 0)
{ // If the allocation was impossible …
buffer_ = 0; // Make the buffer point to memory address 0
bufferSize_ = 0; // Reset the bufferSize_ to 0 since the buffer is null again
bufferBytesInUse_ = 0; // No byte is in use in the buffer
throw std::runtime_error(
"Exception in SocketBuffer::receiveBytesAndReplace(): Unable to allocate memory."); // Throw an runtime_error
}
else
{ // If the allocation was possible …
buffer_ = (char*) address; // Make the buffer_ point to the first byte in the allocated sequence
bufferSize_ = numberOfBytes; // Set the buffer size to be the number of bytes allocated
}
}

try
{
Socket::result result = socket_->recv(buffer_, numberOfBytes); // Receive up to numberOfBytes bytes from the socket and store them in buffer(in memory starting from buffer_)
bufferBytesInUse_ = result.count; // Set the number of bytes used in the buffer to be result.count (the number of bytes received — might be less than numberOfBytes)
// std::cout << "used/allocated = " << bufferBytesInUse_ << "/" << bufferSize_ << " value=""; for(size_t i=0; i<bufferBytesInUse_; i++) std::cout << (short int)((unsigned char)*(buffer_+i)) << ",";std::cout << """ << std::endl;
}
catch (std::exception& e)
{
throw std::runtime_error(
"Exception in SocketBuffer::receiveBytesAndReplace(): Unable to read from socket."); // Throw an runtime_error
}
}

void SocketBuffer::freeFirstNBytes(size_t numberOfBytes)
throw (std::runtime_error)
{
void* address = malloc(bufferBytesInUse_ – numberOfBytes); // Allocate (bufferBytesInUse_ – numberOfBytes) bytes to move bytes in use(>=numberOfBytes) to this new buffer and free the old one.
if (address == 0)
{ // If the allocation was impossible …
throw std::runtime_error(
"Exception in SocketBuffer::freeFirstNBytes(): Unable to allocate memory."); // Throw an runtime_error
}
else
{ // If the allocation was possible …
memcpy(address, buffer_ + numberOfBytes,
bufferBytesInUse_ – numberOfBytes); // Copy (bufferBytesInUse_ – numberOfBytes) used bytes from the old buffer (buffer_) to the new buffer(address)
free (buffer_); // Free the bytes reserved for buffer_
buffer_ = (char*) address; // Make the buffer_ pointer point to the new address (new buffer)
bufferSize_ = bufferBytesInUse_ – numberOfBytes; // The new size of the buffer is the number of bytes in use minus the number of bytes removed from the beginning of the old buffer.
bufferBytesInUse_ -= numberOfBytes; // Subtract numberOfBytes bytes from the buffer
// std::cout << "Buffer freed !" << std::endl;
}
// std::cout << "used/allocated = " << bufferBytesInUse_ << "/" << bufferSize_ << " buffer=""; for(size_t i=0; i<bufferBytesInUse_; i++) std::cout << (short int)((unsigned char)*(buffer_+i)) << ",";std::cout << """ << std::endl;
}

void SocketBuffer::freeUnusedMemory() throw (std::runtime_error)
{
void* address = malloc(bufferBytesInUse_); // Allocate bufferBytesInUse_ bytes to move bytes in use to this new buffer and free the old one.
if (address == 0)
{ // If the allocation was impossible …
throw std::runtime_error(
"Exception in SocketBuffer::freeUnusedMemory(): Unable to allocate memory."); // Throw an runtime_error
}
else
{ // If the allocation was possible …
memcpy(address, buffer_, bufferBytesInUse_); // Copy bufferBytesInUse_ used bytes from the old buffer (buffer_) to the new buffer(address)
free (buffer_); // Free the bytes reserved for buffer_
buffer_ = (char*) address; // Make the buffer_ pointer point to the new address (new buffer)
bufferSize_ = bufferBytesInUse_; // The new size of the buffer is the number of bytes in use
// std::cout << "Buffer freed !" << std::endl;
}
// std::cout << "used/allocated = " << bufferBytesInUse_ << "/" << bufferSize_ << " value=""; for(size_t i=0; i<bufferBytesInUse_; i++) std::cout << (short int)((unsigned char)*(buffer_+i)) << ",";std::cout << """ << std::endl;
}
```

---
---
---

### 48. Convert from 12 hours (AM/PM) to 24 hours format using awk

Before conversion:
```txt
Linux 3.10.0-123.9.2.el7.x86_64 (mo) 11/29/2014 _x86_64_ (8 CPU)

12:00:01 AM CPU %user %nice %system %iowait %steal %idle
12:00:02 AM all 1.64 0.00 1.39 0.00 0.00 96.98
12:00:02 AM 0 1.00 0.00 1.00 0.00 0.00 98.00
12:00:02 AM 1 1.01 0.00 1.01 0.00 0.00 97.98
12:00:02 AM 2 0.99 0.00 1.98 0.00 0.00 97.03
12:00:02 AM 3 1.01 0.00 1.01 0.00 0.00 97.98
12:00:02 AM 4 0.00 0.00 2.00 0.00 0.00 98.00
12:00:02 AM 5 7.00 0.00 1.00 0.00 0.00 92.00
12:00:02 AM 6 0.00 0.00 0.00 0.00 0.00 100.00
12:00:02 AM 7 4.04 0.00 3.03 0.00 0.00 92.93

12:00:02 AM CPU %user %nice %system %iowait %steal %idle
12:00:03 AM all 0.38 0.00 0.76 0.00 0.00 98.86
12:00:03 AM 0 0.00 0.00 0.00 0.00 0.00 100.00
12:00:03 AM 1 0.00 0.00 1.01 0.00 0.00 98.99
12:00:03 AM 2 0.00 0.00 2.04 0.00 0.00 97.96
12:00:03 AM 3 0.00 0.00 1.02 0.00 0.00 98.98
12:00:03 AM 4 0.00 0.00 0.00 0.00 0.00 100.00
```
CONVERSION:
```bash
cat sar.log | awk '{if(NF == 9 && ($2 == "AM" || $2 =="PM")){ cmd="date -d \"\$(date -d \""$1" "$2"\")\" \"+%H:%M:%S\""; cmd | getline val; close(cmd); printf val; print "\t"$3"\t"$4"\t"$5"\t"$6"\t"$7"\t"$8"\t"$9 }else{ print } }' > sar.24hrsFormat.log
```
After conversion:
```txt
Linux 3.10.0-123.9.2.el7.x86_64 (foca) 11/29/2014 _x86_64_ (8 CPU)

00:00:01 CPU %user %nice %system %iowait %steal %idle
00:00:02 all 1.64 0.00 1.39 0.00 0.00 96.98
00:00:02 0 1.00 0.00 1.00 0.00 0.00 98.00
00:00:02 1 1.01 0.00 1.01 0.00 0.00 97.98
00:00:02 2 0.99 0.00 1.98 0.00 0.00 97.03
00:00:02 3 1.01 0.00 1.01 0.00 0.00 97.98
00:00:02 4 0.00 0.00 2.00 0.00 0.00 98.00
00:00:02 5 7.00 0.00 1.00 0.00 0.00 92.00
00:00:02 6 0.00 0.00 0.00 0.00 0.00 100.00
00:00:02 7 4.04 0.00 3.03 0.00 0.00 92.93

00:00:02 CPU %user %nice %system %iowait %steal %idle
00:00:03 all 0.38 0.00 0.76 0.00 0.00 98.86
00:00:03 0 0.00 0.00 0.00 0.00 0.00 100.00
00:00:03 1 0.00 0.00 1.01 0.00 0.00 98.99
00:00:03 2 0.00 0.00 2.04 0.00 0.00 97.96
00:00:03 3 0.00 0.00 1.02 0.00 0.00 98.98
00:00:03 4 0.00 0.00 0.00 0.00 0.00 100.00
```

---
---
---

### 49. Memory usage of a process in MiB
The following command gets the %MEM usage from ps, and calculates the value in MB, knowing the total memory from free.

All you need to do is filter the output of ps by using a grep condition. Replace “firefox” by anything that’s specific for your process.
```bash
ps aux | grep "firefox" | grep -v grep | awk -v totalMB=$(free -m | grep "Mem:" | awk '{print $2}') '{ print $4/100*totalMB }'
```

---
---
---

### 50. MySQL / MariaDB procedure with cursor

The following example creates a simple function, then it calls it and removes it.
The function defines a string variable v, then runs a select, gets the returned
value and prints it on the screen. Run all the following lines at once. Also,
remember that "delimiter $" will change the delimiter to `$`, therefore, each
command will end with a `$`. After the end of the procedure you should put that
character(`$`) and change the delimiter back to the default one of MySQL/MariaDB
: `;`. Also, you should dropt the procedure after running it, if you don’t want
it to remain on the server (as a table does, once created).

```sql
delimiter $
CREATE PROCEDURE dorepeat()
BEGIN
DECLARE v CHAR(250);
DECLARE curs CURSOR FOR SELECT NOW() as timp;
DECLARE CONTINUE HANDLER FOR NOT FOUND SELECT "NOT FOUND";
OPEN curs;
FETCH curs into v;
select CONCAT("RESULT:",v);
CLOSE curs;
END$
delimiter ;
call dorepeat();
drop procedure dorepeat;
```
The calling the procedure above on a database returns:
```txt
+----------------------------+
| CONCAT("RESULT:",v)        |
+----------------------------+
| RESULT:2015-01-21 13:47:29 |
+----------------------------+
```

---
---
---

### 51. Grep log for values between two timestamps

Say you have a log and you are interested in getting the lines that were logged
between two timestamps - say from `2015-02-12 09:15:00` to `2015-02-12 09:45:00`.
Now, you’ll need to specify the two timestamps (`D1` and `D2`), the log (`LOG`),
and the date format used inside the log(`LOG_DATE_FORMAT`).
```bash
(
D1="2015-02-12 09:15:00" ;
D2="2015-02-12 09:45:00" ;
LOG="error.log" ;
LOG_DATE_FORMAT="^[%-Y/%-m/%-d %-H:%-M:%-S" ;
diff=$(echo "$(date -d "$D2" +"%s")-$(date -d "$D1" +"%s")" | bc) ;
for (( s=0; s<=$diff; s++ )); do
sd=$(date -d "@$(echo "$(date -d "$D1" +"%s") + $s" | bc)" +"$LOG_DATE_FORMAT") ;
grep "$sd" $LOG ;
done ;
)
```
You’ll run it this way:
```bash
( D1="2015-02-12 09:15:00" ; D2="2015-02-12 09:45:00" ; LOG="/var/log/err20150212.log" ; LOG_DATE_FORMAT="^[%-Y/%-m/%-d %-H:%-M:%-S " ; diff=$(echo "$(date -d "$D2" +"%s")-$(date -d "$D1" +"%s")" | bc) ; for (( s=0; s<=$diff; s++ )); do sd=$(date -d "@$(echo "$(date -d "$D1" +"%s") + $s" | bc)" +"$LOG_DATE_FORMAT") ; grep "$sd" $LOG ; done ; )
```
For values like:
```txt
...
12:30:44 11 4.3 6.865
12:30:46 5 2.3 3.62
12:30:48 26 11.6 15.895
12:30:50 11 15.8 22.805
12:30:52 9 19.6 26.605
12:30:54 11 5.6 9.025
12:30:56 17 7.4 11.98
12:30:58 4 2.7 3.71
12:31:00 18 8.6 12.43
12:31:02 158 82.4 108.225
12:31:04 131 170.9 210.28
...
```
You might also use:
```bash
cat your.log | perl -lane 'print if $F[0] ge "09:32:54" and $F[0] lt "09:34:56"'
```

---
---
---

### 52. Debian sending mail through exim4

Please run as root:
```bash
dpkg-reconfigure exim4-config
```
And select the following:

1. General type of mail configuration: **internet site; mail is sent and received directly using SMTP**
2. System mail name: **munin.server.paul.grozav.info** (*Type your Fully Qualified Domain Name here*)
3. IP-addresses to listen on for incoming SMTP connections: **127.0.0.1 ; ::1**
4. Other destinations for which mail is accepted: **munin.server.paul.grozav.info** (*Type your Fully Qualified Domain Name here*)
5. Domains to relay mail for:                    (Do not type anything - let the field be empty)
6. Machines to relay mail for: **127.0.0.1**
7. Keep number of DNS-queries minimal (Dial-on-Demand)?    Select **No**
8. Delivery method for local mail: **Maildir format in home directory**
9. Split configuration into small files?    Select **No**

Great ! Now try sending an email using this command:
```bash
echo "This is a mail from command line" | mail -s "Linux mail" "your@email.com"
```
You can check for any errors in /var/log/exim4/mainlog :
```bash
cat /var/log/exim4/mainlog
```

---

If you’d like to send your emails through your ISP you can do something like this:

1. General type of mail configuration: internet site; **mail is sent and received directly using SMTP**
2. System mail name: **munin.server.paul.grozav.info** (*Type your Fully Qualified Domain Name here*)
3. IP-addresses to listen on for incoming SMTP connections: **127.0.0.1 ; ::1**
4. Other destinations for which mail is accepted: **munin.server.paul.grozav.info**
5. Machines to relay mail for: (*Do not type anything – let the field be empty*)
6. IP address or host name of the outgoing smarthost: **smtp.rdslink.ro**
7. Hide local mail name in outgoing mail? Select **No**
8. Visible domain name for local users: **munin.server.paul.grozav.info**
9. Keep number of DNS-queries minimal (Dial-on-Demand)? Select **No**
10. Delivery method for local mail: **Maildir format in home directory**
11. Split configuration into small files? Select **No**

---
---
---

### 53. C++ keypress

I’m not sure if you’ve ever wondered how C++ games can "feel" when you press a
key. I was curious about this, especially because (by default) when you read
some value from stdin you don’t get it character by character, as the user types
it, but you get the entire string, the moment the user hits Enter.

```cpp
#include <stdio.h>
#include <termios.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/time.h>

void changemode(int dir)
{
        static struct termios oldt, newt;
        if ( dir == 1 )
        {
                tcgetattr( STDIN_FILENO, &oldt);
                newt = oldt;
                newt.c_lflag &= ~( ICANON | ECHO );
                tcsetattr( STDIN_FILENO, TCSANOW, &newt);
        }else
                tcsetattr( STDIN_FILENO, TCSANOW, &oldt);
}

int kbhit(void)
{
        struct timeval tv;
        fd_set rdfs;
        tv.tv_sec = 0;
        tv.tv_usec = 0;
        FD_ZERO(&rdfs);
        FD_SET (STDIN_FILENO, &rdfs);
        select(STDIN_FILENO+1, &rdfs, NULL, NULL, &tv);
        return FD_ISSET(STDIN_FILENO, &rdfs);
}

int main(void)
{
        changemode(1);

        while(true)
        {
                while( !kbhit() ){} // While no key is pressed, do nothing
                char ch = getchar(); // When a key was pressed, we get the character
                printf("Got %i=%cn", ch, ch); // Print the info about the key that was pressed
                if(ch==113) break; // We might need a break condition
        }

        changemode(0);
        return 0;
}
```
Tested on Linux only with g++ :)

---
---
---

### 54. C++ project makefile
**Version 2**:
```makefile
################################################################################
#
# Author: Tancredi-Paul Grozav <paul@grozav.info>
#
# Makefile for a C++ project. Run "make addsubdir DIR=src" to add a source folder named src
# Please keep this header if you copy this file :-)
#
################################################################################

# ========================- SETTINGS -========================
# Please give your build a name:
BUILD_NAME := tst1

# Comment or uncomment an option for BUILD_TYPE, OPTIMIZATION_TYPE, LINKING_TYPE:
#BUILD_TYPE := library# Uncomment this if you want to build a library out of your sources
BUILD_TYPE := executable# Uncomment this if you want to build an executable out of your sources

#OPTIMIZATION_TYPE := release# Uncomment this if you want to build a release vesion ( -O3 )
OPTIMIZATION_TYPE := debug# Uncomment this if you want to build a debug version ( -O0 -ggdb -g3 )

#LINKING_TYPE := static# Uncomment this if you want your build to have static linkage (built in dependencies)
LINKING_TYPE := dynamic# Uncomment this if you want your build to have dynamic linkage (load dependencies at runtime from LD_LIBRARY_PATH)

# Please specify the libraries and paths:
GLOBAL_INCLUDE_PATHS := # Where should the compiler search for source files (#include's)
LIBRARY_PATHS := # Where should the linker search for libraries
LIBS := # What libraries should be linked with this build

# Type here any arguments that you want to pass to your executable when running it
RUN_ARGUMENTS :=# Arguments passed to the executable when running: make run
# ======================- END SETTINGS -======================

#SOURCE_FILES_REGEX := .*.(cpp|CPP)# Accepted extensions for source files
all: $(BUILD_NAME)
#       @echo 'Building objects:$(OBJS)'


# Create the list of objects that need to be compiled
#$(eval OBJECTS += $(shell find .. -type f -regex "$(SOURCE_FILES_REGEX)" | awk '{print substr($$0,1,length($$0)-3)"o"}'))
#$(eval OBJECTS += $(shell find .. -type f -name "*.cpp" | awk '{print substr($$0,1,length($$0)-3)"o"}'))
$(eval OBJECTS += $(shell find .. -type f -name "*.cpp" | awk '{print substr($$0,2,length($$0)-3-1)"o"}'))

# create the dependency list - helps to recompile only what changed from last compile
$(eval -include $(shell find .. -type f -name "*.cpp" | awk '{print substr($$0,2,length($$0)-3-1)"d"}'))

# Link all objects into the final binary
$(BUILD_NAME): $(OBJECTS)
        @echo "Linking objects into $(BUILD_NAME) ..."
ifeq ($(BUILD_TYPE),executable)
ifeq ($(LINKING_TYPE),static) # Executable static
#       @g++ -o $(BUILD_NAME) $(OBJECTS) -Wl,-Bstatic $(LIBS) $(LIBRARY_PATHS)
        g++ -static -o $(BUILD_NAME) $(OBJECTS) $(LIBS) $(LIBRARY_PATHS)
else
ifeq ($(LINKING_TYPE),dynamic) # Executable dynamic
        @g++ -o $(BUILD_NAME) $(OBJECTS) -Wl,-Bdynamic $(LIBS) $(LIBRARY_PATHS)
else
        @echo "How do I link the objects using LINKING_TYPE=$(LINKING_TYPE) ?"
endif
endif
else
ifeq ($(BUILD_TYPE),library)
ifeq ($(LINKING_TYPE),static) # Library static
#       g++ -o $(BUILD_NAME) $(OBJECTS) -Wl,-Bstatic $(LIBS) $(LIBRARY_PATHS)
        ar -r $(BUILD_NAME) $(OBJECTS) $(LIBS)
else
ifeq ($(LINKING_TYPE),dynamic) # Library dynamic
        g++ -shared -o $(BUILD_NAME) $(OBJECTS) $(LIBS) $(LIBRARY_PATHS)
else
        @echo "How do I link the objects using LINKING_TYPE=$(LINKING_TYPE) ?"
endif
endif
else
        @echo "How do I link the objects as BUILD_TYPE=$(BUILD_TYPE) ?"
endif
endif

# Compile each source file
%.o:
        @echo "Compiling $@ ..."
        @if [ ! -d $$(dirname $@) ]; then mkdir -p $$(dirname $@); fi
ifeq ($(OPTIMIZATION_TYPE),debug)
        @g++ $(GLOBAL_INCLUDE_PATHS) -O0 -g3 -ggdb -Wall -c -fmessage-length=0 -fPIC -MMD -MP -MF"$(shell echo "$@" | awk '{print substr($$0,1,length($$0)-1)"d"}')" -MT"$(shell echo "$@" | awk '{print substr($$0,1,length($$0)-1)"d"}')" -o "$@" "$(shell echo "$@" | awk '{print "../"substr($$0,1,length($$0)-1)"cpp"}')"
else
ifeq ($(OPTIMIZATION_TYPE),release)
        @g++ $(GLOBAL_INCLUDE_PATHS) -O3 -Wall -c -fmessage-length=0 -fPIC -MMD -MP -MF"$(shell echo "$@" | awk '{print substr($$0,1,length($$0)-1)"d"}')" -MT"$(shell echo "$@" | awk '{print substr($$0,1,length($$0)-1)"d"}')" -o "$@" "$(shell echo "$@" | awk '{print "../"substr($$0,1,length($$0)-1)"cpp"}')"
else
        @echo "How do I compile in $(OPTIMIZATION_TYPE) mode?"
endif
endif

# Clean project target
clean:
        @echo "Cleaning project ..."
        @rm -Rf $(shell ls -l | grep -vw "makefile" | awk '{printf $$9" "}')

# Run executable target
run:
ifeq ($(BUILD_TYPE),executable)
        @echo "Running program ..."
        ./$(BUILD_NAME) $(RUN_ARGUMENTS)
else
        @echo "No need to run library $(BUILD_NAME)"
endif

.PHONY: clean all run
```

**Version 1**:
```makefile
################################################################################
#
# Author: Tancredi-Paul Grozav <paul@grozav.info>
#
# Makefile for a C++ project. Run "make addsubdir DIR=src" to add a source folder named src
#
################################################################################

BUILD_NAME := some_decoder_dbg
#BUILD_TYPE := library# Uncomment this if you want to build a library out of your sources
BUILD_TYPE := executable# Uncomment this if you want to build an executable out of your sources

#OPTIMIZATION_TYPE := release# Uncomment this if you want to build a release vesion ( -O3 )
OPTIMIZATION_TYPE := debug# Uncomment this if you want to build a debug version ( -O0 -ggdb -g3 )

LINKING_TYPE := static# Uncomment this if you want your build to have static linkage (built in dependencies)
#LINKING_TYPE := dynamic# Uncomment this if you want your build to have dynamic linkage (load dependencies at runtime from LD_LIBRARY_PATH)

GLOBAL_INCLUDE_PATHS := -I/home/pgrozav/git/someLib# Where should the compiler search for source files (#include's)
LIBRARY_PATHS := -L/home/pgrozav/git/someLib/lib/DebugStatic# Where should the linker search for libraries
LIBS := -lliba_dbg -llibb_dbg# What libraries should be linked with this build

RUN_ARGS := -f file1.log -template template.xml# Arguments passed to the executable when running: make run

# OTHER variables
#ER_OBJS :=
#O_SRCS :=
CPP_SRCS :=
#C_UPPER_SRCS :=
#C_SRCS :=
#S_UPPER_SRCS :=
#OBJ_SRCS :=
#ASM_SRCS :=
#CXX_SRCS :=
#C++_SRCS :=
#CC_SRCS :=
OBJS :=
#C++_DEPS :=
#C_DEPS :=
#CC_DEPS :=
CPP_DEPS :=
#EXECUTABLES :=
#CXX_DEPS :=
#C_UPPER_DEPS :=

# All Target
ifdef BUILD_TYPE
all: $(BUILD_NAME)
#       @echo 'Building objects:$(OBJS)'
else
all:
        @echo 'Please specify one BUILD_TYPE'
endif

# Every subdirectory with source files must be described here
SUBDIRS :=
src

# All of the sources participating in the build are defined here. Please add a subdir.mk to each source folder describing it's content.
#-include subdir.mk
-include src/subdir.mk

ifneq ($(MAKECMDGOALS),clean)
ifneq ($(strip $(C++_DEPS)),)
-include $(C++_DEPS)
endif
ifneq ($(strip $(C_DEPS)),)
-include $(C_DEPS)
endif
ifneq ($(strip $(CC_DEPS)),)
-include $(CC_DEPS)
endif
ifneq ($(strip $(CPP_DEPS)),)
-include $(CPP_DEPS)
endif
ifneq ($(strip $(CXX_DEPS)),)
-include $(CXX_DEPS)
endif
ifneq ($(strip $(C_UPPER_DEPS)),)
-include $(C_UPPER_DEPS)
endif
endif

# All Target
#ifdef BUILD_TYPE
#all: $(BUILD_NAME)
#       @echo 'Building objects:$(OBJS)'
#else
#all:
#       @echo 'Please specify one BUILD_TYPE'
#endif

## Tool invocations
$(BUILD_NAME): $(OBJS) $(USER_OBJS)
        @echo 'Building target: $@ as $(BUILD_TYPE)'
ifeq ($(BUILD_TYPE),library)
# Build as Library
ifeq ($(LINKING_TYPE),static)
# Static
#       @echo 'Invoking: GCC Archiver'
        ar -r $(BUILD_NAME) $(OBJS) $(USER_OBJS) $(LIBS)
else
ifeq ($(LINKING_TYPE),dynamic)
# Dynamic
#       @echo 'Invoking: GCC C++ Linker'
        g++ -o $(BUILD_NAME) $(OBJS) $(USER_OBJS) $(LIBS) $(LIBRARY_PATHS)
else
        @echo 'Invalid linking type: $(LINKING_TYPE)'
endif
endif
else
ifeq ($(BUILD_TYPE),executable)
# Build as executable
#       @echo 'Invoking: GCC C++ Linker'
        g++ $(LIBRAY_PATHS) -o $(BUILD_NAME) $(OBJS) $(USER_OBJS) $(LIBS) $(LIBRARY_PATHS)
else
        @echo "Invalid build type: I don't know how to build: $(BUILD_TYPE)"
endif
endif
        @echo -e 'e[5me[92mFinished building target: $@e[39me[25m'





# clean target
clean:
        rm -Rf $(OBJS)$(C++_DEPS)$(C_DEPS)$(CC_DEPS)$(CPP_DEPS)$(EXECUTABLES)$(CXX_DEPS)$(C_UPPER_DEPS) $(BUILD_NAME)


# run target
run:
        @./$(BUILD_NAME) $(RUN_ARGS)



# Add all .cpp files to subdir.mk
addsubdir:
ifdef DIR
        @if [ ! -d $(DIR) ]; then mkdir $(DIR); fi
        @if [ -f ./$(DIR)/subdir.mk ]; then mv ./$(DIR)/subdir.mk ./$(DIR)/subdir.mk.bakup; fi
        @ls -la ../$(DIR)/*.cpp | awk 'BEGIN{ nrFiles=0; }function printFiles(variable){ print variable" += "; for(i=0;i<nrFiles;i++){ fileName=files[i]; if( variable == "OBJS" || variable == "CPP_DEPS" ) fileName=substr(fileName,2,length(fileName)-1) ; exten="cpp"; if(variable == "OBJS") exten="o"; if(variable == "CPP_DEPS") exten="d"; fileName=substr(fileName,1,length(fileName)-3)exten; printf fileName; if(i<nrFiles-1) print " "; else print ""; }}{ files[nrFiles++]=$$9; }END{ printFiles("CPP_SRCS"); print ""; printFiles("OBJS"); print ""; printFiles("CPP_DEPS"); print ""; }' > ./$(DIR)/subdir.mk
# Append compilation rules to the subdir.mk file
        @echo -ne "src/%.o: ../src/%.cppn
t@echo 'Building file: $$<'n
t@echo 'Invoking: GCC C++ Compiler'n
ifeq ($$(OPTIMIZATION_TYPE),debug)n
tg++ $$(GLOBAL_INCLUDE_PATHS) -O0 -g3 -ggdb -Wall -c -fmessage-length=0 -fPIC -MMD -MP -MF"$$(@:%.o=%.d)" -MT"$$(@:%.o=%.d)" -o "$$@" "$$<"n
elsen
ifeq ($$(OPTIMIZATION_TYPE),release)n
tg++ $$(GLOBAL_INCLUDE_PATHS) -O3 -Wall -c -fmessage-length=0 -fPIC -MMD -MP -MF"$$(@:%.o=%.d)" -MT"$$(@:%.o=%.d)" -o "$$@" "$$<"n
elsen
t@echo 'Invalid optimization type: '$$(OPTIMIZATION_TYPE)n
endifn
endifn
t@echo 'Finished building: $$<'n
t@echo ''
" >> ./$(DIR)/subdir.mk
        @echo 'Please add the folder $(DIR) to SUBDIRS := and -include $(DIR)/subdir.mk to makefile !'
else
        @echo "Please specify a DIR=src or something"
endif


.PHONY: all clean dependents
.SECONDARY:
```

---
---
---

### 55. OpenBSD install packets

Install Packages in OpenBSD:
```sh
#!/bin/ksh
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# Date: 2016-03-01
# Package management
# Run with no parameters to see help

mirror="ftp.openbsd.org"
os_version="$(uname -r)"
architecture="$(uname -p)"
if [ "$1" == "install" ]; then
        PKG_PATH=ftp://$mirror/pub/OpenBSD/$os_version/packages/$architecture pkg_add $2
elif [ "$1" == "search" ]; then
        (sleep 0.5 ; echo cd pub/OpenBSD/$os_version/packages/$architecture ; echo "ls $2" ; sleep 0.5) | ftp ftp://$mirror/
else
        echo "Search for packagges: ./package search python-*"
        echo "Install package: ./package install python"
fi
```
Or, you could simply run this after boot: `export PKG_PATH=http://ftp.eu.openbsd.org/pub/OpenBSD/$(uname -r)/packages/$(uname -p)`

Shut down the operating system: `halt -p`

http://www.openbsd.org/faq/faq4.html

---
---
---

### 56. Install munin

Tested on Debian 8.

```bash
# Install munin-node package:
apt-get install munin-node

# Set it to respond to your server’s IP:
echo "allow ^192.168.0.3$" >> /etc/munin/munin-node.conf

# Apply the config changes:
/etc/init.d/munin-node force-reload
```
#### Add node to your server

Open `/etc/munin/munin.conf` with your favorite editor and add the following section:
```bash
[server1.mydomain.com]
address 192.168.2.122
use_node_name yes
```
Save and quit. Then apply the config on the server:
```bash
/etc/init.d/munin stop ; /etc/init.d/munin start
```
Note: When adding `mysql_queries` plugin, remember that it depends on package `libcache-cache-perl`. Check status with:
```bash
munin-node-configure --suggest 2>&1 | less
```

---
---
---

### 57. Remove contents of file - make it empty

```bash
# 1. The most efficient would be:
truncate -s 0 filename

# 2. These also work:
> filename

# 3. Also opens /dev/null , this makes it a little slower
cp /dev/null filename
```
Thanks to http://unix.stackexchange.com/a/88810

---
---
---

### 58. Disk cloning

I often had to create a copy of an entire disk, or just some partitions. Maybe I
needed to make a backup copy of the operating system, or clone the OS to another
hard drive, or simply copy a CD to a USB stick. These procedures are very much
alike. They are based on a simple process: Reading from one place, and writing
to another place.

To make such a copy I have been using a simple tool called *dd*. This tool is
already present in most UNIX-like distributions. You can boot any live linux
from a USB stick or CD, and start using dd.

The program has two important parameters:

1. `if` - **Input File** - Used to tell dd where is the source. The disk that it should read from.
2. `of` - **Output File** - Used to tell dd where is the destination. The disk that it should write to.

It’s extremely important to double check the values of these two parameters,
especially the output file, because it’s extremely easy to destroy the contents
of one of your disks. You should also pay attention because the two values are
probably going to be very similar. For example: copying from `/dev/sda` to `/dev/sdb`.
You can imagine what would happen if you swap these values by accident.

To determine the source and destination disks, you can use commands like the following:
```bash
ls -l /dev/sd*          # <-- Gives you details about present devices
fdisk -l                # <-- (or gdisk for GPT)Gives you details about present disks and their partition table
hdparm -I /dev/sda      # <-- Gives you details about a given SATA/IDE devices
```

dd reads from the source and writes to the destination, until end of
source/destination is reached or some other problem occurs. However, this
behaviour can be changed using the bs and count parameters.

1. `bs` - **BlockSize** - Tells dd how many bytes(octets) should be copied "at once".
The program will allocate a block of memory(RAM) of bs octets, then copy bs
octets from the source, to RAM, and then copy the same bs octets from RAM to the
destination. The value of this parameter can also be in the following format:
1MB, 5GB, and so on ...
2. `count` - Tells dd how many blocks should be copied. This is useful when you
don’t want to copy the entire disk (For example, only copy the first 10 GB).

You should know that `dd` copies bit by bit, therefore, it can copy the MBR, the
partition table, ... it’s all the same to dd, just some bits. It even copies
free space. For example if your partition has a size of 100GB with 30GB of data
and 70GB of free space, dd will require the same time and effort to copy "free"
space, as it requires to copy your data ... it’s all bits, dd is not aware of
the meaning of the data it is copying.


Because errors can occur while reading from the source (maybe the source has
some bad sectors), we can use the conv parameter to solve any problems caused by
this:

`conv=noerror,sync`:
1. The `noerror` value instructs dd to continue operation, ignoring all read
errors. The default behavior for dd is to halt at any error.
2. The `sync` value instructs dd to fill input blocks with zeroes if there were
any read errors, so data offsets stay in sync. This can be important if some
data must be at a certain sector(address) on the disk.

dd does not print anything while copying, so you have no idea over the progress.
However, we can send a USR1 signal to the process, and make it show the progress
( For example: `kill -USR1 PID_of_dd` ).

Example of running dd:
```bash
# Copy the entire drive(HDD) sdb to sda. (Skipping any errors and using 0 to keep the date in sync)
dd if=/dev/sdb of=/dev/sda conv=noerror,sync

# Copy the first 500 MB from sdb to sda. (Skipping any errors and using 0 to keep the date in sync)
dd if=/dev/sdb of=/dev/sda bs=1MB count=500 conv=noerror,sync

# Copy the content of the first partition from sda to the first partition on sdc (Skipping any errors and using 0 to keep the data in sync).
dd if=/dev/sda1 of=/dev/sdc1 conv=noerror,sync
```

Note:

1. I have been experiencing issues when copying from a HDD to a SSD (windows
crashed when starting the clone). Might be because of some driver problems.
I’m not sure.

I hope this helps you, thanks for your attention :) !

---
---
---

### 59. VPN tunnel over SSH

Say you have two computers and you need to create a tunnel (virtual interface,
not just port forwarding) that connects them. Maybe you have two routers, at two
different locations on the internet, and you need this secure tunnel between
them, that allows computers from one side to communicate with the computers from
the other side, and vice-versa.

Let call those two servers `A` and `B`. Make sure that `root@A` can connect as
`root@B` through ssh, using a key based authentication. So, if A will connect to
B, then make sure that PermitTunnel is set to yes on B (Check that in
`/etc/ssh/sshd_config`). Once that’s done, restart the ssh server by running:
`/etc/init.d/ssh restart`. Now you can create the tunnel. Run this on `A`:
`ssh -NTC -w 1:0 IP_OF_B` where `IP_OF_B` is a name or ip that you use to
connect to server `B`. You might also need to add `-p PORT` if you need to
connect to another port. Or you might set all these up in `~/.ssh/config` and
just use the name.

This command will create an interface `tun1` on `A`, and an interface `tun0` on
`B`. You’ll need to bring them up, set an IP, and add routes and firewall rules.
Do the following on both `A` and `B`, using the appropriate values.

Check the interfaces with:
```bash
ip addr show tun0
ifconfig tun0
```

1. Bring them up with this command:`ip link set tun0 up`
2. Set the IP with this command:`ip addr add 10.0.0.1/32 peer 10.0.0.2 dev tun0`

Now you should be able to ping the other end. In my case I created `tun0` on `A`
and `tun1` on `B`, then set `10.0.0.4` to `tun1`(on `B`) and `10.0.0.3` to
`tun0`(on `A`). Now I’m able to ping `10.0.0.4` from `A` and `10.0.0.3` from `B`.

The tunnel is set, now in order to fully unite the two networks(the network `A`
is part of, and the network `B` is part of), you’ll need to add a few rules,
to your routing table and to your firewall.

Let's say that server `A` has a network interface `eth0` that connects to
network `192.168.1.0/24` and that server `B` has a network interface `eth0` that
connects to network `192.168.0.0/24`. We would like to allow computers from both
sides, to talk one with the other.

on B: `route add -net 192.168.1.0 gw 10.0.0.4 netmask 255.255.255.0 dev tun1`
on A:
```bash
iptables -A FORWARD -i eth0 -o tun0 -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -s 10.0.0.0/24 -o eth0 -j ACCEPT
iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o eth0 -j MASQUERADE
```
---
#### Scripts - work in progress
**Devel**:
```bash
iface tun1 inet static
pre-up sleep 5
pre-up iptables -A FORWARD -i eth0 -o tun1 -m state --state ESTABLISHED,RELATED -j ACCEPT
pre-up iptables -A FORWARD -s 10.0.0.0/24 -o eth0 -j ACCEPT
pre-up iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o eth0 -j MASQUERADE
address 10.0.0.1
pointtopoint 10.0.0.2
netmask 255.255.255.0
up arp -sD 10.0.0.2 eth0 pub
```
**remoteDC**:
```bash
auto lo eth0 tun1
....
iface tun1 inet static
pre-up ssh -f -w 1:1 devel 'ifdown tun1; ifup tun1'
pre-up sleep 5
pre-up route add -net 192.168.0.0 gw 10.0.0.1 netmask 255.255.255.0 dev tun1
address 10.0.0.2
pointtopoint 10.0.0.1
netmask 255.255.255.0
up ip route add 10.0.0.0/24 via 10.0.0.2
down ip route del 10.0.0.0/24 via 10.0.0.2
```
---
**script**:
```bash
#!/bin/bash
echo "Author: Tancredi-Paul Grozav"

# Config
remoteHost=devel
tunnelNumber=3
tunnelPrefixName="tun"
tunnelName="$tunnelPrefixName$tunnelNumber"
tunnelLocalIP="10.1.0.1"
tunnelRemoteIP="10.1.0.2"
remoteNetworkToJoin="192.168.0.0"
remoteNetworkToJoinNetmask="255.255.255.0"
remoteNetworkToJoinInterfaceName="eth0"
localNetworkToJoin="192.168.1.0"
localNetworkToJoinNetmask="255.255.255.0"
localNetworkToJoinInterfaceName="virbr1"

# Allow traffic comming from remote to local virtual LAN
# route add -net 192.168.1.0 gw 10.1.0.2 netmask 255.255.255.0 dev tun3

if [ "$1" == "start" ]; then

echo "Creating tunnels on both sides ..."
# Create tunnels on both sides. This computer connects to the other one as root over SSH
ssh -f -NTC -w $tunnelNumber:$tunnelNumber $remoteHost
# If the tunnel interfaces were created they should appear in the output of : ip addr

echo "Bringing up the interfaces ..."
ip link set $tunnelName up
ssh $remoteHost ip link set $tunnelName up
# If everything went well the interface should appear in the output of : ifconfig tun3

echo "Assigning IPs to interfaces ..."
ip addr add $tunnelLocalIP/32 peer $tunnelRemoteIP dev $tunnelName
ssh $remoteHost ip addr add $tunnelRemoteIP/32 peer $tunnelLocalIP dev $tunnelName
# If everything went well the IPs should appear in the output of : ip addr

echo "Adding packet rules ..."
# Access remote network through tunnel
route add -net $remoteNetworkToJoin gw $tunnelLocalIP netmask $remoteNetworkToJoinNetmask dev $tunnelName
# Set 192.168.1.0 default internet connection to go through the tunnel, while the default internet connection of the computer remains the same
ip route add default via $tunnelLocalIP dev $tunnelName table 200
ip rule add from $localNetworkToJoin/24 table 200
# On the remote host - Access remote (local) network through tunnel
ssh devel route add -net $localNetworkToJoin gw $tunnelRemoteIP netmask $localNetworkToJoinNetmask dev $tunnelName
# Remote FORWARD & MASQUERADING
ssh $remoteHost iptables -A FORWARD -i $remoteNetworkToJoinInterfaceName -o $tunnelName -m state --state ESTABLISHED,RELATED -j ACCEPT
ssh $remoteHost iptables -A FORWARD -s $tunnelLocalIP -o $remoteNetworkToJoinInterfaceName -j ACCEPT
ssh $remoteHost iptables -t nat -A POSTROUTING -s $tunnelLocalIP -o $remoteNetworkToJoinInterfaceName -j MASQUERADE

echo "DONE !"
elif [ "$1" == "stop" ]; then
echo "Removing packet rules ..."
route del -net $remoteNetworkToJoin gw $tunnelLocalIP netmask $remoteNetworkToJoinNetmask dev $tunnelName
ip route del default via $tunnelLocalIP dev $tunnelName table 200
ip rule del from $localNetworkToJoin/24 table 200
ssh devel route del -net $localNetworkToJoin gw $tunnelRemoteIP netmask $localNetworkToJoinNetmask dev $tunnelName
ssh $remoteHost iptables -D FORWARD -i $remoteNetworkToJoinInterfaceName -o $tunnelName -m state --state ESTABLISHED,RELATED -j ACCEPT
ssh $remoteHost iptables -D FORWARD -s $tunnelLocalIP -o $remoteNetworkToJoinInterfaceName -j ACCEPT
ssh $remoteHost iptables -t nat -D POSTROUTING -s $tunnelLocalIP -o $remoteNetworkToJoinInterfaceName -j MASQUERADE

echo "Stopping the interfaces ..."
ip link set $tunnelName down
ssh $remoteHost ip link set $tunnelName down

echo "Stopping the tunnel ..."
kill -9 $(ps faux | grep "ssh -f -NTC -w $tunnelNumber:$tunnelNumber $remoteHost" | grep -v grep | awk '{print $2}')

echo "DONE !"
# You can watch traffic on both sides with: watch -n 0.5 -t -d "ifconfig eth0 ; ifconfig tun3 ; iptables -nvL ; iptables -nvL -t nat ; route"
# And with : tcpdump -vvv -i eth0 icmp
else
echo "What does "$1" mean? I only know about "start" and "stop""
fi
```
Thanks to : https://help.ubuntu.com/community/SSH_VPN

---
---
---

### 60. BUG: Eclipse makefiles don’t recompile on header change

As you can see in the console below, If the makefile(or `subdir.mk`) uses
`-MT"$(@:%.o=%.d)"` then, when changing the header file, the project is not
recompiled. Makefile simply states "Nothing to be done for all". Which is not
the outcome I’d expect.

However, if I replaced `-MT"$(@:%.o=%.d)"` with `-MT"$@"` then, everything works
as expected. If you change the files, the project is recompiled, if you make the
project again(without changing sources), nothing happens, and if you change the
sources again, the files are recompiled.

```bash
paul@grozav:~/data/tmp/mftst$ for f in *; do echo —$f—; cat $f; done
—a.cpp—
#include "a.h"
int main(){}
—a.h—
//
—makefile—
cpps := a.cpp
objs := a.o
deps := a.d

-include $(deps)

%.o:%.cpp
g++ -c "$&lt;" -o "$@" -MMD -MP -MF"$(@:%.o=%.d)" -MT"$(@:%.o=%.d)"

all: a

a: $(objs)
g++ $(objs) -o a

clean:
rm -rf $(objs) $(deps) a
paul@grozav:~/data/tmp/mftst$ make all
g++ -c "a.cpp" -o "a.o" -MMD -MP -MF"a.d" -MT"a.d"
g++ a.o -o a
paul@grozav:~/data/tmp/mftst$ echo "//test" &gt;&gt; a.h
paul@grozav:~/data/tmp/mftst$ make all
make: Nothing to be done for ‘all’.
paul@grozav:~/data/tmp/mftst$ vim makefile # Replaced -MT"$(@:%.o=%.d)" with -MT"$@"
paul@grozav:~/data/tmp/mftst$ echo "//test" &gt;&gt; a.h
paul@grozav:~/data/tmp/mftst$ make all
g++ -c "a.cpp" -o "a.o" -MMD -MP -MF"a.d" -MT"a.o"
g++ a.o -o a
paul@grozav:~/data/tmp/mftst$ make all
make: Nothing to be done for ‘all’.
paul@grozav:~/data/tmp/mftst$ echo "//test" &gt;&gt; a.h
paul@grozav:~/data/tmp/mftst$ make all
g++ -c "a.cpp" -o "a.o" -MMD -MP -MF"a.d" -MT"a.o"
g++ a.o -o a
```

---
---
---

### 61. Google Mock Turtle-Painter example
```bash
pgrozav:ex2>for f in turtle.h mock_turtle.h painter.h mock_turtle_test.cc run.sh; do echo ==========$f==========; cat $f; done && ./turtle_test
==========turtle.h==========
#pragma once

class Turtle
{
public:
        virtual ~Turtle(){}
        virtual void PenUp() = 0;
        virtual void PenDown() = 0;
        virtual void Forward(int distance) = 0;
        virtual void Turn(int degrees) = 0;
        virtual void GoTo(int x, int y) = 0;
        virtual int GetX() const = 0;
        virtual int GetY() const = 0;
};
==========mock_turtle.h==========
#pragma once

#include "turtle.h"
#include "gmock/gmock.h" // Brings in Google Mock

class MockTurtle : public Turtle
{
public:
        MOCK_METHOD0(PenUp, void());
        MOCK_METHOD0(PenDown, void());
        MOCK_METHOD1(Forward, void(int distance));
        MOCK_METHOD1(Turn, void(int degrees));
        MOCK_METHOD2(GoTo, void(int x, int y));
        MOCK_CONST_METHOD0(GetX, int());
        MOCK_CONST_METHOD0(GetY, int());
};
==========painter.h==========
#pragma once
#include "turtle.h"

class Painter
{
        Turtle * turtle;
public:
        Painter( Turtle * turtle): turtle(turtle){}
        bool DrawCircle(int, int, int)
        {
                turtle->PenDown();
                return true;
        }
};
==========mock_turtle_test.cc==========
#include "mock_turtle.h"
#include "painter.h"

#include "gtest/gtest.h"

using ::testing::AtLeast;

TEST(PainterTest, CanDrawSomething)
{
        MockTurtle turtle;
        EXPECT_CALL(turtle, PenDown()).Times(AtLeast(1));
        Painter painter(&turtle);

        EXPECT_TRUE(painter.DrawCircle(0,0,10));
}

int main(int argc, char** argv)
{
        // The following line must be executed to initialize Google Mock
        // (and Google Test) before running the tests.
        ::testing::InitGoogleMock(&argc, argv);
        return RUN_ALL_TESTS();
}
==========run.sh==========
g++ mock_turtle_test.cc -o turtle_test -I../googletest/googlemock/include -I../googletest/googletest/include -lgtest -L../googletest/googletest/build -lgmock -L../googletest/googlemock/build -lpthread &&
./turtle_test
[==========] Running 1 test from 1 test case.
[----------] Global test environment set-up.
[----------] 1 test from PainterTest
[ RUN      ] PainterTest.CanDrawSomething
[       OK ] PainterTest.CanDrawSomething (0 ms)
[----------] 1 test from PainterTest (0 ms total)

[----------] Global test environment tear-down
[==========] 1 test from 1 test case ran. (0 ms total)
[  PASSED  ] 1 test.
```

---
---
---

### 62. Static/Dynamic typing programming

I like C++ for being a strongly typed language that is more likely to generate
an error or refuse to compile if the value assigned to a variable does not
closely match the expected type. On the other hand, a very weakly typed language
may produce unpredictable results or may perform implicit type conversion.

**Static typing** is where the type is bound to the *variable*. Types are
checked at compile time.

**Dynamic typing** is where the type is bound to the *value*. Types are checked
at run time.

So in Java for example:
```java
String s = "abcd";
```
`s` will "forever" be a `String`. During it's life it may point to different `String`s but it will never point to an `Integer` or a `List`. That's static typing.

In PHP:
```php
$s = "abcd";          // $s is a string
$s = 123;             // $s is now an integer
$s = array(1, 2, 3);  // $s is now an array
$s = new DOMDocument; // $s is an instance of the DOMDocument class
$s = function($a){};  // $s is a function
```
That’s dynamic typing.

Thanks to http://stackoverflow.com/a/2690593 for a clear explanation of the concepts.

---
---
---

### 63. Syslog and custom C app
```cpp
#include <syslog.h>

int main()
{
  setlogmask(LOG_UPTO(LOG_NOTICE)); // Returns the previous log priority mask.
  openlog("my_program", LOG_CONS | LOG_PID | LOG_NDELAY, LOG_LOCAL7);
  syslog(LOG_WARNING, "[thread_nr1] Program started by User %d", 44);
  closelog();
  return 0;
}
```

Compile the program above (no special linking is needed). This program will make
a [syslog system call](http://man7.org/linux/man-pages/man2/syslog.2.html).

You should then configure your syslog daemon to handle these messages. For
example, I use rsyslogd and I created: `/etc/rsyslog.d/example_program.conf`:
```txt
template(name="logline" type="string" string="%TIMESTAMP% %HOSTNAME% %syslogtag%%msg%\n")

# Filter by program identifier string
if ($programname == ‘my_program’) then
{
# :msg, regex, "\\[thread_nr1\\].*" /var/log/example_program.log
action( type="omfile" file="/var/log/example_program.log" template="logline" )
}
```

Then simply restart the daemon, tail on your log and run the program.
```bash
/etc/init.d/rsyslog restart
tail -F /var/log/example_program.log
```

---
---
---

### 65. Upgrade Debian to another major version
```bash
# Get list of new packages
apt-get update

# Install newer versions of packages
apt-get upgrade

# Besides upgrade, also handles changing dependencies with new versions
apt-get dist-upgrade

# Performs database sanity and consistency checks
dpkg -C

# List of packages on hold
apt-mark showhold

# Backup and replace debian version
cp /etc/apt/sources.list /etc/apt/sources.list_jessie
sed -i ‘s/jessie/stretch/g’ /etc/apt/sources.list

# Get list of new packages
apt-get update

# Survey of packages to be installed, updated and removed
# without affecting the system
apt list –upgradable

# Install newer versions of packages
apt-get upgrade

# Besides upgrade, also handles changing dependencies with new versions
apt-get dist-upgrade

# List packages which were removed from repository
aptitude search ‘~o’
```

---
---
---

### 66. Processes and /proc filesystem
**Work In Progress**

(not only) For system administrators !

Not all the content from this document applies to OpenBSD.

#### Processes

Print all processes:
```bash
# ps faux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...
paul     12648  0.1  0.7 527000 62676 ?        Sl   Jun10   9:37 konsole
paul     12656  0.0  0.0  24672  6640 pts/1    Ss+  Jun10   0:01  \_ /bin/bash
...
```
In the output above you can see the following columns:
1. **USER** - The owner of the process. The process is limited by limits that apply to that user.
2. **PID** - **P**rocess **I**dentifier. A unique natural number.
3. **%CPU** - The CPU usage of that process. 100 means one full core, 650 means 6 cores and a half, and so on.
4. **%MEM** - Percent of total memory(see `free -m`).
5. **VSZ** - Virtual memory size of the process in `KiB` (1024-byte units). All the memory a process can access(including swapped memory).
6. **RSS** - Resident set size, the non-swapped physical memory that a task has used (inkiloBytes). Actual RAM usage (stack & heap).
7. **TTY** - Controlling tty (terminal).
8. **STAT** - Multi-character process state. See section PROCESS STATE CODES for the different values meaning.
```txt
PROCESS STATE CODES
       Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display to describe the state of a process:

               D    uninterruptible sleep (usually IO)
               R    running or runnable (on run queue)
               S    interruptible sleep (waiting for an event to complete)
               T    stopped, either by a job control signal or because it is being traced
               W    paging (not valid since the 2.6.xx kernel)
               X    dead (should never be seen)
               Z    defunct ("zombie") process, terminated but not reaped by its parent

       For BSD formats and when the stat keyword is used, additional characters may be displayed:

               <    high-priority (not nice to other users)
               N    low-priority (nice to other users)
               L    has pages locked into memory (for real-time and custom IO)
               s    is a session leader
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
               +    is in the foreground process group
```
9. **START** - Time the command started.
10. **TIME** - Accumulated cpu time, user + system.  The display format is usually 'MMM:SS", but can be shifted to the right if the process used more than 999 minutes of cpu time.
11. **COMMAND** - Command with all its arguments as a string. The output in this column may contain spaces. A process marked <defunct> is partly dead, waiting to be fully destroyed by its parent.

Because we used the `f` flag, in the COMMAND column we can see a tree like
structure that tells us about parent-child inter-process relationships. For
example, in the output above, we can see that the `konsole` process started a
`/bin/bash` process. Therefore, the `konsole` process is the parent and the
`/bin/bash` process is the child of `konsole`.

#### Process threads
```bash
pgrozav:/>ps aux -e -T
USER       PID  SPID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...
paul     15534 15534  0.1  4.9 2028272 393016 ?      Sl   Jun10   9:43 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15535  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:29 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15554  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15592  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15593  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534  3552  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534  3553  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 21970  0.0  4.9 2028272 393016 ?      Sl   Jun14   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
...
```
In the example above, we ran `ps aux -e -T` and an extra column was added:
`SPID`, which is a light weight process (thread) ID. In the output above we have
the qtcreator application that started 8 threads. The qtcreator is a process
that has the PID of `15534` and each of it’s 8 threads has it’s own thread
identifier(SPID).

#### /proc filesystem

**IF** your operating system supports the proc filesystem, you can see more
details about each process in `/proc/PID` like this:
```bash
pgrozav:/>tree /proc/12656
/proc/12656
├── attr
│   ├── current
│   ├── exec
│   ├── fscreate
│   ├── keycreate
│   ├── prev
│   └── sockcreate
├── autogroup
├── auxv
├── cgroup
├── clear_refs
├── cmdline
├── comm
├── coredump_filter
├── cpuset
├── cwd -> /home/paul
├── environ
├── exe -> /bin/bash
├── fd
│   ├── 0 -> /dev/pts/1
│   ├── 1 -> /dev/pts/1
│   ├── 2 -> /dev/pts/1
│   └── 255 -> /dev/pts/1
├── fdinfo
│   ├── 0
│   ├── 1
│   ├── 2
│   └── 255
├── gid_map
├── io
├── limits
├── loginuid
├── map_files
├── maps
├── mem
├── mountinfo
├── mounts
├── mountstats
├── net
│   ├── anycast6
│   ├── arp
│   ├── bnep
│   ├── connector
│   ├── dev
│   ├── dev_mcast
│   ├── dev_snmp6
│   │   ├── eth0
│   │   ├── lo
│   │   ├── tun3
│   │   ├── virbr0
│   │   ├── virbr0-nic
│   │   ├── virbr1
│   │   ├── virbr1-nic
│   │   ├── vnet0
│   │   └── vnet1
│   ├── fib_trie
│   ├── fib_triestat
│   ├── hci
│   ├── icmp
│   ├── icmp6
│   ├── if_inet6
│   ├── igmp
│   ├── igmp6
│   ├── ip6_flowlabel
│   ├── ip6_mr_cache
│   ├── ip6_mr_vif
│   ├── ip_conntrack
│   ├── ip_conntrack_expect
│   ├── ip_mr_cache
│   ├── ip_mr_vif
│   ├── ip_tables_matches
│   ├── ip_tables_names
│   ├── ip_tables_targets
│   ├── ipv6_route
│   ├── l2cap
│   ├── mcfilter
│   ├── mcfilter6
│   ├── netfilter
│   │   └── nf_log
│   ├── netlink
│   ├── netstat
│   ├── nf_conntrack
│   ├── nf_conntrack_expect
│   ├── nfsfs
│   │   ├── servers
│   │   └── volumes
│   ├── packet
│   ├── protocols
│   ├── psched
│   ├── ptype
│   ├── raw
│   ├── raw6
│   ├── route
│   ├── rpc
│   │   ├── auth.rpcsec.context
│   │   │   ├── channel
│   │   │   └── flush
│   │   ├── auth.rpcsec.init
│   │   │   ├── channel
│   │   │   └── flush
│   │   ├── auth.unix.gid
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── auth.unix.ip
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfs
│   │   ├── nfs4.idtoname
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfs4.nametoid
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfsd
│   │   ├── nfsd.export
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfsd.fh
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   └── use-gss-proxy
│   ├── rt6_stats
│   ├── rt_acct
│   ├── rt_cache
│   ├── sco
│   ├── snmp
│   ├── snmp6
│   ├── sockstat
│   ├── sockstat6
│   ├── softnet_stat
│   ├── stat
│   │   ├── arp_cache
│   │   ├── ip_conntrack
│   │   ├── ndisc_cache
│   │   ├── nf_conntrack
│   │   └── rt_cache
│   ├── tcp
│   ├── tcp6
│   ├── udp
│   ├── udp6
│   ├── udplite
│   ├── udplite6
│   ├── unix
│   └── wireless
├── ns
│   ├── ipc -> ipc:[4026531839]
│   ├── mnt -> mnt:[4026531840]
│   ├── net -> net:[4026531956]
│   ├── pid -> pid:[4026531836]
│   ├── user -> user:[4026531837]
│   └── uts -> uts:[4026531838]
├── numa_maps
├── oom_adj
├── oom_score
├── oom_score_adj
├── pagemap
├── personality
├── projid_map
├── root -> /
├── sched
├── sessionid
├── setgroups
├── smaps
├── stack
├── stat
├── statm
├── status
├── syscall
├── task
│   └── 12656
│       ├── attr
│       │   ├── current
│       │   ├── exec
│       │   ├── fscreate
│       │   ├── keycreate
│       │   ├── prev
│       │   └── sockcreate
│       ├── auxv
│       ├── cgroup
│       ├── children
│       ├── clear_refs
│       ├── cmdline
│       ├── comm
│       ├── cpuset
│       ├── cwd -> /home/paul
│       ├── environ
│       ├── exe -> /bin/bash
│       ├── fd
│       │   ├── 0 -> /dev/pts/1
│       │   ├── 1 -> /dev/pts/1
│       │   ├── 2 -> /dev/pts/1
│       │   └── 255 -> /dev/pts/1
│       ├── fdinfo
│       │   ├── 0
│       │   ├── 1
│       │   ├── 2
│       │   └── 255
│       ├── gid_map
│       ├── io
│       ├── limits
│       ├── loginuid
│       ├── maps
│       ├── mem
│       ├── mountinfo
│       ├── mounts
│       ├── ns
│       │   ├── ipc -> ipc:[4026531839]
│       │   ├── mnt -> mnt:[4026531840]
│       │   ├── net -> net:[4026531956]
│       │   ├── pid -> pid:[4026531836]
│       │   ├── user -> user:[4026531837]
│       │   └── uts -> uts:[4026531838]
│       ├── numa_maps
│       ├── oom_adj
│       ├── oom_score
│       ├── oom_score_adj
│       ├── pagemap
│       ├── personality
│       ├── projid_map
│       ├── root -> /
│       ├── sched
│       ├── sessionid
│       ├── setgroups
│       ├── smaps
│       ├── stack
│       ├── stat
│       ├── statm
│       ├── status
│       ├── syscall
│       ├── uid_map
│       └── wchan
├── timers
├── uid_map
└── wchan

29 directories, 211 files
```
TO DO: Explain cwd, exe, task, fd, and other known members

Thanks to:
1. http://man7.org/linux/man-pages/man5/proc.5.html

#### Socket file descriptors
```bash
(0)paul@server:/home/paul $ ls -l /proc/23674/fd | grep socket
lrwx——. 1 paul paul 64 May 17 11:37 10 -> socket:[1842046202]
lrwx——. 1 paul paul 64 May 17 11:37 16 -> socket:[1842046212]
lrwx——. 1 paul paul 64 May 17 11:37 18 -> socket:[1842046213]
lrwx——. 1 paul paul 64 May 17 11:37 26 -> socket:[1887900892]
lrwx——. 1 paul paul 64 May 17 11:37 7 -> socket:[1842045162]
lrwx——. 1 paul paul 64 May 17 11:37 8 -> socket:[1897079922]
```
You will see that those IDs between square brackets are device ids, and you can
match them with these:
```bash
0)paul@server:/home/paul $ lsof -i -a -p 23674
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
serverapp 23674 paul 7u IPv4 1842045162 0t0 TCP *:54321 (LISTEN)
serverapp 23674 paul 8u IPv4 1897079922 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:56558 (ESTABLISHED)
serverapp 23674 paul 10u IPv4 1842046202 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:65008 (ESTABLISHED)
serverapp 23674 paul 16u IPv4 1842046212 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:59215 (ESTABLISHED)
serverapp 23674 paul 18u IPv4 1842046213 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:52462 (ESTABLISHED)
serverapp 23674 paul 26u IPv4 1887900892 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:59341 (ESTABLISHED)
```

---
---
---

### 67. Windows Server multiple remote desktop session per user

Tested on Windows Server™ 2012 R2 Standard.
1. Run `regedit` as the Administrator user.
2. Go to `HKEY_LOCAL_MACHINE` -> `SYSTEM` -> `CurrentControlSet` -> `Control` -> `Terminal Server`
3. Double-click on the `fSingleSessionPerUser` variable and change it's Value data from `1` (true) to `0` (false)

Later, if you will connect to a user that has multiple disconnected sessions,
you will have to choose one to sign into.choose session to sign into.

There seem to be a hardcoded limit in the windows server that only allows two
sessions to run at a given time. When two sessions are running, if a third user
connects it will receive a dialog and can ask one of the other users to leave
it’s session so that this new guy can use that session.

If you want more sessions it seems that you have to buy some license for each
new session that you need to create, or buy another windows server license,
install the server on another hardware and have the users connect there, but
then you have to sync the servers somehow.

It seems that in order to increase this limit of 2 sessions, you need a
**Terminal Server Licence**.

---
---
---

### 68. Docker commands

A container is an instance of an image. An image is created by running a recipe.

Install : https://docs.docker.com/engine/installation/linux/docker-ce/debian/

Check this docker hub repo: https://hub.docker.com/explore/

List of useful docker commands:
```bash
# Show available docker images
docker images

# Download image
docker pull debian

# Delete image
docker image rm <IMAGE_ID>

# Show containers
docker ps -a

# Create container from image
docker run -it --name="<CONTAINER_NAME>" <IMAGE_ID>
# You can forward ports with -p <HOST_PORT>:<CONTAINER_PORT>
# You can mount folders with -v /host/dir:/container/dest

# Delete container
docker rm <CONTAINER_ID>

# Run image interactively (creates container and removes it on exit)
docker run -it --rm=true <IMAGE_ID>

# Build docker image from Dockerfile, will be accessible by name <IMAGE_NAME>
docker build -t <IMAGE_NAME> .

# Stop container
docker stop <CONTAINER_ID>

# Start existing container

docker start <CONTAINER_ID>

# List volumes
docker volume ls

# Rename container
docker rename <CURRENT_CONTAINER_NAME> <NEW_CONTAINER_NAME>

# Add docker network
docker network create --subnet=172.18.0.0/16 <NETWORK_NAME>

# List docker networks
docker network list

# Delete docker network
docker network remove <NETWORK_ID>

# Run image (by starting new container) with given IP in given network
docker run --net <NETWORK_NAME> --ip 172.18.0.2 -it --rm=true <IMAGE_NAME>

# Run image (by starting new container) container with given host name
docker run -it --rm=true -h <HOST_NAME> <IMAGE_NAME>

# Rename container
docker rename <CONTAINER_ID> <NEW_CONTAINER_NAME>

# Update a running container to auto-restart
docker update --restart=always <CONTAINER_ID>

# Save docker image to file:
docker save -o <PATH_TO_FILE> <IMAGE_NAME>

# Load docker image from file:
docker load -i <PATH_TO_FILE>
# ============================================================================ #
# This seems like a nice UI for docker:
docker run -d -p 10086:10086 -v /var/run/docker.sock:/var/run/docker.sock tobegit3hub/seagull
# Then go to http://127.0.0.1:10086
# Or you can use portainer: https://www.portainer.io/
# ============================================================================ #
# Clean up your package manager when building images, to keep them small !

# To undo apt-get update you can:
rm -Rf /var/lib/apt/lists/*
# or, even better
apt clean

# Run GUI apps in containers
# It seems that doing "xhost +local:paul" and a "xhost -..." after, is not needed !
podman run -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix:rw -e DISPLAY debian:10.7 bash -c "apt update && apt install -y x11-apps && xeyes"
```

---
---
---

### 69. Conan C/C++ package manager

Work in progress. Useful commands:
```bash
# List remotes
conan remote list

# Add remote
conan remote add <remote_name> http://localhost:9300

# Remove remote
conan remote remove <remote_name>

# List all recipes in remote
conan search -r <remote_name>

# A recipe looks like:
# cool_soft/0.1@michael/stable
# And it contains 4 fields: A/B@C/D
# These fields are:
# 1. "cool_soft" – is the name of the package, usually the same of the project
# 2. "0.1" – is the version, usually matching the one of the packaged project
# Can be any string, not necessarily a number, so it is possible to have a
# "develop" or "master" version. Packages can be overwritten, so it is also
# OK to have packages like "nightly" or "weekly", that are regenerated
# periodically.
# 3. "michael" – is the owner of this package version. It is basically a
# namespace that allows different users to have their own packages for the
# same library with the same name, and interchange them. So, for example,
# you can easily upload a certain library under your own user name "lasote",
# and later those packages can be uploaded without modifications to another
# official, group or company username.
# 4. "stable" – is the channel. Channels also allow to have different packages
# for the same library and use them interchangeably. They usually denote the
# maturity of the package, as an arbitrary string: "stable", "testing", but
# it can be used for any purpose, like package revisions (the library
# version has not changed, but the package recipe has evolved)

# Remove recipe from remote
conan remove cool_soft/0.1@michael/stable -r <remote_name>

# List existing packages for recipe
conan search -r <remote_name> cool_soft/0.1@michael/stable

# Create conan recipe
conan new

# Build package for recipe
conan create

# Exports a recipe & creates a package locally
conan export-pkg . cool_soft/0.1@michael/stable

# Upload conan recipe to server:
conan upload cool_soft/0.1@michael/stable -r <remote_name>

# Upload package in recipe to server:
conan upload cool_soft/0.1@michael/stable -p <package_id> -r <remote_name>

# Upload conan recipe and all packages to server:
conan upload cool_soft/0.1@michael/stable –all -r <remote_name>

# Generate table of binaries for recipe:
conan search cool_soft/0.1@michael/stable –table file.html -r <remote_name>

# Upgrade conan client to a newer version:
pip install conan –upgrade

# Remove package from recipe
conan remove -p <package_name> -r <remote_name>
conan remove cool_soft/0.1@michael/stable -p <package_name>

# Download package from remote recipe:
conan download -p <package_id> -r <remote_name> cool_soft/0.1@michael/stable
```
Thanks to http://docs.conan.io/en/latest/index.html .

---
---
---

### 70. ESXI CLI management
```bash
# List running machines:
esxcli vm process list
```

---
---
---

### 71. C/C++ e-mail with libcurl
**C way**:
```cpp
#include <stdio.h>
#include <string.h>
#include <curl/curl.h>

/*
* For an SMTP example using the multi interface please see smtp-multi.c.
*/

/* The libcurl options want plain addresses, the viewable headers in the mail
* can very well get a full name as well.
*/
#define FROM_ADDR "<user1@example.com>"
#define TO_ADDR "<user2@example.com>"
#define CC_ADDR "<user3@example.com>"

#define FROM_MAIL "User One " FROM_ADDR
#define TO_MAIL "User Two " TO_ADDR
#define CC_MAIL "User Three " CC_ADDR

static const char *payload_text[] = {
"Date: Mon, 29 Nov 2017 21:54:29 +1100\r\n",
"To: " TO_MAIL "\r\n",
"From: " FROM_MAIL "\r\n",
"Cc: " CC_MAIL "\r\n",
// "Message-ID: <acd7cb36-11db-487a-9f3a-e652a9458efd@"
// "rfcpedant.example.org>\r\n",
"Subject: TEST SMTP example message\r\n",
"Mime-Version: 1.0\r\n",
"Content-Type: text/html; charset=\"ISO-8859-1\"\r\n",
"Content-Transfer-Encoding: quoted-printable;\r\n",
"\r\n", /* empty line to divide headers from body, see RFC5322 */
"TEST <b>The</b> body of the <a href=\"http://paul.grozav.info\">"
"message</a> starts here.\r\n",
"\r\n",
"It could be a lot of lines, could be MIME encoded, whatever.\r\n",
"Check RFC5322.\r\n",
NULL
};

struct upload_status {
int lines_read;
};

static size_t payload_source(void *ptr, size_t size, size_t nmemb, void *userp)
{
struct upload_status *upload_ctx = (struct upload_status *)userp;
const char *data;

if((size == 0) || (nmemb == 0) || ((size*nmemb) < 1)) {
return 0;
}

data = payload_text[upload_ctx->lines_read];

if(data) {
size_t len = strlen(data);
memcpy(ptr, data, len);
upload_ctx->lines_read++;

return len;
}

return 0;
}

int main(void)
{
CURL *curl;
CURLcode res = CURLE_OK;
struct curl_slist *recipients = NULL;
struct upload_status upload_ctx;

upload_ctx.lines_read = 0;

curl = curl_easy_init();
if(curl) {
/* This is the URL for your mailserver */
curl_easy_setopt(curl, CURLOPT_URL, "smtp://mail.example.com");

/* Note that this option isn’t strictly required, omitting it will result
* in libcurl sending the MAIL FROM command with empty sender data. All
* autoresponses should have an empty reverse-path, and should be directed
* to the address in the reverse-path which triggered them. Otherwise,
* they could cause an endless loop. See RFC 5321 Section 4.5.5 for more
* details.
*/
curl_easy_setopt(curl, CURLOPT_MAIL_FROM, FROM_ADDR);

/* Add two recipients, in this particular case they correspond to the
* To: and Cc: addressees in the header, but they could be any kind of
* recipient. */
recipients = curl_slist_append(recipients, TO_ADDR);
recipients = curl_slist_append(recipients, CC_ADDR);
curl_easy_setopt(curl, CURLOPT_MAIL_RCPT, recipients);

/* We’re using a callback function to specify the payload (the headers and
* body of the message). You could just use the CURLOPT_READDATA option to
* specify a FILE pointer to read from. */
curl_easy_setopt(curl, CURLOPT_READFUNCTION, payload_source);
curl_easy_setopt(curl, CURLOPT_READDATA, &upload_ctx);
curl_easy_setopt(curl, CURLOPT_UPLOAD, 1L);

/* Send the message */
res = curl_easy_perform(curl);

/* Check for errors */
if(res != CURLE_OK)
fprintf(stderr, "curl_easy_perform() failed: %s\n",
curl_easy_strerror(res));

/* Free the list of recipients */
curl_slist_free_all(recipients);

/* curl won’t send the QUIT command until you call cleanup, so you should
* be able to re-use this connection for additional messages (setting
* CURLOPT_MAIL_FROM and CURLOPT_MAIL_RCPT as required, and calling
* curl_easy_perform() again. It may not be a good idea to keep the
* connection open for a very long time though (more than a few minutes
* may result in the server timing out the connection), and you do want to
* clean up in the end.
*/
curl_easy_cleanup(curl);
}

return (int)res;
}
```
**C++ way**:
```cpp
#include <cstring>

#include <fstream>
#include <string>
#include <sstream>
#include <vector>

#include <curl/curl.h>

using namespace ::std;

//—————————————————————————-//
class base64
{
private:
static const std::string BASE64_CHARS;
public:
static inline bool is_base64(unsigned char c);
static ::std::string base64_encode(unsigned char const* bytes_to_encode,
unsigned int in_len);
static ::std::string base64_decode(::std::string const& encoded_string);
};
const ::std::string base64::BASE64_CHARS =
"ABCDEFGHIJKLMNOPQRSTUVWXYZ"
"abcdefghijklmnopqrstuvwxyz"
"0123456789+/";
bool base64::is_base64(unsigned char c) {
return (isalnum(c) || (c == ‘+’) || (c == ‘/’));
}
::std::string base64::base64_encode(unsigned char const* bytes_to_encode,
unsigned int in_len)
{
::std::string ret;
int i = 0;
int j = 0;
unsigned char char_array_3[3];
unsigned char char_array_4[4];

while (in_len–)
{
char_array_3[i++] = *(bytes_to_encode++);
if (i == 3)
{
char_array_4[0] = (char_array_3[0] & 0xfc) >> 2;
char_array_4[1] = static_cast<unsigned char>(
((char_array_3[0] & 0x03) << 4) + ((char_array_3[1] & 0xf0) >> 4));
char_array_4[2] = static_cast<unsigned char>(
((char_array_3[1] & 0x0f) << 2) + ((char_array_3[2] & 0xc0) >> 6));
char_array_4[3] = char_array_3[2] & 0x3f;

for(i = 0; (i <4) ; i++)
{
ret += BASE64_CHARS[char_array_4[i]];
}
i = 0;
}
}

if (i)
{
for(j = i; j < 3; j++)
{
char_array_3[j] = ‘\0’;
}

char_array_4[0] = (char_array_3[0] & 0xfc) >> 2;
char_array_4[1] = static_cast<unsigned char>(
((char_array_3[0] & 0x03) << 4) + ((char_array_3[1] & 0xf0) >> 4));
char_array_4[2] = static_cast<unsigned char>(
((char_array_3[1] & 0x0f) << 2) + ((char_array_3[2] & 0xc0) >> 6));
char_array_4[3] = char_array_3[2] & 0x3f;

for (j = 0; (j < i + 1); j++)
{
ret += BASE64_CHARS[char_array_4[j]];
}

while((i++ < 3))
{
ret += ‘=’;
}
}
return ret;
}
::std::string base64::base64_decode(::std::string const& encoded_string)
{
int in_len = static_cast<int>(encoded_string.size());
int i = 0;
int j = 0;
size_t in_ = 0;
unsigned char char_array_4[4], char_array_3[3];
::std::string ret;

while (in_len– && (encoded_string[in_] != ‘=’) &&
is_base64(static_cast<unsigned char>(encoded_string[in_])))
{
char_array_4[i++] = static_cast<unsigned char>(encoded_string[in_]);
in_++;
if (i ==4)
{
for (i = 0; i <4; i++)
{
char_array_4[i] = static_cast<unsigned char>(BASE64_CHARS.find(
static_cast<char>(char_array_4[i])));
}

char_array_3[0] = static_cast<unsigned char>((char_array_4[0] << 2) + ((char_array_4[1] & 0x30) >> 4));
char_array_3[1] = static_cast<unsigned char>(((char_array_4[1] & 0xf) << 4) + ((char_array_4[2] & 0x3c) >> 2));
char_array_3[2] = static_cast<unsigned char>(((char_array_4[2] & 0x3) << 6) + char_array_4[3]);

for (i = 0; (i < 3); i++)
{
ret += static_cast<char>(char_array_3[i]);
}
i = 0;
}
}

if (i)
{
for (j = i; j <4; j++)
{
char_array_4[j] = 0;
}

for (j = 0; j <4; j++)
{
char_array_4[j] = static_cast<unsigned char>(BASE64_CHARS.find(
static_cast< char>(char_array_4[j])));
}

char_array_3[0] = static_cast<unsigned char>((char_array_4[0] << 2) + ((char_array_4[1] & 0x30) >> 4));
char_array_3[1] = static_cast<unsigned char>(((char_array_4[1] & 0xf) << 4) + ((char_array_4[2] & 0x3c) >> 2));
char_array_3[2] = static_cast<unsigned char>(((char_array_4[2] & 0x3) << 6) + char_array_4[3]);

for (j = 0; (j < i – 1); j++)
{
ret += static_cast<char>(char_array_3[j]);
}
}
return ret;
}
//—————————————————————————-//

//—————————————————————————-//
class curl_email
{
public:
struct contact
{
::std::string name;
::std::string email;
};
typedef ::std::vector< contact > recipients;

private:
struct upload_status {
int lines_read = 0;
::std::vector<::std::string> *mail_content = nullptr;
};
recipients rcpts;
contact from;
::std::vector<::std::string> mail_content;
static size_t payload_source(void *ptr, size_t size, size_t nmemb,
void *userp);

public:
void set_recipients(const recipients &recipients);
void add_field(const ::std::string &key, const ::std::string &value);
void add_empty_field();
void add_to(const contact &to);
void add_cc(const contact &cc);
void add_from(const contact &from);
void add_subject(const ::std::string &subject);
void add_data(const ::std::string &data);
int send();
};
size_t curl_email::payload_source(void *ptr, size_t size, size_t nmemb,
void *userp)
{
struct upload_status *upload_ctx = static_cast<struct upload_status *>(userp);
::std::vector<::std::string> &mail_content = *(upload_ctx->mail_content);
const char *data;

if((size == 0) || (nmemb == 0) || ((size*nmemb) < 1))
{
return 0;
}

if(mail_content.size()>static_cast<unsigned int>(upload_ctx->lines_read))
{
data = mail_content[static_cast<size_t>(upload_ctx->lines_read)].c_str();
size_t len = strlen(data);
memcpy(ptr, data, len);
upload_ctx->lines_read++;
// len should not be larger than size*nmemb
return len;
}
return 0;
}
void curl_email::set_recipients(const recipients &recipients)
{
this->rcpts = recipients;
}
void curl_email::add_field(const ::std::string &key, const ::std::string &value)
{
this->mail_content.push_back(key+": "+value+"\r\n");
}
void curl_email::add_empty_field()
{
this->mail_content.push_back("\r\n");
}
void curl_email::add_to(const contact &to)
{
this->add_field("To", to.name+" <"+to.email+">");
}
void curl_email::add_cc(const contact &cc)
{
this->add_field("Cc", cc.name+" <"+cc.email+">");
}
void curl_email::add_from(const contact &from)
{
this->from = from;
this->add_field("From", from.name+" <"+from.email+">");
}
void curl_email::add_subject(const ::std::string &subject)
{
this->from = from;
this->add_field("Subject", subject);
}
void curl_email::add_data(const ::std::string &data)
{
this->mail_content.push_back(data+"\r\n");
}
int curl_email::send()
{
CURL *curl;
CURLcode res = CURLE_OK;
struct curl_slist *recipients = NULL;
struct upload_status upload_ctx;

upload_ctx.mail_content = &(this->mail_content);

curl = curl_easy_init();
if(curl) {
/* This is the URL for your mailserver */
curl_easy_setopt(curl, CURLOPT_URL, "smtp://mail.example.com");

/* Note that this option isn’t strictly required, omitting it will result
* in libcurl sending the MAIL FROM command with empty sender data. All
* autoresponses should have an empty reverse-path, and should be directed
* to the address in the reverse-path which triggered them. Otherwise,
* they could cause an endless loop. See RFC 5321 Section 4.5.5 for more
* details.
*/
curl_easy_setopt(curl, CURLOPT_MAIL_FROM, from.email.c_str());

/* Add two recipients, in this particular case they correspond to the
* To: and Cc: addressees in the header, but they could be any kind of
* recipient. */
for(recipients::iterator it = rcpts.begin(); it != rcpts.end(); it++)
{
recipients = curl_slist_append(recipients, it->email.c_str());
}
curl_easy_setopt(curl, CURLOPT_MAIL_RCPT, recipients);

/* We’re using a callback function to specify the payload (the headers and
* body of the message). You could just use the CURLOPT_READDATA option to
* specify a FILE pointer to read from. */
curl_easy_setopt(curl, CURLOPT_READFUNCTION, payload_source);
curl_easy_setopt(curl, CURLOPT_READDATA, &upload_ctx);
curl_easy_setopt(curl, CURLOPT_UPLOAD, 1L);
curl_easy_setopt(curl, CURLOPT_VERBOSE, 1L);

/* Send the message */
res = curl_easy_perform(curl);

/* Check for errors */
if(res != CURLE_OK)
{
fprintf(stderr, "curl_easy_perform() failed: %s\n",
curl_easy_strerror(res));
}

/* Free the list of recipients */
curl_slist_free_all(recipients);

/* curl won’t send the QUIT command until you call cleanup, so you should
* be able to re-use this connection for additional messages (setting
* CURLOPT_MAIL_FROM and CURLOPT_MAIL_RCPT as required, and calling
* curl_easy_perform() again. It may not be a good idea to keep the
* connection open for a very long time though (more than a few minutes
* may result in the server timing out the connection), and you do want to
* clean up in the end.
*/
curl_easy_cleanup(curl);
}

return static_cast<int>(res);
}
//—————————————————————————-//

int main()
{
typedef curl_email::contact rec;
rec pg{"Tancredi-Paul Grozav", "paul@example.com"};
curl_email ce;
ce.set_recipients({pg});
ce.add_to(pg);
ce.add_from(pg);
ce.add_subject("Test");
ce.add_field("Date", "Wed, 20 Dec 2017 16:45:43 +0200");
ce.add_field("MIME-Version", "1.0");

::std::string boundary = "————–231BDD24AA49E9F331EE4B89";
ce.add_field("Content-Type", "multipart/mixed; boundary=\""+boundary+"\"");
ce.add_empty_field();
ce.add_data("–"+boundary);
ce.add_field("Content-Type", "text/html; charset=\"ISO-8859-1\"");
ce.add_field("Content-Transfer-Encoding", "7bit");
ce.add_empty_field();
ce.add_data("<b>The</b> <a href=\"http://paul.grozav.info\">message</a>.");
ce.add_empty_field();
ce.add_empty_field();

bool has_attachment = true;
if(has_attachment)
{
string file_name = "20170823.pdf";
ce.add_data("–"+boundary);
ce.add_field("Content-Type", "image/pdf; name=\""+file_name+"\"");
ce.add_field("Content-Transfer-Encoding", "base64");
ce.add_field("Content-Disposition", "attachment; filename=\""+file_name+"\"");
ce.add_empty_field();
::std::ifstream file("/home/paul/Documents/"+file_name);
if(file.is_open())
{
::std::stringstream buffer;
buffer << file.rdbuf();
file.close();
::std::string file_content = buffer.str();
file_content = base64::base64_encode(
reinterpret_cast<const unsigned char*>(file_content.c_str())
, static_cast<unsigned int>(file_content.size()));
// you have to split file in chunks and add it to mail. libcurl has max
// size. See https://curl.haxx.se/mail/lib-2013-04/0324.html
size_t file_size = file_content.size();
const unsigned int chunk_size = 15000;
for(unsigned int i=0; i< file_size; i+=chunk_size)
{
if(i+chunk_size < file_size)
{
ce.add_data(file_content.substr(i, chunk_size));
}else{
ce.add_data(file_content.substr(i, file_size-i));
}
}
}
}
ce.add_data("–"+boundary+"–");
ce.send();
return EXIT_SUCCESS;
}
```
Compile with:
```bash
g++ -c main.cpp -o main.o
g++ main.o -lcurl -o main.bin
./main.bin
```

---
---
---

### 72. Filebeat
```bash
apt-get install wget gnupg apt-transport-https
wget -qO – https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add –
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-6.x.list
apt-get update && apt-get install filebeat

# Set filebeat to start automatically on boot
update-rc.d filebeat defaults 95 10

# Changes to /etc/filebeat/filebeat.yml :
enabled: true
output.elasticsearch:
hosts: ["kibana.my.host:9200"]
index: "docker_c1"
setup.template.enabled: true
setup.template.name: "docker_c1"
setup.template.pattern: "docker_c1"
```

---
---
---

### 74. Compiling C/C++ code with GCC

[They say](https://gcc.gnu.org/onlinedocs/gcc/Invoking-G_002b_002b.html) that:
"On many systems, g++ is also installed with the name c++".
```bash
# Compile a C++ file into an object file:
g++ -c main.cpp -o main.o

# Link object file(s) into shared library:
g++ -shared math.o -o libmath.so

# Link object file(s) into static library:
ar qcs libmath.a math.o

# Link object files, static libraries and shared libraries into an executable binary:
g++ -o main.bin main.o libmath.a libdef.so

# Link using -l instead of full path, and have some static libs and some dynamic libs:
gcc <objects> -o <binary> -Wl,-Bstatic <static_lib_list> -Wl,Bdynamic <dynamic_lib_list>
```
**Advanced**:
```bash
# Detect GLibC versions required by an executable file - The greatest version detected is required.
objdump -T bin/main.bin820 | grep GLIBC_ | awk '{print substr($0, index($0, "GLIBC_"))}' | awk '{print $1}' | sort -u

# Print include paths and linking paths:
g++ -E -x c++ - -v < /dev/null
```

---
---
---

### 75. Artix Linux

This is a relative small and new distribution that comes from Arch Linux and is
influenced by Gentoo. Since things are continuously changing, the info below
might become obsolete.

Installing:

According to https://forum.artixlinux.org/index.php/topic,41.0.html :
1. You boot [the iso](http://mirrors.dotsrc.org/artix-linux/iso/base/archive/),
then check that you have a working internet connection(ping google.com or
something).
2. You use `fdisk` to partition you hdd. I created only one partition
3. You format it using `mkfs.ext4`
4. And mount it to `/mnt`
5. I've commented all servers in `/etc/pacman.d/mirrorlist` and added: `https://mirrors.dotsrc.org/artix-linux/repos/$repo/os/$arch`.
6. then:

```bash
# Then synchronized package databases with that server using:
pacman -Syy
# Then you fix a bug in their script:
sed -e ‘s/${hostcache} && pacman/${hostcache} || pacman/g’ -e ‘s/${interactive} && pacman/${interactive} || pacman/g’ -i /usr/bin/basestrap

#Install base stuff on you new disk:
basestrap -i /mnt base openrc-system openrc-world systemd-dummy libsystemd-dummy # Accept defaults (ENTER)

# Generate your fstab:
fstabgen -U /mnt >> /mnt/etc/fstab

# Chroot to the new system:
artools-chroot /mnt

# Set timezone:
ln -sf /usr/share/zoneinfo/Europe/Bucharest /etc/localtime
hwclock –systohc

# Set locale
echo "LANG=en_US.UTF-8" > /etc/locale.conf

# Set hostname
echo "hostname=\"myhostname\"" > /etc/conf.d/hostname
echo "127.0.0.1 localhost.localdomain localhost" >> /etc/hosts
echo "::1 localhost.localdomain localhost" >> /etc/hosts
echo "127.0.1.1 myhostname.localdomain myhostname" >> /etc/hosts

# Init.d scripts:
rc-update add udev boot
rc-update add elogind boot
rc-update add dbus default

# Set root password
passwd

# Install grub bootloader:
pacman -S grub
grub-mkconfig -o /boot/grub/grub.cfg
# Exit chroot and install grub on vda
grub-install –root-directory=/mnt /dev/vda
```
Other Artix stuff:
1. You might want to have sshd running after boot:
```bash
useradd nobody -d / -s /usr/bin/nologin -g nobody
rc-update add sshd default
```
2. You might want to install docker:
```bash
pacman -S docker docker-openrc
rc-service docker start
rc-update add docker default
```
3. You might want to create a base docker image containing an Artix system:

```bash
# Install artools-base to be able to call basestrap:
pacman -S artools-base
# Create a folder for the root filesystem of the new base image:
mkdir ./rootfs

# Install basic stuff on system:
basestrap -i ./rootfs artix-sysvcompat bash bzip2 coreutils diffutils e2fsprogs file filesystem findutils gawk gcc-libs gettext glibc grep gzip inetutils iputils less licenses logrotate pacman perl procps-ng psmisc sed shadow sysfsutils tar texinfo util-linux which

# Create docker image from rootfs:
tar –numeric-owner –xattrs –acls -C ./rootfs -c . | docker import – artix-base

# Test the newly created image:
docker run –rm=true -t artix-base echo Success.

# Remove rootfs contents:
rm -Rf ./rootfs/*

# Then simply save the image to a file:
docker image save -o artix-base.tar artix-base

# Load the image on another computer:
docker image load -i artix-base.tar
```

---
---
---

### 76. /sys/firmware/efi/vars/OsIndicationsSupported bug ?

I found `/var/lib/dpkg/lock` locked, so I looked for instances of apt through
`ps faux` and found this:
```bash
root 22549 0.0 0.0 4288 688 ? Ss 06:57 0:00 /bin/sh /usr/lib/apt/apt.systemd.daily install
root 22553 0.0 0.0 4288 1512 ? S 06:57 0:00 \_ /bin/sh /usr/lib/apt/apt.systemd.daily lock_is_held install
root 22584 0.0 0.5 156364 93128 ? S 06:57 0:15 \_ /usr/bin/python3 /usr/bin/unattended-upgrade
root 22592 0.0 0.5 158764 86188 ? S 06:57 0:04 \_ /usr/bin/python3 /usr/bin/unattended-upgrade
root 22735 0.0 0.0 23700 9044 pts/10 Ss+ 06:58 0:00 \_ /usr/bin/dpkg –status-fd 10 –configure –pending
root 22736 0.0 0.0 4288 724 pts/10 S+ 06:58 0:00 \_ /bin/sh -e /var/lib/dpkg/info/linux-image-4.9.0-6-amd64.postinst configure 4.9.82-1+deb9u3
root 22740 0.0 0.0 4184 1552 pts/10 S+ 06:58 0:00 \_ run-parts –report –exit-on-error –arg=4.9.0-6-amd64 –arg=/boot/vmlinuz-4.9.0-6-amd64 /etc/kernel/postinst.d
root 26294 0.0 0.0 4288 1616 pts/10 S+ 06:58 0:00 \_ /bin/sh /usr/sbin/grub-mkconfig -o /boot/grub/grub.cfg
root 28492 0.0 0.0 4288 724 pts/10 S+ 06:58 0:00 \_ /bin/sh /etc/grub.d/30_uefi-firmware
root 28494 0.0 0.0 4288 140 pts/10 S+ 06:58 0:00 \_ /bin/sh /etc/grub.d/30_uefi-firmware
root 28495 0.0 0.0 4288 144 pts/10 S+ 06:58 0:00 \_ /bin/sh /etc/grub.d/30_uefi-firmware
root 28496 0.0 0.0 5980 720 pts/10 S+ 06:58 0:00 \_ cat /sys/firmware/efi/vars/OsIndicationsSupported-8be4df61-93ca-11d2-aa0d-00e098032b8c/data
root 28497 0.0 0.0 5848 692 pts/10 S+ 06:58 0:00 \_ cut -b1
```
It seems that:
```bash
cat /sys/firmware/efi/vars/OsIndicationsSupported-8be4df61-93ca-11d2-aa0d-00e098032b8c/data
```
was blocked, so I simply killed this `cat` process. I don’t know what went wrong.

---
---
---

### 77. virsh KVM CLI management
```bash
# List all machines(with id, name and state - running or shut off):
virsh list --all

# Stop vm by name - will send a gracefully shutdown signal
virsh shutdown vm1

# Force stop VM by name - Immediately terminate the VM. No chance to react. Cuts power.
virsh destroy vmx4

# Start vm by name
virsh start vt6

# Edit VM config
virsh edit vm1

# List snapshots for domain (domain is VM name, it seems)
virsh snapshot-list --domain test_domain

# Suspend domain(vm) - when listed will appear as "paused"
virsh suspend vmx4

# Resume domain(vm)
virsh resume vmx4

# Create snapshot for domain
virsh snapshot-create-as --domain test_domain --name "test_domain_backup20190812" --description "Backing up everything before the OS update."

# Show snapshot info
virsh  snapshot-info --domain test_domain --snapshotname test_domain_backup20190812

# DHCP allocated static IP for MAC
root@ci1:/kvm# virsh net-edit default
<network>
  <name>default</name>
  <uuid>35068935-4133-4b8e-b675-78a94e81d66c</uuid>
  <forward mode='nat'/>
  <bridge name='virbr0' stp='on' delay='0'/>
  <mac address='52:54:00:09:b9:70'/>
  <ip address='192.168.122.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.122.2' end='192.168.122.199'/>
      <host mac='52:54:00:d9:55:39' name='windows_2019' ip='192.168.122.200'/>
      <host mac='52:54:00:29:57:8a' name='windows_2019_new' ip='192.168.122.201'/>
    </dhcp>
  </ip>
</network>
# This will show us the virbr0 interface defined on the hypervisor machine
# that acts as a gateway for the virtualized machines and connects their
# LAN to the outside network.
# It also shows the DHCP range used to allocate dynamic IPs to VMs.
# You can also add hosts to define DHCP static IPs for MACs/HostName

# The DHCP leases can be seen using:
root@ci1:/kvm# virsh net-dhcp-leases default
 Expiry Time           MAC address         Protocol   IP address           Hostname          Client ID or DUID
-------------------------------------------------------------------------------------------------------------------
 2020-04-07 14:34:15   52:54:00:29:57:8a   ipv4       192.168.122.201/24   windows_2019_new  01:52:54:00:29:57:8a
 2020-04-07 14:11:50   52:54:00:d9:55:39   ipv4       192.168.122.200/24   windows_2019      01:52:54:00:d9:55:39

# You can also create port forwardings on the hypervisor, to VMs, using:
iptables -t nat -A PREROUTING -d 192.168.0.11 -p tcp --dport 22375 -j DNAT --to 192.168.122.200:2375 && iptables -I FORWARD -d 192.168.122.200/32 -p tcp -m state --state NEW -m tcp --dport 2375 -j ACCEPT

# To apply a newly defined static IP you'll have to recreate the network and
# power off & on (not just restart) the VM
root@ci1:/kvm# virsh net-destroy default
root@ci1:/kvm# virsh net-start default
```
---
---
---

### 78. etcd v3
```bash
# Show endpoint status
etcd>docker-compose exec etcd3 /bin/sh -c "ETCDCTL_API=3 etcdctl --insecure-skip-tls-verify=true --key=/etcd3.etcd/fixtures/client/key.pem --cert=/etcd3.etcd/fixtures/client/cert.pem -w table --endpoints=etcd1:2379,etcd2:2379,etcd3:2379 endpoint status"
+------------+------------------+---------+---------+-----------+-----------+------------+
|  ENDPOINT  |        ID        | VERSION | DB SIZE | IS LEADER | RAFT TERM | RAFT INDEX |
+------------+------------------+---------+---------+-----------+-----------+------------+
| etcd1:2379 | 1f6fd35e3327767a |  3.3.22 |   20 kB |     false |         2 |          9 |
| etcd2:2379 | 4acd0a1e9189cd7a |  3.3.22 |   20 kB |     false |         2 |          9 |
| etcd3:2379 | 2a6277f8728ef760 |  3.3.22 |   20 kB |      true |         2 |          9 |
+------------+------------------+---------+---------+-----------+-----------+------------+

# List members
etcd>docker-compose exec etcd3 /bin/sh -c "ETCDCTL_API=3 etcdctl --insecure-skip-tls-verify=true --key=/etcd3.etcd/fixtures/client/key.pem --cert=/etcd3.etcd/fixtures/client/cert.pem -w table member list"
+------------------+---------+-------+--------------------+--------------------+
|        ID        | STATUS  | NAME  |     PEER ADDRS     |    CLIENT ADDRS    |
+------------------+---------+-------+--------------------+--------------------+
| 1f6fd35e3327767a | started | etcd1 | https://etcd1:2380 | https://etcd1:2379 |
| 2a6277f8728ef760 | started | etcd3 | https://etcd3:2380 | https://etcd3:2379 |
| 4acd0a1e9189cd7a | started | etcd2 | https://etcd2:2380 | https://etcd2:2379 |
+------------------+---------+-------+--------------------+--------------------+

# Save value
etcd>docker-compose exec etcd3 /bin/sh -c "ETCDCTL_API=3 etcdctl --insecure-skip-tls-verify=true --key=/etcd3.etcd/fixtures/client/key.pem --cert=/etcd3.etcd/fixtures/client/cert.pem put joe is_cool"
OK

# Get value
etcd>docker-compose exec etcd3 /bin/sh -c "ETCDCTL_API=3 etcdctl --insecure-skip-tls-verify=true --key=/etcd3.etcd/fixtures/client/key.pem --cert=/etcd3.etcd/fixtures/client/cert.pem get joe"
joe
is_cool

# Get all key-value pairs
etcd>docker-compose exec etcd3 /bin/sh -c "ETCDCTL_API=3 etcdctl --insecure-skip-tls-verify=true --key=/etcd3.etcd/fixtures/client/key.pem --cert=/etcd3.etcd/fixtures/client/cert.pem get \"\" --prefix=true" | sed 's/\x0d//g' | sed '$!N;s/\n/=/'
a=2
b=4
joe=is_cool

```
