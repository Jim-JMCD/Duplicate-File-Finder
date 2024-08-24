## Duplicate File Finder ( *duplicate_FF* )
A bash script will search supplied directory(s) and compare files using sha256 checksum and produce reports on what it finds.                  
                                                                  
Script created on MS Windows WSL-Ubuntu, tested with MSYS2 and Gitbash shells.  It should work on Cygwin (MSYS2 and Gitbash are Cygwin derivates) and other Linux.                                                     

It requires a least one directory to search many directories can be compared.  Files names and maximum file sizes can be used as filters to narrow searches and save time. Reports are CSV format which can be imported into a spreadsheet. 

List(s) of files to move or delete can be created from either of CSV reports. 

Directories with name '$RECYCLE.BIN' are ignored. Linux sees some MS Windows directories as executable only, a user or app can go into them but can't read them. If the Windows "executable only" directory is user accessible, it be easily corrected by respondiong to "You don't currently have permission to access this folder".  If the directory is not user accessible then its probably a system directory that is not worth checking for dulicate files.  
__________________________________________________________________________________________

__Usage__ 
duplicate_FF -f 'filter' -d 'source directory' 

**_duplicate_FF -f '.mp4' -m 300 -d './video/' -d '/home/fred/down loads' -o /tmp_** ..... Check all mp4 files that are smaller 300MB in Fred's down loads directory and ./video. Output will be placed in /tmp.   

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
* The 'filter' and 'source directory' require single or double quotes for spaces in the filter and directory input.
* If filtered by 'rar' that will pick both the word 'LIBRARY' and suffix '.rar'
* Full file names can be used, but it only reports on contents using checksum.  
* Only the first instances of -f used, the rest ignored.
* The script is designed to be thorough, not designed for speed.
* Windows file systems occasionally produce some odd stuff that cannot be processed when mounted on Linux.

### OUTPUTS 

NOTE: If MS Excel is the default application for CVS files, any files names in the spreadsheet containing non-Latin alphabet characters MS Excel will not display those characters correctly. 

FIX: Change the default app for CSV to Notepad or Wordpad and manually import into excel, alternatively rename *.csv file to a *.txt and import manually into MS Excel.  
  
__../duplicate_files1_yymmdd-hhmm.csv__  Format – One file per row

CSV Columns 
1. sha256 checksum
2. fully pathed file name
3. full path of containing directory
4. file size in KiB
------------------------------------

__../duplicate_files2_yymmdd-hhmm.csv__  Format – Every row is as unique sha256 value with file size and all files of the same shar256 value. If more files match the checksum they are added as columns in <file> <directory> pairs i.e. repeats of columns 3 and 4.  

CSV Columns
1. sha256 checksum
2. file size in KiB 
3. fully pathed file name 
4. full path of containing directory
------------------------------------
A CSV list of all files processed __../all_files_yymmdd-hhmm.csv__   ...... Format: check_sum,\"\<full path\>\/\<file name\>\"

A CSV list of all unique files  __../unique_files_yymmdd-hhmm.csv__  ... Format: check_sum,\"\<full path\>\/\<file name\>\"

Basic logging __../log__yymmdd-hhmm.txt__  
   

