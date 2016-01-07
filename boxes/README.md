- For split file
# split --bytes=10M file.tar file

- For Recovery
# cat file* > file.tar

- For Load Image
# docker load -i file.tar

NOTE: If the load fails, try to import the file using the next command
# cat file.tar | docker import - Image-name
