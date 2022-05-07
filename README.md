# operating_system_project
Virtual_Disk_Project
	To write a clean organized code you may mimic the following your way:
First:
	You can implement a class that has methods that:
1-	Split the user input to a command (and its arguments), checks that this command is from our commands or not.
2-	You may need to implement methods to know the type of an argument.
Second:
	You may want to implement a class that check the validity of command arguments (parsing) and if the whole command with its arguments is valid syntactically then Execute that command.
Third:
	You may need to implement a class that has a method for each command calling it executes that command.
Fourth:
	You should implement a class calls Virtual_Disk that has the following methods:
1-	A method to initialize the virtual disk that takes the name of the file that represents our virtual disk and check if that file does exist or not:
a-if the file does not exist means that we know in the phase of producing the virtual disk. so, it creates a file with that name, opens it then call a method from the class Mini_FAT (mentioned below) to prepare our FAT then write the first cluster as 0s (write the first 1024 bytes as 0s), then it also calls the method from Mini_FAT class to write the FAT into the file.
b-if the file exists then it opens it, calls the method from the Mini_FAT class to read the fat from the virtual disk.
2-	A method to WriteCluster that takes 1024 bytes and the cluster number and writes the 1024 bytes in the place of that cluster in the virtual disk.
3-	A method that ReadCluster that takes the cluster number so go to that place (seek) in the virtual disk and read 1024 bytes.
Fifth:
	You will need to write the FAT array many times as we progress in the project so, it is better to implement a class for example name it Mini_FAT.
this class has the array of int which called FAT and its size equals to 1024 items.
You may need to add these methods to this class:
1-	A method to prepare the FAT array: this method which assign the values -1,2,3,4,-1 or -1 for the items with indices 0,1,2,3,4 respectively the rest items is assigned 0, we will use this function only one time when create our virtual disk.
2-	A method to print the FAT array for Debugging.
3-	A method that get the index of an available cluster (the index of item with value 0) (if that item does not exist this means that our disk is full and all of our clusters is full so the method return in that case -1). 
4-	A method for getting a cluster pointer it takes the cluster number which you will use it as an index to a FAT array item to get the value in that item which (may be 0 ”empty cluster” ,-1 ”full cluster and this cluster is the end of the content of what this cluster is belonged to”, any value between (5 and 1023) “ indicates that a cluster is full and the cluster with that number completes its content”.
5-	A method for setting a cluster pointer which takes cluster number to use it as an index to a FAT array item and assign to that item the second parameter (cluster pointer) which represents the status of that cluster.
6-	A method to set the FAT array which take an array of int as parameter and assign it to the FAT array.
7-	A method to write the FAT array into our virtual Disk, it writes it in clusters 1,2,3,and 4.
it makes use of WriteCluster method (that you have implemented in Virtual_Disk class) to write the FAT array cluster by cluster into the virtual disk.
8-	A method to read FAT array from our Virtual Disk, it makes use of ReadCluster method (that you have implemented in the Virtual_Disk class) to read the FAT array from the virtual disk cluster by cluster.
Sixth:
	You must implement the class which called Directory_Entry which any instance of it will represent a file or directory in our file system.
its attributes are mentioned in the table in the project description slides.
Seventh:
	After thinking you may ask your self how can I represent a directory with an object of class Directory_Entry. So, you may need to implement a class called Directory that inherits from the class Directory_Entry and add the following attributes to it:
a- any dynamic data structure that you prefer to use its data type will be of Directory_Entry. So, that data structure will represent the directories and files that will be in that directory.
b- an attribute of type Directory that represents the parent of that directory.
-you may need to add these methods for now:
1-	A method to write the content of that directory to the virtual disk, what we will write we will write the items in the data structure that represents directory_entries of that directory.
-you agree with me that any object of Directory_Entry its attributes have the size 32 bytes.
-and we write every time a cluster to the disk that equals to 1024 bytes.
-so define an array of bytes with size equals to the number of the data structure items * 32
-now loop on the data structure items and CONVERT each item to an array of bytes of size 32, put that array inside the big array we have defined later.
-now you have the big array of bytes, its size may or may not exceed 1024 so if it exceeds you should split it more than one array of size 1024
-so you have a list of array of bytes each of which of size 1024 ready to be written to the virtual disk.
-so you begin writing in this directory firstcluster if it equals to 0, you should call the Mini_FAT method to get for you the available cluster to be the first cluster to that directory.
-so, loop on the list of array of bytes items and write every item with taking care of getting the cluster number of the available cluster to write and also setting its status or pointer in the FAT table.
-after writing this directory content you may need to update its entry in its parent list of Directory_Entry (may that directory was empty at first so its dir_firstcluster equals to zero but if a directory entry is added to it so we will write its content to it and its dir_firstcluster now have a value) and the parent rewrites its content.
- rewrite the FAT into the virtual disk.
2-	A method to read directory from the virtual disk it starts reading from the firstcluster of that directory if it does not equal to zero (non empty directory), you read cluster by cluster each of which is 1024 bytes, combine them and loop on them, every 32 byte try to convert them to a Directory_Entry and add it to the directory list of Directory_Entry.
Eighth: 
	You should implement a class called File_Entry that represents a file.
- it looks like class Directory except instead of the list of directory_Entries we have a string represents the content of the file.
-and when we write the file or read it we write that string and read it (in the manner we have used in reading and writing directory cluster by cluster).
Ninth:
You may want to implement a class that has methods to convert things from thier datatype to byte[] and vice versa.
After implementing the two classes (Directory_Entry and Directory).
-	What you will write to the virtual disk is the list of Directory_Entry of that Directory and when you read you will read it.
-	Last section we have stopped our creation of the virtual Disk at writing the FAT array to The virtual Disk.
-	Let’s create the root directory which is the directory that will contain all the directories and files (in other words all the Directory_Entries) that will be saved in our Virtual Disk.
-	The root Directory will be an object of the class Directory
Directory root = new Directory("K:", 0x10, 0);
the first value in the constructor "K:" indicates the name of the root directory (you may think it as the name of the disk).
0x10 indicates that this object is directory not a file.
0 this is the number of the first cluster that will contain the list of directory_entries of that Directory after we create directory_entries in it(zero here means that it is now empty but after adding entries to it and writing its content to virtual disk it will be 5).
-	Make the root directory as the current directory (you may define an instance of Directory in the Main class called directory to represent your current directory).
-	Now our virtual Disk is Ready to Execute commands in it.
-	Let’s talk about the algorithm of every command.
md - Creates a directory.
Creates a directory.
md command syntax is
 md [directory]
[directory] can be a new directory name or fullpath of a new directory
Algorithm:
1.	The user will input md [directory]
2.	You have two ways now.
-the first is when [directory] was only a directory name
-the second is when the directory is a full path to directory
3.	If first way, it is easy the user wants you to create that directory in the current directory 
a- you will create an object of Directory_Entry class and add this object to the current directory’s list of Directory_Entries
Directory_Entry d = new Directory_Entry([directory], 0x10,0);
//0 means that this Directory is Empty so its first cluster is 0
current.DirOrFiles.Add(d);
b- now rewrite the content of current again to the virtual disk
c-rewrite the FAT array also in the virtual disk.

