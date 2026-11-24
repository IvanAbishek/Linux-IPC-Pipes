# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```

#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    int fd[2];
    pid_t pid;
    char message[] = "Hello from Parent Process!";
    char buffer[100];

    // Create an unnamed pipe
    if (pipe(fd) == -1) {
        perror("Pipe failed");
        return 1;
    }

    // Create child process
    pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        return 1;
    }

    if (pid > 0) {
        // Parent process: write to pipe
        close(fd[0]);
        write(fd[1], message, strlen(message) + 1);
        close(fd[1]);

        wait(NULL);
    } else {
        // Child process: read from pipe
        close(fd[1]);
        read(fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd[0]);
    }

    return 0;
}
```




## OUTPUT

<img width="1716" height="881" alt="image" src="https://github.com/user-attachments/assets/1999e5af-172e-46e0-af68-6e57c7986bb1" />

## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>

int main() {
    int fd;
    char message[] = "Hello from Process 1!";
    char buffer[100];

    // Create a named pipe
    mkfifo("myfifo", 0666);

    if (fork() == 0) {
        // Child process: read from named pipe
        fd = open("myfifo", O_RDONLY);
        read(fd, buffer, sizeof(buffer));
        printf("Process 2 received: %s\n", buffer);
        close(fd);
    } else {
        // Parent process: write to named pipe
        fd = open("myfifo", O_WRONLY);
        write(fd, message, strlen(message) + 1);
        close(fd);
        wait(NULL);
    }

    unlink("myfifo");
    return 0;
}
```



## OUTPUT
<img width="590" height="56" alt="image" src="https://github.com/user-attachments/assets/f3f707ab-97ab-468c-98ea-ec1dd333ef03" />
<img width="602" height="277" alt="image" src="https://github.com/user-attachments/assets/ee2a5217-9af8-4710-8c4c-8f6ecf2da24c" />


# RESULT:
The program is executed successfully.
