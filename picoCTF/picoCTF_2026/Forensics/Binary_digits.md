### Goal: 
This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything 
meaningful from this? Download the file here.

### Walkthrough:
- Ran file digits.bin and observed output: ASCII text with very long lines and no line terminators
- Recognized that the file contains readable characters, not actual binary data
- Opened the file and saw only 0 and 1 values
- Identified the data as binary encoded in ASCII text format
- Converted the binary string into raw bytes using a Python script
- Saved the decoded output as out.bin
- Opened out.bin and noticed strange/unreadable characters instead of normal text
- Observed patterns like ÿØÿ, indicating non-text binary content
- Identified ÿØÿ as hex FF D8 FF, which is the JPEG file signature
- Confirmed file type using file and xxd
- Renamed out.bin to out.jpg
- Opened the file successfully and recovered the image

### What I learned:
- Binary data written as ASCII (0 and 1) must be converted into raw bytes to reveal actual file content
- Files are identified by magic bytes (headers), not by their file extension
- Viewing raw bytes as text produces strange characters (e.g., ÿØÿ -> JPEG header FF D8 FF)
