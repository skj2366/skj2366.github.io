---
layout: post
title:  "php ftp_connect undefined 임시방편?"
date:   2022-03-20 07:40:00 +0900
categories: jekyll php
---

## php 서버에서 ftp_connect 함수를 찾지 못함?
```
 ftp_connect() undefined
```

회사에서 백엔드를 laravel을 사용 중이다.
현재 유튜브 라이브 스트리밍 서비스를 새로운 프로젝트에 적용하는 작업을 하는데,
스트리밍 서버에 ftp서버를 열어두고 로컬에서 php의 ftp_connect() 함수를 이용하여 접속을 하려고 했지만 ftp_connect() undefined 오류가 난다.

이래저래 검색해서 php.ini 파일에 ftp extension을 설정 하고 다시 시도를 했지만 여전히 같은 현상이 일어난다. 
아무래도 프로젝트를 여러개 운영해서 여러가지 php를 설치해두고 사용을 해서 설정이 꼬인 것 같다.

개발 및 운영서버는 ftp extension이 제대로 적용되어 있고, 현재 로컬도 ftp_connect 함수를 사용할 수 있게 설정 해 두었지만, 
개발 일정으로 인하여 임시방편을 사용했던 방법을 기록해두려고 한다.

보내야하는 파일이 xml파일이고, xml 파일을 작성 후 그것을 그대로 스트리밍 서버로 전송하는 방식이었고
기존에 있던 파일을 보내는 것이 아닌 ftp서버에 파일을 직접 쓰는 방법으로 임시해결을 했었다.

기존과 같이 FTP서버에 접속하여 전송하는 경우 보통 이와 같은 코드를 사용할 것이다.

``` php
$ftp_server = ‘ftp서버의 ip 혹은 도메인’;
$ftp_port = 21;
$ftp_user_name = ‘’ftp;
$ftp_user_pass = ‘ftp’;
$ftp_send_file = ‘xml파일경로 및 파일명’;
$ftp_remote_file = ‘xml파일명’;

// FTP서버 접속
$connect_id = ftp_connect($ftp_server, $ftp_port);

// FTP서버 로그인
$login_result = ftp_login($connect_id, $ftp_user_name, $ftp_user_pass);

// 패시브 모드 설정
ftp_pasv($conn_id, true);

// FTP 서버에 파일 전송
if (ftp_put($connect_id, $ftp_remote_file, $ftp_send_file, FTP_BINARY)) {
    print_r(“file send complete”);
} else {
    print_r(“file send failed”);
}

// FTP 서버와 연결 끊음
ftp_close($connect_id);
```

php의 파일함수를 이용한 파일 쓰기.


``` php
// xml 파일생성
$xml = new \SimpleXMLElement('<test></test>');
$head = $xml->addChild('head');
$body =  $xml->addChild('body');

…. 생략 ….

$ftp_user_name = ‘ftp’;
$ftp_user_pass = ‘ftp’;
$ftp_server_ip = ‘ftp ip 혹은 도메인’;
$ftp_port = 21;
$fileSmilName = ‘파일명’;

$ftp_path = 'ftp://'.$ftp_user_name.':'.urlencode($ftp_user_pass).'@'.$ftp_server_ip.':'.$ftp_port.'/'.$fileSmilName;
$stream_options = array('ftp' => array('overwrite' => true));
$stream_context = stream_context_create($stream_options);

if ($fh = fopen($ftp_path, 'w', 0, $stream_context)) {
    fputs($fh, $xml->saveXML());
    print_r("[Upload] - SUCCESS");
    fclose($fh);
} else {
    print_r("[Upload] - FAIL");
    die('Could not open file.');
}
```


Php fopen 함수로 ftp서버에 직접 접근하는 방식을 이용하여 ftp서버에 직접 파일 쓰기를 진행 하였다.

물론 파일을 직접 만들지 않고 기존에 있던 파일을 보내는 것이라면 이것은 알맞는 방법이 아니라고 생각은 들고
나와 같은 상황을 겪는 사람이 없겠지만 일단 기록.

