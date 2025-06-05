Rhymix\Framework\Filters\IpFilter
---------------------------------

이 클래스는 IP 주소를 필터링하고, IP 주소 범위를 검사하는 기능을 제공합니다.
라이믹스는 IP 주소를 기반으로 방문자를 차단하거나 제재하는 기능을 제공하지 않지만,
서드파티 자료에서 이 클래스를 활용하여 다양한 기능을 구현할 수 있습니다.

#### inRange()

```
public static function inRange(
    string $ip,
    string $range
): bool
```

주어진 IP 주소가 특정 대역에 속하는지 확인합니다.
IP 대역의 표기 방법에 대해서는 `validateRange()` 메소드의 설명을 참고하세요.

#### inRanges()

```
public static function inRanges(
    string $ip,
    array $ranges
): bool
```

주어진 IP 주소가 여러 대역 중 하나에 속하는지 확인합니다.
주어진 대역 중 하나라도 일치하면 `true`를 반환합니다.

#### validateRange()

```
public static function validateRange(string $range): bool
```

이 클래스가 처리할 수 있는 유효한 형식으로 표현된 IP 대역인지 확인합니다.

라이믹스는 아래와 같은 IP 대역 표기를 지원합니다.

| 표기 방법   | 예시                        |
|-----------|-----------------------------|
| IPv4 주소 | `192.168.0.42`
| IPv4 CIDR | `192.168.0.0/24` (권장)
| IPv4 범위 | `192.168.0.1-192.168.0.255`
| IPv4 와일드카드 | `192.168.0.*`, `192.168.*.*`, `192.168.*`, `192.168.*.42`
| IPv6 주소 | `2001:0db8:85a3:0000:0000:1234:5678:abcd`
| IPv6 CIDR | `2001:0db8:85a3:0000::/64` (권장)

범위 및 와일드카드 표기는 XE 1.x에서 지원하던 것을 물려받은 것으로,
하위 호환을 위해 IPv4 범위에서만 지원합니다.

#### validateRanges()

```
public static function validateRanges(array $ranges): bool
```

배열로 주어진 다수의 IP 대역이 모두 유효한지 확인합니다.
주어진 대역이 모두 유효해야 `true`를 반환합니다.

#### getCloudFlareRealIP()

```
public static function getCloudFlareRealIP()
```

Cloudflare를 통해 접속한 사용자의 실제 IP 주소를 반환합니다.
라이믹스는 사용자의 IP 주소와 Cloudflare의 `CF-Connecting-IP` 헤더를 비교하여
실제 IP 주소를 반환합니다.

일반적으로 이 작업은 라이믹스 초기화 과정에서 이루어지므로,
이 메소드를 직접 호출할 필요는 없습니다.
변환된 IP 주소는 `RX_CLIENT_IP` 상수에 저장되며, 일반적으로 이 상수를 사용하는 것을 권장합니다.
원본 데이터가 담겨 있는 `$_SERVER['REMOTE_ADDR']`를 직접 참조하는 것은 권장하지 않습니다.