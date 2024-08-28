---
title: "[LINUX] Swap Memory"
excerpt: "swap memory"

categories:
  - TIL
tags:
  - [TIL, LINUX]
# toc(Table of Contents.) : true시 포스트의 목차가 보임.
toc: true
# true로 해주면 목차가 스크롤을 따라 움직이게 됨.
toc_sticky: true

# date : 글 처음 작성일
date: 2024-08-28
# last_modified_at : 글 마지막 수정일
last_modified_at: 2024-08-28
---

### 정의

스왑 메모리는 물리적 RAM이 부족할 때, 하드 드라이브의 일부를 가상 메모리처럼 사용하는 방법입니다. 디스크 접근 속도 때문에 성능이 저하될 수 있습니다. 하지만, 메모리가 부족하여 시스템이 다운되거나 장애를 발생 시키는 것을 방지하기 위해 스왑메모리를 활용하여 안정성을 높일 수 있습니다.

### 가상 메모리 VS Swap 메모리

- 구현 위치

  - 가상 메모리 : 운영 체제 수준에서 구현
  - Swap 메모리 : 하드 디스크 또는 SSD에 할당된 영역

- 사용 목적
  - 가상 메모리 : 프로세스에 더 큰 메모리 공간을 제공하고 `메모리 사용량을 최적화`하는 데 중점을 둠
  - Swap 메모리 : 메모리 부족 시 비활성화된 페이지를 저장하여 `시스템의 안정성`을 유지하는 데 주로 사용

### Swap 메모리를 사용하는 이유

- 보통 AWS EC2 프리티어을 사용할 경우 메모리가 1GB라서 Jenkins, node, spring만 띄우더라도 메모리가 부족하여 멈출 수도 있다. 이를 방지하기 위해서는 스왑메모리를 설정해야한다.

### 메모리 상태 확인 명령어

```
> free -h
              total        used        free      shared  buff/cache   available
Mem:            31G         15G        1.2G        638M         14G        7.4G
Swap:          4.0G        2.7G        1.3G
```

- Memory 영역
  - total: 메모리의 총 크기
  - buff: 커널 버퍼로 사용중인 메모리
  - cache: 페이지 캐시라고 불리는 캐시 영역에 있는 메모리
  - buff/cache: 사용중인 메모리(버퍼 + 캐시)
  - used: 사용중인 메모리 (total - (free + buff/cache))
  - shared: 여러 프로세스에서 사용할 수 있는 공유 메모리
  - available: 실질적으로 사용 가능한 메모리
- Swap 영역
  - total: 설정된 swap의 총 크기
  - used: 사용중인 swap의 크기
  - free: 사용되지 않은 swap의 크기

### Swap 파일 설정

```
// swap 파일을 생성해준다. (메모리 상태 확인 시 swap이 있었지만 디렉토리 파일은 만들어줘야한다.)
$ sudo mkdir /var/spool/swap
$ sudo touch /var/spool/swap/swapfile
$ sudo dd if=/dev/zero of=/var/spool/swap/swapfile count=2048000 bs=1024

// swap 파일을 설정한다.
$ sudo chmod 600 /var/spool/swap/swapfile
$ sudo mkswap /var/spool/swap/swapfile
$ sudo swapon /var/spool/swap/swapfile

// swap 파일을 등록한다.
$ sudo vim /etc/fstab
파일이 열리면 해당 파일 아래쪽에 하단 내용 입력 후 저장
- 입력 할 수 있도록 하는 명령어 -> if
- 파일 수정 후 저장하는 명령어-> esc키 누른 후 :wq 입력 후 엔터
/var/spool/swap/swapfile    none    swap    defaults    0 0
```
