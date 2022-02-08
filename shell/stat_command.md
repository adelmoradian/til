# Stat command

The stat is a  command which gives information about the file and filesystem.

Following is the information we get about the file when we run the stat command:

- **File**: The name of the provided file. If the provided file is a symlink, then the name will be different.
- **Size**: The size of a given file in Bytes.
- **Blocks**: Total number of allocated blocks to the file to store on the hard disk.
- **IO Block**: The size of every allocated block in bytes.
- **File type**: The file may be of the following types: Regular files, special files, directories, or symbolic links.
- **Device**: Device number in hexadecimal format.
- **Inode**: Inode number of the file.
- **Links**: Number of hard links of the file.
- **Access**: Access permissions in the numeric and symbolic methods.
- **Context**: The field stores SELinux security context.
- **Access**: The last time at which the file was accessed.
- **Modify**: The last time at which file was modified.
- **Change**: The last time the at which file’s attribute or content was changed.
- **Birth**: The time at which the file was created.
