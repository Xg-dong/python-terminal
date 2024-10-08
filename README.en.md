# py-terminal

## Describes

py-terminal integrates local and remote terminal operations.

# Install

```bash
pip install PyCmdTerminal

```

## Example

Tip: To close() the end of the process, please use the close() method to ensure that the open resources are released and to ensure that the current process is terminated normally, it is recommended to use the context manager (`with`);
     Do not call close in destructor (__del__), which may lead to uncontrollable results.

```python
from py_terminal import LocalTerminal
from py_terminal import RemoteTerminal

with LocalTerminal() as terminal:
    terminal.get_os()

with RemoteTerminal(host="xx.xx.xx.xx", username="user") as r_terminal:
    r_terminal.get_os()

```

# API

## <a id="base-terminal">Class ` _Terminal`</a>

#### <a id="base-terminal-parm">Parameter</a>

- <h3>logger (Optional)</h3>

  The logger instance used to log messages (the default is the logger named "RemoteTerminal").


- <h3>log_level (Optional)</h3>

  Logging level (default is `NOTSET`).

#### <a id="base-terminal-method">Methods</a>

- <h3>execute_command(command: `str`, timeout: `Optional[int]`, password: `Optional[str]`) -> `dict`</h3>

  Executes the command and returns detailed information about the execution. blocking

  `command` (Required)': the command to be executed.

  `timeout` (Optional)': the timeout of command execution, in seconds (default is `None`).

  `password` (Optional)': the password used for authentication (default is `None`).


- <a id="base-asyncexecute-command"><h3>async_execute_command(command: `str`) -> `List[int]`</h3></a>

  Executes a command without waiting for it to complete, and returns the process and subprocess ID of the command. Non-blocking

  `command (Required)`：Command to be executed.


- <h3>get_sync_process_output(pid: `int`, _callable_f: `Callable[[bytes, bytes], None]`, wait: `bool`) -> `Union[Tuple[AsyncThread, asyncio.Event], None]`</h3>

  Get the standard output and standard error of the given [asynchronous execution ](#base-asyncexecute-command) process.

  `pid (Required)`：Process PID, which is used to obtain the output of the process.

  `callable_f (可选）`：Callback function that handles standard output and standard error. By default, `print` output is used.

  `wait (可选）`：Whether to wait for the process to complete the output synchronously. If `True`, the output of the waiting process is blocked; If `False`, the output processing of the process is executed asynchronously. (default is `True`)


- <h3>read(path: `str`, mode:`str`, decode="utf-8", chunk_size: `int`, position: `int`) -> `Union[Iterator[Any], str, Generator[str, Any, Any]]`</h3>

  Read the contents of the file under the specified path.
  This method reads files according to the values of Block Size and Mode. If `chunk_size` is -1, the whole file is read.
  If `chunk_size` is greater than -1, it will read the file content block by block and return an iterator to read it block by block.
  The Mode parameter determines how the contents of the file are interpreted.

  `path (Required)`：The path of the file to read.

  `mode (Optional)`：The mode of interpreting the contents of the file. The default is "r" (text mode). Use "rb" to represent binary mode.

  `decode (Optional)`：Decode is the encoding used to decode files.

  `chunk_size (Optional)`：The size of each block to be read. The default is -1, which means reading the entire file. If it is greater than -1, it is read in blocks.

  `position (Optional)`：The starting position to read from the file. The default is 0, which means starting from the beginning of the file.


- <h3>write(path: `str`, data: `Union[str, bytes]`, mode: `str`) -> `None`</h3>

  Writes data to a file at the specified path.

  `path (Required)`：The path of the file to be written.

  `data (Required)`：The data to be written to the file. It should be a string for text mode and a byte for binary mode.

  `mode (Optional)`：The mode of opening the file. The default is "w" (write mode). Use "wb" for binary write mode, "A" for append mode, and so on.


- <h3>delete(path: `str`) -> `bool`</h3>

  Delete the file or directory at the specified path.
  If the path does not exist, a "FileNotFoundError" will be thrown.
  If any other errors occur during the deletion process, a "RuntimeError" will be raised.

  `path (Required)`：The path of the file or directory to delete.


- <h3>open_file(path: `str`, mode: `str`) -> `Union[BinaryIO, TextIO]`</h3>

  Opens a file based on the specified mode. Return file object

  `path (Required)`：The path to the file to open.

  `sftp (Optional)`：The mode of opening the file. The default is "r" (text mode). Supported patterns include "R", "W", "X", "A", "B", "T" and their combinations with "+". (default is `r`)


- <h3>get_size(path: `str`, sftp) -> `int`</h3>

  Gets the size of the path.
  If it is a file, return the file size.
  If it is a directory, recursively calculate the total size of all files in the directory.

  `path (Required)`：The path of the file or directory.

  `sftp (Optional)`：SFTP client instance for interacting with the server.


- <h3>is_dir(path: `str`, sftp) -> `bool`</h3>

  Check whether the specified path is a directory.

  `path (Required)`：The file or directory path to check.

  `sftp (Optional)`：SFTP client instance for interacting with the server.


- <h3>is_file(path: `str`, sftp) -> `bool`</h3>

  Check whether the specified path is a file.

  `path (Required)`：The file or directory path to check.

  `sftp (Optional)`：SFTP client instance for interacting with the server.


- <h3>is_link(path: `str`, sftp) -> `bool`</h3>

  Used to check whether a given path is a symbolic (symlink).

  `path (Required)`：The file or directory path to check.

  `sftp (Optional)`：SFTP client instance for interacting with the server.


- <h3>path_exists(path: `str`, sftp) -> `bool`</h3>

  Checks whether the specified path exists.

  `path (Required)`：The file or directory path to check.

  `sftp (Optional)`：SFTP client instance for interacting with the server.


- <h3>pid_exists(pid: `int`) -> `bool`</h3>

  Checks whether the specified process ID exists.

  `pid (Required)`：ID of the process to check.


- <h3>process_exists(identifier: `Union[str, int]`) -> `bool`</h3>

  Use PID or process name to check whether it is running.

  `identifier (Required)`：The process ID (PID) or process name to check.


- <h3>get_pname_by_pid(pid: `int`) -> `Optional[str]`</h3>

  Retrieves the process name associated with the specified process ID.

  `pid (Required)`：Process ID.


- <h3>get_pids_by_pname(pname: `str`) -> `List[int]`</h3>

  Retrieves all process ids associated with the specified process name.

  `pname (Required)`：The name of the process that retrieves the id.


- <h3>kill(process_identifier: `Optional[Union[int, str]]`, force: `bool`, timeout: `int`, kill_all: `bool`) -> `bool`</h3>

  Terminate a process on a remote server by PID or process name.

  `process_identifier (Required)`： The PID of the process to be terminated or the name of the process.

  `force (Optional)`： If it's `True`, if 'SIGTERM' doesn't work, upgrade to Force Kill ('SIGKILL') (default is `False`).

  `timeout (Optional)`： The time (seconds) to wait for the process to terminate after sending a SIGTERM (default is 5), timeouts throw an error.

  `kill_all (Optional)`： If true, all processes that match the process name are terminated. If False, only a single process is terminated and an error is thrown when multiple PIDs are found (default is False).


- <h3>command_exists(cmd: `str`) -> `bool`</h3>

  Tell if a given command exists.

  `cmd (Required)`：Indicates the name of the command you want to check (e.g., ls, python, git, etc.).


- <h3>get_os() -> `str`</h3>

  Get the operating system of the machine.


- <h3>close()</h3>

  Close open resources, including output threads, coroutines, and open file resources that execute asyn


- <h3>list_dir(self, path: `str`, sftp=None) -> `List[str]`</h3>

  Returns a list of the names of all files and directories in the specified directory.

  `path (Required)`: The path of the directory to list the contents. This parameter is optional and defaults to the current directory (.).


- <h3>mkdir(self, path: `str`, mode=0o777, exist_ok=False, sftp=None)</h3>

  Create a single directory.

  `path (Required)`: The path of the directory to create. Can be a relative path or an absolute path.

  `mode (Optional)`: used to specify the permission to create a directory; the default is 0o777. Note that this permission may not work on some platforms.

  `exist_ok (Optional)`: If set to True, no exception will be thrown when the target directory already exists; If it is False and the directory already exists, a FileExistsError is thrown.


- <h3>makedirs(self, path: str, mode=0o777, exist_ok=False, sftp=None)</h3>

  Recursively create multilevel directories.

  `path (Required)`: The path of the directory to create. Can be a relative path or an absolute path.

  `mode (Optional)`: used to specify the permission to create a directory; the default is 0o777. Note that this permission may not work on some platforms.

  `exist_ok (Optional)`: If set to True, no exception will be thrown when the target directory already exists; If it is False and the directory already exists, a FileExistsError is thrown.


- <h3>get_subprocess_pids(pid: `int`) -> `List[int]`</h3>

  Get all subprocess ids of the specified pid.

  `pid (Required)`: ID of process.

- <h3>move(source_path: `str`, dest_path: `str`) -> `bool`</h3>

  Move a file or directory to the specified location.

  `source_path (Required)`: Source path
  `dest_path (Required)`: Target path


- <h3>copy(source_path: `str`, dest_path: `str`) -> `bool`</h3>

  Copy a file or directory to the specified location.

  `source_path (Required)`: Source path
  `dest_path (Required)`: Target path

- <h3>get_async_process_status(pid: `Union[int, Tuple[int, ...]]`) -> `Union[int, None]`</h3>

  Gets the exit status of asynchronous executors.

  `pid (Required)`：Use `async_execute_command` to execute the returned pid.

## Class `RemoteTerminal`

#### Describes

Inheritance [`paramiko. SSHClient`](https://docs.paramiko.org/en/latest/api/client.html#paramiko.client.SSHClient) and [`_Terminal(base class)`](#base-terminal) classes to implement terminal operations on remote devices

#### Parameter

Inherit all parameters from [`_Terminal(base class)`](#base-terminal-parm)

- <h3>host (Required)</h3>

  The hostname or IP address of the SSH server.


- <h3>username (Required)</h3>

  The username used for authentication.


- <h3>password (Optional)</h3>

  The password for authentication (default `None`).


- <h3>port (Optional)</h3>

  Port number of SSH connection (default is `22`).


- <h3>try_retry_password (Optional)</h3>

  Whether to retry password input when it fails. (default is `False`).


- <h3>try_retry_password_times (Optional)</h3>

  Maximum number of retries for password input (default is `3`).


- <h3>use_terminal_input_password (Optional)</h3>

  Whether to allow the terminal to enter the password (default is `False`).


- <h3>terminal_password_show (Optional)</h3>

  Whether to display the password when typing in the terminal (default is `False`).


- <h3>timeout (Optional)</h3>

  Timeout for connecting to the remote (seconds) (default is `10`).


- <h3>allow_agent: (Optional)</h3>

  Whether the SSH proxy is allowed (default is `True`).


- <h3>look_for_keys (Optional)</h3>

  Whether to search for discoverable private key files (default is `True`).


- <h3>key_filename (Optional)</h3>

  File name or file name list of private key (default is `None`).


- <h3>host_key_policy (Optional)</h3>

  Customize the host key policy (default is `WarningPolicy`).


- <h3>known_hosts_file (Optional)</h3>

  The path of the known host file used for host key verification (default is `None`).

#### Methods

Inherit the parent class [`paramiko.SSHClient`](https://docs.paramiko.org/en/latest/api/client.html#paramiko.client.SSHClient) and [`_Terminal(基类)`](#base-terminal-method) All methods

- <h3>check_connect() -> `bool`</h3>

  Check whether the SSH connection is active by executing a simple command.


- <h3>push(local_path: `str`, remote_path: `str`) -> `bool`</h3>

  Upload a file or directory from a local system to a remote server, including metadata.

  `local_path (Required)`：The path to the local file or directory.

  `remote_path (Required)`：The path where the file or directory is saved on the remote server.


- <h3>pull(remote_path: str, local_path: str) -> `bool`</h3>

  Download a file or directory from a remote server to the local system, including metadata.

  `remote_path (Required)`：The path to the remote file or directory.

  `local_path (Required)`：The path to save the file or directory locally.


- <h3>async_execute_command(command: `str`) -> `Optional[int]`</h3>

  Executes a command on a remote server without waiting for it to complete, and returns the process ID of the command. [*Overload Base Class Method*](#base-asyncexecute-command); Note: Windows is not supported

  `command (Required)`：Commands to be executed on the remote server.

- <h3>reboot(force: `bool`, wait_time: `int`, max_retries: `int`) -> `bool`</h3>

  Restart the remote server and wait for the connection. By default, it tries to do a normal reboot with "reboot" or "shutdown -r now".

  `force (Optional)`：If it's `True`, restart with a more forced method (if available) (default is `False`).

  `wait_time (Optional)`：Time to wait before trying to reconnect after reboot (seconds) (default is `10`)

  `max_retries (Optional)`：The maximum number of times a reconnection is attempted after a new start. (Default is `10`)


- <h3>get_transport() -> `Transport`</h3>

  Get a custom SSH transfer.


- <h3>reconnect(timeout: `str`=None) -> `bool`</h3>

  Reconnect to the remote.

  `timeout`： Set the connection timeout, The length of time when the instance is created by default.

## Class `LocalTerminal`

#### Describes

Inherit the [`_Terminal(base class)`](#base-terminal) class to implement local terminal operations and basic operations

#### Parameter

Inherit all parameters from [`_Terminal(base class)`](#base-terminal-parm).

#### Methods

Inherit all methods from [`_Terminaur (base class)`](#base-terminal-method).

