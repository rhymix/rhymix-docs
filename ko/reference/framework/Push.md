Rhymix\Framework\Push
---------------------

#### addDriver()

```
public static function addDriver(
    string $name,
    Rhymix\Framework\Drivers\PushInterface $driver
): void
```

Add a custom Push driver.

#### getDriver()

```
public static function getDriver(string $name): ?object
```

Get the default driver.

#### getSupportedDrivers()

```
public static function getSupportedDrivers(): array
```

Get the list of supported Push drivers.

#### __construct()

```
public function __construct()
```

The constructor.

#### setFrom()

```
public function setFrom(int $member_srl): bool
```

Set the sender's member_srl.

#### getFrom()

```
public function getFrom(): int
```

Get the sender's phone number.

#### addTo()

```
public function addTo(int $member_srl): bool
```

Add a recipient.

#### getRecipients()

```
public function getRecipients(): array
```

Get the list of recipients.

#### addTopic()

```
public function addTopic(string $topic): bool
```

Add a topic.

#### getTopics()

```
public function getTopics(): array
```

Get the list of topics.

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

#### setContent()

```
public function setContent(string $content): bool
```

Set the content.

#### getContent()

```
public function getContent(): string
```

Get the content.

#### setImage()

```
public function setImage(string $url): bool
```

Set the image.

#### getImage()

```
public function getImage(): string
```

Get the image.

#### setClickAction()

```
public function setClickAction(string $click_action): bool
```

Set an click-action to associate with this push notification.

#### getClickAction()

```
public function getClickAction(): string
```

Get the click-action associated with this push notification.

#### setSound()

```
public function setSound(string $sound): bool
```

Set a sound to associate with this push notification.

#### setBadge()

```
public function setBadge(string $badge): bool
```

Set a badge to associate with this push notification.

#### setIcon()

```
public function setIcon(string $icon): bool
```

Set an icon to associate with this push notification.

#### setTag()

```
public function setTag(string $tag): bool
```

Set a tag to associate with this push notification.

#### setColor()

```
public function setColor(string $color): bool
```

Set a color to associate with this push notification.

#### setAndroidChannelId()

```
public function setAndroidChannelId(string $android_channel_id): bool
```

Set an android-channel-id to associate with this push notification.

#### getMetadata()

```
public function getMetadata(): array
```

Get notification array

#### setData()

```
public function setData(array $data): bool
```

Set a data to associate with this push notification.

#### getData()

```
public function getData(): array
```

Get the data associated with this push notification.

#### setURL()

```
public function setURL(string $url): bool
```

Set a URL to associate with this push notification.

#### getURL()

```
public function getURL(): string
```

Get the URL associated with this push notification.

#### send()

```
public function send(bool $sync = false): bool
```

Send the message.

#### sendAsync()

```
public static function sendAsync(self $push): void
```

Send asynchronously (for Queue integration).

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

#### getSuccessTokens()

```
public function getSuccessTokens(): array
```

Get success tokens.

#### getDeletedTokens()

```
public function getDeletedTokens(): array
```

Get deleted tokens.

#### getUpdatedTokens()

```
public function getUpdatedTokens(): array
```

Get updated tokens.

#### addError()

```
public function addError(string $message): void
```

Add an error message.
