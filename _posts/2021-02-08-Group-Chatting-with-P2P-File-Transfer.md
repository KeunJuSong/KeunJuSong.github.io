---
title : "Group Chatting with P2P File Transfer"
permalink : /projects/group-chatting-with-p2p-file-transfer/
excerpt: "This Project is Group Chatting with file transfer using TCP/IP protocol in Ubuntu Linux Server."
tags: [ubuntu-server, tcp-ip, socket-programming, p2p-file-transfer]
toc: true
---

# **Overview**

* Term project in Embedded OS course (Undergraduate)
* Chatting program that have with P2P file transfer function
* If you want to know more information about its source code, visit my **[Github](https://github.com/KeunJuSong/Group-Chatting-with-P2P-File-Transfer)**

# **Idea**

* Design Server-Client architecture with socket programming (Developed with C language)
* Using TCP/IP protocol (Transfer layer in OSI 7 layers) in communication
* Based in Ubuntu server, so need to know Linux command and vim code editor

# **Description**

* This project is required port assignment to build up server and client
  *  In this project, it needs 1 server and 2 clients (Server ↔ Client#1, Client#2)
* To implement this program, it should be followed step by step process
  * **Server-Client Model**
  * **Group Chatting**
  * **P2P File Transfer**

## **Server-Client Model**

* Properties
  * A user accesses the server
  * Each user has to log-in the server with ID/PW
  * Background for implementing group chatting

* Model structure

    <figure>
      <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Server-Client-model-structure.png' | relative_url }}" alt="Server-Client model structure">
      <figcaption>Server-Client Model structure</figcaption>
    </figure>

* Key Idea

  * How much know about ```fork()``` method

  * ```fork()``` 
    * Create child process from parent process (Copy-Paste form) 

* Result

    <figure>
      <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Server-Client-model-result.png' | relative_url }}" alt="Server-Client model result">
      <figcaption>Result of Server-Client Model</figcaption>
    </figure>

## **Group Chatting**

* Properties
  * 1 server, 2 more clients architecture based on **Server-Client Model**
  * In this part, several fork() calls are required

* Model structure

    <figure>
      <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Group-Chatting-model-structure.png' | relative_url }}" alt="Group Chatting Model structure">
      <figcaption>Group Chatting Model structure</figcaption>
    </figure>

* Key Idea

  * There are many other methods to implement Group Chatting, **Client#1 ↔ Server ↔ Client#2**

  * For me, I used ```pipe()``` method. Refer more about [pipe()](https://www.geeksforgeeks.org/pipe-system-call/)

  * ```pipe()``` 

    * Connection between two processes, useful for communication between related processes
    * **Parent and child sharing a pipe**

  * My Group Chatting architecture using ```pipe()``` method

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Group-Chatting-architecture-pipe.png' | relative_url }}" alt="Group Chatting system architecture">
        <figcaption>Group Chatting architecture - pipe()</figcaption>
      </figure>

* Result

  * Server

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Group-Chatting-model-Server-result.png' | relative_url }}" alt="Group Chatting Model-Server result">
        <figcaption>Group Chatting Model-Server result</figcaption>
      </figure>

  * Client 1

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Group-Chatting-model-Client1-result.png' | relative_url }}" alt="Group Chatting Model-Client1 result">
        <figcaption>Group Chatting Model-Client1 result</figcaption>
      </figure>

  * Client 2

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_Group-Chatting-model-Client2-result.png' | relative_url }}" alt="Group Chatting Model-Client2 result">
        <figcaption>Group Chatting Model-Client2 result</figcaption>
      </figure>

## **P2P File Transfer**

* Properties

  * When log in, make clients are sending their P2P IP/Port#
  * If a special msg(e.g., "[FILE]") is received, a P2P file transfer process is starting
  * **P2P file transfer process - when Client#1 require file transfer to Client#2**
    * Client#1 receive file lists from Client#2
    * Client#1 choose file, and send file's number to  Client#2
    * Client#1 receive file successfully
  * P2P Server-Client is newly generated with P2P IP/Port#. (additional fork() calls are required)
  * In P2P network, there are no master-slave relationship
    * So original communication(Server) need to be disconnected and direct communication between client1,2 should be established

* Model Structure

    <figure>
      <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_P2P-File-Transfer-model-structure.png' | relative_url }}" alt="P2P File Transfer Model structure">
      <figcaption>P2P File Transfer Model structure</figcaption>
    </figure>

* Key Idea

  * There are many other methods to implement file transfer

  * For me, I used ```dirent.h```  header file to show file lists of Client

  * ```dirent.h```

    * Header file that can handle directory files

    * Structure

      ```
      struct dirent {
        long d_ino; 	//l-노드 번호
        	off_t	d_off;		//offset
        	unsigned short d_reclen;	//파일 이름 길이
        	char d_name[NAME_MAX+1];		//파일 이름
      }
      ```

    * Usage function (In this project)

      * ```opendir()```
      * ```readdir()```
      * ```closedir()```

  * About file transfer between Client#1 and Client#2, I devised some ~~trick(?)~~ solution

    * When Client#2 send file lists, **send its content additionally** to Client#1
    * So if Client#1 select one file, then it can be reasonable to create that file in Client#1 itself

* Result

  * Server - User1(client1) request [FILE]

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_P2P-File-Transfer-model-Server-result.png' | relative_url }}" alt="P2P File Transfer Model Server result">
        <figcaption>P2P File Transfer Model result - Server</figcaption>
      </figure>

  * Client#1 - Change P2P network and Select 'a-2.txt' file that is third in file list

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_P2P-File-Transfer-model-Client#1-result.png' | relative_url }}" alt="P2P File Transfer Model Client#1 result">
        <figcaption>P2P File Transfer Model result - Client#1</figcaption>
      </figure> 

  * Client#2 - Change P2P network in IP/Port#1 of client#1 and Send file list

      <figure>
        <img src="{{ '/assets/images/Group-Chatting-with-P2P-File-Transfer_P2P-File-Transfer-model-Client#2-result.png' | relative_url }}" alt="P2P File Transfer Model Client#2 result">
        <figcaption>P2P File Transfer Model result - Client#2</figcaption>
      </figure>

# **Development Environment**

* **Putty** - Ubuntu Server (OS : Linux)
* **WinSCP**
* **Vim** - Code Editor (Used C language)

# **Reference**

* Socket Programming
  * http://beej.us/guide/bgnet/
  * http://www.stanford.edu/class/cs244a/socket-links.html
  * http://www.tenouk.com/cnlinuxsockettutorials.html
* Linux/Unix Programming Environment
  * http://www.unix-ag.uni-siegen.de/docs/unix/unix.html
* Vi Editor
  * http://www.cs.rit.edu/~cslab/vi.html
