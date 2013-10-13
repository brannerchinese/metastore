## Usage Notes

  1. Used with `git`, allows user to store file metadata before a commit and then apply it after a pull. Without such a tool, `git` simply overwrites file metadata, which the user may wish to retain.
  1. Compiles straightforwardly using `make` on OS 10.8.5. `./metadata` then returns a concise description of usage:

        Usage: ./metastore ACTION [OPTION...] [PATH...]
        
        Where ACTION is one of:
          -c, --compare	Show differences between stored and real metadata
          -s, --save	Save current metadata
          -a, --apply	Apply stored metadata
          -h, --help	Help message (this text)
        
        Valid OPTIONS are:
          -v, --verbose	Print more verbose messages
          -q, --quiet	Print less verbose messages
          -m, --mtime	Also take mtime into account for diff or apply
          -e, --empty-dirs	Recreate missing empty directories (experimental)
          -f, --file   <file>	Set metadata file

  1. `example/` contains two sample scripts for use before commits and in place of `git pull`.
  1. Is there fuller documentation somewhere?

## Original README content

metastore stores or restores metadata (owner, group, permissions, xattrs and optionally mtime) for a filesystem tree.

See the manpage (metastore.1) for more details.

The file format of the .metastore files is as follows:

### Data types

~~~
CSTRING = NULL-terminated binary string
BSTRING(N) = binary string of length N
INT(N) = N byte integer in big endian byte order
~~~


### File format

~~~
HEADER
N * ENTRY
~~~


### HEADER format

~~~
BSTRING(10) - Magic header - "MeTaSt00r3"
BSTRING(8)  - Version - "\0\0\0\0\0\0\0\0" (currently)
~~~


### ENTRY format

~~~
CSTRING - Path (absolute or relative)
CSTRING - Owner (owner name, not uid)
CSTRING - Group (group name, not gid)

INT(8)  - Mtime (seconds)
INT(8)  - Mtime (nanoseconds)
INT(2)  - Mode (st_mode from struct stat st_mode AND 0177777,
                i.e. unix permissions and type of file)

INT(4)  - num_xattrs
FOR (i = 0; i < num_xattrs; i++) {
    CSTRING           - xattr name
    INT(4)            - xattrlen
    BSTRING(xattrlen) - xattr value
}
~~~

## Other notes

Original repo (git://git.hardeman.nu/metastore.git) has not been accessible on 20131012-13.

[end]
