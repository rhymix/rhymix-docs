Rhymix\Framework\Storage
------------------------

#### exists()

```
public static function exists(string $path): bool
```

Check if a path really exists.

#### isFile()

```
public static function isFile(string $path): bool
```

Check if the given path is a file.

#### isEmptyFile()

```
public static function isEmptyFile(string $path): bool
```

Check if the given path is an empty file.

#### isDirectory()

```
public static function isDirectory(string $path): bool
```

Check if the given path is a directory.

#### isEmptyDirectory()

```
public static function isEmptyDirectory(string $path): bool
```

Check if the given path is an empty directory.

#### isSymlink()

```
public static function isSymlink(string $path): bool
```

Check if the given path is a symbolic link.

#### isValidSymlink()

```
public static function isValidSymlink(string $path): bool
```

Check if the given path is a valid symbolic link.

#### isReadable()

```
public static function isReadable(string $path): bool
```

Check if the given path is readable.

#### isWritable()

```
public static function isWritable(string $path): bool
```

Check if the given path is writable.

#### isExecutable()

```
public static function isExecutable(string $path): bool
```

Check if the given path is executable.

#### getSize()

```
public static function getSize(string $filename)
```

Get the size of a file.
This method returns the size of a file, or false on error.

#### read()

```
public static function read(
    string $filename,
    bool $stream = false
)
```

Get the content of a file.
This method returns the content if it exists and is readable.
If $stream is true, it will return the content as a stream instead of
loading the entire content in memory. This may be useful for large files.
If the file cannot be opened, this method returns false.

#### readPHPData()

```
public static function readPHPData(string $filename)
```

Read PHP data from a file, formatted for easy retrieval.
This method returns the data on success and false on failure.

#### write()

```
public static function write(
    string $filename,
    $content,
    string $mode = 'w',
    ?int $perms = null
)
```

Write $content to a file.
If $content is a stream, this method will copy it to the target file
without loading the entire content in memory. This may be useful for large files.
This method returns true on success and false on failure.

#### writePHPData()

```
public static function writePHPData(
    string $filename,
    $data,
    ?string $comment = null,
    bool $serialize = true
)
```

Write PHP data to a file, formatted for easy retrieval.
This method returns true on success and false on failure.
Resources and anonymous functions cannot be saved.

#### copy()

```
public static function copy(
    string $source,
    string $destination,
    ?int $destination_perms = null
): bool
```

Copy $source to $destination.
This method returns true on success and false on failure.
If the destination permissions are not given, they will be copied from the source.

#### move()

```
public static function move(
    string $source,
    string $destination
): bool
```

Move $source to $destination.
This method returns true on success and false on failure.

#### moveUploadedFile()

```
public static function moveUploadedFile(
    string $source,
    string $destination,
    string $type = ''
): bool
```

Move uploaded $source to $destination.
This method returns true on success and false on failure.

#### delete()

```
public static function delete(string $filename): bool
```

Delete a file.
This method returns true if the file exists and has been successfully
deleted, and false on any kind of failure.

#### createDirectory()

```
public static function createDirectory(
    string $dirname,
    ?int $mode = null
): bool
```

Create a directory.

#### readDirectory()

```
public static function readDirectory(
    string $dirname,
    bool $full_path = true,
    bool $skip_dotfiles = true,
    bool $skip_subdirs = true
)
```

Read the list of files in a directory.

#### copyDirectory()

```
public static function copyDirectory(
    string $source,
    string $destination,
    string $exclude_regexp = ''
): bool
```

Copy a directory recursively.

#### moveDirectory()

```
public static function moveDirectory(
    string $source,
    string $destination
): bool
```

Move a directory.

#### deleteDirectory()

```
public static function deleteDirectory(
    string $dirname,
    bool $delete_self = true
): bool
```

Delete a directory recursively.

#### deleteEmptyDirectory()

```
public static function deleteEmptyDirectory(
    string $dirname,
    bool $delete_empty_parents = false
): bool
```

Delete a directory only if it is empty.

#### getUmask()

```
public static function getUmask(): int
```

Get the current umask.

#### setUmask()

```
public static function setUmask(int $umask): void
```

Set the current umask.

#### recommendUmask()

```
public static function recommendUmask(): string
```

Determine the best umask for this installation of Rhymix.

#### getServerUID()

```
public static function getServerUID()
```

Get the UID of the server process.

#### getLock()

```
public static function getLock(string $name): bool
```

Obtain an exclusive lock.

#### clearLocks()

```
public static function clearLocks(): void
```

Clear all locks.
