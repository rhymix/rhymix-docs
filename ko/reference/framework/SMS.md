Rhymix\Framework\SMS
--------------------

#### setDefaultDriver()

```
public static function setDefaultDriver(Rhymix\Framework\Drivers\SMSInterface $driver): void
```

Set the default driver.

#### getDefaultDriver()

```
public static function getDefaultDriver(): Rhymix\Framework\Drivers\SMSInterface
```

Get the default driver.

#### addDriver()

```
public static function addDriver(Rhymix\Framework\Drivers\SMSInterface $driver): void
```

Add a custom mail driver.

#### getSupportedDrivers()

```
public static function getSupportedDrivers(): array
```

Get the list of supported mail drivers.

#### __construct()

```
public function __construct()
```

The constructor.

#### setFrom()

```
public function setFrom(string $number): bool
```

Set the sender's phone number.

#### getFrom()

```
public function getFrom(): ?string
```

Get the sender's phone number.

#### addTo()

```
public function addTo(
    string $number,
    string $country = '0'
): bool
```

Add a recipient.

#### getRecipients()

```
public function getRecipients(): array
```

Get the list of recipients without country codes.

#### getRecipientsWithCountry()

```
public function getRecipientsWithCountry(): array
```

Get the list of recipients with country codes.

#### getRecipientsGroupedByCountry()

```
public function getRecipientsGroupedByCountry(): array
```

Get the list of recipients grouped by country code.

#### setSubject()

```
public function setSubject(string $subject): bool
```

Set the subject.

#### getSubject()

```
public function getSubject(): string
```

Get the subject.

#### setTitle()

```
public function setTitle(string $subject): bool
```

Set the subject (alias to setSubject).

#### getTitle()

```
public function getTitle(): string
```

Get the subject (alias to getSubject).

#### setBody()

```
public function setBody(string $content): bool
```

Set the content.

#### getBody()

```
public function getBody(): string
```

Get the content.

#### setContent()

```
public function setContent(string $content): bool
```

Set the content (alias to setBody).

#### getContent()

```
public function getContent(): string
```

Get the content (alias to getBody).

#### attach()

```
public function attach(
    string $local_filename,
    ?string $display_filename = null
): bool
```

Attach a file.

#### getAttachments()

```
public function getAttachments(): array
```

Get the list of attachments to this message.

#### setExtraVar()

```
public function setExtraVar(
    string $key,
    $value
): void
```

Set an extra variable.

#### getExtraVar()

```
public function getExtraVar(string $key)
```

Get an extra variable.

#### getExtraVars()

```
public function getExtraVars(): array
```

Get all extra variables.

#### setExtraVars()

```
public function setExtraVars(array $vars): void
```

Set all extra variables.

#### setDelay()

```
public function setDelay(int $when): bool
```

Delay sending the message.
Delays (in seconds) less than 1 year will be treated as relative to the
current time. Greater values will be interpreted as a Unix timestamp.
This feature may not be implemented by all drivers.

#### getDelay()

```
public function getDelay(): int
```

Get the Unix timestamp of when to send the message.
This method always returns a Unix timestamp, even if the original value
was given as a relative delay.
This feature may not be implemented by all drivers.

#### forceSMS()

```
public function forceSMS(): void
```

Force this message to use SMS (not LMS or MMS).

#### unforceSMS()

```
public function unforceSMS(): void
```

Unforce this message to use SMS (not LMS or MMS).

#### isForceSMS()

```
public function isForceSMS(): bool
```

Check if this message is forced to use SMS.

#### allowSplitSMS()

```
public function allowSplitSMS(): void
```

Allow this message to be split into multiple SMS.

#### allowSplitLMS()

```
public function allowSplitLMS(): void
```

Allow this message to be split into multiple LMS.

#### disallowSplitSMS()

```
public function disallowSplitSMS(): void
```

Disallow this message to be split into multiple SMS.

#### disallowSplitLMS()

```
public function disallowSplitLMS(): void
```

Disallow this message to be split into multiple LMS.

#### isSplitSMSAllowed()

```
public function isSplitSMSAllowed(): bool
```

Check if splitting this message into multiple SMS is allowed.

#### isSplitLMSAllowed()

```
public function isSplitLMSAllowed(): bool
```

Check if splitting this message into multiple LMS is allowed.

#### send()

```
public function send(bool $sync = false): bool
```

Send the message.

#### sendAsync()

```
public static function sendAsync(self $sms): void
```

Send an SMS asynchronously (for Queue integration).

#### isSent()

```
public function isSent(): bool
```

Check if the message was sent.

#### getCaller()

```
public function getCaller(): string
```

Get caller information.

#### getErrors()

```
public function getErrors(): array
```

Get errors.

#### addError()

```
public function addError(string $message): void
```

Add an error message.
