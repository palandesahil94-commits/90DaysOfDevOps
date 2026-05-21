# File I/O Practice (Linux)

## Step 1: Create a file
Command:
touch notes.txt
Observation: Created an empty file named notes.txt

## Step 2: Write first line (overwrite)
Command:
echo "Line 1 - Learning Linux basics" > notes.txt
Observation: Added first line, overwrites existing content

## Step 3: Append second line
Command:
echo "Line 2 - Practicing file operations" >> notes.txt
Observation: Appended second line without deleting previous content

## Step 4: Append third line using tee
Command:
echo "Line 3 - Using tee command" | tee -a notes.txt
Observation: Line added and displayed on terminal

## Step 5: Read full file
Command:
cat notes.txt
Observation: Displayed all lines in the file

## Step 6: Read first 2 lines
Command:
head -n 2 notes.txt
Observation: Displayed first 2 lines only

## Step 7: Read last 2 lines
Command:
tail -n 2 notes.txt
Observation: Displayed last 2 lines only


# What I Learned (Important)

### 🔹 `>`

- Overwrites file
👉 Use carefully

### 🔹 `>>`

- Appends to file
👉 Safe for logs

### 🔹 `tee`

- Writes + prints output
👉 Useful in pipelines

Interview Tip

How do you write and read files in Linux?

“I use redirection operators like > and >> for writing, tee for simultaneous output, and commands like cat, head, and tail for reading files.”