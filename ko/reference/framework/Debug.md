Rhymix\Framework\Debug
----------------------

#### enable()

```
public static function enable(): void
```

Enable log collection.

#### disable()

```
public static function disable(): void
```

Disable log collection.

#### getEntries()

```
public static function getEntries(): array
```

Get all entries.

#### clearEntries()

```
public static function clearEntries(): void
```

Clear all entries.

#### getErrors()

```
public static function getErrors(): array
```

Get all errors.

#### clearErrors()

```
public static function clearErrors(): void
```

Clear all errors.

#### getQueries()

```
public static function getQueries(): array
```

Get all queries.

#### getSlowQueries()

```
public static function getSlowQueries(): array
```

Get all slow queries.

#### clearQueries()

```
public static function clearQueries(): void
```

Clear all queries.

#### getTriggers()

```
public static function getTriggers(): array
```

Get all triggers.

#### getSlowTriggers()

```
public static function getSlowTriggers(): array
```

Get all slow triggers.

#### clearTriggers()

```
public static function clearTriggers(): void
```

Clear all triggers.

#### getWidgets()

```
public static function getWidgets(): array
```

Get all widgets.

#### getSlowWidgets()

```
public static function getSlowWidgets(): array
```

Get all slow widgets.

#### clearWidgets()

```
public static function clearWidgets(): void
```

Clear all widgets.

#### getRemoteRequests()

```
public static function getRemoteRequests(): array
```

Get all remote requests.

#### getSlowRemoteRequests()

```
public static function getSlowRemoteRequests(): array
```

Get all slow remote requests.

#### clearRemoteRequests()

```
public static function clearRemoteRequests(): void
```

Clear all remote requests.

#### clearAll()

```
public static function clearAll(): void
```

Clear all records.

#### addFilenameAlias()

```
public static function addFilenameAlias(
    string $display_filename,
    string $real_filename
): void
```

Add a filename alias.

#### addSessionStartTime()

```
public static function addSessionStartTime(float $session_start_time): void
```

Add session start time.

#### addEntry()

```
public static function addEntry($message): void
```

Add an arbitrary entry to the log.

#### addError()

```
public static function addError(
    int $errno,
    string $errstr,
    string $errfile,
    int $errline
): void
```

Add a PHP error to the log.

#### addQuery()

```
public static function addQuery(array $query): void
```

Add a query to the log.

#### addTrigger()

```
public static function addTrigger(array $trigger): void
```

Add a trigger to the log.

#### addWidget()

```
public static function addWidget(array $widget): void
```

Add a widget to the log.

#### addRemoteRequest()

```
public static function addRemoteRequest(array $request): void
```

Add a remote request to the log.

#### exceptionHandler()

```
public static function exceptionHandler(Throwable $e): void
```

The default handler for catching exceptions.

#### shutdownHandler()

```
public static function shutdownHandler(): void
```

The default handler for catching fatal errors.

#### formatBacktrace()

```
public static function formatBacktrace(array $backtrace): string
```

Format a backtrace for error logging.

#### translateFilename()

```
public static function translateFilename(string $filename)
```

Translate filenames.

#### registerErrorHandlers()

```
public static function registerErrorHandlers(int $error_types): void
```

Register all error handlers.

#### displayErrorScreen()

```
public static function displayErrorScreen(
    string $message,
    string $location = ''
): void
```

Display a fatal error screen.

#### displayError()

```
public static function displayError(string $message): void
```

Display a default error.

#### isEnabledForCurrentUser()

```
public static function isEnabledForCurrentUser(): bool
```

Check if debugging is enabled for the current user.

#### getDebugData()

```
public static function getDebugData(): object
```

Get all debug information as an object.

#### getErrorType()

```
public static function getErrorType(int $errno): string
```

Convert a PHP error number to the corresponding error name.
