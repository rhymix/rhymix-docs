Rhymix\Framework\URL
--------------------

#### getCurrentURL()

```
public static function getCurrentURL(array $changes = []): string
```

Get the current URL.
If $changes are given, they will be appended to the current URL as a query string.
To delete an existing query string, set its value to null.

#### getCurrentDomain()

```
public static function getCurrentDomain(bool $preserve_port = false): string
```

Get the current domain.

#### getCurrentDomainURL()

```
public static function getCurrentDomainURL(string $path = '/'): string
```

Get a URL using the current domain and the path.

#### getCanonicalURL()

```
public static function getCanonicalURL(string $url): string
```

Convert a URL to its canonical format.

#### getDomainFromURL()

```
public static function getDomainFromURL(string $url)
```

Get the domain from a URL.

#### isInternalURL()

```
public static function isInternalURL(string $url): bool
```

Check if a URL is internal to this site.

#### modifyURL()

```
public static function modifyURL(
    string $url,
    array $changes = []
): string
```

Modify a URL.
If $changes are given, they will be appended to the current URL as a query string.
To delete an existing query string, set its value to null.

#### fromServerPath()

```
public static function fromServerPath(string $path)
```

Convert a server-side path to a URL.
This method returns false if the path cannot be converted to a URL,
e.g. if the path is outside of the document root.

#### toServerPath()

```
public static function toServerPath(string $url)
```

Convert a URL to a server-side path.
This method returns false if the URL cannot be converted to a server-side path,
e.g. if the URL belongs to an external domain.

#### encodeIdna()

```
public static function encodeIdna(string $url): string
```

Encode UTF-8 domain into IDNA (punycode)

#### decodeIdna()

```
public static function decodeIdna(string $url): string
```

Convert IDNA (punycode) domain into UTF-8
