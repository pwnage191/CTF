### Challenge
MY GIT

### Goal
I have built my own Git server with my own rules!

### Walkthrough
- I cloned the repository from the given link:
    - git clone `ssh://git@foggy-cliff.picoctf.net:65358/git/challenge.git`
    - Entered the password when prompted
- I navigated to the `challenge` directory and checked the files: `cd challenge` , `ls`
- <img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/380eff7b-294f-4d56-a0b7-f50b845d80d5" />
- I read the `README.md` file: `cat README.md`
- <img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/b001cc3e-aac2-4cc5-93b7-3278e2a8d4a9" />
- It instructed to create and push `flag.txt`
- It also specified the required identity: `root:root@picoctf`
- First, I configured the Git username and email:
    `git config user.name "root"` 
    `git config user.email "root@picoctf"`
- I checked the branch before pushing: `git branch`
- The branch was `master`
- Then, I created the file: `touch flag.txt`
    `git add flag.txt`  
    `git commit -m "push flag.txt as root"`
- Then, I pushed the file: `git push origin master`
- The server validated the commit and returned the flag
- <img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/3bf684da-9a82-47ab-ab6f-d4560a4bf62a" />

### Key Learnings
Commands used to push a file to GitHub:
    - `git add filename`
    - `git commit -m "message"`
    - `git push`
- Commands used to configure name & email:
    - `git config user.name root`
    - `git config user.email root@picoctf`
- Git uses `user.name` and `user.email` as commit metadata, which can be modified locally
- The server validated the commit author information, not the actual system user
