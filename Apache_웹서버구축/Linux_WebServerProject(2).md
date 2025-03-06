## 웹 서버 운영
Apache를 통해 웹 서버를 구축했기 때문에, 이후 추가적인 작업을 해보려 한다.

### 1️⃣ 정적 웹 페이지 적용

- 나의 Apache 폴더의 아래 경로로 이동한다.
```bash
sudo /opt/homebrew/var/www   
```
- `www 폴더` 내부에 따로 작성한 `html`, `css` 파일을 넣어준다. 
- 이후 나의 로컬호스트로 접속하면
![](https://velog.velcdn.com/images/ghkdehs/post/86098070-a5f3-4b8b-a08b-8da2ad83cf64/image.png)

정상적으로 적용이 된 것을 볼 수 있다.

> 이러한 작업으로 웹서버의 기본 동작 원리를 알 수 있는데, `클라이언트에게 이러한 파일들을 제공하는 역할` 을 한다.

---

### 2️⃣ 로컬 네트워크 공유
지금 나의 Apache 서버를 **같은 Wi-Fi에 있는 다른 기기(스마트폰, 태블릿, 노트북)** 에서도 보이게 만들 것이다.

- 터미널에서 현재 로컬 IP 주소를 확인한다.
```bash
ipconfig getifaddr en0
```
위 명령어를 실행하면  Wi-Fi로 연결된 로컬 IP가 출력될 것이다.

> 출력 예시: 192.168.0.10 같은 형식

 유선 연결이라면 `en0` 대신 `en1` 일 수 있다.

- 이후 `같은 네트워크(Wi-Fi)`에 연결된 스마트폰 혹은 다른 PC의 브라우저를 연 뒤.
- 주소창에 **http://[로컬 IP]** 를 입력하면

![](https://velog.velcdn.com/images/ghkdehs/post/1d5f8a12-0366-41c0-9be9-0afdc924009d/image.png)

동작원리
- 로컬 네트워크에서 연결된 기기들은 라우터(또는 스위치)가 연결된 네트워크 상의 같은 공간에 있다.

- 라우터는 로컬 네트워크 내에서만 데이터가 오가도록 하며, 이 안에 있는 기기들은 서로 데이터를 주고받을 수 있다.
  
- 따라서 **웹 서버(아파치)** 가 내 컴퓨터에서 실행 중이라면, 같은 네트워크에 있는 다른 기기들은 내 컴퓨터의 IP 주소를 통해 요청을 보낼 수 있다.

> 다시 한 번 강조하자면 이 과정에서 Apache는 나의 어떠한 `파일` 을 `웹 페이지 형태`로 클라이언트에게 보여주는 역할을 한다. 

---

### 3️⃣ 가상호스트 설정

### 가상 호스트
`가상 호스트(Virtual Host)`는 하나의 서버에서 여러 개의 도메인을 처리할 수 있도록 해주는 설정이다. 

예를 들어, 하나의 Apache 서버가 example1.com과 example2.com 두 도메인에 대해 각각 다른 웹사이트를 보여줄 수 있게 해주는 것을 의미한다.

Apache 서버는 들어오는 요청을 도메인이나 IP에 맞춰서 어느 디렉토리의 파일을 보여줄지 결정할 수 있어. 이걸 가능하게 해주는 게 바로 가상 호스트 설정이야.

### 1. 가상호스트 설정 추가
```bash
sudo nano /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf   
```
위 명령어를 통해 가상호스트 설정 파일을 열어준다.

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.local
    DocumentRoot "/opt/homebrew/var/www/example1"
    ServerName example1.local
    ErrorLog "/opt/homebrew/var/log/httpd/example1-error_log"
    CustomLog "/opt/homebrew/var/log/httpd/example1-access_log" common
</VirtualHost>

<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.local
    DocumentRoot "/opt/homebrew/var/www/example2"
    ServerName example2.local
    ErrorLog "/opt/homebrew/var/log/httpd/example2-error_log"
    CustomLog "/opt/homebrew/var/log/httpd/example2-access_log" common
</VirtualHost>
```

설정을 열면 위와 같은 가상호스트를 설정할 수 있는 부분이 있는데, 아래 세 가지를 수정해준다.
- `포트번호`를 `80` 으로 설정하는 것이 좋음 (만약 포트번호를 달리 설정한다면 Apache 파일 내부의 `Listen`을 포트번호에 맞게 바꿔주어야 한다.)
- `DocumentRoot`를 웹사이트 루트 디렉토리로 설정한다.
- `ServerName`을 도메인 이름을 설정한다. (원하는 이름으로 수정가능)
  
### 2. Apache 파일에서 `httpd-vhosts.conf` 활성화
- `httpd.conf` 파일로 들어가서

```bash
# Virtual hosts
# Include /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf
```
위와 같이 작성된 부분을 찾을 수 있을 것이다. 여기서 Include 부분의 주석을 해제해 `httpd-vhosts.conf` 파일을 활성화한다.

### 3.가상호스트 루트 디렉토리 생성
- 2번의 설정 파일에서 `DocumentRoot`가 지정한 디렉토리가 없으면 아래와 같이 생성해준다.

```bash
mkdir -p /opt/homebrew/var/www/example1
mkdir -p /opt/homebrew/var/www/example2
```
- 디렉토리 생성 후 내부에 `index.html`파일을 넣어주어야 홈페이지 화면이 출력된다. 없을 경우 `index of/` 라는 현상이 발생할 수 있음

### 4.퍼미션 확인
- Apache가 해당 디렉토리에 접근할 수 있도록 적절한 퍼미션이 설정되어 있어야 한다.
```bash
sudo chown -R _www:_www /opt/homebrew/var/www/example1
sudo chown -R _www:_www /opt/homebrew/var/www/example2
```

### 5. `/etc/hosts` 파일 수정
- `example1.local`과 `example2.local`을 로컬에서 사용하려면 `/etc/hosts` 파일을 수정해서 해당 도메인이 127.0.0.1로 연결되도록 해야한다.

```bash
sudo nano /etc/hosts
```
- 파일에 아래를 추가한다.
```bash
127.0.0.1 example1.local
127.0.0.1 example2.local
```

### 6. Apache 재시작
```bash
sudo apachectl restart
```
모든 작업을 완료하고 재시작하여 브라우저에서 **http://example1.local** 과 **http://example2.local** 을 입력하면 정상적으로 페이지가 출력되는 것을 확인할 수 있다.

---

### 4️⃣ 기초 보안 설정
### 1. 디렉토리 리스팅 차단
- 목적 : 디렉토리 목록(파일 리스트)이 노출되는 걸 방지
- 디렉토리 리스팅 차단을 하지 않으면 관리자 외의 웹 이용자들도 특정 경로 입력 시 디렉토리 리스트(설정 파일, 백업 파일, API Key 등)를 확인할 수 있게 된다.

1. 우선 테스트를 하기 위해 `testdir` 디렉토리를 생성
```bash
sudo mkdir /opt/homebrew/var/www/testdir
```

2. 여기서 나는 가상호스트 설정 때문에 `testdir`로 접속시 `NotFound` 오류가 발생해 가상호스트 관련 설정을 모두 주석처리 했다.
```bash
sudo nano /opt/homebrew/etc/httpd/httpd.conf
```
2.1 Apache 기본 설정에 들어가서
```bash
Include /opt/homebrew/etc/httpd/extra/httpd-ssl.conf
```
위 명령어를 주석처리 한다.

2.2 이후 `도메인 매핑(로컬 호스트 이름 해석)` 설정 파일을 열고,

```bash
sudo nano /etc/hosts
```
가상호스트 파트에서 설정한 local1,2 로컬호스트 주소를 주석처리 한다.
```bash
127.0.0.1    example1.local
127.0.0.1    example2.local
```

3. 그리고 디렉토리 파일을 볼 수 있는지 확인하기 위해 위 설정들을 적용 후 `http://localhost/testdir/` 주소로 이동하면 

![](https://velog.velcdn.com/images/ghkdehs/post/42c75894-7f81-44b3-9fea-f725617d944b/image.png)
디렉토리를 읽을 수 있다.


3.1 다시 돌아와서 Apache 기본 파일로 이동한다.
```bash
sudo nano /opt/homebrew/etc/httpd/httpd.conf
```
3.2 `Options Indexes FollowSymLinks` 부분을 아래와 같이 수정한다.
```bash
<Directory "/opt/homebrew/var/www">
    Options -Indexes +FollowSymLinks
</Directory>
```
- `-Indexes`은 디렉토리 리스팅을 차단하여 디렉토리 목록을 볼 수 없게 지정하는 것이고, `FoloowSymLinks`는 심볼릭 링크를 따라가도록 설장하는 것이다.

- 만약 Indexes에만 기호 `-`를 붙였다면 Apache 기호 규칙에 따라 오류가 발생한다.

> 모든 옵션은 + 또는 **-** 로 시작하거나, 아예 아무 것도 붙이지 않은 형태로 설정해야 한다.

3.3 다시 `testdir/`로 이동하면 `디렉토리 리스팅(목록 보기) 차단`이 된 것을 알 수 있다.
![](https://velog.velcdn.com/images/ghkdehs/post/f5eb2ed5-e0d4-47a9-bbd6-43b8ff16722b/image.png)


### 2. 접근 제한 설정
특정 디렉토리나 IP에서만 접근할 수 있도록 제한하는 방법
1. 