import os, sys, string, time

# scons options:
#    implicit_cache: Cache implicit dependencies, like *.d files
SetOption('implicit_cache', 1)

#-----------------------------------------------------------------------------------------------------------------------
# top root
#-----------------------------------------------------------------------------------------------------------------------
def backtrack(str):
	tops = ( '.', '/' )
	cldir=os.getcwd()

	while cldir not in tops:
		if os.path.exists(os.path.join(cldir, str)):
			break
		cldir=os.path.dirname(cldir)
	return cldir


#-----------------------------------------------------------------------------------------------------------------------
# Recusive get all directories in folder, and excludes some folders and files by theirs name
#-----------------------------------------------------------------------------------------------------------------------
def RecusiveGetDirs(self, abs_path_dir, exclude_folders = []):
    glob_folders = []
    lst = [ name for name in os.listdir(abs_path_dir) if os.path.isdir(os.path.join(abs_path_dir, name)) and name[0] != '.' and exclude_folders.count(name) == 0 ]
    lst.sort()
    # resuive get folder
    for dir in lst:
        sub_dir = os.path.join(abs_path_dir, dir)
        glob_folders.append(sub_dir)
        glob_folders += RecusiveGetDirs(self, sub_dir, exclude_folders)
    return glob_folders

#-----------------------------------------------------------------------------------------------------------------------
# Recusive get all files in folder, and excludes some folders and files by theirs name
#-----------------------------------------------------------------------------------------------------------------------
def RecusiveGlobFiles(self, abs_path_dir, glob_list, exclude_folders = [], exclude_files = []):
    glob_files = []
    lst = [ name for name in os.listdir(abs_path_dir) if os.path.isdir(os.path.join(abs_path_dir, name)) and name[0] != '.' and exclude_folders.count(name) == 0 ]
    lst.sort()
    # get all files in current folder
    for glob in glob_list:
        globstr = os.path.join(abs_path_dir, glob)
        folder_files = Glob(globstr)
        # exclude files from exclude file list
        file_list = [ name for name in folder_files if os.path.isdir(name.abspath) == False and exclude_files.count(name.name) == 0 ]
        # add files to file list
        glob_files += file_list
    # recusive get files in sub folder
    for dir in lst:
        sub_dir = os.path.join(abs_path_dir, dir)
        glob_files += RecusiveGlobFiles(self, sub_dir, glob_list, exclude_folders, exclude_files)
    return glob_files

# Initialize
topdir = backtrace('Sconstruct')
output_root_dir = os.path.join(topdir, 'output')
bin_dir = os.path.join(output_root_dir, 'bin')
libs_dir = os.path.join(output_root_dir, 'libs')

AddMethod(Environment, RecusiveGetDirs)
AddMethod(Environment, RecusiveGlobFiles)


