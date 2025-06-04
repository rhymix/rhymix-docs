Rhymix\Framework\Security
-------------------------

#### sanitize()

```
public static function sanitize(
    string $input,
    string $type
): string
```

Sanitize a variable.

#### encrypt()

```
public static function encrypt(
    string $plaintext,
    ?string $key = null
): string
```

Encrypt a string using AES.

#### decrypt()

```
public static function decrypt(
    string $ciphertext,
    ?string $key = null
)
```

Decrypt a string using AES.

#### createSignature()

```
public static function createSignature(string $string): string
```

Create a digital signature to verify the authenticity of a string.

#### verifySignature()

```
public static function verifySignature(
    string $string,
    string $signature
): bool
```

Check whether a signature is valid.

#### getRandom()

```
public static function getRandom(
    int $length = 32,
    string $format = 'alnum'
): string
```

Generate a cryptographically secure random string.

#### getRandomNumber()

```
public static function getRandomNumber(
    int $min = 0,
    int $max = 9223372036854775807
): int
```

Generate a cryptographically secure random number between $min and $max.

#### getRandomUUID()

```
public static function getRandomUUID(int $version = 4): string
```

Generate a random UUID.
The code for UUIDv4 is based on https://stackoverflow.com/a/15875555/481206

#### compareStrings()

```
public static function compareStrings(
    string $a,
    string $b
): bool
```

Compare two strings in constant time.

#### checkCSRF()

```
public static function checkCSRF(?string $referer = null): bool
```

Check if the current request seems to be a CSRF attack.
This method returns true if the request seems to be innocent,
and false if it seems to be a CSRF attack.

#### checkXXE()

```
public static function checkXXE(?string $xml = null): bool
```

Check if the current request seems to be an XXE (XML external entity) attack.
This method returns true if the request seems to be innocent,
and false if it seems to be an XXE attack.
This is the opposite of XE's Security::detectingXEE() method.
The name has also been changed to the more accurate acronym XXE.
