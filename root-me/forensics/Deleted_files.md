### Challenge:
Deleted file

### Goal:
Your cousin found a USB drive in the library this morning. He is not very good with computers, so he asked for your help to find
the owner of the USB drive.
Clue: You can look all you want, but this key is empty.
The flag is the owner’s identity in the form `firstname_lastname`.

### Walkthrough:
- Downloaded the challenge file `ch39.gz`
- The file has a `.gz` extension, so I decompressed it using the command:  `gunzip ch39.gz`
- After decompression, I checked the file type using the `file` command
- It showed that the file is a POSIX tar archive
- Renamed the file from `ch39` to `ch39.tar`.
- Extracted the archive `(tar -xvf ch39.tar)` and found a file named `usb.image`
- <img width="600" height="139" alt="image" src="https://github.com/user-attachments/assets/d1217b36-dcd5-4546-9a27-4f6a132bd864" />
- The challenge title “Deleted file” indicates that the required data might have been removed but still recoverable
- Next, I verified the file type of `usb.image`
- A `.image` file is a disk image, which is a complete copy of a storage device like a USB drive
- The challenge title “Deleted file” indicates that the required data might have been removed but still recoverable.
- Used `foremost`, a file carving tool, to recover deleted files from the disk image:  
    `foremost -i usb.image -o recovered`
- A directory named `recovered` was created
- Inside the directory, I found:
    - `audit.txt`
    - `00000168.png`
- Checked `audit.txt`, which contained details of the recovery process and file sizes
- Verified that `00000168.png` is a valid PNG image file
- Analyzed the metadata of the image using a tool like `exiftool`
- Found the owner’s name in the metadata (author/writer field), which is the required flag
- <img width="600" height="533" alt="image" src="https://github.com/user-attachments/assets/ebe4dfa8-1878-4d66-835f-73d0accf5e2c" />

### Key Learnings
- `foremost` is used for file carving, which means recovering deleted files from raw data
- Disk images contain a full copy of a storage device and can be analyzed to recover hidden or deleted files
- Metadata of files (like images) can reveal important information such as the creator or owner
