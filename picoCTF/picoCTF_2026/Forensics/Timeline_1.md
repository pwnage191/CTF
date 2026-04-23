### Challenge:
Timeline 1

### Goal:
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format. Download the disk image here.

### Walkthrough
- I downloaded `partition4.img.gz` and extracted the disk image.
- I used the Foremost tool to recover deleted files.
- It revealed the files `audit.txt`, `exe`, and `htm`.
- I examined `audit.txt`.
- The `audit.txt` displayed a timestamp for the `.exe` file: `03/03/2016`
- I compared the metadata using `exiftool exe` and `stat exe`
- The `stat` output showed identical MACB timestamps, which correspond to the recovery time since the file was recreated by Foremost.
- The `exiftool` output showed an older timestamp, which likely reflects the original file timestamp.
- A mismatch with unrealistic timestamps can indicate timestomping, but in this case, the difference is explained by file recovery.
- To analyze filesystem metadata, I used `fls -r -d partition4.img > body.txt`
- The `.exe` and `.htm` files were not present in the filesystem listing, even though they were recovered using Foremost.
- This indicates that the files were deleted, meaning the directory entries and metadata were removed, but the file content
  remained on disk until overwritten.
- I converted the raw metadata into a human-readable timeline using `mactime -b body.txt > data.txt`
- The clue mentioned visiting recent timestamps and filtering only new files by grepping for `macb`
- I filtered the files created on December 2, 2025 using `mactime -b body.txt 2025-12-02 | grep "macb"`
- I looked for any suspicious file. One file was `/root/.ash_history`.
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/955e8d93-0d3b-4973-817e-02c36d64edc3" />
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/6bb3a1fd-cf57-4062-be36-7458bb7d955b" />
- I assumed it might contain root user commands and accessed it using its inode, but it only contained `poweroff`.
- Next, I found `/etc/chat`, which does not look like a legitimate location for chat-related files, as such files are usually located in `/etc/ppp/`.
- I accessed the file directly using its inode: `icat partition4.img 147`
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/813b13e5-6a73-4ca9-adda-c56b72b30cd4" />
- This revealed a Base64-encoded string: `NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK`
- I decoded it using `echo "NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK" | base64 -d`
- The decoded output revealed the flag.

### Key Learnings
- Analyze timestamp behavior carefully in disk images
- Foremost can recover deleted files even when metadata is removed
- `stat` may show recovery timestamps rather than original timestamps
- `exiftool` can help identify original file timestamps
- Use `fls` and `mactime` to reconstruct filesystem timelines
- Deleted files may not appear in metadata but still exist in disk space
- Investigate unusual file paths for hidden data
