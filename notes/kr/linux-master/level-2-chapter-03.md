---
layout: article
title: 3. 기본 명령어
permalink: /notes/kr/linux-master/level-2-chapter-03
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
---

## 3.1 사용자 생성 및 계정 관리
### 3.1.1 사용자 관련 명령어

* `[root@localhost ~]# useradd [options] <accountName>`
    - `-c` `--comment` 사용자 계정에 설명 추가
    - `-d` `--home-dir` 사용자 계정의 홈디렉터리 설정
    - `-e` `--expiredate` 사용자 계정의 유효 기간 설정
    - `-f` `--inactive` 패스워드가 만료된 후 계정의 만료 날짜 지정
    - `-G` `--groups` 사용자가 속하게 될 그룹 지정
    - `-s` `--shell` 사용자 계정의 로그인 기본 셸 지정
* `[root@localhost ~]# passwd [options] <accountName>`
    - `-d` `--delete` 사용자 계정의 패스워드 삭제
    - `-l` `--lock` 사용자 계정을 잠금
    - `-S` `--status` 계정 상태 출력
        + PS: 정상
        + NP: 패스워드 미설정
        + LK: 잠금 상태 또는 NP 상태
    - `-u` `--unlock` 잠금 상태의 사용자 계정을 해제
* `[root@localhost ~]# su [options] [-] [<user> [<argument>...]]` Switch User
    - `-` `-l` `--login` 사용자의 환경변수를 적용하여 로그인
    - `-c` `--command` 셸을 실행하지 않고, 주어진 명령어 실행
    - `-s` `--shell` 지정된 셸로 로그인

### 3.1.2 사용자 관련 파일

* **/etc/default/useradd:** 👩사용자 계정 생성 시 가장 먼저 참조
    - 👥**GROUP:** 새로 사용자 계정 생성 시 기본적으로 소속될 GID 지정
    - 🏡**HOME:** 새로 사용자 계정 생성 시 홈 디렉터리 지정
    - **INACTIVE:** 새로 사용자 생성 시 패스워드 사용 기간이 만료된 후 계정이 사용 불가능하게 되는 날짜 지정 (’0’이면 사용 불가능하며 ‘-1’이면 무한대)
    - 📅**EXPIRE:** 새로 사용자 계정 생성 시 패스워드 만료 날짜 지정
    - **🐚SHELL:** 새로 사용자 계정 생성 시 기본 셸 지정
    - **SKEL:** 새로 사용자 계정 생성 시 추가되는 홈 디렉터리에 복사할 파일들이 있는 디렉터리의 경로
    - **📬CREATE_MAIL_SPOOL:** 새로 생성되는 사용자 계정의 메일함 생성 여부 지정
* **/etc/login.defs:** 👨사용자 계정 생성 시 두 번째로 참조하는 파일로 기본값을 정의
    - **💌MAIL_DIR:** 메일 디렉터리 지정
    - **🗝️PASS_MAX_DAYS:** 패스워드 만료일
    - 🔑**PASS_MIN_DAYS:** 패스워드 변경 후 다시 변경할 수 있는 최소 일 수
    - **🔒PASS_MIN_LEN:** 패스워드 최소 길이
    - **PASS_WARN_AGE:** 설정된 일 수가 남았을 때, 패스워드 만료 경고 메시지를 보냄
    - **UID_MIN:** 사용자에게 할당할 수 있는 최소 UID 번호
    - **UID_MAX:** 사용자에게 할당할 수 있는 최대 UID 번호
    - **🏠CREATE_HOME:** 홈 디렉터리 생성 여부
    - **UMASK:** 사용자 계정 생성 시 홈 디렉터리의 UMASK 값을 설정
    - **USERGROUPS_ENAB:** ‘userdel’ 명령 시 사용자가 없는 그룹을 삭제할 것인지 여부
    - **ENCRYPT_METHOD:** 암호화 방법
* **/etc/skel:** ‘/etc/default/useradd’ 파일에서 ‘SKEL’의 값은 ‘/etc/skel’ 디렉터리를 의미. ’useradd’ 명령을 사용하면 ‘/etc/skel’ 디렉터리에 있는 파일들이 새롭게 생성되는 사용자 홈 디렉터리에 복사됨
    - **.bash_logout:** 사용자가 로그아웃하기 바로 직전에 실행하는 프로그램에 관한 배시의 지역적인 시스템 설정과 관련된 파일
    - **.bash_profile:** 환경변수와 bash가 수행될 때 실행되는 프로그램을 제어하는 지역적인 시스템 설정과 관련된 파일
    - **.bashrc:** 별칭(alias)과 bash가 수행될 때 실행되는 함수를 제어하는 지역적인 시스템 설정과 관련된 파일
* **/etc/passwd:** 사용자 계정 정보를 저장하고 있는 파일로 로그인 시 사용. 7개의 필드로 구성되어 있으며 ‘:’(colon)으로 구분
    - **username : password : uid : gid : comment : homedirectory : shell**
* **/etc/shadow:** 사용자의 패스워드가 저장되어 있는 파일. 9개의 필드로 구성되어 있으며 ‘:’(colon)으로 구분
    - **username : password : lastchange : mindays : maxdays : warndays : inactive : expire : flag**

### 3.1.3 사용자 계정 관리

* `[root@localhost ~]# usermod [options] [values] <accountName>`
    - `-c` `--comment` 설명 변경
    - `-d` `--home-dir` 👤사용자 홈디렉터리 변경
    - `-e` `--expiredate` 계정 만료일 변경
    - `-f` `--inactive` 사용자 계정 유효일 지정
    - `-g` `--group` GID 변경
    - `-G` `--groups` 지정한 그룹에 사용자 추가
    - `-s` `--shell` 로그인 시 사용할 기분 셸 지정
    - `-u` `--uid` UID 변경
* `[root@localhost ~]# usermod [options] <accountName>`
    - `-r` `--remove` ’/etc/passwd’, ‘/etc/shadow’, ‘/etc/group’ 파일의 정보와 ‘/var/spool/mail/계정 파일’과 홈디렉터리의 모든 내용을 삭제

### 3.1.4 사용자 정보 조회 명령어

* `[root@localhost ~]# users` 💻시스템에 로그인한 사용자 정보 출력
* `[root@localhost ~]# w` 시스템에 로그인한 사용자 정보를 자세히 출력
* `[root@localhost ~]# who [options] [file | arg1 arg2]`
    - `-a` `--all` 모든 정보 출력
    - `-H` `--Heading` 각 필드의 제목과 함께 출력
    - `-u` `--users` 사용자의 Idle Time🕜 확인
* `[root@localhost ~]# whoami` 시스템에 로그인한 🧚🏿‍♀️사용자 정보 출력
* `[root@localhost ~]# id` 시스템에 로그인한 사용자의 uid, gid, group 정보 출력

## 3.2 그룹 생성 및 그룹 관리
### 3.2.1 그룹 관리 명령어

* `[root@localhost ~]# gruopadd [options] [groupName]` 👥그룹을 생성
    - `-g` GID 지정
    - `-o` GID 중복을 허용
    - `-r` 시스템 그룹 생성 시 사용
* `[root@localhost ~]# gruopdel [groupName]` 👩‍👩‍👧‍👧그룹을 삭제
* `[root@localhost ~]# gruopmod [options] [groupName]` 👩‍👧‍👦그룹을 변경
    - `-g` GID 변경
    - `-n` 그룹명 변경

### 3.2.2 그룹 관련 파일

* **/etc/group:** 👥사용자 그룹 정보가 저장된 파일
    - **groupName : password : gid : members**
* **/etc/gpasswd:** 👥그룹의 패스워드가 암호화되어 저장되어 있는 파일
    - **groupName : password : owner : members**

### 3.2.3 그룹 정보 조회 명령어

* `[root@localhost ~]# group` 현재 사용자의 그룹 정보를 확인

## 3.3 디렉터리 및 파일

### 3.3.1 디렉터리 관련 명령어

* `[root@localhost ~]# mkdir [options] [directoryName]` 디렉터리를 생성
    - `-m` 디렉터리 권한 설정
    - `-p` 하위 디렉터리를 한 번에 생성
    - `-v` 디렉터리 생성 후 디렉터리 정보 출력
* `[root@localhost ~]# rmdir [directoryName]` 디렉터리를 삭제
* `[root@localhost ~]# cd [directory]` 디렉터리 이동
    - `~` 사용자 홈 디렉터리
    - `.` 현재 디렉터리
    - `..` 한 단계 상위 디렉터리
* `[root@localhost ~]# pwd [options]` 현재 작업 중인 디렉터리를 출력
    - `-L` 현재 작업 중인 디렉터리가 심볼릭 링크일 경우 심볼릭 링크의 경로를 표시
    - `-P` 현재 작업 중인 디렉터리가 심볼릭 링크일 경우 원래 디렉터리의 경로를 표시

### 3.3.2 파일 관련 명령어

* `[root@localhost ~]# ls [options] [directoryName]` 디렉터리 안의 파일 또는 디렉터리 목록을 출력
    - `-a` 숨김 파일을 포함한 모든 목록을 출력
    - `-d` 디렉터리 목록만 출력
    - `-F` 파일이 디렉터리인 경우 /, 실행 파일인 경우 *, 소켓 파일인 경우 =, 심볼릭 링크인 경우 @를 파일 뒤에 표시하여 출력
    - `-l` 파일의 모드, 링크 수, 소유자, 크기 등을 상세하게 출력
    - `-m` 쉼표로 구분하여 출력
    - `-r` 알파벳 역순으로 정렬하여 출력
    - `-R` 하위 디렉터리 목록까지 순차적 출력
    - `-s` 킬로바이트 단위로 정렬하여 출력
    - `-t` 최종 수정 시간을 기준으로 정렬하여 출력
* `[root@localhost ~]# cp [options] [target] [target]` 파일이나 디렉터리 복사
    - `-b` 동일한 파일명이 존재할 경우 복사본 생성
    - `-f` 동일한 파일명이 존재할 경우 덮어쓰기 여부 확인 없이 덮어씀
    - `-i` 동일한 파일명이 존재할 경우 덮어쓰기 여부 확인 후 덮어씀
    - `-p` 원본과 동일한 모드, 소유자, 시간 정보를 유지하여 복사
    - `-P` 원본이 디렉터리 경로와 함께 지정되었을 경우 지정된 디렉터리 경로를 포함하여 복사
    - `-S` 동일한 파일명이 존재할 경우 백업 파일의 확장자를 지정
    - `-u` 동일한 파일명이 존재할 경우 원본 파일이 대상 파일보다 최신 파일일 경우에만 복사
* `[root@localhost ~]# mv [options] [source] [target]` 파일이나 디렉터리 이동, 파일이나 디렉터리 이름 변경
    - `-b` 동일한 파일명이 존재할 경우 복사본 생성
    - `-f` 동일한 파일명이 존재할 경우 덮어쓰기 여부 확인 없이 덮어씀
    - `-i` 동일한 파일명이 존재할 경우 덮어쓰기 여부 확인 후 덮어씀
    - `-u` 동일한 파일명이 존재할 경우 원본 파일이 대상 파일보다 최신 파일일 경우에만 이동
    - `-v` 과정을 출력
* `[root@localhost ~]# rm [options] [file/directory]` 파일이나 디렉터리 삭제
    - `-d` 디렉터리 삭제
    - `-f` 삭제 여부 묻지 않고 삭제
    - `-i` 삭제 여부 묻고 삭제
    - `-r` 하위 디렉터리를 포함하여 모두 삭제
    - `-v` 과정을 출력
* `[root@localhost ~]# touch [options] [value] [file/directory]` 크기가 0Byte인 빈 파일을 생성, 현재 시간으로 파일의 타임 스탬프를 변경
    - `-a` 현재 시간으로 파일의 접근 시간, 변경 시간 수정
    - `-c` 기존에 파일이 없으면 파일이 생성되지 않음
    - `-d` 지정한 시간으로 접근 시간, 수정 시간이 수정
    - `-m` 현재 시간으로 파일의 수정 시간, 변경 시간 수정
    - `-r` 지정한 파일의 접근 시간, 수정 시간으로 파일을 수정
    - `-t` 지정한 시간으로 접근 시간, 수정 시간이 수정
* `[root@localhost ~]# file [options] [file/directory]` 파일의 유형 및 속성 확인
    - `-b` 파일명을 제외하고 유형만 출력
    - `-f` 파일 목록에서 지정한 파일들에 대해서만 명령 실행
    - `-i` MIME 타입 문자로 출력
    - `-L` 심볼릭 링크의 원본 파일을 추적하여 출력
    - `-z` 압축 파일의 내용 출력
* `[root@localhost ~]# find [path] [options] [arg1, arg2 ...]` 조건에 맞는 파일이나 디렉터리를 검색하여 경로 출력
    - `-delete` 검색된 파일 또는 디렉터리 삭제
    - `-empty` 크기가 0인 파일 또는 빈 디렉터리 검색
    - `-exec` 검색된 파일에 대해 명령 실행
    - `-name` 문자열 패턴을 기준으로 검색
    - `-print` 검색 결과 출력, 검색 항목은 새로운 행으로 구분
    - `-size` 파일 크기를 기준으로 검색
    - `-type` 파일 유형을 기준으로 검색
    - `-atime` 접근 시각을 기준으로 검색
    - `-ctime` 속성 변경 시각을 기준으로 검색
    - `-mtime` 데이터 수정 시각을 기준으로 검색
* `[root@localhost ~]# locate [options] [fileName]` 파일의 위치를 출력
    - `-e` 검색에서 제외할 디렉토리 지정
    - `-n` 지정한 개수만큼 출력
* `[root@localhost ~]# whereis [keyword]` 명령어 바이너리, 소스, 매뉴얼 파일 위치 검색
* `[root@localhost ~]# which [keyword]` 명령서 실행 파일의 위치 검색

### 3.3.3 파일 내용 출력 명령어

* `[root@localhost ~]# cat [options] [fileName]` 파일의 내용 출력, 파일의 내용 합치기
    - `-b` 화면 왼쪽에 행 번호 표시 (빈 행 제외)
    - `-e` 제어 문자를 ‘^’ 형태로 출력, 각 행의 마지막에 ‘$’ 문자 추가
    - `-n` 화면 왼쪽에 행 번호 표시 (빈 행 포함)
    - `-s` 2개 이상의 연속되는 빈 행을 한 행으로 출력
* `[root@localhost ~]# head [options] [value] [fileName]` 파일의 첫 행부터 지정한 만큼 출력
    - `-c` Kbyte 단위로 지정하여 출력
    - `-n` ’n’ 행까지 출력
    - `-q` 파일의 이름을 함께 출력하지 않음
    - `-v` 파일의 이름을 함께 출력
* `[root@localhost ~]# tail [options] [value] [fileName]` 파일의 마지막 행부터 지정한 만큼 출력
    - `-c` Kbyte 단위로 지정하여 출력
    - `-f` 10줄을 실시간으로 출력
    - `-F` 10줄을 실시간으로 출력, 특정 시간 이후 파일 변동이 있으면 새로운 파일을 출력
    - `-n` ’n’ 행까지 출력
    - `-n +n` ’+n’ 행 이후부터 출력
    - `--byte` byte 단위로 지정하여 출력
* `[root@localhost ~]# more [fileName]` 파일의 내용을 화면 단위로 출력, 위에서 아래로만 이동 가능
    - `b` 한 화면씩 이동
    - `n:/검색어` 문자열을 검색
    - `q` 종료
    - `v` 현재 위치에서 vi 편집기 실행
    - `=` 현재 위치 행 번호 출력
    - Enter 한 행씩 이동
    - Space Bar 한 화면씩 이동
* `[root@localhost ~]# less [fileName]` 파일의 내용을 화면 단위로 출력, 위 · 아래로 이동 가능
    - `-c` 전체화면 갱신
    - `-i` 대·  소문자 구분하여 검색
    - `-s` 2개 이상의 연속되는 빈 행을 한 행으로 출력
    - `-x` 수치를 지정해서 탭 간격 조정
    - `q` 종료
    - 행 번호 지정된 행 다음부터 출력
    - ↑ 한 행씩 위로 이동
    - ↓ 한 행씩 아래로 이동
    - Page Up 한 화면씩 위로 이동
    - Page Down 한 화면씩 아래로 이동
    - Enter 한 행씩 아래로 이동
    - Space Bar 한 화면씩 아래로 이동
* `[root@localhost ~]# grep [string] [options] [fileName]` 파일 안에서 지정한 패턴 또는 문자열을 검색하여 그 결과를 출력, 한 디렉터리 안에서 지정한 패턴 또는 문자열을 포함하는 파일을 출력 (`grep` 다중 패턴 검색 / `egrep` 정규 표현식 검색 / `fgrep` 단순 패턴 검색)
    - `-c` 패턴이 일치하는 행의 수 출력
    - `-i` 대·  소문자 구분하여 검색
    - `-I` 패턴이 포함된 파일 이름 출력
    - `-n` 행 번호 출력
    - `-v` 지정한 패턴과 일치하지 않는 행만 출력
    - `-w` 패턴이 전체 문자열과 일치하는 행만 출력
* `[root@localhost ~]# wc [options] [fileName]` 파일 안의 행, 문자수를 출력
    - `-c` 문자 개수 출력
    - `-I` 행 개수 출력
    - `-w` 단어 개수 출력
* `[root@localhost ~]# sort [options] [fileName]` 명령어 수행 결과 또는 파일 내용을 정렬하여 출력
    - `-b` 선행 공백 무시
    - `-c` 정렬 여부 검사
    - `-f` 대 · 소문자를 구분하지 않고 정렬
    - `-m` 정렬된 파일 병합
    - `-n` 숫자로 한정하여 정렬
    - `-o` 저장할 파일명 지정
    - `-r` 역순(내림차순)으로 정렬
    - `-R` 해시값을
    - `-t` 필드 구분자 지정
    - `-u` 정렬 후 중복된 내용 제거
* `[root@localhost ~]# cut [options] [fileName]` 파일에서 특정 필드를 출력
    - `-b` 바이트 단위 지정
    - `-c` 문자 단위 지정
    - `-d` 필드 구분자로 DELIM 사용
    - `-f` 지정한 필드만 출력
    - `-s` 필드 구분자를 포함한 행만 출력
* `[root@localhost ~]# split [options] [fileName]` 하나의 파일을 여러 개로 분할
    - `-a` 바이트 단위 지정
    - `-b` 문자 단위 지정
    - `-l` 필드 구분자로 DELIM 사용
    - `--additional-suffix` 확장자명 지정

### 3.3.4 파일 비교 명령어

* `[root@localhost ~]# diff [options] [fileName1] [fileName2]` 두 개의 파일을 비교하여 다른 내용을 출력 (return 0: 차이 없음, 1: 차이 있음, 2: ERROR)
    - `-b` 화면 왼쪽에 행 번호 표시 (빈 행 제외)
    - `-e` 제어 문자를 ‘^’ 형태로 출력, 각 행의 마지막에 ‘$’ 문자 추가
    - `-n` 화면 왼쪽에 행 번호 표시 (빈 행 포함)
    - `-s` 2개 이상의 연속되는 빈 행을 한 행으로 출력
    - `-b` 화면 왼쪽에 행 번호 표시 (빈 행 제외)
    - `-e` 제어 문자를 ‘^’ 형태로 출력, 각 행의 마지막에 ‘$’ 문자 추가
    - `-n` 화면 왼쪽에 행 번호 표시 (빈 행 포함)
    - `-s` 2개 이상의 연속되는 빈 행을 한 행으로 출력
* `[root@localhost ~]# cmp [options] [fileName1] [fileName2]` 두 개의 파일을 바이트 단위로 비교
    - `-b` 두 파일을 비교하여 다른 바이트 수 출력
    - `-i` 최초의 Skip 바이트를 무시
    - `-l` 두 파일을 비교하여 다른 문자 개수를 출력
    - `-s` 종료 코드만 출력 (return 0: 차이 없음, 1: 차이 있음, 2: ERROR)
* `[root@localhost ~]# comm [options] [fileName1] [fileName2]` 두 파일을 행 단위로 비교
    - `-1` 파일1에 있는 행은 출력하지 않음
    - `-2` 파일2에 있는 행은 출력하지 않음
    - `-3` 파일1과 파일2에 공통으로 있는 행은 출력하지 않음

### 3.3.5 리다이렉션과 파이프

* `[root@localhost ~]# cat [file1] > [file2]` 리다이렉션 : 표준 입출력을 재지정하는 기능
(표준 입력 : 키보드 / 표준 출력 : 모니터)
    - `>` 명령을 프린터나 파일로 출력 (파일이 있으면 오버라이드, 없으면 생성)
    - `>>` 지정한 파일 존재 여부에 따라 추가 또는 새로운 파일 생성
    - `<` 키보드가 아닌 지정한 파일에서 입력을 읽어 옴
    - `>&` 명령의 출력을 다른 명령의 입력으로 보냄
    - `&<` 명령의 입력을 읽고 다른 명령의 출력으로 보냄
* `[root@localhost ~]# [command1]|[command2]|[command3]` 파이프 : 여려 개의 명령어를 연결, 프로세스와 프로세스간 통로 역할
* `[root@localhost ~]# [command1];[command2];[command3]` 세미콜론 : 하나의 라인에 여려 개의 명령어를 실행 (먼저 실행된 명령어가 실패하더라도 다음 명령어를 실행)

## 3.4 기타 명령

### 3.4.1 네트워크 관련 명령어

* `[root@localhost ~]# ping [options] [IP Address/Domain]` 네트워크에 연결된 호스트와 호스트 간의 연결 상태를 확인
    - `-c` 지정한 횟수 만큼 패킷 전송
    - `-d` 소켓이 사용하는 SO_DEBUG 기능 사용
    - `-f` 다량의 패킷을 전송
    - `-i` 패킷 전송 사이 대기 시간 지정
    - `-l` 패킷을 전송할 인터페이스 지정
    - `-n` 호스트 이름을 찾지 않고 IP 주소만 찾음
    - `-r` 패킷을 로컬 인터페이스에만 보냄
* `[root@localhost ~]# traceroute [options] [IP Address/Domain]` 목적지 호스트까지 경로를 기록 및 출력, 장애 구간 파악 가능
    - `-m` 홉 수 지정
    - `-n` 호스트 이름 검색을 하지 않음
    - `-p` 시작 포트 지정
    - `-q` 패킷 수 지정
    - `-w` 타임아웃 지정
* `[root@localhost ~]# nslookup [-type=record] [IP Address/Domain]` 도메인 이름으로 IP 주소 조회 및 도메인 이름 조회
    - `a` IPv4 주소 지정
    - `aaaa` IPv6 주소 지정
    - `mx` 메일서버 지정
    - `ns` 네임서버 지정
    - `soa` 도메인에 대한 선언 부분
    - `srv` 특정 서비스에 대한 특정 도메인을 연결
    - `txt` 도메인 이름을 문자열에 매칭
    - `ptr` 역방향 질의로 IP 주소에 대한 도메인 응답
    - `cname` 별칭 지정
* `[root@localhost ~]# dig [@server] [query-type] [query-class]` 도메인 이름으로 IP 주소를 조회 및 IP 주소로 도메인 조회
    - 옵션(query-type)
        + `@server` 질의를 위한 DNS 서버를 지정 (default : ‘resolv.conf’에 있는 네임서버)
        + `domain` 질의 대상 도메인 또는 호스트 이름
        + `type` Resource Record 타입
    - 타입(type)
        + `a` IP 주소 정보
        + `any` 지정된 도메인의 모든 정보
        + `mx` 메일서버 정보
        + `ns` 네임서버 정보
        + `soa` 도메인에 대한 선언 부분
        + `hinfo` 호스트 정보
        + `txt` 도메인 이름을 문자열에 매칭
* `[root@localhost ~]# host [option] [domain/IPaddress] [DNS]` DNS 서버를 이용하여 도메인 이름에 대한 IP 주소를 획득
    - `-d` 디버깅 모드
    - `-l` zone 이하 모든 정보 출력
    - `-r` 반복 처리를 하지 않음
    - `-t` type을 지정하여 정보를 조회
        + `A` 호스트 IP 주소
        + `NS` 검색한 호스트의 네임 서버
        + `PTR` 도메인 이름 포인터
        + `ANY` Type의 모든 정보
    - `-w` DNS 서버의 응답을 기다림
* `[root@localhost ~]# hostname [option] [fileName]` 호스트 이름 확인 및 변경
    - `-a` 별칭 출력
    - `-d` 도메인 이름 출력
    - `-f` FQDN 출력
    - `-F` 지정한 파일에 호스트 이름 설정
    - `-i` 호스트의 IP 주소 출력
    - `-s` 짧은 형식의 호스트 이름 출력
    - `-y` NIS 도메인 이름 출력

### 3.4.2 기타 명령어

* `[root@localhost ~]# date [option] [MMDDhhmm[CC][YY][.ss]]`
`[root@localhost ~]# date [option] +FORMAT` 시스템의 날짜 출력 및 변경
* `[root@localhost ~]# rdate [option] [Time Server IP/domain]` 원격지 타임 서버에서 시간 정보를 가져와 로컬 시스템과 동기화
    - `-4` IPv4 주소만 사용
    - `-6` IPv6 주소만 사용
    - `-o` 지정한 포트로 연결
    - `-p` 호스트 정보만 출력, 설정은 X
    - `-s` 설정만 하고, 호스트 정보 출력은 X
    - `-u` UDP 사용
    - `-v` 상세 정보 출
* `[root@localhost ~]# cal [option] [Year]` 달력을 출력
    - `-i` 율리우스력 출력
    - `-y` 현재 연도 기준 모든 월 출력
* `[root@localhost ~]# time` 프로그램의 실행 시간을 출력
* `[root@localhost ~]# tty` 현재 사용하고 있는 단말 장치의 경로와 파일명 출력
* `[root@localhost ~]# clear` 터미널 화면을 모두 지움
* `[root@localhost ~]# wall` 현재 로그인된 모든 사용자에게 터미널 메시지 전송
* `[root@localhost ~]# write [userName] [ttyName]` 지정한 사용자에게 메시지를 전송
* `[root@localhost ~]# mesg [y|n]` 수신 메시지 여부 확인 및 제어