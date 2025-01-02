# OS_xv6-for-riscv
*compressed in zip folder 

# Priority Scheduling in xv6

This project implements a priority-based scheduler for the **xv6** operating system, replacing the default round-robin scheduler with one that schedules processes based on their priority. The system allows processes to be assigned priorities, and the scheduler ensures that the process with the highest priority is executed first.

Additionally, system calls are implemented to manage process priorities and retrieve process information, similar to the `ps` command in Unix-like systems. The project modifies several core files in the **xv6** kernel and implements new system calls.

---

## Features

- **Priority-Based Scheduler**: Replaces the default round-robin scheduling with a priority-based scheduler.
  - The scheduler selects the process with the highest priority and, if there are multiple processes with the same priority, uses round-robin scheduling.
- **New System Calls**:
  1. **`setpriority(int num)`**: Sets the priority of the calling process to the specified value, where valid values are between 1 (highest priority) and 20 (lowest priority). The default priority is 10.
  2. **`getpinfo(struct pstat *)`**: Retrieves information about all active processes, including `pid`, `ppid`, priority, state, and more. This information is returned in the provided `struct pstat`.
- **`ps` Command**: A user-level program that uses the `getpinfo` system call to display information about all active processes in a manner similar to the `ps` command in Linux.

---

## Modified Files

1. **`proc.h`**: Extended the `struct proc` to store process priority.
2. **`proc.c`**: 
   - Modified the scheduler to implement priority-based scheduling.
   - Implemented the logic for round-robin scheduling among processes with the same priority.
3. **`syscall.c`**: Added helper functions for handling the new system calls.
4. **`syscall.h`**: Added mappings for the new system calls.
5. **`sysproc.c`**: Implemented the logic for the `setpriority` and `getpinfo` system calls.
6. **`user.h`**: Declared the new system calls.
7. **`usys.pl`**: Generated the appropriate system call numbers for the new system calls.
8. **`ps.c`**: Implemented the `ps` command to print the process information.

---

## System Call Details

### `setpriority(int num)`

This system call allows a process to set its own priority. The valid range of priorities is from 1 (highest) to 20 (lowest). The default priority is 10. If the priority value provided is out of range, the system call returns `-1`; otherwise, it returns `0` to indicate success.

### `getpinfo(struct pstat *)`

This system call copies the status of all active processes into a user-provided `struct pstat`. The structure is populated with process information such as:
- `pid`: Process ID
- `ppid`: Parent Process ID
- `priority`: The current priority of the process
- `state`: The state of the process (e.g., RUNNING, SLEEPING, etc.)
- Other process-related data.

---

## Priority-Based Scheduler

The scheduler has been modified to prioritize processes based on their assigned priority. The scheduler works as follows:
- It searches for the highest-priority process that is **RUNNABLE**.
- If multiple processes have the same priority, it schedules them in a **round-robin** fashion.
- The process priority is stored in the `struct proc` and used during scheduling to determine the next process to execute.

### Scheduling Logic

1. The scheduler iterates over all processes, checking their priority and state.
2. If a process is **RUNNABLE** and has the highest priority, it is selected to execute.
3. If multiple processes share the highest priority, the scheduler selects them in a round-robin manner, maintaining the last selected process to continue the search from there.

---

## User Program: `ps`

The `ps` program is implemented to display information about the active processes in the system. It uses the `getpinfo` system call to retrieve process information and prints it in a user-readable format. The displayed information includes:
- Process ID (`pid`)
- Parent Process ID (`ppid`)
- Process priority
- Process state (e.g., RUNNABLE, SLEEPING, etc.)

---

## Steps to Compile and Run
**using QEMU emulator

1. Clone the repository and navigate to the `xv6-project-2023` directory:
   ```bash
   unzip
   git clone 
   make qemu 
