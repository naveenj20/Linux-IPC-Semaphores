# Linux-IPC-Semaphores
Ex05-Linux IPC-Semaphores

# AIM:
To Write a C program that implements a producer-consumer system with two processes using Semaphores.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Sempahores

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## Write a C program that implements a producer-consumer system with two processes using Semaphores.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <sys/wait.h>

union semun {
    int val;
};

int main() {
    int semid, pid;
    union semun s;
    struct sembuf wait_op = {0, -1, 0};
    struct sembuf signal_op = {0, 1, 0};

    // Create semaphore
    semid = semget(IPC_PRIVATE, 1, 0666 | IPC_CREAT);

    // Initialize semaphore to 0
    s.val = 0;
    semctl(semid, 0, SETVAL, s);

    pid = fork();

    if (pid == 0) {
        // Consumer
        for (int i = 0; i < 5; i++) {
            semop(semid, &wait_op, 1);
            printf("Consumer consumed %d\n", i);
        }
    } else {
        // Producer
        for (int i = 0; i < 5; i++) {
            printf("Producer produced %d\n", i);
            semop(semid, &signal_op, 1);
            sleep(1);
        }

        wait(NULL);

        // Remove semaphore
        semctl(semid, 0, IPC_RMID, s);
    }

    return 0;
}
```


## OUTPUT
$ ./sem.o 

<img width="408" height="480" alt="image" src="https://github.com/user-attachments/assets/07278da1-a90a-4630-a282-705025095ede" />


$ ipcs

<img width="711" height="481" alt="image" src="https://github.com/user-attachments/assets/74ffe8b3-3049-4047-ae9b-61c224a48145" />




# RESULT:
The program is executed successfully.
