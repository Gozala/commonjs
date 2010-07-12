---
layout: default
title: Filesystem/A
status: proposal
---
---

# Filesystem/A

**STATUS: PROPOSAL, DISCUSSION** 

Implementations

[Narwhal on Rhino][5] (draft 4 in 0.1), [Narwhal on Node][6] (0.5), [RingoJS][7] (minus globbing) 

Proposal A, Draft 6 

## Contents[

[hide][8]]

*   [1 Overview][9] 
    *   [1.1 Security][10]
    *   [1.2 Regarding Paths][11]
*   [2 Specification][12] 
    *   [2.1 Files][13]
    *   [2.2 Directories][14] 
        *   [2.2.1 Listing][15]
        *   [2.2.2 Globbing][16]
        *   [2.2.3 Path Objects][17]
        *   [2.2.4 Iterator Objects][18]
    *   [2.3 Links][19]
    *   [2.4 Tests][20]
    *   [2.5 Attributes][21]
    *   [2.6 Extended Attributes (optional)][22]
    *   [2.7 Security][23]
    *   [2.8 Paths][24] 
        *   [2.8.1 Paths as Text][25]
        *   [2.8.2 Path Type][26]
*   [3 Notes][27] 
    *   [3.1 For Future Versions][28]
*   [4 Relevant Discussions][29]

#  Overview 

The "fs" module provides a file system API for the manipulation of paths, directories, files, links, and the construction of file streams. The [IO stream API][30] is beyond the scope of this specification. 

*Note: Other objects, apart from the "fs" module exports, may claim to conform to this specification.* 

This specification builds on the normal primitives exported by "fs-base" specified in [Filesystem/A/0][31]. All methods implemented by "fs-base" must be exported by and override methods of this module. 

##  Security 

Objects implementing the File System API, including the "file" module object, are capability bearing objects that carry and mediate authority to read and write to the underlying storage. As such, the "fs" module can return other objects that implement and attenuate the File System API for sandboxing. Furthermore, streams returned by the file system object are implicitly attenuated to only give the receiver authority to manipulate the given file, without knowledge of the path on which it resides or access to references that would permit it to manipulate other parts of the file system. 

##  Regarding Paths 

Many methods accept paths as arguments. In every case, the path may be any object that is coercible to a `String` with the `String` constructor, that defers to `toString` for most objects. This includes `Path` objects. 

#  Specification 

##  Files 

**`open(path, mode|options)`** 

returns an [IO][30] stream that supports an appropriate interface for the
given options and mode, which include reading, writing, updating, byte, 
character, unbuffered, buffered, and line buffered streams. 

*   `path`: any value coercible to a String, including a Path, that can be interpreted as a fully-qualified path, or a path relative to the current working directory. 
*   `mode` String: any subtring of "rwa+bxc" meaning "read", "write", "append", "update", "binary", and "exclusive" flags respectively, in any order. 
*   `options` 
*   `mode` String: conforming to the above mentioned mode string pattern 
*   `charset` String: an IANA, case insensitive, charset name. `open` must throw a (**TODO**) error if the charset is not supported. The `ascii` and `utf-8` charsets must be supported. 
*   `read` Boolean: open for reading, do not create, set position to beginning of the file, throw an error if the file does not exist. 
*   `write` Boolean: open for writing, create or truncate, set position to the beginning of the file. 
*   `append` Boolean: open for appending, create if it doesn't exist, do not truncate, set position to end of the file. 
*   `update` Boolean: open for updating, create if it doesn't exist, do not truncate, set position to the beginning of the file. 
*   `binary` Boolean return a raw stream instead of a buffered, charset encoded, text stream. 
*   `exclusive` Boolean: open for write only if it does not already exist, otherwise throw an error. 

The "open" function mediates the construction of various kinds of streams. As
"open" is the only method with the authority to manipulate files, it
constructs these types on behalf of a potentially unpriviledged caller. Stream
constructors are not directly callable in a secure sandbox, so where and how
these stream types are implemented is beyond the necessary scope of this
specification. The "open" function always creates a byte stream, and by
default wraps that in a textual IO wrapper. "open" returns a stream resulting
from the following algorithm: 

1.  `create` a "raw" byte stream. If "x" mode (with either "w" or "a" mode), only open if the file does not already exist. Create the file and open the stream atomically. 
    *   if "r" mode, make "raw" a ByteReader 
    *   if "w" mode, make "raw" a ByteWriter 
    *   if "u" mode, make "raw" a ByteUpdater 
2.  if "+", seek to end. 
3.  if not "b" mode, return "raw". 
4.  return a wrapper encoded/decoded string stream, wrapped around a buffered stream, wrapped around "raw". 
    *   if "r" mode, wrap "raw" in a TextReader 
    *   if "w" mode, wrap "raw" in a TextWriter 
    *   if "u" mode, wrap "raw" in a TextUpdater 

**`read(path, (mode|options)_opt)`** 

opens, reads, and closes a file, returning its content. Equivalent to:  
`open(source, mode).read()`. 

**`write(path, content String|Binary, (mode|options)_opt)`** 

opens, writes, flushes, and closes a file with the given content. If the
content is a ByteArray or ByteString, the binary mode is implied. Equivalent
to:  
`open(source, mode + "w" + (content instanceof Binary&nbsp;? "b"&nbsp;: "")).write(content).flush()`
, for mode Strings. 

**`copy(source, target)`** 

reads one file and writes another in byte mode. Equivalent to:  
`open(source, "b").copy(target, "b")` 

**`rename(path, name String)`** 

Changes the name of a file at a given path. This differs from move in that the
target is relative to the source instead of the current working directory.
This method in particular should be implemented by the native "fs-base"
module overriding any pure JavaScript implementation, if possible. 

**`move`**  

[Filesystem/A/0][31] 

**`remove`**  

[Filesystem/A/0][31] 

**`touch`**  

[Filesystem/A/0][31] 

##  Directories 

**`makeDirectory`**  

[Filesystem/A/0][31] 

**`removeDirectory`**  

[Filesystem/A/0][31] 

**`makeTree(path)`** 

Creates the directory specified by "path" including any missing parent 
directories. 

**`removeTree(path)`** 

Removes whatever exists at the given path, regardless of whether it is a 
file, direcotory, or otherwise. If the path refers to a directory, but 
not a symbolic link to a directory, calls `removeTree` on each member of 
the directory. 

**`copyTree(source, target)`** 

copies files from a source path to a target path, copying the files of the
source tree to the corresponding locations relative to the target,
copying but not traversing into symbolic links to directories. 

###  Listing 

**`list(path) Array * String`** 

[Filesystem/A/0][31] 

**`listTree(path) Array * String`** 

returns an Array that starts with the given path, and all of the paths 
relative to the given path, discovered by a depth first traversal of 
every directory in any visited directory, reporting but not traversing 
symbolic links to directories, in lexically sorted order within 
directories. The first path is always "", the path relative to itself.

**`listDirectoryTree(path) Array * String`** 

returns an Array that starts with the given directory, and all the
directories relative to the given path, discovered by a depth first
traversal of every directory in any visited directory, not traversing 
symbolic links to directories, in lexically sorted order within 
directories. 

###  Globbing 

**`glob(pattern)`** 

returns an Array of all paths that match the given glob pattern from the current working directory. 

glob patterns may include asterisks for 0 or more of any character except 
directory separator matches, question marks for exactly one of any 
character except directory separator matches, non-delimited square 
bracket notation for unions and escapes, comma delimited curly brace 
notation for nestable unions of substrings, and double asterisk for 0 or 
more of any character including directory separator matches (by way of a 
depth first traversal of intervening directories, visiting but not 
following symbolic links). 

**`match(path, pattern)`** 

returns whether a path matches a given glob pattern. 

**`escape(path)`** 

returns a path or path component as a glob pattern that would only match 
the given path component. 

###  Path Objects 

For each of the previous "list" and "glob" methods, there is a corresponding "-Paths" method that returns Path object instances instead of Strings. 

**`listPaths(path) Array * Path`** 

**`listTreePaths(paths) Array * Path`** 

**`listDirectoryTreePaths(paths) Array * Path`** 

**`globPaths(path) Array * Path`** 

###  Iterator Objects 

**`iterate(path) Iterator * String`** 

returns an iterator that lazily browses a directory, backward and 
forward, for the base names of entries in that directory. 

**`iteratePaths(path) Iterator * Path`** 

returns an iterator that lazily browses a directory, backward and 
forward, for the full Paths of entries in that directory, joined on the 
given path. 

Iterator objects have the following members: 

**`next() String or Path `**

returns the next path in the iteration or throws a `StopIteration` if 
there is none. 

**`prev() String or Path `**

returns the previous path in the iteration or throws a `StopIteration` if 
there is none. 

**`iterator() `**

returns itself 

**`close() `**

closes the iteration. After calling close, all calls to next and prev 
must throw `StopIteration`. 

Directory iterators also support the following Array methods: 

*   `forEach` 
*   `map` 
*   `every` 
*   `some` 
*   `reduce` 
*   `reduceRight` 

##  Links 

Symbolic and hard links must not be emulated with Windows Shortcuts. 

**`link(source, target) `**

[Filesystem/A/0][31] 

**`hardLink(source, target) `**

[Filesystem/A/0][31]

**`readLink(path) String `**

[Filesystem/A/0][31] 

##  Tests 

**`exists(path) `**

[Filesystem/A/0][31] 

**`isFile(path) `**

[Filesystem/A/0][31] 

**`isDirectory(path) `**

[Filesystem/A/0][31] 

**`isLink(path) `**

[Filesystem/A/0][31] 

**`isReadable(path) Boolean `**

[Filesystem/A/0][31] 

**`isWritable(path) Boolean `**

[Filesystem/A/0][31] 

**`same(source, target) Boolean `**

[Filesystem/A/0][31] 

##  Attributes 

**`size(path) `**

[Filesystem/A/0][31] 

**`lastModified(path) Date `**

[Filesystem/A/0][31] 

##  Extended Attributes (optional) 

Extended attribute methods may be defined on systems that may support the feature on some volumes. 

**`getAttribute(path, key String, default ByteString) ByteString `**

[Filesystem/A/0][31] 

**`setAttribute(path, key String, value ByteString) `**

[Filesystem/A/0][31] 

**`removeAttribute(path, key String) `**

[Filesystem/A/0][31] 

**`discardAttribute(path, key String) `**

Removes the extended attribute for a given key, if there is a corresponding attribute. 

**`getAttributes(path) Object `**

Returns an Object that maps all of the extended attribute keys to their corresponding values. 

**`listAttributeNames(path) Array * String `**

[Filesystem/A/0][31] 

##  Security 

**`owner(path) String `**

[Filesystem/A/0][31] 

**`changeOwner(path, name String) `**

[Filesystem/A/0][31] 

**`permissions(path) Permissions `**

[Filesystem/A/0][31] 

**`changePermissions(path, permissions Permissions) `**

[Filesystem/A/0][31] 

##  Paths 

**`workingDirectory() String `**

[Filesystem/A/0][31] 

**`changeWorkingDirectory(path) `**

[Filesystem/A/0][31] 

**`workingDirectoryPath() Path `**

returns the current working directory as an absolute Path object 

###  Paths as Text 

**`join(...) String `**

takes a variadic list of path Strings, joins them on the file system's 
path separator, and normalizes the result. [details][32] 

**`split(path) Array * String `**

returns an array of path components. If the path is absolute, the first 
component will be an indicator of the root of the file system; for file 
systems with drives (such as Windows), this is the drive identifier with 
a colon, like "c:"; on Unix, this is an empty string "". The intent is 
that calling "join.apply" with the result of "split" as arguments will 
reconstruct the path. 

**`normal(path) String `**

removes '.' path components and simplifies '..' paths, if possible, for a 
given path. 

**`absolute(path) String `**

returns the absolute path, starting with the root of this file system 
object, for the given path, resolved from the current working directory. 
If the file system supports home directory aliases, absolute resolves 
those from the root of the file system. The resulting path is in normal 
form. On most systems, this is equivalent to expanding any user directory 
alias, joining the path to the current working directory, and normalizing 
the result. "absolute" can be implemented in terms of "workingDirectory", 
"join", and "normal". 

**`canonical(path) String `**

[Filesystem/A/0][31] 

**`readLink(path) String `**

returns the text of a symbolic link at a given path. Throws a **TODO** 
error if there is no accessible symbolic link at the given path. 

**`directory(path) String `**

returns the path of a file's containing directory, albeit the parent 
directory if the file is a directory. A terminal directory separator is 
ignored. 

**`base(path, extension_opt String) String `**

returns the part of the path that is after the last directory separator. 
If an extension is provided and is equal to the file's extension, the 
extension is removed from the result. 

**`extension(path) String `**

returns the extension of a file. The extension of a file is the last dot 
(excluding any number of initial dots) followed by one or more non-dot 
characters. Returns an empty string if no valid extension exists.
[unit test][33]. 

**`resolve(...) `**

a function like "join" except that it treats each argument as as either 
an absolute or relative path and, as is the convention with URL's, treats 
everything up to the final directory separator as a location, and 
everything afterward as an entry in that directory, even if the entry 
refers to a directory in the underlying storage. Resolve starts at the 
location "" and walks to the locations referenced by each path, and 
returns the path of the last file. Thus, `resolve(file, "")` idempotently 
refers to the location containing a file or directory entry, and 
resolve(file, neighbor) always gives the path of a file in the same 
directory. "resolve" is useful for finding paths in the "neighborhood" of 
a given file, while gracefully accepting both absolute and relative paths 
at each stage. [unit test][34]. 

**`relative(source, target_opt) `**

returns the relative path from one path to another using only ".." to 
traverse up to the two paths' common ancestor. If the target is omitted, 
returns the path to the source from the current working directory. 

###  Path Type 

Path instances inherit from the String prototype. 

**`[new] Path(path...) Path `**

returns a Path object. Path is a chainable shorthand for working with 
paths. Path objects have no more or less authority to manipulate the file 
system that produces them. If multiple arguments are passed to the Path 
constructor, they are joined to construct the corresponding path. If an 
argument is an Array, as ascertained by `Array.isArray`, it must conform 
to normal output of `split`, meaning that if the first value is a drive 
or root, the components are joined absolutely, and otherwise they are 
joined relatively. 

**`path(path...) Path `**

a shorthand for creating a new Path that does not use the `new` keyword, 
and also can be more easily applied variadically with the apply and call 
methods. 

The path constructor accepts as its first argument either a String, Path, 
or Array. If the path is an Array (as tested by `Array.isArray`, not
merely `typeof path == "array"`), it must conform to the specification 
for values returned by `fs.split`. 

Every path object has the members **`absolute`**, **`base`**, 
**`canonical`**, **`directory`**, **`normal`**, and **`relative`**. All 
of these return new `Path` objects constructed by converting the path to 
a string, passing it through the likewise named method of the file system 
API, and converting it back to a `Path`. Thus, all of these methods are 
chainable. In addition, **`join`** and **`resolve`** are variadic, so 
additional paths can be passed as arguments in either `String`, `Path`, 
or `Array` form. 

Every path object has the members **`listDirectoryTreePaths`**, 
**`listPaths`**, and **`listTreePaths`**. All of these return objects 
that provide `Path` objects constructed by converting the path to a 
string, passing it through the likewise named method of the file system 
API, and converting it back to a `Path`. 

Every path object has the members **`copy`**, **`copy`**, **`copyTree`**, 
**`discardAttribute`**, **`exists`**, **`extension`**, 
**`getAttribute`**, **`getAttributes`**, **`isDirectory`**, **`isFile`**, 
**`isLink`**, **`isReadable`**, **`isWritable`**, **`iterate`**, 
**`iterateDirectoryTree`**, **`iterateTree`**, **`lastModified`**, 
**`link`**, **`linkExists`**, **`list`**, **`listDirectoryTree`**, 
**`listTree`**, **`makeDirectory`**, **`makeTree`**, **`move`**, 
**`open`**, **`read`**, **`remove`**, **`removeAttribute`**, 
**`removeDirectory`**, **`removeTree`**, **`removeTree`**, **`rename`**, 
**`setAttribute`**, **`size`**, **`split`**, **`symbolicLink`**, 
**`touch`**, and **`write`**. All of these functions convert themselves 
to strings and pass the results through the likewise named method of the 
file system API. 

In addition, paths implement: 

**`toString()`** 

converts the path to its String representation 

**`to(target)`**

returns the path from this path to the target. Equivalent to 
`Path(relative(this, target))`. 

**`from(source) `**

returns the path from the source to this path. Equivalent to `Path(relative(source, this))`. 

**`glob(pattern) `**

returns an Array of path Strings that match the given pattern at this path. 

**`globPaths(pattern) `**

returns an Array of Path objects that match a given pattern at this path. 

  


#  Notes 

*   Path separator constants are deliberately omitted from this 
specification to encourage the use of the provided cross-platform 
methods. 
*   Full Posix stat functionality, including stat, has been left for an 
exercise for a separate specification. 

##  For Future Versions 

*   locks 
*   comprehensive stat access and modification 
*   ACLs 
*   temporary files and directories 
*   more open options: permissions, owner, groupOwner 
*   copy with metadata 
*   other [proposed names][35]. 

#  Relevant Discussions 

*   [writ or write?][36] 
*   [Filesystem API - Hobgoblin of little minds in little bike sheds with little bicycles][37] 
*   [Filesystem API: stat][38] 
*   [Filesystem API - Path(path, [fs])][39] 
*   [Filesystem API, Pure-JS glob implementation][40] 
*   [Return value of return? Partial writing?][41] 
*   [Simplifying core filesystem methods][42] 

[Category][43]: [Spec][44]

 [1]: /wiki/Filesystem "Filesystem"
 [2]: /index.php?title=CommonJS/File/A&redirect=no "CommonJS/File/A"
 [3]: #column-one
 [4]: #searchInput
 [5]: /wiki/Implementations/NarwhalRhino "Implementations/NarwhalRhino"
 [6]: /wiki/Implementations/NarwhalNode "Implementations/NarwhalNode"
 [7]: /wiki/Implementations/RingoJS "Implementations/RingoJS"
 [8]: javascript:toggleToc()
 [9]: #Overview
 [10]: #Security
 [11]: #Regarding_Paths
 [12]: #Specification
 [13]: #Files
 [14]: #Directories
 [15]: #Listing
 [16]: #Globbing
 [17]: #Path_Objects
 [18]: #Iterator_Objects
 [19]: #Links
 [20]: #Tests
 [21]: #Attributes
 [22]: #Extended_Attributes_.28optional.29
 [23]: #Security_2
 [24]: #Paths
 [25]: #Paths_as_Text
 [26]: #Path_Type
 [27]: #Notes
 [28]: #For_Future_Versions
 [29]: #Relevant_Discussions
 [30]: /wiki/IO "IO"
 [31]: /wiki/Filesystem/A/0 "Filesystem/A/0"
 [32]: /wiki/Filesystem/Join "Filesystem/Join"
 [33]: http://github.com/kriskowal/narwhal-test/blob/master/src/test/file/extension.js "http://github.com/kriskowal/narwhal-test/blob/master/src/test/file/extension.js"
 [34]: http://github.com/kriskowal/narwhal-test/blob/master/src/test/file/resolve.js "http://github.com/kriskowal/narwhal-test/blob/master/src/test/file/resolve.js"
 [35]: /index.php?title=API/file/Names&action=edit&redlink=1 "API/file/Names (page does not exist)"
 [36]: http://groups.google.com/group/commonjs/browse_thread/thread/a1b175c20ad61ed7 "http://groups.google.com/group/commonjs/browse_thread/thread/a1b175c20ad61ed7"
 [37]: http://groups.google.com/group/commonjs/browse_thread/thread/5ed547baaba11f6c "http://groups.google.com/group/commonjs/browse_thread/thread/5ed547baaba11f6c"
 [38]: http://groups.google.com/group/commonjs/browse_thread/thread/eb721e00452c4d02 "http://groups.google.com/group/commonjs/browse_thread/thread/eb721e00452c4d02"
 [39]: http://groups.google.com/group/commonjs/browse_thread/thread/abef650ba0c4f76d "http://groups.google.com/group/commonjs/browse_thread/thread/abef650ba0c4f76d"
 [40]: http://groups.google.com/group/commonjs/browse_thread/thread/f71e34667e1d5bde "http://groups.google.com/group/commonjs/browse_thread/thread/f71e34667e1d5bde"
 [41]: http://groups.google.com/group/commonjs/browse_thread/thread/c4af0c8d278fd34b "http://groups.google.com/group/commonjs/browse_thread/thread/c4af0c8d278fd34b"
 [42]: http://groups.google.com/group/commonjs/browse_thread/thread/c35d10239fade9ab "http://groups.google.com/group/commonjs/browse_thread/thread/c35d10239fade9ab"
 [43]: /wiki/Special:Categories "Special:Categories"
 [44]: /wiki/Category:Spec "Category:Spec"