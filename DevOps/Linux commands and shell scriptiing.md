# Linux Commands & Shell Scripting Cheat Sheet - Beginner to Pro

## Table of Contents
- [File Operations](#file-operations)
- [Directory Operations](#directory-operations)
- [File Permissions](#file-permissions)
- [Text Processing](#text-processing)
- [File Search](#file-search)
- [Process Management](#process-management)
- [System Information](#system-information)
- [Network Commands](#network-commands)
- [Package Management](#package-management)
- [Shell Scripting Basics](#shell-scripting-basics)
- [Advanced Scripting](#advanced-scripting)
- [Best Practices](#best-practices)

---

## File Operations

### Basic File Commands
```bash
# List files
ls
ls -l          # Long format
ls -la         # Include hidden files
ls -lh         # Human readable sizes
ls -lt         # Sort by modification time
ls -lS         # Sort by size

# Create file
touch file.txt
touch file1.txt file2.txt

# View file content
cat file.txt
cat file1.txt file2.txt    # Concatenate multiple files

# View with line numbers
cat -n file.txt

# View file page by page
less file.txt
more file.txt

# View first/last lines
head file.txt              # First 10 lines
head -n 20 file.txt        # First 20 lines
tail file.txt              # Last 10 lines
tail -n 20 file.txt        # Last 20 lines
tail -f file.txt           # Follow file (real-time)

# Copy files
cp source.txt destination.txt
cp file1.txt file2.txt /destination/
cp -r directory/ /destination/    # Copy directory recursively
cp -i source.txt dest.txt         # Interactive (ask before overwrite)

# Move/Rename files
mv old.txt new.txt
mv file.txt /destination/
mv *.txt /destination/

# Remove files
rm file.txt
rm -f file.txt            # Force remove
rm -i file.txt            # Interactive

# Create symbolic link
ln -s /path/to/file link_name

# View file type
file filename
```

---

## Directory Operations

### Directory Commands
```bash
# Print working directory
pwd

# Change directory
cd /path/to/directory
cd ~                      # Home directory
cd ..                     # Parent directory
cd -                      # Previous directory

# Create directory
mkdir directory
mkdir -p path/to/directory    # Create parent directories

# Remove directory
rmdir directory               # Remove empty directory
rm -r directory              # Remove directory recursively
rm -rf directory             # Force remove recursively

# Copy directory
cp -r source/ destination/

# Move directory
mv directory/ /new/location/

# List directory tree
tree
tree -L 2                # Limit depth to 2 levels
```

---

## File Permissions

### Permission Basics
```bash
# View permissions
ls -l

# Change permissions (symbolic)
chmod u+x file.sh         # Add execute for user
chmod g+w file.txt        # Add write for group
chmod o-r file.txt        # Remove read for others
chmod a+x file.sh         # Add execute for all
chmod u+rwx,g+rx,o+r file.txt

# Change permissions (numeric)
chmod 755 file.sh         # rwxr-xr-x
chmod 644 file.txt        # rw-r--r--
chmod 777 file.txt        # rwxrwxrwx
chmod 600 file.txt        # rw-------

# Permission numbers:
# 4 = read (r)
# 2 = write (w)
# 1 = execute (x)
# 7 = rwx (4+2+1)
# 6 = rw- (4+2)
# 5 = r-x (4+1)

# Change owner
chown user file.txt
chown user:group file.txt
chown -R user:group directory/

# Change group
chgrp group file.txt
chgrp -R group directory/
```

---

## Text Processing

### grep (Search Text)
```bash
# Basic search
grep "pattern" file.txt

# Case-insensitive
grep -i "pattern" file.txt

# Recursive search
grep -r "pattern" /path/

# Show line numbers
grep -n "pattern" file.txt

# Invert match (show non-matching lines)
grep -v "pattern" file.txt

# Count matches
grep -c "pattern" file.txt

# Show only matching part
grep -o "pattern" file.txt

# Extended regex
grep -E "pattern1|pattern2" file.txt
egrep "pattern1|pattern2" file.txt

# Show context
grep -A 3 "pattern" file.txt    # 3 lines after
grep -B 3 "pattern" file.txt    # 3 lines before
grep -C 3 "pattern" file.txt    # 3 lines before and after
```

### sed (Stream Editor)
```bash
# Replace text
sed 's/old/new/' file.txt              # First occurrence per line
sed 's/old/new/g' file.txt             # All occurrences
sed 's/old/new/gi' file.txt            # Case-insensitive

# Edit file in place
sed -i 's/old/new/g' file.txt

# Delete lines
sed '1d' file.txt                      # Delete first line
sed '1,3d' file.txt                    # Delete lines 1-3
sed '/pattern/d' file.txt              # Delete matching lines

# Print specific lines
sed -n '1p' file.txt                   # Print line 1
sed -n '1,5p' file.txt                 # Print lines 1-5
sed -n '/pattern/p' file.txt           # Print matching lines
```

### awk (Text Processing)
```bash
# Print columns
awk '{print $1}' file.txt              # Print first column
awk '{print $1, $3}' file.txt          # Print columns 1 and 3
awk '{print $NF}' file.txt             # Print last column

# Field separator
awk -F ':' '{print $1}' /etc/passwd    # Use : as separator

# Pattern matching
awk '/pattern/ {print $1}' file.txt

# Conditional
awk '$3 > 100 {print $1}' file.txt

# Sum column
awk '{sum+=$1} END {print sum}' file.txt

# Calculate average
awk '{sum+=$1; count++} END {print sum/count}' file.txt
```

### cut (Extract Sections)
```bash
# Cut by character
cut -c 1-5 file.txt                    # Characters 1-5

# Cut by field
cut -d ',' -f 1 file.csv               # First field (comma delimiter)
cut -d ':' -f 1,3 /etc/passwd          # Fields 1 and 3
```

### sort & uniq
```bash
# Sort
sort file.txt
sort -r file.txt                       # Reverse
sort -n file.txt                       # Numeric sort
sort -k 2 file.txt                     # Sort by second column

# Unique lines
uniq file.txt
sort file.txt | uniq                   # Remove duplicates
sort file.txt | uniq -c                # Count occurrences
sort file.txt | uniq -d                # Show only duplicates
```

### tr (Translate)
```bash
# Convert case
tr 'a-z' 'A-Z' < file.txt              # Lowercase to uppercase
tr 'A-Z' 'a-z' < file.txt              # Uppercase to lowercase

# Delete characters
tr -d ' ' < file.txt                   # Delete spaces
tr -d '\n' < file.txt                  # Delete newlines

# Replace characters
tr ' ' '_' < file.txt                  # Replace spaces with underscores
```

---

## File Search

### find
```bash
# Find by name
find /path -name "*.txt"
find /path -iname "*.txt"              # Case-insensitive

# Find by type
find /path -type f                     # Files only
find /path -type d                     # Directories only

# Find by size
find /path -size +10M                  # Larger than 10MB
find /path -size -1M                   # Smaller than 1MB

# Find by time
find /path -mtime -7                   # Modified in last 7 days
find /path -mtime +30                  # Modified more than 30 days ago

# Find by permissions
find /path -perm 755

# Execute command on results
find /path -name "*.log" -delete
find /path -name "*.txt" -exec cat {} \;
find /path -type f -exec chmod 644 {} \;

# Find and move
find /path -name "*.txt" -exec mv {} /destination/ \;
```

### locate
```bash
# Quick file search (uses database)
locate filename

# Update database
sudo updatedb

# Case-insensitive
locate -i filename
```

---

## Process Management

### Process Commands
```bash
# View processes
ps                        # Current shell processes
ps aux                    # All processes
ps aux | grep nginx       # Filter processes

# Process tree
pstree

# Top processes
top                       # Interactive
htop                      # Better interface (if installed)

# Kill process
kill PID
kill -9 PID              # Force kill
killall process_name     # Kill by name
pkill process_name       # Kill by pattern

# Background/Foreground
command &                # Run in background
jobs                     # List background jobs
fg %1                    # Bring job 1 to foreground
bg %1                    # Resume job 1 in background
Ctrl+Z                   # Suspend current process

# Process priority
nice -n 10 command       # Start with priority
renice -n 5 -p PID      # Change priority
```

---

## System Information

### System Commands
```bash
# System info
uname -a                 # All system info
uname -r                 # Kernel version
hostname                 # Computer name
uptime                   # System uptime

# Memory
free -h                  # Memory usage (human readable)
free -m                  # Memory in MB

# Disk space
df -h                    # Disk space (human readable)
df -i                    # Inode usage
du -h /path              # Directory size
du -sh /path             # Total size only
du -h --max-depth=1      # First level only

# CPU info
lscpu
cat /proc/cpuinfo

# Users
whoami                   # Current user
who                      # Logged in users
w                        # Logged in users with activity
last                     # Login history
id                       # User ID and groups

# Date and time
date
date +"%Y-%m-%d %H:%M:%S"
```

---

## Network Commands

### Network Operations
```bash
# Network interfaces
ifconfig                 # (deprecated)
ip addr                  # Show IP addresses
ip link                  # Show network interfaces

# Ping
ping google.com
ping -c 4 google.com     # 4 packets only

# DNS lookup
nslookup google.com
dig google.com
host google.com

# Network connections
netstat -tuln            # Listening ports
ss -tuln                 # Socket statistics (modern)
ss -tulnp                # With process info

# Download files
wget https://example.com/file.zip
wget -O output.zip https://example.com/file.zip

curl -O https://example.com/file.zip
curl -o output.zip https://example.com/file.zip

# Transfer files
scp file.txt user@server:/path/
scp user@server:/path/file.txt ./

# SSH
ssh user@server
ssh -i key.pem user@server
```

---

## Package Management

### APT (Debian/Ubuntu)
```bash
# Update package list
sudo apt update

# Upgrade packages
sudo apt upgrade
sudo apt full-upgrade

# Install package
sudo apt install package-name

# Remove package
sudo apt remove package-name
sudo apt purge package-name      # Remove with config

# Search package
apt search package-name

# Show package info
apt show package-name

# Clean cache
sudo apt autoremove
sudo apt clean
```

### YUM/DNF (RedHat/CentOS/Fedora)
```bash
# Install
sudo yum install package-name
sudo dnf install package-name

# Update
sudo yum update
sudo dnf update

# Remove
sudo yum remove package-name
sudo dnf remove package-name

# Search
yum search package-name
dnf search package-name
```

---

## Shell Scripting Basics

### First Script
```bash
#!/bin/bash
# This is a comment

echo "Hello, World!"
```

### Variables
```bash
#!/bin/bash

# Variable assignment (no spaces)
name="John"
age=25

# Use variables
echo "Name: $name"
echo "Age: $age"
echo "Name: ${name}"

# Command substitution
current_date=$(date)
files=$(ls)

# Read input
read -p "Enter your name: " username
echo "Hello, $username"
```

### Conditionals
```bash
#!/bin/bash

# If statement
if [ "$age" -gt 18 ]; then
    echo "Adult"
elif [ "$age" -eq 18 ]; then
    echo "Just turned adult"
else
    echo "Minor"
fi

# String comparison
if [ "$name" = "John" ]; then
    echo "Hello John"
fi

# File tests
if [ -f "file.txt" ]; then
    echo "File exists"
fi

if [ -d "directory" ]; then
    echo "Directory exists"
fi

# Multiple conditions
if [ "$age" -gt 18 ] && [ "$name" = "John" ]; then
    echo "Adult named John"
fi

if [ "$age" -gt 18 ] || [ "$name" = "John" ]; then
    echo "Either adult or John"
fi
```

### Loops
```bash
#!/bin/bash

# For loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# For loop with range
for i in {1..10}; do
    echo "Number: $i"
done

# For loop with files
for file in *.txt; do
    echo "Processing: $file"
done

# While loop
counter=1
while [ $counter -le 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done

# Until loop
counter=1
until [ $counter -gt 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done

# Break and continue
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue
    fi
    if [ $i -eq 8 ]; then
        break
    fi
    echo $i
done
```

### Functions
```bash
#!/bin/bash

# Function definition
greet() {
    echo "Hello, $1!"
}

# Call function
greet "John"

# Function with return value
add() {
    local sum=$(($1 + $2))
    echo $sum
}

result=$(add 5 3)
echo "Result: $result"

# Function with multiple parameters
print_info() {
    echo "Name: $1"
    echo "Age: $2"
    echo "City: $3"
}

print_info "John" 25 "New York"
```

### Arrays
```bash
#!/bin/bash

# Array declaration
fruits=("Apple" "Banana" "Orange")

# Access elements
echo ${fruits[0]}        # First element
echo ${fruits[@]}        # All elements
echo ${#fruits[@]}       # Array length

# Add element
fruits+=("Mango")

# Loop through array
for fruit in "${fruits[@]}"; do
    echo $fruit
done

# Array with indices
for i in "${!fruits[@]}"; do
    echo "Index $i: ${fruits[$i]}"
done
```

---

## Advanced Scripting

### Error Handling
```bash
#!/bin/bash

# Exit on error
set -e

# Exit on undefined variable
set -u

# Pipeline failure detection
set -o pipefail

# Check command success
if command; then
    echo "Success"
else
    echo "Failed"
    exit 1
fi

# Trap errors
trap 'echo "Error on line $LINENO"' ERR

# Cleanup on exit
trap 'rm -f /tmp/tempfile' EXIT
```

### Arguments
```bash
#!/bin/bash

# Script arguments
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"

# Check argument count
if [ $# -lt 2 ]; then
    echo "Usage: $0 arg1 arg2"
    exit 1
fi

# Shift arguments
shift
echo "After shift: $1"

# Parse options
while getopts "a:b:c" opt; do
    case $opt in
        a) arg_a=$OPTARG ;;
        b) arg_b=$OPTARG ;;
        c) flag_c=true ;;
        *) echo "Invalid option" ;;
    esac
done
```

### String Operations
```bash
#!/bin/bash

string="Hello World"

# Length
echo ${#string}

# Substring
echo ${string:0:5}       # "Hello"
echo ${string:6}         # "World"

# Replace
echo ${string/World/Universe}
echo ${string//o/0}      # Replace all

# Upper/Lower case
echo ${string^^}         # HELLO WORLD
echo ${string,,}         # hello world

# Remove prefix/suffix
filename="document.txt"
echo ${filename%.txt}    # document
echo ${filename#*.}      # txt
```

### File Operations
```bash
#!/bin/bash

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# Write to file
echo "content" > file.txt      # Overwrite
echo "more" >> file.txt        # Append

# Check file exists
if [ -f "file.txt" ]; then
    echo "File exists"
fi

# File tests
[ -r file ]  # Readable
[ -w file ]  # Writable
[ -x file ]  # Executable
[ -s file ]  # Not empty
[ -d dir ]   # Is directory
[ -L link ]  # Is symbolic link
```

---

## Best Practices

### Script Structure
```bash
#!/bin/bash
# Script: backup.sh
# Description: Backup important files
# Author: Your Name
# Date: 2024-01-01

# Exit on error
set -e
set -u
set -o pipefail

# Constants
readonly BACKUP_DIR="/backup"
readonly LOG_FILE="/var/log/backup.log"

# Functions
log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

backup_files() {
    local source=$1
    local dest=$2
    
    log "Backing up $source to $dest"
    cp -r "$source" "$dest"
}

# Main script
main() {
    log "Starting backup"
    
    if [ $# -lt 1 ]; then
        echo "Usage: $0 <source_dir>"
        exit 1
    fi
    
    backup_files "$1" "$BACKUP_DIR"
    
    log "Backup completed"
}

# Run main function
main "$@"
```

### Tips
```bash
# ✅ Good: Use quotes around variables
echo "$variable"

# ❌ Bad: No quotes
echo $variable

# ✅ Good: Use meaningful names
user_count=10

# ❌ Bad: Cryptic names
uc=10

# ✅ Good: Use functions
calculate_sum() {
    echo $(($1 + $2))
}

# ✅ Good: Check if command exists
if command -v git &> /dev/null; then
    echo "Git is installed"
fi

# ✅ Good: Use shellcheck to lint scripts
shellcheck script.sh
```

---

**Pro Tip:** Always use `set -euo pipefail` at the start of scripts for better error handling, quote variables to handle spaces properly, use functions to organize code, and leverage `shellcheck` for script validation!