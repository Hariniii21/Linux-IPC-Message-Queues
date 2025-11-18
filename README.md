## Linux-IPC-Message-Queues

## AIM:
To write a C program that receives a message from message queue and display them

## DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

## PROGRAM:

## C program that receives a message from message queue and display them
```
// C Program for Message Queue (Writer Process) 
#include <stdio.h> 
#include <sys/ipc.h> 
#include <sys/msg.h> 

// structure for message queue 
struct mesg_buffer 
{ 
	long mesg_type; 
	char mesg_text[100]; 
}message; 
int main() 
{ 	
        key_t key; 
	int msgid; 
        // ftok to generate unique key 
	key = ftok("progfile", 65); 
	// msgget creates a message queue 
	// and returns identifier 
	msgid = msgget(key, 0666 | IPC_CREAT); 
        message.mesg_type = 1; 
	printf("Write Data : "); 
	gets(message.mesg_text); 
	// msgsnd to send message 
	msgsnd(msgid, &message, sizeof(message), 0); 
	// display the message 
	printf("Data sent is : %s \n", message.mesg_text); 
	return 0; 
}


// C Program for Message Queue (Reader Process)
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

// structure for message queue
struct mesg_buffer 
{
	long mesg_type;
	char mesg_text[100];
}message;
int main()
{
	key_t key;
	int msgid;
        // ftok to generate unique key
	key = ftok("progfile", 65);
	// msgget creates a message queue
	// and returns identifier
	msgid = msgget(key, 0666 | IPC_CREAT);
	// msgrcv to receive message
	msgrcv(msgid, &message, sizeof(message), 1, 0);
	// display the message
        printf("Data Received is : %s \n",message.mesg_text);
        // to destroy the message queue
	msgctl(msgid, IPC_RMID, NULL);
	return 0;
}
```

## OUTPUT:
#### Writer process
<img width="501" height="108" alt="Screenshot at 2025-11-17 12-26-43" src="https://github.com/user-attachments/assets/cbbcd65e-0d84-4eb4-8661-31aca9a29d6d" />

#### Reader process
<img width="595" height="80" alt="msgq" src="https://github.com/user-attachments/assets/fb66cc31-79ce-4d5e-aa1e-e9cac0567a41" />

## RESULT:
Thus,the programs are executed successfully.
