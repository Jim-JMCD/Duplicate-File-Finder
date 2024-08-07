## Duplicate File Finder ( *duplicate_FF* )
A bash script will search supplied directory(s) and compare files using sha256 checksum and produce reports on what it finds.                  
                                                                  
Script created on MS Windows WSL-Ubuntu and also tested with MSYS2 and Gitbash shells.  It should work on Cygwin (MSYS2 and Gitbash are Cygwin derivates) and other Linux.                                                     

It requires a least one directory to search many directories can be compared.  Files names and maximum file sizes can be used as filters to narrow searches and save time. Reports are CSV format which camn be imported into a spreadsheet. 

List(s) of files to move or delete can be created from either of duplicate file CSV reports. 

Directories with name '$RECYCLE.BIN' are ignored. Linux sees some MS Windows directories as executable only, a user or app can go into them but can't read them. If the Windows "executable only" directory is user accessible, it be easily corrected by respondiong to "You don't currently have permission to access this folder".  If the directory is not user accessible then its probably a system directory that not worth checking for dulicate files.  
__________________________________________________________________________________________
Script created on MS Windows WSL-Ubuntu and also tested with MSYS2 and Gitbash shells.  It should work on Cygwin (MSYS2 and Gitbash are Cygwin derivates) and other Linux. 

__Usage__ 
_duplicate_FF -f 'filter' -d 'source directory' 

**_duplicate_FF -f '.mp4' -m 300 -d './video/' -d '/home/fred/down loads' -o /tmp_** Check all mp4 files that are smaller 300MB in Fred's home directory and the subdirectory ./video. Output will be placed in /tmp.   

Inputs of 'filter' 'source directory' 'output directory' should have single or double quotes otherwise any names with white space will not be processed.


**-f** Optional case insensitive filter, filter the list files by part or the whole name of a file. 

**-d** One or many directories can be entered each must start with -d.  

Setting maximum file size is optional, default is 20 GiB.  Ignoring large files can save time.

**-k or -K** kilobytes (KiB)

**-m or -M** megabytes (Mib)

**-g or -G** gigabytes (GiB)
    
### Notes
* Output directory is created in the directory from which the script is run.
* Temporary files are created in the directory from which the script is run - all removed or moved on scipt termination
* 'filter' and 'source directory' require single or double quotes for spaces in the filter and directory input.
* If filtered by 'rar' that will pick both the word 'LIBRARY' and suffix '.rar'
* Full file names can be used, but it only reports on contents using checksum.  
* Only the first instances of -f used, the rest ignored.
* The script is designed to be thorough, not designed for speed.
* Windows file systems occasionally produce some odd stuff that cannot be processed when mounted on Linux.

### OUTPUTS 

NOTE: If MS Excel is the default application for CVS files and files names containing non-Latin alphabet characters excel will not display those characters correctly. Change the default app for CSV to Notepad/Wordpad and manually import into excel or rename *.csv file to a *.txt and import manually.  
  
__../duplicate_files1_yymmdd-hhmm.csv__  
Format – One file per row

column 1 : sha256 checksum

column 2 : fully pathed file name

column 3 : full path of containing directory

column 4 : file size in KiB

__../duplicate_files2_yymmdd-hhmm.csv__  
Format – Every row is as unique sha256 value with file size and all files with that shar256 value.

column 1 : sha256 checksum

column 2 : file size in KiB 

column 3 : fully pathed file name

column 4 : full path of containing directory

column 5 : fully pathed file name

column 6 : full path of containing directory

If more files match the checksum they are added as columns in <file> <directory> pairs        

__../all_files_yymmdd-hhmm.csv__     Format: check_sum,\"\<full path\>\/\<file name\>\"

__../unique_files_yymmdd-hhmm.csv__  Format: check_sum,\"\<full path\>\/\<file name\>\"

__../log__yymmdd-hhmm.txt__ 	 Basic logging. 
   
### USING THE REPORT TO MOVE or REMOVE FILES
Removing files can be tricky and best done with care, best to move files before removal.  A file may use its directory name to give it meaning or the directory name may irrelevant and the file name may be important.
 
A removal or move list can created from either spreadsheet (__duplicate_files1_yymmdd-hhmm.csv__ or __duplicate_files2_yymmdd-hhmm.csv__).

#### Bulk moving of all duplicates 
This example moves all files on a list to a single directory, only one copy (by name) is kept. Suggest testing with a small sample. 
Have all the files to move in a single spreadsheet column. All columns with any other contents should be deleted. 
Save as text (don't NOT save as a DOS file if file names have non-Latin characters, usually will save to the UTF-8 format, which preserves non-Latin characters). For each line in the file has to be transformed from something like this


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

