### Challenge Name:
Undo

### Goal:
Can you reverse a series of Linux text transformations to recover the original flag?

### Walkthrough:
- Step 1
  - Hint: Base64 encoded
  - Action: Decode it
  - Command: base64 -d
  - <img width="600" height="190" alt="image" src="https://github.com/user-attachments/assets/638b94d1-89be-425b-80bf-b00bbef17731" />

- Step 2
  - Hint: Reversed text
  - Action: Reverse it back
  - Command: rev
  - <img width="600" height="188" alt="image" src="https://github.com/user-attachments/assets/9e580962-86f7-4e63-89b6-b09a8db6d1af" />

- Step 3
  - Hint: (underscores replaced with dashes)
  - Action: Reverse it, - back to _
  - tr '-' '_'
  - <img width="600" height="374" alt="image" src="https://github.com/user-attachments/assets/786d9670-07a1-4e09-822c-7fd444d1c0e2" />

- Step 4
  - Hint: (curly braces replaced with parentheses)
  - Action: Reverse it, () back to {}
  - tr '()' '{}'
  - <img width="600" height="184" alt="image" src="https://github.com/user-attachments/assets/4cf59f2e-8de4-49c8-85da-e5770d4f528b" />

- Step 5
  - Hint: ROT13 applied
  - Action: Apply ROT13 again (it reverses itself)
  - tr 'A-Za-z' 'N-ZA-Mn-za-m'
  - <img width="600" height="295" alt="image" src="https://github.com/user-attachments/assets/df99300c-7631-4bf0-8eff-db000f124ca0" />

### Takeaways
- tr replaces or translates characters in text (example: swapping symbols like - and _)
- base64 -d decodes Base64-encoded data back into readable text
- rev reverses the entire string (last character becomes first)
