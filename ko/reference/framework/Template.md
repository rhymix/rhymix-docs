Rhymix\Framework\Template
-------------------------

#### getInstance()

```
public static function getInstance(): self
```

Provided for compatibility with old TemplateHandler.

#### __construct()

```
public function __construct(
    ?string $dirname = null,
    ?string $filename = null,
    ?string $extension = null
)
```

You can also call the constructor directly.

#### setCachePath()

```
public function setCachePath(?string $cache_path = null)
```

Set the path for the cache file.

#### disableCache()

```
public function disableCache(): void
```

Disable caching.

#### exists()

```
public function exists(): bool
```

Check if the template file exists.

#### getParent()

```
public function getParent(): ?self
```

Get the parent template.

#### setParent()

```
public function setParent(self $parent): void
```

Set the parent template.

#### getVars()

```
public function getVars(): ?object
```

Get vars.

#### setVars()

```
public function setVars($vars): void
```

Set vars.

#### addVars()

```
public function addVars($vars): void
```

Add vars.

#### compile()

```
public function compile(
    ?string $dirname = null,
    ?string $filename = null,
    ?string $override_filename = null
)
```

Compile and execute a template file.
You don't need to pass any paths if you have already supplied them
through the constructor. They exist for backward compatibility.
$override_filename should be considered deprecated, as it is only
used in faceOff (layout source editor).

#### compileDirect()

```
public function compileDirect(
    string $dirname,
    string $filename
): string
```

Compile a template and return the PHP code.

#### parse()

```
public function parse(?string $content = null): string
```

Convert template code to PHP using a version-specific parser.
Directly passing $content as a string is not available as an
official API. It only exists for unit testing.

#### execute()

```
public function execute(): string
```

Execute the converted template and return the output.

#### getFragment()

```
public function getFragment(string $name): ?string
```

Get a fragment of the executed output.

#### getStack()

```
public function getStack(string $name): ?array
```

Get the contents of a stack.

#### isRelativePath()

```
public function isRelativePath(string $path): bool
```

Check if a path should be treated as relative to the path of the current template.

#### convertPath()

```
public function convertPath(
    string $path,
    ?string $basepath = null
): string
```

Convert a relative path using the given basepath.

#### normalizePath()

```
public function normalizePath(string $path): string
```

Normalize a path by removing extra slashes and parent directory references.
