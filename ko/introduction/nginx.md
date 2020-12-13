nginx 설정 방법
---------------

아파치는 라이믹스에 포함된 `.htaccess` 파일을 그대로 사용하는 것만으로도 짧은 주소 사용이 가능하지만,
nginx는 `.htaccess` 파일을 사용한 설정 변경을 지원하지 않으므로 서버 설정을 직접 수정해야 합니다.

### Ubuntu 18.04 이상

Ubuntu에서는 최신 버전에 가까운 nginx를 제공하므로 패키지 설치를 권장합니다.

    apt-get install nginx

설치를 마치면 `/etc/nginx/sites-enabled/default` 위치에 기본 설정 파일이 생성됩니다.
이 파일의 내용을 아래와 같이 수정하거나, 같은 폴더 내에 다른 이름의 파일을 만들어 아래의 내용을 넣습니다.

    server {
        listen 80 default;    # 다른 파일을 만들어 쓰는 경우 default 삭제
        root /var/www/html;   # 라이믹스를 설치할 경로 (반드시 최상단에 위치해야 합니다.)
        server_name _;        # 사용할 도메인을 여기에 추가해도 됨 (예: server_name example.com www.example.com;)
        index index.html index.htm index.php;   # index.php가 반드시 포함되어 있어야 합니다.
        client_max_body_size 32m;   # 업로드 허용 용량 (라이믹스는 분할 업로드를 지원하므로 약 10MB만 넘으면 사실상 무한대가 됩니다.)
        
        include snippets/rhymix.conf;   # 라이믹스 rewrite 규칙 인클루드
        
        # location 구문을 사용하는 다른 설정은 반드시 라이믹스 rewrite 규칙보다 나중에 선언해야 합니다.
        location ~ \.php$ {
            fastcgi_pass unix:/run/php/php7.4-fpm.sock;    # PHP-FPM을 유닉스 소켓으로 연동하는 경우
            fastcgi_pass 127.0.0.1:9000;                   # PHP-FPM을 로컬 포트로 연동하는 경우
            include snippets/fastcgi-php.conf;
        }
    }

주의: 라이믹스 2.0부터는 rewrite 규칙에 `location / { try_files ... }` 블럭이 포함되어 있으므로
사이트 설정에 동일한 블럭을 추가하면 오류가 발생합니다.

PHP-FPM 연동 경로는 아래의 명령으로 확인할 수 있습니다. (PHP 버전에 따라 경로가 달라질 수 있습니다.)

    grep -r "listen =" /etc/php/7.4/fpm/pool.d

최근 Ubuntu에서는 nginx와 관련된 잡다한 설정파일들은 `/etc/nginx/snippets` 폴더에 넣는 것이 관례입니다.
라이믹스에서 제공하는 [rewrite 규칙](https://github.com/rhymix/rhymix/blob/master/common/manual/server_config/rhymix-nginx.conf) 파일을 다운받아
이 폴더에 `rhymix.conf`라는 이름으로 넣습니다.

    curl https://raw.githubusercontent.com/rhymix/rhymix/master/common/manual/server_config/rhymix-nginx.conf > /etc/nginx/snippets/rhymix.conf

설정을 변경한 후에는 nginx를 재시작해 주어야 합니다.

    service nginx restart

만약 재시작시 오류가 발생한다면 `/var/log/nginx/error.log` 에러 로그를 확인하시기 바랍니다.

### CentOS 7 이상

CentOS는 기본 제공되는 nginx 버전이 낮으므로 [nginx 공식 홈페이지](https://nginx.org/en/linux_packages.html)에서 제공하는
RPM 저장소를 사용하여 최신 버전을 설치하는 것이 좋습니다.

    rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
    yum install nginx

설치를 마치면 `/etc/nginx/conf.d/default.conf` 위치에 기본 설정 파일이 생성됩니다.
이 파일을 수정하거나, 같은 폴더 내에 다른 이름의 파일을 만들어서
위의 Ubuntu 설명와 같은 내용을 넣습니다.

PHP-FPM 연동 경로는 아래의 명령으로 확인할 수 있습니다.

    grep -r "listen =" /etc/php-fpm.d

CentOS에서는 `/etc/nginx/snippets` 폴더에 잡다한 설정파일들을 모으는 관례가 없으나,
`conf.d` 폴더에 넣으면 인클루드 순서가 맞지 않아 오류가 발생할 수 있으므로
Ubuntu와 마찬가지로 `snippets` 폴더를 만들어 사용하면 편리합니다.

    mkdir /etc/nginx/snippets
    curl https://raw.githubusercontent.com/rhymix/rhymix/master/common/manual/server_config/rhymix-nginx.conf > /etc/nginx/snippets/rhymix.conf

설정을 변경한 후에는 nginx를 시작해 주어야 합니다. (CentOS에서는 설치시 자동으로 시작되지 않습니다.)

    systemctl start nginx
    systemctl enable nginx

만약 재시작시 오류가 발생한다면 `/var/log/nginx/error.log` 에러 로그를 확인하시거나, 아래의 명령으로 서비스 상태를 점검하시기 바랍니다.

    systemctl status nginx
