Rhymix\Framework\Timer
----------------------

#### start()

```
public static function start(string $name = ''): float
```

Start a timer.
This method returns the current microtime.

#### stop()

```
public static function stop(string $name = ''): float
```

Stop a timer and return the elapsed time.
If the name is not given, the most recently started timer will be stopped.
If no timer has been started, this method returns zero.

#### stopFormat()

```
public static function stopFormat(string $name = ''): string
```

Stop a timer and return the elapsed time in a human-readable format.
If the name is not given, the most recently started timer will be stopped.
If no timer has been started, this method returns '0'.

#### sinceStartup()

```
public static function sinceStartup(): float
```

This method returns how much time has elapsed since Rhymix startup.

#### sinceStartupFormat()

```
public static function sinceStartupFormat(): string
```

This method returns how much time has elapsed since startup in a human-readable format.
