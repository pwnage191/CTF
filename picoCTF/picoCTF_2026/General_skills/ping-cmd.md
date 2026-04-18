### Challenge
ping-cmd

### Goal
Can you make the server reveal its secrets? It seems to be able to ping Google DNS, but what happens if you get a little creative 
with your input? You can connect to the service here `nc mysterious-sea.picoctf.net 55089`

### Walkthrough
- I connected to the given service using `nc`.
- The service prompted me to enter an IP address and displayed which IP was allowed.
- <img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/d38407b2-0532-4326-ad63-34dc6a8a8ece" />
- After sending two packets, the connection was closed.
- This behavior suggested that the input might be passed directly into a system command.
- I tested this by appending `ls` to the IP input to check for command execution.
- <img width="600" height="439" alt="image" src="https://github.com/user-attachments/assets/58a73f23-8b65-4d29-8736-f69e91558041" />
- The command successfully executed and revealed two files:
    - `flag.txt`
    - `script.sh`
- This confirmed that the service was vulnerable to **command injection**.
- Next, I appended `cat flag.txt` to the input.
- This allowed me to read the contents of the file and retrieve the flag.
- <img width="600" height="388" alt="image" src="https://github.com/user-attachments/assets/46b211a4-691b-4efe-ad6b-e428aac8baa6" />

### Key learnings
- Command Injection occurs when user input is not properly sanitized and is executed as part of a system command.
- && is used to execute multiple commands sequentially, the second command runs only if the first command succeeds.
