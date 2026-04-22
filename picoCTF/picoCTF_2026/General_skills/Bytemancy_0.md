### Challenge:
Bytemancy 0

### Goal:
Can you conjure the right bytes? The program's source code can be downloaded here. Connect to the program with netcat:  
`nc candy-mountain.picoctf.net 56003`

### Walkthrough:
- The challenge provides the source code (`app.py`) along with a network connection.
- After connecting using netcat, the program prompts:  
    "Send me ASCII DECIMAL 101, 101, 101, side-by-side, no space."
- ASCII decimal 101 corresponds to the character 'e'.
- Since the input requires three values side-by-side with no spaces, the correct input is:  `eee`
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/2a7aa3f4-923c-4077-86e5-1e1f4f792d5c" />
- It works because \x65 is the hex representation of ASCII 101, which is 'e', so "\x65\x65\x65" equals "eee".
- Upon entering this, the program returns the flag.
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/5add6a87-a551-46b2-9ad0-930c9d14152c" />

### Key learnings:
- ASCII decimal to character conversion
- Understanding input formatting requirements (no spaces, exact pattern)
