# Build a To-do command-line app in Python

Project to build functional to-do application for the command line using Python and [Typer](https://typer.tiangolo.com/), 
which is a relatively young library for creating powerful command-line interface (CLI) applications in almost no time. 

## Functionalities provided
1. A functional to-do application with a Typer CLI in Python
2. Use of Typer to add commands, arguments, and options to our to-do app
3. Testing the Python to-do application with Typer’s CliRunner and pytest
4. Processing JSON files by using Python’s json module, and 
5. Managing configuration files with Python’s configparser module.

## Project Overview
To start off, this CLI provides the following global options:

- `-v` or `--version` shows the current version and exits the application.
- `--help` shows the global help message for the entire application.

You’ll see these same options in many other CLI applications out there.
This application provides commands to initialize the app, add and remove to-dos, 
and manage the to-do completion status:

| Command               | Description                                           |
| --------------------- |:-----------------------------------------------------:|
| `init`                | Initializes the application’s to-do database          |
| `add DESCRIPTION`     | Adds a new to-do to the database with a description   |
| `list`                | Lists all the to-dos in the database                  |
| `complete TODO_ID`    | Completes a to-do by setting it as done using its ID  |
| `remove TODO_ID`      | Removes a to-do from the database using its ID        |
| `clear`               | Removes all the to-dos by clearing the database       |


Tasks [Model-View-Controller design] done to provide all these features in the to-do application:
1. Building a command-line interface capable of taking and processing commands, options, and arguments
2. Selecting an appropriate data type to represent our to-dos
3. Implementing a way to persistently store our to-do list
4. Defining a way to connect that user interface with the to-do data

## Tools and libraries used
The following stack has been used to build the application:
- Typer to build the to-do application’s CLI
- Named tuples and dictionaries to handle the to-do data
- Python’s json module to manage persistent data storage
- Configparser module from the Python standard library to handle the application’s initial settings in a configuration file.
- Pytest as a tool for testing our CLI application.


### Step 1: Set Up the To-Do Project
Setting up Python virtual environment [this project is built using Python3]
```shell
$ cd todo_project/
$ python3 -m venv ./venv
$ source venv/bin/activate
```
Now that you have a working virtual environment, you need to install project's requirements;
Typer to create the CLI application and pytest to test your application’s code. 
To install project's requirements, run the following command:

`(venv) $ python -m pip install -r requirements.txt`

### Step 2: Set Up the To-Do CLI App Database
On your terminal, run the following:
```shell
(venv) $ python -m todo init
to-do database location? [/home/user/.user_todo.json]:
The to-do database is /home/user/.user_todo.json
```
This command presents you with a prompt for entering a database location. 
You can press Enter to accept the default path in square brackets, or you can type in a custom path and then press Enter. 
The application creates the to-do database and tells you where it’ll reside from this point on.

Alternatively, you can provide a custom database path directly by using init with the -db or --db-path 
options followed by the desired path. In all cases, your custom path should include the database file name.

`(venv) $ python -m todo init --db-path .user_todo.json`

### Step 4: Add your To-Do items
To add items, run the application with the `list` command. To set the priority, you use the `-p` or `--priorityoption`.
Once you press Enter, the application adds the to-do and informs you about the successful addition. 
If you provide a to-do description without supplying a priority, the app uses the default priority value, which is 2.
```shell
(venv) $ python -m todo add Get some milk -p 1
to-do: "Get some milk." was added with priority: 1

(venv) $ python -m todo add Clean the house --priority 3
to-do: "Clean the house." was added with priority: 3

(venv) $ python -m todo add Wash the car
to-do: "Wash the car." was added with priority: 2

(venv) $ python -m todo add Go for a walk -p 5
Usage: todo add [OPTIONS] DESCRIPTION...
Try 'todo add --help' for help.

Error: Invalid value for '--priority' / '-p': 5 is not in the valid range...
```

### Step 5: List your To-Do items
If you run the application with the `list` command, then you get the following output:
```shell
(venv) $ python -m todo list

My to-do list:

ID.  | Priority  | Done  | Description
----------------------------------------
1    | (1)       | False | Get some milk.
2    | (3)       | False | Clean the house.
3    | (2)       | False | Wash the car.
----------------------------------------
```
This output shows all the current to-dos in a nicely formatted table.

### Step 6: Complete a To-Do item
You can complete a to-do item by running the application with the `complete` command
```shell
(venv) $ python -m todo list

My to-do list:

ID.  | Priority  | Done  | Description
----------------------------------------
1    | (1)       | False | Get some milk.
2    | (3)       | False | Clean the house.
3    | (2)       | False | Wash the car.
----------------------------------------

(venv) $ python -m todo complete 1
to-do # 1 "Get some milk." completed!

(venv) $ python -m todo list

My to-do list:

ID.  | Priority  | Done  | Description
----------------------------------------
1    | (1)       | True  | Get some milk.
2    | (3)       | False | Clean the house.
3    | (2)       | False | Wash the car.
----------------------------------------
```

### Step 7: Removing To-Dos
The first command is `remove`. It allows users to remove a to-do by its ID. 
The second command is `clear` and enables users to remove all the current to-dos from the database.
```shell
(venv) $ python -m todo list
My to-do list:
ID.  | Priority  | Done  | Description
----------------------------------------
1    | (1)       | True  | Get some milk.
2    | (3)       | False | Clean the house.
3    | (2)       | False | Wash the car.
----------------------------------------

(venv) $ python -m todo remove 1
Delete to-do # 1: Get some milk.? [y/N]:
Operation canceled

(venv) $ python -m todo remove 1
Delete to-do # 1: Get some milk.? [y/N]: y
to-do # 1: 'Get some milk.' was removed

(venv) $ python -m todo list

My to-do list:

ID.  | Priority  | Done  | Description
----------------------------------------
1    | (3)       | False | Clean the house.
2    | (2)       | False | Wash the car.
----------------------------------------
```
Try to remove the to-do with the ID number 1. This presents you with a yes (y) or no (N) confirmation prompt. 
If you press Enter, then the application runs the default option, N, and cancels the remove action.

To give this `clear` command a try, go ahead and run the following on your terminal:
```shell
(venv) $ python -m todo clear
Delete all to-dos? [y/N]:
Operation canceled

(venv) $ python -m todo clear
Delete all to-dos? [y/N]: y
All to-dos were removed

(venv) $ python -m todo list
There are no tasks in the to-do list yet
```
In the first example, you run clear. 
Once you press Enter, you get a prompt asking for yes (y) or no (N) confirmation. 
The uppercase N means that no is the default answer, so if you press Enter, you effectively cancel the clear operation.