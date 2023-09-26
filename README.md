## Duplicate File Finder ( *duplicate_FF* )
The script will search supplied directory(s) and compare files using sha256 checksum and produce reports on what it found.                  
                                                                  
Script created on MS Windows WSL-Ubuntu and also tested with MSYS2 and Gitbash shells.  It should work on Cygwin (MSYS2 and Gitbash are Cygwin derivates) and other Linux.                                                     

It requires a least one directory to search many directories can be compared.  Files names and maximum file sizes can be used as filters to narrow searches and save time. Reports are CSV format which camn be imported into a spreadsheet. 

List(s) of file to move or delete can created from either duplicate file CSV files. 
__________________________________________________________________________________________
Script created on MS Windows WSL-Ubuntu and also tested with MSYS2 and Gitbash shells.  It should work on Cygwin (MSYS2 and Gitbash are Cygwin derivates) and other Linux. 

__Usage__ 
_duplicate_FF -f 'filter' -d 'source directory' -o 'output directory'_

**_duplicate_FF -f '.mp4' -m 300 -d './video/' -d '/home/fred/down loads' -o /tmp_** Check all mp4 files that are smaller 300MB in Fred's home directory and the subdirectory ./video. Output will be placed in /tmp.   

Inputs of 'filter' 'source directory' 'output directory' should have single or double quotes otherwise any names with white space will not be processed.


**-f** Optional case insensitive filter, filter the list files by part or the whole name of a file. 

**-d** One or many directories can be entered each must start with -d.  

**-o** Optional output directory. If not set, the current directory will used. In the output directory a unique subdirectory is created containing all output files
  
Setting maximum file size is optional, default is 20 GiB.  Ignoring large files can save time.

**-k or -K** kilobytes (KiB)

**-m or -M** megabytes (Mib)

**-g or -G** gigabytes (GiB)
    
### Notes      
* If filtered by 'rar' that will pick both the word 'LIBRARY' and suffix '.rar'
* Full file names can be used, but it only reports on exact contents using checksum.  
* Only the first instances of -f and -o used, the rest ignored

### OUTPUTS 
NOTE: If MS Excel is the default application for CVS files and files names contain non-Latin alphabet characters excel will not display those characters correctly. Rename *.csv file to a *.txt and import manually.  
  
__../duplicate_files1_yymmdd-hhmm.csv__  
Format – one file per row

column 1 : sha256 Checksum

column 2 : fully pathed file name

column 3 : full path of containing directory

column 4 : file size in KiB

__../duplicate_files2_yymmdd-hhmm.csv__  
Format – For each sha256 value list size and all files

column 1 : sha256 Checksum

column 2 : file size in KiB 

column 3 : fully pathed file name

column 4 : full path of containing directory

column 5 : fully pathed file name

column 6 : full path of containing directory

If more files match the checksum they are added as columns in <file><directory> pairs        

__../all_files_yymmdd-hhmm.csv__     Format: check_sum,\"\<full path\>\/\<file name\>\"

__../unique_files_yymmdd-hhmm.csv__  Format: check_sum,\"\<full path\>\/\<file name\>\"

__../log__yymmdd-hhmm.txt__ 	 Basic logging. 

Windows file systems occasionally produce some odd stuff that cannot be processed when mounted on Linux.     

### USING THE REPORT TO REMOVE FILES
Removing files can be tricky and best done with care, best to move files before removal.  A file may use its directory name to give it meaning or the directory name may irrelevant and the file name may be important.
 
A removal or move list can created from either spreadsheet (__duplicate_files1_yymmdd-hhmm.csv__ or __duplicate_files2_yymmdd-hhmm.csv__).
                                                               
