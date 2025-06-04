Rhymix\Framework\Request
------------------------

#### __construct()

```
public function __construct(
    string $method = '',
    string $url = '',
    string $hostname = '',
    string $protocol = ''
)
```

Constructor.

#### get()

```
public function get(
    string $name,
    string $type = ''
)
```

Get a request argument, optionally coerced into a type.

#### getAll()

```
public function getAll(): object
```

Get all request arguments.

#### getFullUrl()

```
public function getFullUrl(): string
```

Get the complete URL of this request.

#### getMethod()

```
public function getMethod(): string
```

Get the HTTP method of this request.

#### getCallbackFunction()

```
public function getCallbackFunction(): string
```

Get the JS callback function.

#### getRouteStatus()

```
public function getRouteStatus(): int
```

Get route status.

#### set()

```
public function set(
    string $name,
    $value
): void
```

Set a request argument.

#### setAll()

```
public function setAll(array $args): void
```

Set all request arguments.

#### setRouteStatus()

```
public function setRouteStatus(int $status): void
```

Set route status.

#### getRouteOption()

```
public function getRouteOption(string $name)
```

Get route options.

#### getRouteOptions()

```
public function getRouteOptions(): object
```

Get all route options.

#### setRouteOption()

```
public function setRouteOption(
    string $name,
    $value
): void
```

Set route option.
