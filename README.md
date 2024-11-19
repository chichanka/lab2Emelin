# lab2Emelin
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define NUM_PROCESSES 5
#define TIME_QUANTUM 2

typedef struct {
int id;
int arrival_time;
int burst_time;
int priority;
int remaining_time;
} Process;

void generate_processes(Process processes[], int num_processes) {
srand(time(NULL));

for (int i = 0; i < num_processes; i++) {
processes[i].id = i + 1;
processes[i].arrival_time = rand() % 5;
processes[i].burst_time = rand() % 6 + 1;
processes[i].priority = rand() % 10 + 1;
processes[i].remaining_time = processes[i].burst_time;
}
}

void print_processes(Process processes[], int num_processes) {
printf("ID\tArrival Time\tBurst Time\tPriority\tRemaining Time\n");
for (int i = 0; i < num_processes; i++) {
printf("%d\t%d\t\t%d\t\t%d\t\t%d\n",
processes[i].id, processes[i].arrival_time,
processes[i].burst_time, processes[i].priority,
processes[i].remaining_time);
}
}

void round_robin(Process processes[], int num_processes) {
int time = 0;
int completed = 0;
while (completed < num_processes) {
for (int i = 0; i < num_processes; i++) {
if (processes[i].arrival_time <= time && processes[i].remaining_time > 0) {
if (processes[i].remaining_time > TIME_QUANTUM) {
processes[i].remaining_time -= TIME_QUANTUM;
time += TIME_QUANTUM;
printf("Process %d executed for %d units, remaining time: %d\n", processes[i].id, TIME_QUANTUM, processes[i].remaining_time);
} else {
time += processes[i].remaining_time;
printf("Process %d executed for %d units, remaining time: 0\n", processes[i].id, processes[i].remaining_time);
processes[i].remaining_time = 0;
completed++;
}
}
}
}
}

void fcfs(Process processes[], int num_processes) {
int time = 0;
int waiting_time = 0;
printf("\nFCFS Scheduling:\n");
for (int i = 0; i < num_processes; i++) {
if (processes[i].arrival_time > time) {
time = processes[i].arrival_time;
}
int start_time = time;
int end_time = start_time + processes[i].burst_time;
waiting_time += start_time - processes[i].arrival_time;
time = end_time;
printf("Process %d started at %d, ended at %d, waiting time: %d\n", processes[i].id, start_time, end_time, start_time - processes[i].arrival_time);
}
printf("Average waiting time: %.2f\n", (float)waiting_time / num_processes);
}

void priority_scheduling(Process processes[], int num_processes) {
int time = 0;
int completed = 0;
int waiting_time = 0;
Process temp;

for (int i = 0; i < num_processes - 1; i++) {
for (int j = i + 1; j < num_processes; j++) {
if (processes[i].priority < processes[j].priority) {
temp = processes[i];
processes[i] = processes[j];
processes[j] = temp;
}
}
}

printf("\nPriority Scheduling:\n");
for (int i = 0; i < num_processes; i++) {
if (processes[i].arrival_time > time) {
time = processes[i].arrival_time;
}
int start_time = time;
int end_time = start_time + processes[i].burst_time;
waiting_time += start_time - processes[i].arrival_time;
time = end_time;
printf("Process %d started at %d, ended at %d, waiting time: %d, priority: %d\n",
processes[i].id, start_time, end_time, start_time - processes[i].arrival_time, processes[i].priority);
}
printf("Average waiting time: %.2f\n", (float)waiting_time / num_processes);
}

int main() {
Process processes[NUM_PROCESSES];
generate_processes(processes, NUM_PROCESSES);

printf("Generated processes:\n");
print_processes(processes, NUM_PROCESSES);

printf("\nRound Robin Scheduling:\n");
round_robin(processes, NUM_PROCESSES);

generate_processes(processes, NUM_PROCESSES);
fcfs(processes, NUM_PROCESSES);

generate_processes(processes, NUM_PROCESSES);
priority_scheduling(processes, NUM_PROCESSES);

return 0;
}
