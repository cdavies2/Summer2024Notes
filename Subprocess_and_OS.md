# Subprocess
* The subprocess module allows you to spawn new processes, connect to their input/output/error pipes, and obtain their return codes.
* The recommended approach to invoking subprocesses is to use the run() function for all use cases it can handle. For more advanced use cases, the underlying Popen interface can be used directly.
* subprocess.run(args, *, stdin-None, input=None, stdout=None, stderr=None, capture_output=False, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None, text=None, env=None, universal_newlines=None, **other_popen_kwargs)
  *  The command that is run is described by args.
  *  If capture_output is true, stdout and stderr will be captured.
  *  When used, the internal Popen object is automatically created with stdout and stderr both set to PIPE (indicating a pipe to the standard stream should be opened).
  *  If you wish to capture stdout and stderr arguments at the same time as capture_output, set stdout to PIPE and stderr to STDOUT.
  *  If check is true, and the process exits with a non-zero exit code, a CalledProcessError execption will be raised.
  *  If encoding or errors are specified, or text is true, file objects for stdin, stdout, and stderr are opened in textmode using specified encoding and errors or the io.TextIOWrapper default. By default, file objects are opened in binary mode.
  *  If env isn't None, it must be a mapping that defines the environment variables for the new process; they are used instead of the default behavior of inheriting the current process' environment. The mapping can be str to str on any platform or bytes to bytes on POSIX platforms like os.eviron or os.environb.
## Important Terms
* class subprocess.CompletedProcess-the return value from run(), representing a process that has finished
* args-the arguments used to launch the process. This can be a list or a string.
* returncode-exit status of the child process, an exit status of 0 indicates that it can successfully.
* stdout-captured output from the child process. A bytes sequence, or a string if run() was called with an encoding, errors, or text-True.
* stderr-Captured stderr from the child process. A bytes sequence, or a string if run() was called with an encoding, errors, or text=True. None if stderr was not captured
* check_returncode()-if returncode is non-zero, raise a CalledProcessError.
* subprocess.DEVNULL-special value that can be used as the stdin, stdout, or stderr argument to Popen and indicates that the special file os.devnull will be used.
* cmd-command that was used to spawn the child process
* timeout-Timeout in seconds
* output-output of the child process if it was captured by run() or check_output(). OTherise none. This is always bytes regardless of the text=True setting.
## Frequently Used Arguments
* args is required for all calls and should be a string, or a sequence of program arguments. Providing a sequence of arguments is preferred, as it allows the module to take care of any required escaping and quoting of arguments. If passing a single string, either shell=True or the string must simply name the program to be executed without specifying any arguments.
* stdin, stdout, and stderr specify the executed program's standard input, standard output and standard error file hanfles, respectively.
* If encoding or errors are specified, or text is true, stdin, stdout, and stderr will be opened in text mode using the encoding and errors specified in the call or the defaults for io.TextIOWrapper.
* For stdin, line ending characters '\n' in the input will be converted to the default line separator os.linesep. For stdout and stderr, all line endings in the output will be converted to '\n'. For more information see the documentation of the io.TextIOWrapper class when the newline argument to its constructor is None.
* If shell is True, the specified command will be executed through the shell.
## Popen Constructor
* The Popen class offers a lot of flexibility so developers are able to handle the less common cases not covered by the convenience functions.
* class subprocess.Popen(args, bufsize=-1, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=True, shell=False, cwd=None, env=None, universal_newlines=None, startupinfo=None, creationflags=0, restore_signals=True, start_new_session=False, pass_fds=(), *, group=None, extra_groups=None, user=None, umask=-1, encoding=None, errors=None, text=None, pipesize=-1, process_group=None)
* args should be a sequence of program arguments or else a single string or path-like object. By default, the program to execute is the first item in args if args is a sequence. If args is a string, the interpretation is platform-dependent and described below. Unless otherwise stated, it's recommendded to pass args as a sequence.
* An example of passing some arguments to an external program as a sequence is...
  * Popen(["/usr/bin/git", "commit", "-m", "Fixes a bug."])
  * It may not be obvious how to break a shell command into a sequence of arguments, especially in complex cases. shlex.split() can illustrate how to determine the correct tokenization for args.
    *  import shlex, subprocess
    *  command_line = input()
    *  /bin/vikings -input eggs.txt -output "spam spam.txt" -cmd "echo '$MONEY'"
    *  args = shlex.split(command_line)
    *  print(args)
    *  ['/bin/vikings', '-input', 'eggs.txt', '-output', 'spam spam.txt', '-cmd', "echo $MONEY'"]
    *  p = subprocess.Popen(args) # Success!
* On POSIX with shell=True, the shell defaults to /bin/sh. If args is a string, the string specifies the command to execute through the shell. This means the string must be formatted exactly as it would be when typed at the shell prompt. This includes quoting or backslash escaping filenames filenames with spaces in them.
* If args is a sequence, the first item specifies the command string, and any additional items will be treated as additional arguments to the shell itself.
  * Popen(['/bin/sh', '-c', args[0], args[1],...])
  *  On Windows with shell=True, the COMSPEC environment variable specifies the default shell. The only time you need to specify shell=True on Windows is when the command you wish to execute is built into the shell (dir or copy). You don't need shell=True to run a batch file or console-based executable.
  *  On POSIX, the args name becomes the display name for the executable in utilities such as ps. If shell=True, on POSIX the executable argument specifies a replacement shell for the default /bin/sh.
  *  If preexec_fn is set to a callable object, the object will be called in the child process just before the child is executed (POSIX only).
  *  If close_fds is true, all file descriptors except 0, 1, and 2 will be closed before the child process is executed. Otherwise when close_fds is false, file descriptors obey their inheritable flag.
  *  If cwd is not None, the function changes the working directory to cwd before executing the child. On POSIX, the function looks for executable (or for the first item in args) relative to cwd if the executable path is a relative path.
  *  If user is not None, the setreuid() system call will be made in the child process prior to the execution of the subprocess. If the provided value is a string, it'll be looked up via pwd.getpwnam() and the value in pw_uid will be used. If the value is an integer, it will be passed verbatim (POSIX only).
  *  The universal_newlines argument is equivalent to text and is provided for backwards compatibility.
## Exceptions
* The most common exception raised is OSError, which could occur when trying to execute a non-existent file. Applications should prepare for OSError exceptions. When shell=True, OSError will be raised by the child only if the selected shell itself was not found.
* A ValueError will be raised if Popen is called with invalid arguments.
* check_call() and check_output() will raise CalledProcessError if the called process returns a non-zero return code.
* Exceptions defined in this module all inherit from Subprocess error.
## Popen Objects
* Popen.poll()- check if child process has terminated.
* Popen.wait(timeout=None)-wait for child process to terminate. If the process doesn't terminate after timeout seconds, raise a TimeoutExpired exception. It is safe to catch this exception and retry the wait.
* Popen.communicate(input=None, timeout=None)-interact with process:
    * Send data to stdin
    * Read data from stdout and stderr, until end-of-file is reached.
    * Wait for process to terminate and set the returncode attribute
    * communicate() returns a tuple (stdout_data, stderr_data). The data will be strings if streams were opened in text mode; otherwise, bytes.
*   Popen.terminate()-stop the child
*   Popen.kill()-kills the child
*   Popen.args-the args argument as it was passed to Popen
*   Popen.stdin/Popen.stdout/Popen.stderr-if the argument was PIPE, this attribute is a writeable stream object as returned by open(). If the text or universal_newlines argument was True, the stream is a text stream, otherwise it's a byte stream.

# OS
* Provides a portable way of using operating system dependent functionality.
* os.name-the name of the operating system dependent module imported. The following names have currently been registered: 'Posix', 'nt', 'java'.
## Process Parameters
* os.environ-a mapping object where keys and values are strings that represent the process environment. environ['HOME'] is the pathname of your home directory, for instance.
   * This mapping is captured the first time the os module is imported, typically during Python startup. This mapping may be used to modify or query the environment.
 * os.getenv(key, default=None)-return the value of the environment variable key as a string if it exists, or default if it doesn't (Availability: Unix, Windows).
 * os.getcwd()-returns a string representing the current working directory.
## Pathname Manipulations
* os.path.abspath(path)-returns a normalized absolutized version of the pathname path
* os.path.dirname(path)-return the directory name of pathname path. This is the first element of the pair returned by passing path to the function split().
* os.path.exists(path)-returns True if path refers to an existing path or an open file descriptor. Returns False for broken symbolic links.
* os.path.isabs(path)-return True if path is an absolute pathname (on Unix, it begins with a slash, on Windows it begins with a backslash after chopping off a potential drive letter).
* os.path.isfile(path)-return True is path is an existing regular file.
* os.path.islink(path)-return True if path refers to an existing directory entry that is a symbolic link. Always False if symbolix links aren't supported by the Python runtime.
* os.path.join(path, *paths)-join one or more paths intelligently. The return value is the concatenation of path and all members of *paths, with exactly one directory separator following each non-empty part, except the last. The result will only end in a separator if the last part is either empty or ends in a separator.
  * If a segment is an absolute path, then all previous segments are ignored and joining continues from the absolute path segment.
  * On Windows, the drive is not reset when a rooted path segment (e.g., r'\foo') is encountered. IF a segment is on a different drive or is an absolute path, all previous segments are ignored and the drive is reset. 
