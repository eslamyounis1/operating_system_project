# Virtual Disk Project

## Overview
This project is an implementation of a virtual disk management system. It includes components for handling user commands, managing a virtual file allocation table (FAT), and providing a structured representation of directories and files within the virtual disk.

## Project Structure
The project is structured into several classes, each responsible for a specific aspect of virtual disk management:

### 1. Command Processing
- Implement a class that handles user input:
  1. Splits the input into a command and its arguments.
  2. Verifies if the command is valid.
  3. Implements methods to determine argument types.

- Another class is responsible for:
  1. Checking the validity of command arguments (parsing).
  2. Executing commands if they are syntactically correct.

### 2. Command Execution
- A dedicated class provides a method for each command, ensuring that the appropriate operation is executed.

### 3. Virtual Disk Management
The **Virtual_Disk** class manages the virtual disk file, providing methods for:
- **Initialization**
  - Creates a virtual disk file if it does not exist.
  - Calls the **Mini_FAT** class to prepare and write the FAT.
  - Writes an initial empty cluster (1024 bytes of zeros).
- **Writing Data**
  - `WriteCluster(clusterNumber, data)`: Writes 1024 bytes to a specific cluster.
- **Reading Data**
  - `ReadCluster(clusterNumber)`: Reads 1024 bytes from a specific cluster.

### 4. File Allocation Table (FAT) Management
The **Mini_FAT** class manages an array representing the FAT (size: 1024 entries). It includes methods for:
- Preparing the FAT array during initialization.
- Printing the FAT array for debugging.
- Finding an available cluster.
- Retrieving and setting cluster pointers.
- Writing and reading the FAT array from the virtual disk.

### 5. Directory and File Management
#### **Directory_Entry Class**
Represents files and directories within the virtual disk.

#### **Directory Class**
Inherits from **Directory_Entry** and includes:
- A data structure to store subdirectories and files.
- A reference to the parent directory.
- Methods to:
  - Write directory content to the virtual disk.
  - Read directory content from the virtual disk.

#### **File_Entry Class**
Represents a file in the virtual disk, similar to **Directory**, but contains a string representing file content instead of subdirectories.

### 6. Data Conversion Utilities
A utility class provides methods for converting data types to and from byte arrays.

## Virtual Disk Initialization
1. The FAT array is written to clusters 1–4 of the virtual disk.
2. The root directory is created:
   ```cpp
   Directory root = new Directory("K:", 0x10, 0);
   ```
   - "K:" represents the disk name.
   - `0x10` signifies it is a directory.
   - `0` means the root is initially empty.
3. The root directory is set as the current directory.

## Command Execution
### **`md` Command (Create Directory)**
**Syntax:**
```
md [directory]
```
**Algorithm:**
1. Parse user input.
2. Two cases:
   - If `[directory]` is a name, create it inside the current directory.
   - If `[directory]` is a full path, create it at the specified location.
3. Steps for first case:
   - Create a **Directory_Entry** object.
   - Add it to the current directory’s list.
   - Write updated directory content to the virtual disk.
   - Update the FAT array.

## Conclusion
With the root directory set up, the system is ready to execute commands. Additional functionalities, such as file operations and directory traversal, can be built upon this foundation.

