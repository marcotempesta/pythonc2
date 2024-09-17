This code implements an SSH Command and Control (C&C) server using Python, which is designed to manage SSH sessions. 
The server allows users to authenticate using a username and password, execute commands stored in a JSON file, and handle user-specific roles, plans, and permissions. 
Here's a detailed explanation of the code's features:


1. User and Command Database
user_db, command_db, plans_db, and ranks_db are file paths that store users, commands, plans (e.g., basic, VIP), and ranks (e.g., admin, user). They are JSON files from which data is loaded or saved.
load_users, load_plans, load_ranks, and load_commands: Functions to load the respective databases into memory.
save_users and save_commands: Save the modified users or commands back to the JSON file.
2. SSH Server and Authentication
check_auth_password: Verifies the username and password from the users.json file. If successful, it stores the user details (username, plan, and VIP status) in the server instance.
get_allowed_auths: Informs the SSH client that password-based authentication is allowed.
check_channel_request: Ensures that the requested channel type is a session channel, as this is required for terminal-like interactions.
check_channel_shell_request: Indicates that the server supports shell access.
3. Command Execution with User-Specific Branding
replace_placeholders: Replaces specific placeholders in command templates with the user’s details like username, plan, rank, etc. This allows personalized responses.
align_text_no_indent: Removes unnecessary indentation from multi-line text (used for branding content).
execute_command: This function executes commands based on the user's permissions (plan and rank). It first checks the command against the user’s plan and rank and fetches the associated command's branding (if any). It sends the appropriate response to the user via the SSH channel.
4. SSH Client Handling
handle_ssh_client: This function manages an individual SSH client connection. It handles authentication and continuously listens for commands until the client disconnects or sends an exit command. It uses the execute_command function to process commands.
5. Starting the SSH Server
start_ssh_server: Sets up a server socket to listen on a specific port (e.g., 99 in this case), accepts incoming client connections, and spawns a new thread for each connection to handle it concurrently.
main: The entry point of the script. It validates the command-line arguments to ensure a valid port number is provided and starts the SSH server on that port.

6. How It Works:
Server Setup: When you run the script, the SSH server listens on the provided port for incoming SSH connections.
User Authentication: When a client connects, it authenticates using a username and password stored in users.json.
Command Execution: After authentication, the client can send commands in JSON format, which are processed and checked against the user’s rank and plan. 
The command's output can be customized with branding before being sent back to the client.
Permissions: Users' access to commands is restricted based on their plan and VIP status. If a command requires a higher level of access, the server denies the execution and informs the client.
Multithreading: Multiple clients can connect to the server simultaneously since each client session runs in a separate thread.
Key Features:
Authentication and User Management: User credentials, plans, and ranks are stored in JSON and checked during login.
Command Execution with Branding: Commands can be personalized and are restricted based on user privileges.
SSH Protocol:  Establish an SSH server, which allows for secure communication.
This setup could be used for controlling services or systems via SSH, allowing privileged or restricted access to certain commands depending on user roles or plans.

STILL IN DEVELOPMENT!!!
