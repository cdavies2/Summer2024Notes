# shutil
* The shutil module offers several high-level operations on files and collections of files. In particular, functions are provided which support file copying and removal.
## Directory and Files Operations
* shutil.copyfileobj(fsrc,fdst[,length])-copy the contents of the file-like object fsrc to the file-like object fdst. The integer length is the buffer size.
* shutil.copyfile(src, dst, *, follow_symlinks=True)-copy the contents (no metadata) of the file named src to a file named dst and return dst. dst must be the complete target file name, if src and dst specify the same file, SameFileError is raised.
* shutil.copy(src, dst, *, follow_symlinks=True)-copies the file src to the file or directory dst.src and dst should be path-like objects or strings. If dst specifies a directory, the file will be copied into dst using the base filename from src. copy() copies the file data and the file's permission mode. Other metadata, like file creation and modification times, is not preserved. To preserve all metadata, use copy2().
* shutil.copytree(src, dst, symlinks=False, ignore=None, copy_function=copy2, ignore_dangling_symlinks=False, dirs_exist_ok=False)-recursively copy an entire directory tree rooted at src to a directory named dst and return the destination directory. All immediate directories needed to contain dst will also be created by default. If symlinks is true, symbolic links in the source tree are represented as symbolic links in the new tree and the metadata of the original links will be copied as the platform allows.
* shutil.move(src, dst, copy_function=copy2)-recursively move a file or directory (src) to another location and return the destination. If dst is an existing directory or a symlink to a directory, then src is moved inside that directory. If the destination is on the current filesystem, then os.rename() is used. Otherwise, src is copied to the destination using copy_function and then removed. The default copy_function is copy2(), but if the course is a directory, copytree() is called.

# Os.path Functions
* os.path.abspath(path)-return a normalized absolutized version of the pathname path.
* os.path.realpath(path, *, strict=False)-return the canonical path of the specified filename, eliminating any symbolic links encountered in the path (if they are supported by the operating system).
* os.path.relpath(path, start=os.curdir)-return a relative filepath to path either from the current directory or from an optional start directory. This is a path computation, the filesystem is not accessed to confirm the existence or nature of path or start. Start defaults to os.curdir. On Windows, ValueError is raised when path and start are on different drives.
  * Example
  * path="/home/user/Desktop/file.txt"
  * start="/home/User"
  * relative_path=os.path.relpath(path, start) #this computes the relative file path to the given path from the given start directory.
