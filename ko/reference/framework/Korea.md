Rhymix\Framework\Korea
----------------------

#### formatPhoneNumber()

```
public static function formatPhoneNumber(string $num): string
```

Format a phone number.

#### isValidPhoneNumber()

```
public static function isValidPhoneNumber(string $num): bool
```

Check if a Korean phone number contains a valid area code and the correct number of digits.

#### isValidMobilePhoneNumber()

```
public static function isValidMobilePhoneNumber(string $num): bool
```

Check if a Korean phone number is a mobile phone number.

#### isValidJuminNumber()

```
public static function isValidJuminNumber(string $code): bool
```

Check if the given string is a valid resident registration number (주민등록번호)
or foreigner registration number (외국인등록번호).
This method only checks the format.
It does not check that the number is actually in use.

#### isValidCorporationNumber()

```
public static function isValidCorporationNumber(string $code): bool
```

Check if the given string is a valid corporation registration number (법인등록번호).
This method only checks the format.
It does not check that the number is actually in use.

#### isValidBusinessNumber()

```
public static function isValidBusinessNumber(string $code): bool
```

Check if the given string is a valid business registration number (사업자등록번호).
This method only checks the format.
It does not check that the number is actually in use.

#### isKoreanIP()

```
public static function isKoreanIP(string $ip): bool
```

Check if the given IP address is Korean.
This method may return incorrect results if the IP allocation databases
(korea.ipv4.php, korea.ipv6.php) are out of date.

#### isKoreanEmailAddress()

```
public static function isKoreanEmailAddress(
    string $email_address,
    bool $clear_cache = false
): bool
```

Check if the given email address is hosted by a Korean portal site.
This can be used to tell which recipients may subscribe to the KISA RBL (kisarbl.or.kr).
If the domain is not found, this method returns false.
