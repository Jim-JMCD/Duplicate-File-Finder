# UNDER CONSTRUCTION - Contents not reliable.
## NOTES for those that want to use MS Excel or Cal in the moving or deleting of duplicate files: 
1. The MS Windows CMD terminal window can't work with non-latin charaters, use the MS __Windows Terminal__ app.  
2. MANDITORY: Any action list (move or delete) created in Excel or Cal must be saved as a *.txt file and converted to Linux format using the dos2linux command to preswerve non-latin characters. 
   * Run __dos2unix \<file name\>__  
3. A removal or move list can be created from either report
   * __duplicate_files1_yymmdd-hhmm.csv__ or
   * __duplicate_files2_yymmdd-hhmm.csv__ 
4. Test with a small sample.   

#### Example: Bulk moving of all duplicates 
This example moves all dulpicate files in _/mnt/c/photos 2022_ to _/mnt/c/Temp/Duplicates_, only one copy is kept.
From the CSV spreadsheet report create a single column of 



__/mnt/c/photos 2022/IMG-202201291815__  

To

__mv -f ‘/mnt/c/photos 2022/IMG-202201291815’ ‘/mnt/c/Temp/Duplicates/’__

Alternative to using an editor to modify each line this can be done in bulk using linux commands. First convert the Windows text list to a Linux format list.  _This is mandatory_ 

Run __dos2unix windows_list tmp_list__   This command will preserve UTF-8 characters.
  
The following commands will add:

* _mv -f ‘_  to the start of every line

* _‘ ‘/mnt/c/Temp/Duplicates’_  to the end of every line

__awk '{printf "mv -f \x27%s\x27 \x27/mnt/c/Temp/Duplicates/\x27\n",$0}' tmp_list  > move_list__

alternatively

__sed 's/^/mv -f \x27/' tmp_list | sed 's/$/\x27 \x27\\/mnt\\/c\\/Temp\\/Duplicates\\/\x27/' > move_list__

Where 
* \x27 is used to denote a ' 
* each / of the path is preceding by a \ 

Once done, check the list.  If all is OK run 

__bash move_list__

