# File Permissions
rwx stands for read, write and execute. Permissions are split into three sections: user (the owner), group and others, and each section gets its own rwx.
I got "Permission denied" because the file was missing execute permission. I fixed it with `chmod u+x test.sh`, which gives the owner the x permission.
