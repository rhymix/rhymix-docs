Rhymix\Framework\Filters\IpFilter
---------------------------------

#### inRange()

```
public static function inRange(
    string $ip,
    string $range
): bool
```

Check whether the given IP address belongs to a range.

#### inRanges()

```
public static function inRanges(
    string $ip,
    array $ranges
): bool
```

Check whether the given IP address belongs to a set of ranges.

#### validateRange()

```
public static function validateRange(string $range): bool
```

Check whether a range definition is valid.

#### validateRanges()

```
public static function validateRanges(array $ranges): bool
```

Check whether a set of range definitions is valid.

#### getCloudFlareRealIP()

```
public static function getCloudFlareRealIP()
```

Get real IP from CloudFlare headers.
