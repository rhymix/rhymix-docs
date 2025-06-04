Rhymix\Framework\DateTime
-------------------------

#### formatTimestamp()

```
public static function formatTimestamp(
    string $format,
    ?int $timestamp = null
): string
```

Format a Unix timestamp using the internal timezone.

#### formatTimestampForCurrentUser()

```
public static function formatTimestampForCurrentUser(
    string $format,
    ?int $timestamp = null
): string
```

Format a Unix timestamp for the current user's timezone.

#### getTimezoneForCurrentUser()

```
public static function getTimezoneForCurrentUser(): string
```

Get the current user's timezone.

#### getTimezoneList()

```
public static function getTimezoneList(): array
```

Get the list of time zones supported on this server.

#### getTimezoneOffset()

```
public static function getTimezoneOffset(
    string $timezone,
    ?int $timestamp = null
): int
```

Get the absolute (UTC) offset of a timezone.

#### getTimezoneOffsetFromInternal()

```
public static function getTimezoneOffsetFromInternal(
    string $timezone,
    ?int $timestamp = null
): int
```

Get the relative offset between a timezone and Rhymix's internal timezone.

#### getTimezoneOffsetByLegacyFormat()

```
public static function getTimezoneOffsetByLegacyFormat(string $timezone): int
```

Get the absolute (UTC) offset of a timezone written in XE legacy format ('+0900').

#### getTimezoneNameByOffset()

```
public static function getTimezoneNameByOffset(int $offset): string
```

Get a PHP time zone by UTC offset.
Time zones with both (a) fractional offsets and (b) daylight saving time
(such as Iran's +03:30/+04:30) cannot be converted in this way.
However, if Rhymix is installed for the first time in such a time zone,
the internal time zone will be automatically set to UTC,
so this should never be a problem in practice.

#### getRelativeTimestamp()

```
public static function getRelativeTimestamp(?int $timestamp = null): string
```

Get a relative timestamp (3 hours ago, etc.)
