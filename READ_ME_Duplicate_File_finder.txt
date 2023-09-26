Script created on MS Windows WSL-Ubuntu and also tested with MSYS2 and Gitbash shells.  It should work on Cygwin (MSYS2 and Gitbash are Cygwin derivates) and other Linux. 

Usage: duplicate_file_finder -f 'filter' -d 'source directory' -o 'output directory'

Inputs of 'filter' 'source directory' 'output directory' must have single or double quotes otherwise any names with white space will not be processed.

-f Optional filter, filter file list by part of the file name. 
-d One or many directories can be entered each must start with -d.  
-o Optional output directory. If not set, the current directory will used  
   In the output directory a unique subdirectory is created containing all output files
  
Setting maximum file size is optional, default is 20 GiB.  Ignoring large files can save time.
-k or -K kilobytes (KiB)
-m or -M megabytes (Mib)
-g or -G gigabytes (GiB)
    
Notes      
* If filtered by 'rar' that will pick both the word 'library' and suffix '.rar'
* Full file names can be used, but it only reports on exact contents using checksum.  
* Only the first instances of -f and -o used, the rest ignored

OUTPUTS 
NOTE: If MS Excel is the default application for CVS files and files names contain non-Latin alphabet characters excel will not display those characters correctly. Rename *.csv file to a *.txt and import manually.  
  
../duplicate_files1_yymmdd-hhmm.csv  
Format – one file per row
        column 1 : sha256 Checksum 
        column 2 : fully pathed file name
        column 3 : full path of containing directory   
        column 4 : file size in KiB 

../duplicate_files2_yymmdd-hhmm.csv  
Format – For each sha256 value list size and all files
        column 1 : sha256 Checksum
        column 2 : file size in KiB 
        column 3 : fully pathed file name
        column 4 : full path of containing directory
        column 5 : fully pathed file name
        column 6 : full path of containing directory
        If more files match the checksum they are added as columns in <file><directory> pairs        

../all_files_yymmdd-hhmm.csv     Format: check_sum,"<full pathed>/<file name>"
../unique_files_yymmdd-hhmm.csv  Format: check_sum,"<full pathed>/<file name>"
../log__yymmdd-hhmm.txt 	 Basic logging. Windows file systems occasionally produce some odd stuff that cannot be processed when mounted on Linux.     

USING REPORT TO REMOVE FILES
emoving files can be tricky and best done with care, best to move files before removal.  A file may use its directory name to give it meaning or the directory name may irrelevant and the file name may be important.
 
A removal or move list can created from either spreadsheet (duplicate_files1_yymmdd-hhmm.csv, duplicate_files2_yymmdd-hhmm.csv).
