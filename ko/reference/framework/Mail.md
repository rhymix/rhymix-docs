Rhymix\Framework\Mail
---------------------

#### setDefaultDriver()

```
public static function setDefaultDriver(Rhymix\Framework\Drivers\MailInterface $driver): void
```

Set the default driver.

#### getDefaultDriver()

```
public static function getDefaultDriver(): Rhymix\Framework\Drivers\MailInterface
```

Get the default driver.

#### addDriver()

```
public static function addDriver(Rhymix\Framework\Drivers\MailInterface $driver): void
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
public function setFrom(
    string $email,
    ?string $name = null
): bool
```

Set the sender (From:).

#### getFrom()

```
public function getFrom(): ?string
```

Get the sender (From:).

#### addTo()

```
public function addTo(
    string $email,
    ?string $name = null
): bool
```

Add a recipient (To:).

#### addCc()

```
public function addCc(
    string $email,
    ?string $name = null
): bool
```

Add a recipient (CC:).

#### addBcc()

```
public function addBcc(
    string $email,
    ?string $name = null
): bool
```

Add a recipient (BCC:).

#### getRecipients()

```
public function getRecipients(): array
```

Get the list of recipients.

#### setReplyTo()

```
public function setReplyTo(string $replyTo): bool
```

Set the Reply-To: address.

#### setReturnPath()

```
public function setReturnPath(string $returnPath): bool
```

Set the Return-Path: address.

#### setMessageID()

```
public function setMessageID(string $message_id): bool
```

Set the Message ID.

#### setInReplyTo()

```
public function setInReplyTo(string $in_reply_to): bool
```

Set the In-Reply-To: header.

#### setReferences()

```
public function setReferences(string $references): bool
```

Set the References: header.

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
public function setBody(
    string $content,
    ?string $content_type = null
): void
```

Set the body content.

#### getBody()

```
public function getBody(): string
```

Get the body content.

#### setContent()

```
public function setContent(
    string $content,
    ?string $content_type = null
): void
```

Set the body content (alias to setBody).

#### getContent()

```
public function getContent(): string
```

Get the body content (alias to getBody).

#### setContentType()

```
public function setContentType(string $type = 'text/html'): void
```

Set the content type.

#### getContentType()

```
public function getContentType(): string
```

Get the content type.

#### attach()

```
public function attach(
    string $local_filename,
    ?string $display_filename = null
): bool
```

Attach a file.

#### embed()

```
public function embed(
    string $local_filename,
    ?string $cid = null
)
```

Embed a file.

#### getAttachments()

```
public function getAttachments(): array
```

Get the list of attachments to this message.

#### send()

```
public function send(bool $sync = false): bool
```

Send the email.

#### sendAsync()

```
public static function sendAsync(self $mail): void
```

Send an email asynchronously (for Queue integration).

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
