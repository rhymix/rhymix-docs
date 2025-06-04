Rhymix\Framework\Config
-----------------------

#### init()

```
public static function init(): array
```

Load system configuration.

#### getAll()

```
public static function getAll(): array
```

Get all system configuration.

#### getDefaults()

```
public static function getDefaults(): array
```

Get default system configuration.

#### get()

```
public static function get(string $key)
```

Get a system configuration value.

#### set()

```
public static function set(
    string $key,
    $value
): void
```

Set a system configuration value.

#### setAll()

```
public static function setAll(array $config): void
```

Set all system configuration.

#### save()

```
public static function save(?array $config = null): bool
```

Save the current system configuration.

#### serialize()

```
public static function serialize($value): string
```

Serialize a value for insertion into a PHP-based configuration file.
