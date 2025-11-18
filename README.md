import csv

#create a Linked List Node Class, with different nodes
class Node:
    def __init__(self,task_name, duration, priority):
        self.task_name = task_name
        self.duration = duration
        self.priority = priority
        self.next = None

#-----------------------------------------------------------------

#implement a Linked List Class
class linkedList:
    def __init__(self):
        self.__head = None
        self.__tail = None
        self.__size = 0

#-----------------------------------------------------------------

#add_task(task_name, duration, priority): Adds a new task to the end of the list.
    def add_task(self, task_name, duration, priority):
        newNode = Node(task_name, duration, priority)
        if self.__tail == None:
            self.__head = self.__tail = newNode
        else:
            self.__tail.next = newNode
            self.__tail = self.__tail.next
        self.__size += 1

#-----------------------------------------------------------------

#remove_task(task_name): Removes a task with the given name from the list.
    def remove_task(self, task_name):
        current =self.__head
        previous = None

        while current and current.task_name != task_name:
            previous = current
            current = current.next
        if not current:
            print(f"Task {task_name} not found.")
            return
        if not previous:
            self.__head = current.next
        else:
            previous.next = current.next

#-----------------------------------------------------------------

#display_tasks(): Prints all tasks in the linked list in the order they appear.
    def display_tasks(self):
        current = self.__head
        while current:
            print(f"Task: {current.task_name}, Duration: {current.duration}, Priority: {current.priority}")
            current = current.next

#-----------------------------------------------------------------

#find_task(task_name): Searches for a task by name and returns its details.
    def find_task(self, task_name):
        current = self.__head
        while current:
            if current.task_name == task_name:
                return f"Task: {current.task_name}, Duration: {current.duration}, Priority: {current.priority}"
            current = current.next
        return f"Task '{task_name}' not found."

#-----------------------------------------------------------------

#calculate_total_duration(): Calculates the total duration of all tasks in the current order.
    def calculate_total_duration(self):
        total_duration = 0
        current = self.__head
        while current:
            total_duration += current.duration
            current = current.next
        return f"The total duration of all tasks is {total_duration}."

#-----------------------------------------------------------------

#read_tasks_from_csv(file_path): Reads tasks from a CSV file and adds them to the linked list.
    def read_tasks_from_csv(self, file_path):
        with open(file_path, 'r') as file:
            reader = csv.reader(file)
            next(reader)
            for row in reader:
                task_name, duration, priority = row
                self.add_task(task_name, duration, priority)

#-----------------------------------------------------------------

#reorder_tasks_by_priority(): Reorders the tasks in the linked list based on priority. (lowest priority number comes first).
    def reorder_tasks_by_priority(self):
        sorted_head = None
        current = self.__head
        while current:
            next_node = current.next
            sorted_head = self.sorted_insert_by_priority(sorted_head, current)
            current = next_node
        self.__head = sorted_head

#-----------------------------------------------------------------

#reorder_tasks_by_priority_duration(): Reorders the tasks in the linked list based on priority, then duration.
    def reorder_tasks_by_priority_duration(self):
        sorted_head = None
        current = self.__head
        while current:
            next_node = current.next
            sorted_head = self.sorted_insert_by_priority_duration(sorted_head, current)
            current = next_node
        self.__head = sorted_head


#-----------------------------------------------------------------

#sorted_insert_by_priority: This method has input a head (i.a linked list) and a node. The method adds the node to linked list by looking at the duration.
    # Nodes with the lowest duration are added in the beginning of the LinkedList.
    def sorted_insert_by_priority(self, head, node):
        if not head or node.priority > head.priority:
            node.next = head
            return node
        current = head
        while current.next and current.next.priority <= node.priority:
            current = current.next
        node.next = current.next
        current.next = node
        return head

#-----------------------------------------------------------------

#sorted_insert_by_priority_duration: This method has input a head (i.a linked list) and a node. The method adds the node to linked list by looking first
    # at the priority and next at duration. Nodes with the lowest priority and then duration are added in the beginning of the LinkedList.
    def sorted_insert_by_priority_duration(self, head, node):
        if not head or (node.priority < head.priority or
                        (node.priority == head.priority and node.duration < head.duration)):
            node.next = head
            return node
        current = head
        while current.next and (current.next.priority < node.priority or (current.next.priority == node.priority and current.next.duration <= node.duration)):
            current = current.next
        node.next = current.next
        current.next = node
        return head

