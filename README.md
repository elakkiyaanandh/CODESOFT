import json

def load_task():
    try:
        with open("tasks.json", "r") as task_file:
            return json.load(task_file)
    except FileNotFoundError:
        return []


def save_task(tasks):
    with open("tasks.json", "w") as task_file:
        json.dump(tasks, task_file, indent=4)


def add_task(tasks):
    task_name = input("Enter the task: ")

    for task in tasks:
        if task_name in task:
            print(f"'{task_name}' already exists")
            return

    task = {task_name: "Pending"}
    tasks.append(task)
    save_task(tasks)
    print("Your task is added successfully")


def view_task(tasks):
    if len(tasks)==0:
        print("No Task to do")
    else:
        print("\nYour Tasks:")
        for i, t in enumerate(tasks, start=1):
            for name, status in t.items():
                print(f"{i}. {name} - {status}")


def mark_task(tasks):
    if len(tasks)==0:
        print("No Task to mark.")
        return

    view_task(tasks)
    choice=int(input("Enter the task to mark as completed:"))
    if 1<=choice<=len(tasks):
        tasks[choice-1]["status"]="Completed"
        print("Task marked as completed")
    else:
        print("Invalid task number")
    with open("tasks.json","w") as task_file:
        json.dump(tasks,task_file,indent=4)




def delete_task(tasks):
     with open("tasks.json", "r") as task_file:
            tasks = json.load(task_file)

     if len(tasks) == 0:
            print("No task to delete.")
            return

     view_task(tasks)

     choice = int(input("Enter the task number to delete: "))

     if 1 <= choice <= len(tasks):
            del tasks[choice - 1]
            save_tasks(tasks)
            print("Your task is deleted successfully")
     else:
            print("Invalid task number")
     save_task(tasks)



def main():
    tasks = load_task()
    print("Here is your TO-DO list")

    while True:
        print("\n" + "*" * 50)
        print("                      TO-DO LIST                         ")
        print("*" * 50)
        print("1. Add your task")
        print("2. View your task")
        print("3. Mark done")
        print("4. Delete task")
        print("5. Quit")
        print("*" * 50)

        try:
            choice = int(input("Enter your choice: "))
            match choice:
                case 1:
                    add_task(tasks)
                case 2:
                    view_task(tasks)
                case 3:
                    mark_task(tasks)
                case 4:
                    delete_task(tasks)
                case 5:
                    print("Thank you byee...!")
                    break
                case _:
                    print("Invalid choice! Try 1-5.")
        except ValueError:
            print("Please enter a number (1-5)!")


if __name__ == "__main__":
 main()


