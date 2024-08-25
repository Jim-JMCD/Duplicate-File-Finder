## Backup files before before deleting or moving anything. 

The MS Windows CMD terminal window will not work with non-latin charaters, use the MS __Windows Terminal__ app.  

### NOTES for those that want to use MS Excel or Libre Cal to make the task a lot simpler. 
1. MANDITORY: Any action list (move or delete) created in Excel or Cal on MS Windows must be saved as a *.txt file and converted to Linux format using the dos2linux command. 
   * Run __dos2unix \<file name\>__  
2. A removal or move list can be created from either report
   * __duplicate_files1_yymmdd-hhmm.csv__ or
   * __duplicate_files2_yymmdd-hhmm.csv__ 
3. Test with a small sample.
4. Progress can be monitored with the creation of the all_files CSV report, that is created first. Once that has been completed  the creation of the other reports can be monitored in the same way.
   * Run __tail -f all_files_YYMMDD-hhmm.csv__   

### Bulk moving of duplicates 
Import the __duplicate_files1_yymmdd-hhmm.csv__ into excel or cal 
1. Delete all the rows belonging to files you don't want to move.
2. Delete the SHA256 and Directory columns and save as a text file, the __FILE_LIST.txt__
3. If on MS Windows WSL, MSYS2 or Gitbash you _must_ convert the  __FILE_LIST.txt__ file to linux format using *dos2unix* command. 
4. Generate the MOVE_LIST where each line is a command to move each file individually.   
    * Run command where TARGET_DIRECTORY is the directory where file are to moved to, something like _"/tmp/duplicates"_  The directroy path must be in double quotes     
__awk -v destination="TARGET_DIRECTORY" '{ printf "mv -f \x27%s\x27 \x27%s\x27\n",$0,destination}' < FILE_LIST.txt > MOVE_LIST__

    * The MOVE_LIST will have lines like: _mv -f  '/file/source/directory/path/file_name' '/file/destination/path/'_
5. To move files run: __bash MOVE_LIST__

### Bulk deletion of duplicates
All the same except the command to generate the output is slightly different

__awk '{ printf "rm -f  \x27%s\x27\n",$0}' < FILE_LIST.txt > REMOVE_LIST__

The REMOVE_LIST will have lines like: _rm -f '/file/source/directory/path/file_name'_
    
To remove files run: __bash REMOVE_LIST__

NOTE: If want to be cautious remove the __-f__ from the commands. You will be prompted for every delete or move. 
____________________      
   

