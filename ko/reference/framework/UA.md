Rhymix\Framework\UA
-------------------

#### isMobile()

```
public static function isMobile(?string $ua = null): bool
```

Check whether the current visitor is using a mobile device.

#### isTablet()

```
public static function isTablet(?string $ua = null): bool
```

Check whether the current visitor is using a tablet.

#### isRobot()

```
public static function isRobot(?string $ua = null): bool
```

Check whether the current visitor is a robot.

#### getLocale()

```
public static function getLocale(?string $header = null): string
```

This method parses the Accept-Language header to guess the browser's default locale.

#### getBrowserInfo()

```
public static function getBrowserInfo(?string $ua = null): self
```

This method parses the User-Agent string to guess what kind of browser it is.

#### encodeFilenameForDownload()

```
public static function encodeFilenameForDownload(
    string $filename,
    ?string $ua = null
): string
```

This method encodes a UTF-8 filename for downloading in the current visitor's browser.
See: https://blog.bloodcat.com/302

#### getColorScheme()

```
public static function getColorScheme(): string
```

Get the current color scheme (auto, light, dark)

#### setColorScheme()

```
public static function setColorScheme(string $color_scheme): void
```

Set the color scheme (auto, light, dark)
