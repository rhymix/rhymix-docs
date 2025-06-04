Rhymix\Framework\Session
------------------------

#### get()

```
public static function get(string $key)
```

Get a session variable.

#### set()

```
public static function set(
    string $key,
    $value
): void
```

Set a session variable.

#### start()

```
public static function start(bool $force = false): bool
```

Start the session.
This method is called automatically at Rhymix startup.
There is usually no need to call it manually.

#### checkStart()

```
public static function checkStart(bool $force = false): bool
```

Check if the session needs to be started.
This method is called automatically at Rhymix shutdown.
It is only necessary if the session is delayed.

#### checkLoginStatusCookie()

```
public static function checkLoginStatusCookie(): void
```

Check the login status cookie.
This cookie encodes information about whether the user is logged in,
and helps client software distinguish between different users.

#### checkSSO()

```
public static function checkSSO(object $site_module_info): void
```

Check if this session needs to be shared with another site with SSO.
This method uses more or less the same logic as XE's SSO mechanism.
It may need to be changed to a more secure mechanism later.

#### create()

```
public static function create(): bool
```

Create the data structure for a new Rhymix session.
This method is called automatically by start() when needed.

#### refresh()

```
public static function refresh(bool $refresh_cookie = false): bool
```

Refresh the session.
This helps increase the lifetime for session cookies and autologin cookies
while the user is active on the site.

#### close()

```
public static function close(): void
```

Close the session and write its data.
This method is called automatically at the end of a request, but you can
call it sooner if you don't plan to write any more data to the session.

#### destroy()

```
public static function destroy(): void
```

Destroy the session.
This method deletes all data associated with the current session.

#### login()

```
public static function login(
    int $member_srl,
    bool $refresh = true
): bool
```

Log in.
This method accepts either an integer or a member object.
It returns true on success and false on failure.

#### logout()

```
public static function logout(): void
```

Log out and destroy the session.

#### isStarted()

```
public static function isStarted(): bool
```

Check if the session has been started.

#### isMember()

```
public static function isMember(): bool
```

Check if a member has logged in with this session.
This method returns true or false, not 'Y' or 'N'.

#### isAdmin()

```
public static function isAdmin(): bool
```

Check if an administrator is logged in with this session.
This method returns true or false, not 'Y' or 'N'.

#### isTrusted()

```
public static function isTrusted(): bool
```

Check if the current session is trusted.
This can be useful if you want to force a password check before granting
access to certain pages. The duration of trust can be set by calling
the Session::setTrusted() method.

#### isValid()

```
public static function isValid(int $member_srl = 0): bool
```

Check if the current session is valid for a given member_srl.
The session can be invalidated by password changes and other user action.

#### getMemberSrl()

```
public static function getMemberSrl(): int
```

Get the member_srl of the currently logged in member.
This method returns an integer, or zero if nobody is logged in.

#### getMemberInfo()

```
public static function getMemberInfo(bool $refresh = false): Rhymix\Framework\Helpers\SessionHelper
```

Get information about the currently logged in member.
This method returns an object, or false if nobody is logged in.

#### setMemberInfo()

```
public static function setMemberInfo(Rhymix\Framework\Helpers\SessionHelper $member_info): void
```

Set the member info.
This method is for debugging and testing purposes only.

#### getLanguage()

```
public static function getLanguage(): string
```

Get the current user's preferred language.
If the current user does not have a preferred language, this method
will return the default language.

#### setLanguage()

```
public static function setLanguage(string $language): void
```

Set the current user's preferred language.

#### getTimezone()

```
public static function getTimezone(): string
```

Get the current user's preferred time zone.
If the current user does not have a preferred time zone, this method
will return the default time zone for display.

#### setTimezone()

```
public static function setTimezone(string $timezone): void
```

Set the current user's preferred time zone.

#### getDomain()

```
public static function getDomain(): string
```

Get session domain.

#### setDomain()

```
public static function setDomain(string $domain): bool
```

Set session domain.

#### setTrusted()

```
public static function setTrusted(int $duration = 300): bool
```

Mark the current session as trusted for a given duration.
See isTrusted() for description.

#### getGenericToken()

```
public static function getGenericToken()
```

Get a generic token that is not restricted to any particular key.

#### createToken()

```
public static function createToken(string $key = ''): string
```

Create a token that can only be verified in the same session.
This can be used to create CSRF tokens, etc.
If you specify a key, the same key must be used to verify the token.

#### verifyToken()

```
public static function verifyToken(
    string $token,
    string $key = '',
    bool $strict = true
): bool
```

Verify a token.
This method returns true if the token is valid, and false otherwise.
Strict checking can be disabled if the user is not logged in
and no tokens have been issued in the current session.

#### invalidateToken()

```
public static function invalidateToken(string $token): bool
```

Invalidate a token so that it cannot be verified.

#### getLoginStatus()

```
public static function getLoginStatus(): string
```

Get a string that identifies login status.
Members are identified by a hash that is unique to each member.
Guests are identified as 'none'.

#### getLastLoginTime()

```
public static function getLastLoginTime(): int
```

Get the last login time.
If the user is not logged in, this method returns 0.

#### getValidityInfo()

```
public static function getValidityInfo(int $member_srl): object
```

Get validity information.

#### setValidityInfo()

```
public static function setValidityInfo(
    int $member_srl,
    object $validity_info
): bool
```

Set validity information.

#### encrypt()

```
public static function encrypt(string $plaintext): string
```

Encrypt data so that it can only be decrypted in the same session.
Arrays and objects can also be encrypted. (They will be serialized.)
Resources and the boolean false value will not be preserved.

#### decrypt()

```
public static function decrypt(string $ciphertext)
```

Decrypt data that was encrypted in the same session.
This method returns the decrypted data, or false on failure.
All users of this method must be designed to handle failures safely.

#### setAutologinKeys()

```
public static function setAutologinKeys(
    string $autologin_key,
    string $security_key
): bool
```

Set autologin key.

#### destroyAutologinKeys()

```
public static function destroyAutologinKeys(): bool
```

Destroy autologin keys.

#### destroyOtherSessions()

```
public static function destroyOtherSessions(int $member_srl): bool
```

Destroy all other autologin keys (except the current session).

#### destroyCookiesFromConflictingDomains()

```
public static function destroyCookiesFromConflictingDomains(
    array $cookies,
    bool $include_current_host = false
): bool
```

Destroy cookies from potentially conflicting domains.
