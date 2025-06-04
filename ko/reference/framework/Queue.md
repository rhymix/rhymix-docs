Rhymix\Framework\Queue
----------------------

#### addDriver()

```
public static function addDriver(
    string $name,
    Rhymix\Framework\Drivers\QueueInterface $driver
): void
```

Add a custom Queue driver.

#### getDriver()

```
public static function getDriver(string $name): ?Rhymix\Framework\Drivers\QueueInterface
```

Get a Queue driver instance.

#### getDbDriver()

```
public static function getDbDriver(): Rhymix\Framework\Drivers\Queue\DB
```

Get the DB driver instance, for managing scheduled tasks.

#### getSupportedDrivers()

```
public static function getSupportedDrivers(): array
```

Get the list of supported Queue drivers.

#### addTask()

```
public static function addTask(
    string $handler,
    ?object $args = null,
    ?object $options = null,
    ?string $priority = null
): int
```

Add a task to the queue.
The queued task will be executed as soon as possible.
The handler can be in one of the following formats:
- Global function, e.g. myHandler
- ClassName::staticMethodName
- ClassName::getInstance()->methodName
- new ClassName()->methodName
Once identified and/or instantiated, the handler will be passed $args
and $options, in that order. Each of them must be a single object.
It is strongly recommended that you write a dedicated method to handle
queued tasks, rather than reusing an existing method with a potentially
incompatible structure. If you must to call an existing method,
you should consider writing a wrapper.
Any value returned by the handler will be discarded. If you throw an
exception, it may be logged, but it will not cause a fatal error.

#### addTaskAt()

```
public static function addTaskAt(
    int $time,
    string $handler,
    ?object $args = null,
    ?object $options = null,
    ?string $priority = null
): int
```

Add a task to be executed at a specific time.
The queued task will be executed once at the configured time.
The rest is identical to addTask().

#### addTaskAtInterval()

```
public static function addTaskAtInterval(
    string $interval,
    string $handler,
    ?object $args = null,
    ?object $options = null
): int
```

Add a task to be executed at an interval.
The queued task will be executed repeatedly at the scheduled interval.
The synax for specifying the interval is the same as crontab.
The rest is identical to addTask().

#### getScheduledTask()

```
public static function getScheduledTask(int $task_srl): ?object
```

Get information about a scheduled task if it exists.

#### cancelScheduledTask()

```
public static function cancelScheduledTask(int $task_srl): bool
```

Cancel a scheduled task.

#### checkKey()

```
public static function checkKey(string $key): bool
```

Check the process key.

#### checkIntervalSyntax()

```
public static function checkIntervalSyntax(string $interval): bool
```

Check the interval syntax.
This method returns true if the interval string is well-formed,
and false otherwise. However, it does not check that all the numbers
are in the correct range (e.g. 0-59 for minutes).

#### parseInterval()

```
public static function parseInterval(
    string $interval,
    ?int $time
): bool
```

Parse an interval string check it against a timestamp.
This method returns true if the interval covers the given timestamp,
and false otherwise.

#### process()

```
public static function process(
    int $index,
    int $count,
    int $timeout
): void
```

Process queued and scheduled tasks.
This will usually be called by a separate script, run every minute
through an external scheduler such as crontab or systemd.
If you are on a shared hosting service, you may also call a URL
using a "web cron" service provider.

#### signalHandler()

```
public static function signalHandler(
    int $signal,
    $siginfo
): void
```

Signal handler.

#### signalReceived()

```
public static function signalReceived(): int
```

Has a signal been received?
