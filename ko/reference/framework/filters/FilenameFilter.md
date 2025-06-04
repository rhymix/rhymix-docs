Rhymix\Framework\Filters\FilenameFilter
---------------------------------------

#### clean()

```
public static function clean(string $filename): string
```

Remove illegal and dangerous characters from a filename.

#### cleanPath()

```
public static function cleanPath(string $path): string
```

Clean a path to remove ./, ../, trailing slashes, etc.

#### isDirectDownload()

```
public static function isDirectDownload(
    string $filename,
    bool $include_multimedia = true
): bool
```

Check if a file has an extension that would allow direct download.
