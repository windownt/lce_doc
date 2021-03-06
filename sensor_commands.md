***[Back to documentation root](README.md)***

# Sensor Commands

Sensor commands are commands that can be sent to the sensor (through the backend as an intermediary) to do various things.
These commands may rely on various collectors that may or may not be enabled on the sensor (see [Profiles](profiles.md) 
for instructions on enabling and disabling collectors).

Below is a high level listing of available commands and their purpose. Commands are sent like a command line interface
and may have positional or optional parameters. To get exact usage, either do a `GET` to the REST interface's `/tasks`
endpoint or issue the RPC `get_help` to the `c2/taskingproxy` category.

## Files and Directories

### file_get
Retrieve a file from the sensor.

```
usage: file_get [-h] file

positional arguments:
  file        file path to file to get

optional arguments:
  -h, --help  show this help message and exit
```

### file_info
Get file information, timestamps, sizes etc.

```
usage: file_info [-h] file

positional arguments:
  file        file path to file to get info on

optional arguments:
  -h, --help  show this help message and exit
```

### dir_list
List the contents of a directory.

```
usage: dir_list [-h] [-d DEPTH] rootDir fileExp

positional arguments:
  rootDir               the root directory where to begin the listing from
  fileExp               a file name expression supporting basic wildcards like
                        * and ?

optional arguments:
  -h, --help            show this help message and exit
  -d DEPTH, --depth DEPTH
                        optional maximum depth of the listing, defaults to a
                        single level
```

### file_del
Delete a file from the sensor.

```
usage: file_del [-h] file

positional arguments:
  file        file path to delete

optional arguments:
  -h, --help  show this help message and exit
```

### file_mov
Move / rename a file on the sensor.

```
usage: file_mov [-h] srcFile dstFile

positional arguments:
  srcFile     source file path
  dstFile     destination file path

optional arguments:
  -h, --help  show this help message and exit
```

### file_hash
Compute the hash of the file on the sensor.

```
usage: file_hash [-h] file

positional arguments:
  file        file path to hash

optional arguments:
  -h, --help  show this help message and exit
```

## Memory

### mem_map
Display the map of memory pages from a process including size, access rights etc.

```
usage: mem_map [-h] [-p PID] [-a PROCESSATOM]

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     pid of the process to get the map from
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target proces
```

### mem_read
Retrieve a chunk of memory from a process given a base address and size.

```
usage: mem_read [-h] [-p PID] [-a PROCESSATOM] baseAddr memSize

positional arguments:
  baseAddr              base address to read from, in HEX FORMAT
  memSize               number of bytes to read, in HEX FORMAT

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     pid of the process to get the map from
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target process
```

### mem_handles
List all open handles from a process (or all) on Windows.

```
usage: mem_handles [-h] [-p PID] [-a PROCESSATOM]

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     pid of the process to get the handles from, 0 for all
                        processes
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target process
```

### mem_strings
List strings from a process' memory.

```
usage: mem_strings [-h] [-p PID] [-a PROCESSATOM]

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     pid of the process to get the strings from
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target process
```


### mem_find_string
Find specific strings in memory.

```
usage: mem_find_string [-h] -s [STRINGS [STRINGS ...]] pid

positional arguments:
  pid                   pid of the process to search in

optional arguments:
  -h, --help            show this help message and exit
  -s [STRINGS [STRINGS ...]], --strings [STRINGS [STRINGS ...]]
                        list of strings to look for
```

### mem_find_handle
Find specific open handles in memory on Windows.

```
usage: mem_find_handle [-h] needle

positional arguments:
  needle      substring of the handle names to get

optional arguments:
  -h, --help  show this help message and exit
```

## OS

### os_services
List all services (Windows, launchctl on MacOS and initd on Linux).

```
usage: os_services [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### os_drivers
List all drivers on Windows.

```
usage: os_drivers [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### os_kill_process
Kill a process running on the sensor.

```
usage: os_kill_process [-h] [-p PID] [-a PROCESSATOM]

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     pid of the process to kill
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target process
```

### os_suspend
Suspend a process running on the sensor.

```
usage: os_suspend [-h] [-p PID] [-a PROCESSATOM] [-t TID]

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     process id
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target process
  -t TID, --tid TID     thread id
```

### os_resume
Resume execution of a process on the sensor.

```
usage: os_resume [-h] [-p PID] [-a PROCESSATOM] [-t TID]

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     process id
  -a PROCESSATOM, --processatom PROCESSATOM
                        the atom of the target process
  -t TID, --tid TID     thread id
```

### os_processes
List all running processes on the sensor.

```
usage: os_processes [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### os_autoruns
List pieces of code executing at startup, similar to SysInternals autoruns.

```
usage: os_autoruns [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### os_version
Get detailed OS information on the sensor.

```
usage: os_version [-h]

optional arguments:
  -h, --help  show this help message and exit
```


## Anomalies

### hidden_module_scan
Look for hidden modules in a process' (or all) memory. Hidden modules are DLLs or dylibs loaded manually (not by the OS).

```
usage: hidden_module_scan [-h] pid

positional arguments:
  pid         pid of the process to scan, or "-1" for ALL processes

optional arguments:
  -h, --help  show this help message and exit
```

### exec_oob_scan
Look for threads executing code outside bounds of known modules in memory. Various process injection methodologies.

```
usage: exec_oob_scan [-h] pid

positional arguments:
  pid         pid of the process to scan, or "-1" for ALL processes

optional arguments:
  -h, --help  show this help message and exit
```

### hollowed_module_scan
Look for signs of process/module hollowing.

```
usage: hollowed_module_scan [-h] pid

positional arguments:
  pid         pid of the process to scan

optional arguments:
  -h, --help  show this help message and exit
```

## Management

### exfil_add
Add an LC event to the list of events sent back to the backend by default.

```
usage: exfil_add [-h] -e EXPIRE event

positional arguments:
  event                 name of event to start exfiling

optional arguments:
  -h, --help            show this help message and exit
  -e EXPIRE, --expire EXPIRE
                        number of seconds before stopping exfil of event
```

### exfil_del
Remove an LC event from the list of events always sent back to the backend.

```
usage: exfil_del [-h] event

positional arguments:
  event       name of event to stop exfiling

optional arguments:
  -h, --help  show this help message and exit
```

### exfil_get
List all LC events sent back to the backend by default.

```
usage: exfil_get [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### history_dump
Send to the backend the entire contents of the sensor event cache, so detailed events of everything that happenned recently.

```
usage: history_dump [-h] [-r ROOT] [-a ATOM] [-e EVENT]

optional arguments:
  -h, --help            show this help message and exit
  -r ROOT, --rootatom ROOT
                        dump events present in the tree rooted at this atom
  -a ATOM, --atom ATOM  dump the event with this specific atom
  -e EVENT, --event EVENT
                        dump events of this type only
```

## Yara

### yara_update
Update the compiled yara signature bundle that is being used for constant memory and file scanning on the sensor.

```
usage: yara_update [-h] ruleFile

positional arguments:
  ruleFile    file holding the compiled rules to upload

optional arguments:
  -h, --help  show this help message and exit
```

### yara_scan
Scan for a specific yara signature in memory and files on the sensor.

```
usage: yara_scan [-h] [-p PID] [-f FILEPATH] [-e PROC] ruleFile

positional arguments:
  ruleFile              file holding the compiled rules to upload

optional arguments:
  -h, --help            show this help message and exit
  -p PID, --pid PID     pid of the process to scan
  -f FILEPATH, --filePath FILEPATH
                        path of the file to scan
  -e PROC, --processExpr PROC
                        expression to match on to scan (matches on full
                        process path)
```


## Documents

### doc_cache_get
Retrieve a document / file that was cached on the sensor. (List of extensions/directories cached specified in [Profiles](profiles.md))

```
usage: doc_cache_get [-h] [-f FILE_PATTERN] [-s HASHSTR]

optional arguments:
  -h, --help            show this help message and exit
  -f FILE_PATTERN, --file_pattern FILE_PATTERN
                        a pattern to match on the file path and name of the
                        document, simple wildcards ? and * are supported
  -s HASHSTR, --hash HASHSTR
                        hash of the document to get
```

## Mitigation

### deny_tree
Tells the sensor that all activity starting at a specific process (and its children) should be denied and killed. Great for ransomware mitigation.

```
usage: deny_tree [-h] atom [atom ...]

positional arguments:
  atom        atoms to deny from

optional arguments:
  -h, --help  show this help message and exit
```

### segregate_network
Tells the sensor to stop all network connectivity on the host except LC comms to the backend. So it's network isolation, great to stop lateral movement.

```
usage: segregate_network [-h]

optional arguments:
  -h, --help  show this help message and exit
```

### rejoin_network
Tells the sensor to allow network connectivity again (after it was segregated).

```
usage: rejoin_network [-h]

optional arguments:
  -h, --help  show this help message and exit
```
