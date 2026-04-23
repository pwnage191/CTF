### Challenge
Timeline 0

### Goal
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format.

### Walkthrough
- I downloaded `partition4.img.gz` and extracted the disk image.
- Next, I ran the command: `mmls partition4.img` to inspect the partition layout. It showed no useful partitions.
- I used the foremost tool to recover deleted files.
- It revealed the files: `audit.txt`, `exe`, and `htm`. I examined `audit.txt`.
- The `audit.txt` displayed a timestamp for the `.exe` file: 03/03/2016 23:58:41
- <img width="600"  alt="image" src="https://github.com/user-attachments/assets/9d87bd78-da33-4bbc-9946-650b8c353f89" />
- I compared the metadata using: `exiftool exe` and `stat exe`
- <img width="600"  alt="image" src="https://github.com/user-attachments/assets/18c26f14-ba02-4e52-9185-4110a0d63049" />
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/c091c526-a584-488c-b749-a48319042c0b" />
- The `stat` output showed identical MACB timestamps, which correspond to the recovery time(since the file was recreated by Foremost).
- The `exiftool` output showed an older timestamp, which likely reflects the original file timestamp.
- A mismatch with unrealistic timestamps can indicate timestomping, but in this case, the difference is explained by file recovery.
- To analyze filesystem metadata, I used: `fls -r -d partition4.img > body.txt`
- The `.exe` and `.htm` files were not present in the filesystem listing, even though they were recovered using Foremost.
- This indicates that the files were deleted: the directory entries and metadata were removed, but the file content remained on
  disk until overwritten.
- I converted the raw metadata into a human-readable timeline using: `mactime -b body.txt > data.txt`
- Inspecting the timeline: cat data.txt | head -n 10, revealed an unusual timestamp (`1985`) associated with a file path.
- <img width="600"  alt="image" src="https://github.com/user-attachments/assets/62cd1fda-3b1f-4e82-85c2-744e98b1371d" />
- This abnormal timestamp is a strong indicator of possible timestomping or suspicious activity.
- I accessed the file directly using its inode: `icat partition4.img 4945`
- This revealed a Base64-encoded string:  `NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK` 
- I decoded it using: `echo "NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK" | base64 -d`
- The decoded output revealed the flag.

### Key Learnings
- **Inode** : A unique identifier that stores a file’s metadata and points to its actual data on disk (not the filename).
- **fls** : A tool from The Sleuth Kit that lists files (including deleted ones) from a disk image along with metadata and inode numbers.
- **mactime** : A tool that creates a chronological timeline of file activity using Modified, Accessed, and Changed (MAC) timestamps.
- **stat vs exiftool** : `stat` shows filesystem timestamps (which may change after recovery), while `exiftool` can reveal embedded
 or original metadata timestamps inside the file.
