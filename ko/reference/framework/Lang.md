Rhymix\Framework\Lang
---------------------

#### getInstance()

```
public static function getInstance(string $language): self
```

This method returns the cached instance of a language.

#### langType()

```
public function langType(): string
```

Return language type.

#### loadPlugin()

```
public function loadPlugin(string $name): bool
```

Load translations from a plugin (module, addon).

#### loadDirectory()

```
public function loadDirectory(
    string $dir,
    ?string $plugin_name = null
): bool
```

Load translations from a directory.

#### getSupportedList()

```
public static function getSupportedList(): array
```

Get the list of supported languages.

#### get()

```
public function get(string $key)
```

Generic getter.

#### set()

```
public function set(
    string $key,
    $value
): void
```

Generic setter.

#### getFromDefaultLang()

```
public function getFromDefaultLang(string $key)
```

Fallback method for getting the default translation.

#### __get()

```
public function __get(string $key)
```

Magic method for translations without arguments.

#### __set()

```
public function __set(
    string $key,
    $value
): void
```

Magic method for setting a new custom translation.

#### __isset()

```
public function __isset(string $key): bool
```

Magic method for checking whether a translation exists.

#### __unset()

```
public function __unset(string $key): void
```

Magic method for unsetting a translation.

#### __call()

```
public function __call(
    string $key,
    $args = []
)
```

Magic method for translations with arguments.
