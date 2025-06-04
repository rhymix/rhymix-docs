Rhymix\Framework\Cache
----------------------

#### init()

```
public static function init($config): Rhymix\Framework\Drivers\CacheInterface
```

Initialize the cache system.

#### getSupportedDrivers()

```
public static function getSupportedDrivers(): array
```

Get the list of supported cache drivers.

#### getDriverName()

```
public static function getDriverName(): ?string
```

Get the name of the currently enabled cache driver.

#### getDriverInstance()

```
public static function getDriverInstance(
    $name = null,
    array $config = []
): ?Rhymix\Framework\Drivers\CacheInterface
```

Get the currently enabled cache driver, or a named driver with the given settings.

#### getPrefix()

```
public static function getPrefix(): string
```

Get the automatically generated cache prefix for this installation of Rhymix.

#### getDefaultTTL()

```
public static function getDefaultTTL(): int
```

Get the default TTL.

#### setDefaultTTL()

```
public static function setDefaultTTL(int $ttl): void
```

Set the default TTL.

#### get()

```
public static function get(string $key)
```

Get the value of a key.
This method returns null if the key was not found.

#### set()

```
public static function set(
    string $key,
    $value,
    int $ttl = 0,
    bool $force = false
): bool
```

Set the value to a key.
This method returns true on success and false on failure.
$ttl is measured in seconds. If it is not given, the default TTL is used.
$force is used to cache essential data when using the default driver.

#### delete()

```
public static function delete(string $key): bool
```

Delete a key.
This method returns true on success and false on failure.
If the key does not exist, it should return false.

#### exists()

```
public static function exists(string $key): bool
```

Check if a key exists.
This method returns true on success and false on failure.

#### incr()

```
public static function incr(
    string $key,
    int $amount = 1
): int
```

Increase the value of a key by $amount.
If the key does not exist, this method assumes that the current value is zero.
This method returns the new value, or -1 on failure.

#### decr()

```
public static function decr(
    string $key,
    int $amount = 1
): int
```

Decrease the value of a key by $amount.
If the key does not exist, this method assumes that the current value is zero.
This method returns the new value, or -1 on failure.

#### clearGroup()

```
public static function clearGroup(string $group_name): bool
```

Clear a group of keys from the cache.
This method returns true on success and false on failure.

#### clearAll()

```
public static function clearAll(): bool
```

Clear all keys from the cache.
This method returns true on success and false on failure.

#### getGroupVersion()

```
public static function getGroupVersion(string $group_name): int
```

Get the group version.

#### getRealKey()

```
public static function getRealKey(string $key): string
```

Get the actual key used by Rhymix.
