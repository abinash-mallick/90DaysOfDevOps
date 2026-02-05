# Day – 6

## Task

Create a small text file and practice:
- Creating a file
- Writing text to a file
- Appending new lines
- Reading the file back

---

- `touch notes.txt`  
  This command would create a new file

- To write text to the file we have 2 ways.

### a. Using an editor (vim / nano)

- `vi notes.txt`  
  This command would open the vim editor and we can update the file.

### b. Using echo

- `echo "Hello" > notes.txt`  
  This command would write the text in quotes into the file.  
  Note: This will overwrite the existing contents of the file.

- `echo "Abinash" >> notes.txt`  
  Using this command the existing contents of the file will not be overwritten.  
  The text will be appended to the existing contents.

---

- `cat notes.txt`  
  This command is used to read the contents of the file

- `echo "This is line 2" | tee -a notes.txt`  
  `tee` command is used to capture a command output and send it to both file and standard output (stdout).  
  The `-a` flag is used to append the output to the file without overwriting.  
  If we don’t use the `-a` flag, the contents in the file will be overwritten.

---

- `head notes.txt`  
  This will display the first 10 lines of the file

- `tail notes.txt`  
  This will display the last 10 lines of the file

- `head -n 5 notes.txt`  
  This will display the first 5 lines of the file

- `tail -n 5 notes.txt`  
  This will display the last 5 lines of the file


![Handon](~/img/Image1.png)
