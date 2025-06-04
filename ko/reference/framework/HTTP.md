Rhymix\Framework\HTTP
---------------------

#### resetClient()

```
public static function resetClient(): void
```

Reset the Guzzle client instance.

#### get()

```
public static function get(
    string $url,
    $data = null,
    array $headers = [],
    array $cookies = [],
    array $settings = []
): object
```

Make a GET request.

#### head()

```
public static function head(
    string $url,
    $data = null,
    array $headers = [],
    array $cookies = [],
    array $settings = []
): Psr\Http\Message\ResponseInterface
```

Make a HEAD request.

#### post()

```
public static function post(
    string $url,
    $data = null,
    array $headers = [],
    array $cookies = [],
    array $settings = []
): Psr\Http\Message\ResponseInterface
```

Make a POST request.

#### download()

```
public static function download(
    string $url,
    string $target_filename,
    string $method = 'GET',
    $data = null,
    array $headers = [],
    array $cookies = [],
    array $settings = []
): Psr\Http\Message\ResponseInterface
```

Download a file.
This helps save memory when downloading large files,
by streaming the response body directly to the filesystem
instead of buffering it in memory.

#### request()

```
public static function request(
    string $url,
    string $method = 'GET',
    $data = null,
    array $headers = [],
    array $cookies = [],
    array $settings = []
): Psr\Http\Message\ResponseInterface
```

Make any type of request.

#### async()

```
public static function async(
    string $url,
    string $method = 'GET',
    $data = null,
    array $headers = [],
    array $cookies = [],
    array $settings = []
): GuzzleHttp\Promise\PromiseInterface
```

Make any type of request asynchronously.

#### multiple()

```
public static function multiple(array $requests): array
```

Make multiple concurrent requests.
