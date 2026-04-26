### Challenge
Piece by Piece

### Goal 
After logging in, you will find multiple file parts in your home directory. These parts need to be combined and extracted to reveal the flag.
SSH to `dolphin-cove.picoctf.net`:`58954` and login as `ctf-player` with password `1db87a14`.

### Walkthrough
- The challenge provided the domain, username, and password to connect via SSH.
- I connected to the remote machine using the following command:  `ssh ctf-player@dolphin-cove.picoctf.net -p 58954` , Then I entered the password.
- I listed the files in the current directory using `ls`. It showed:  
    `instructions.txt part_aa part_ab part_ac part_ad part_ae`
- I read `instructions.txt`, which contained the following hints:
    - The flag is split into multiple parts as a zipped file.
    - Use Linux commands to combine the parts into one file.
    - The zip file is password-protected. Use the password `"supersecret"` to extract it.
    - After unzipping, check the extracted text file for the flag.
- I checked the file type by attempting to `unzip part_aa`, which resulted in: `End-of-central-directory signature not found...`
- <img width="600"  alt="image" src="https://github.com/user-attachments/assets/e41a366f-26ed-4eda-a874-b4bb596e1079" />
- This indicates that the file is part of a split archive and not a complete zip file.
- Next, I combined all the parts using:  `cat part_a* > combined_file`
- I then verified the file type (using `file combined_file`), which showed it as a valid zip archive.
- I extracted the archive using:  `unzip combined_file`  and entered the password `"supersecret"` when prompted.
- This extracted `flag.txt`. I read the file using `cat flag.txt`, which revealed the flag.
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/f8217a5f-4185-465c-bd47-16011b9dab11" />

### Key Learnings
- Split files must be combined in the correct order before extraction.
- Tools like `cat` can reconstruct multi-part files efficiently.
- Always verify file types using commands like `file` before proceeding.
