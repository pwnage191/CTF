### Challenge:
Forensics Git 0

### Goal:
Can you find the flag in this disk image?

### Walkthrough
- I extracted the disk image from the compressed file.
- I examined the partition layout using `mmls disk.img` `# shows partition table and offsets`
- It showed multiple partitions:
    - Two Linux partitions `(0x83)` `# usable filesystems`
    - One swap partition `(0x82)` `# not useful for files`
    - <img width="600" alt="image" src="https://github.com/user-attachments/assets/48023aae-b262-408e-84a6-4a9d11a15886" />
- I used the starting sector of the fourth partition (`1140736`) as the offset `# required to access that partition`
- I used `fls` to navigate the filesystem `# like ls for disk images`
- <img width="600"  alt="image" src="https://github.com/user-attachments/assets/804e7449-7f7b-467d-8572-eaf3aa98ac9c" />
- Navigated to the home directory: `fls -o 1140736 disk.img 64770`
- Found the `ctf-player` directory and explored it: `fls -o 1140736 disk.img 64771`
- Found a directory named `secrets`: `fls -o 1140736 disk.img 65664`
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/8175175f-6e46-4ef9-bebe-d5f0d4a11b24" />
- Inside it, I discovered a `.git` directory: `fls -o 1140736 disk.img 65665 | grep '^d'`  `# filter only directories`
- The `.git` directory contained:
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/99645dc4-3ff7-4f3a-86e0-f9f31eedfdbb" />
- I focused on the `objects` directory `# stores Git data`
- Listing its contents: `fls -o 1140736 disk.img 65689`
- It showed: The directories `46`, `18`, and `32` contained files with 40-character hexadecimal names `# SHA-1 hashes`
- These are Git objects:
    - Each object is identified by a 40-character hash `# unique ID`
    - Stored in `.git/objects/`
    - Compressed using zlib `# binary format`
    - <img width="600"  alt="image" src="https://github.com/user-attachments/assets/565e209f-edba-4621-bc74-d9f675fec257" />
- Git internally stores data as:
    - `"blob <size>\0<content>"` `# standard format before hashing`
    - This data is hashed to generate the object ID
    - Then compressed and stored using the hash as filename
- The object files appeared as binary `# because of zlib compression`
- <img width="600" alt="image" src="https://github.com/user-attachments/assets/0d42f3fb-a6db-4510-8f96-8e6b35de520f" />
- I extracted and decompressed them:
- `icat -o 1140736 disk.img <inode> | zlib-flate -uncompress`:  extract + decompress in one step
- One of the decompressed objects was a commit object, contains metadata and message.
- The message revealed the flag.

### Key Learnings
- Used `mmls` and `fls` to analyze and navigate disk images
- Understood Git object storage (blob, tree, commit)
- Learned that Git uses SHA-1 hashes to identify objects based on content
- Explored zlib compression used in Git
- Recovered hidden data from Git history in a forensic scenario
