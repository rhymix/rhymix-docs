Rhymix\Framework\Password
-------------------------

#### addAlgorithm()

```
public static function addAlgorithm(
    string $name,
    string $signature,
    callable $callback
): void
```

Add a custom algorithm.

#### isValidAlgorithm()

```
public static function isValidAlgorithm($algos): bool
```

Check if the given sequence of algorithms is valid.

#### getSupportedAlgorithms()

```
public static function getSupportedAlgorithms(): array
```

Get the list of hashing algorithms supported by this server.

#### getBestSupportedAlgorithm()

```
public static function getBestSupportedAlgorithm(): string
```

Get the best hashing algorithm supported by this server.

#### getDefaultAlgorithm()

```
public static function getDefaultAlgorithm(): string
```

Get the current default hashing algorithm.

#### getBackwardCompatibleAlgorithm()

```
public static function getBackwardCompatibleAlgorithm(): string
```

Get the current default hashing algorithm, unless it will produce
hashes that are longer than 60 characters.
In that case, this method returns the next best supported algorithm
that produces 60-character (or shorter) hashes. This helps maintain
compatibility with old tables that still have varchar(60) columns.

#### getWorkFactor()

```
public static function getWorkFactor(): int
```

Get the currently configured work factor for bcrypt and other adjustable algorithms.

#### getRandomPassword()

```
public static function getRandomPassword(int $length = 16): string
```

Generate a reasonably strong random password.

#### hashPassword()

```
public static function hashPassword(
    string $password,
    $algos = null,
    ?string $salt = null
): string
```

Hash a password.
To use multiple algorithms in series, provide them as an array.
Salted algorithms such as bcrypt, pbkdf2, or portable must be used last.
On error, false will be returned.

#### checkPassword()

```
public static function checkPassword(
    string $password,
    string $hash,
    $algos = null
): bool
```

Check a password against a hash.
This method returns true if the password is correct, and false otherwise.
If the algorithm is not specified, it will be guessed from the format of the hash.

#### checkAlgorithm()

```
public static function checkAlgorithm(string $hash): array
```

Guess which algorithm(s) were used to generate the given hash.
If there are multiple possibilities, all of them will be returned in an array.

#### checkWorkFactor()

```
public static function checkWorkFactor(string $hash): int
```

Check the work factor of a hash.

#### argon2id()

```
public static function argon2id(
    string $password,
    int $work_factor = 10
): string
```

Generate the argon2id hash of a string.

#### bcrypt()

```
public static function bcrypt(
    string $password,
    ?string $salt = null,
    int $work_factor = 10
): string
```

Generate the bcrypt hash of a string.

#### pbkdf2()

```
public static function pbkdf2(
    string $password,
    ?string $salt = null,
    string $algorithm = 'sha512',
    int $iterations = 16384,
    int $length = 24,
    int $iterations_padding = 7
): string
```

Generate the PBKDF2 hash of a string.

#### countEntropyBits()

```
public static function countEntropyBits(string $password): int
```

Count the amount of entropy that a password contains.
