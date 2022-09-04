bitscrub
===

bitscrub walks a directory tree and writes/updates/verifies the CRC32C of each file, stored a xattr.  This is useful for detecting bitrot on filesystems with no data checksum functionality.

`(time_marked, mtime, checksum)` data is stored in the `user._C` xattr.

Modified
---

Modifed (jpl) to use zlib.crc32, which appears to work on arm. 
Previously was using pycrc32c library, which called a sse4 instruction.

Basic usage
---

Write new checksums and update checksums where file has mtime != checksum time:

    bitscrub -w ~/

Verify checksums only:

    bitscrub -v ~/

`-v` will print messages starting with `CORRUPT` if the CRC32C is up to date but does not match the file contents.

Write/update checksums and verify existing checksums:

    bitscrub -vw ~/

Inspect checksum xattr:

    bitscrub -i FILENAME

Specify an exclude file, which should contain folders to exclude separated by whitespace (e.g. one per line): 

    bitscrub -vw -e ~/myexcludefile ~/ 

Typical usage 
---

This prints any files that are modified or corrupt, updates their checksums,
and silently adds checksums to any new files). Look in partciular for "CORRUPT".

    bitscrub -vwq ~/

--help
---

```
# bitscrub --help
usage: scan.py [-h] [-v] [-w] [-i] [-q] [-l LISTING] [-n] PATH [PATH ...]

Walks a directory tree and reads and/or writes the CRC32C of each file to a
xattr "user._C". Useful for detecting bitrot. --verify, --write, and --inspect
can be combined. If none of these are specified, files will be checked only
for lack of checksum data or updated mtime.

positional arguments:
  PATH                  a file or directory

optional arguments:
  -h, --help            show this help message and exit
  -v, --verify          verify already-stored checksums to detect file
                        corruption
  -w, --write           calculate and write checksums for files that have no
                        checksum, or have an updated mtime
  -i, --inspect         print information about existing checksum data
  -q, --quiet           don't print both important and unimportant messages to
                        stderr; still print important messages to stdout
  -l LISTING, --listing LISTING
                        generate a file listing into this file (columns:
                        dentry type, CRC32C, mtime, size, filename)
  -n, --normalize-listing
                        print relative path
```
