### Challenge:
Bytemancy 1

### Goal:
Can you conjure the right bytes? The program's source code can be downloaded here. 
Connect to the program with netcat: `$ nc foggy-cliff.picoctf.net 51657`

### Walkthrough
- The challenge provides the source code and a netcat connection
- First, I read the source code to understand how it works
- It asks the user to enter:  
    "Send me ASCII DECIMAL 101 1751 times, side-by-side, no space."
- In the code: `user_input == "\x65"*1751`
- <img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/4cc3902a-fd23-4d6e-ba60-2178a7caf3d9" />
- `\x65` is a hexadecimal escape sequence, decimal value 101, which corresponds to the ASCII character `'e'`
- So, we need to give 1751 'e' characters continuously without spaces
- I used a Python one-liner to generate the input
- I sent the payload using a pipe to netcat: `python -c "print('e'*1751)" | nc foggy-cliff.picoctf.net 51657`
- sys.stdout.write() writes the string without appending a newline:
   `python -c "import sys; sys.stdout.write('e'*1751)" | nc foggy-cliff.picoctf.net 51657`
- The first command generates the required string
- The pipe (`|`) sends this output as input to the netcat connection
- The program reads the input, checks the condition, and reveals the output
- <img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/e01bdad9-66fb-45da-9848-77f35182f68b" />

### Key Learnings
- A pipe (`|`) connects two commands
- The first command’s output is passed as input to the second command
- Data is transferred as a stream of bytes
- Python is useful for quick scripting and payload generation
- Escape sequences like `\x65` represent hexadecimal byte values
