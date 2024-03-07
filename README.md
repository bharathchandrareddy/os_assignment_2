# os_assignment_2
WSU os assignment 2

Process Class: A process is represented by this class. A specific quantity of lottery tickets and a unique identity are assigned to each process.
schedule class: Processes, their tickets, and the scheduling method used to choose the subsequent process are all managed by the Scheduler Class.

Process class:
class Process:
    def __init__(self, pid, tickets):
        self.pid = pid  # Unique identifier for the process
        self.tickets = tickets  # Number of lottery tickets assigned to this process

    def __str__(self):
        return f"Process {self.pid}"     #this is used to print the output

__init__ Method: Initializes the process with a unique ID, an initial number of tickets (which can be adjusted by the scheduler based on priority), a priority level (to influence the number of tickets the process receives), and a completion status.
execute Method: Simulates the execution of the process by marking it as completed and printing a confirmation message. In a real scenario, this could involve running the process's assigned task.


scheduler class
import random

class Scheduler:
    def __init__(self):
        self.processes = []   #creating an empty list
# __init__ Method: Initializes the scheduler with an empty list to hold Process instances.

    def add_process(self, process):
        self.processes.append(process)    #we are appending a new process to the list created in the above step
        self.allocate_tickets()  # Adjust ticket allocation every time a new process is added
# add_process Method: Adds a new process to the scheduler and reallocates lottery tickets to ensure the distribution reflects the current set of processes and their priorities.

    def allocate_tickets(self):
        # Allocate tickets based on priority (simple example)
        total_priority = sum(process.priority for process in self.processes)
        for process in self.processes:
            process.tickets = (process.priority / total_priority) * 100  # Allocate tickets as a function of priority
# allocate_tickets Method: Calculates the total priority of all processes and reallocates tickets based on each process's share of the total priority. This demonstrates how tickets can be distributed dynamically, rather than being fixed at the creation of each process.

    def select_next_process(self):
        if not self.processes:
            return None
        total_tickets = sum(process.tickets for process in self.processes if not process.completed)
        winning_ticket = random.randint(1, int(total_tickets))
        
        current_ticket = 0
        for process in self.processes:
            if not process.completed:
                current_ticket += process.tickets
                if current_ticket >= winning_ticket:
                    return process
        return None
# select_next_process Method: Implements the lottery by selecting a process to execute next. It calculates the sum of tickets for all non-completed processes, selects a random "winning" ticket, and 
    
    def show_state(self):     #this function is used to represent the current state of the lottery system
        for process in self.processes:
            status = "Completed" if process.completed else "Pending"
            print(f"Process {process.pid}: Tickets = {process.tickets:.2f}, Status = {status}")

# show_state Method: Displays the current state of the scheduler, including each process's ID, ticket count, and completion status. This is useful for tracking the progression of the simulation and understanding how tickets and process status evolve over time.



