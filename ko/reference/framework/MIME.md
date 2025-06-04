Rhymix\Framework\MIME
---------------------

#### getContentType()

```
public static function getContentType(string $filename): ?string
```

Get the MIME type of a file, detected by its content.
This method returns the MIME type of a file, or false on error.

#### getTypeByExtension()

```
public static function getTypeByExtension(string $extension): string
```

Get the MIME type for the given extension.

#### getTypeByFilename()

```
public static function getTypeByFilename(string $filename): string
```

Get the MIME type for the given filename.

#### getExtensionByType()

```
public static function getExtensionByType(string $type): ?string
```

Get the most common extension for the given MIME type.
