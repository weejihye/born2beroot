# born2beroot

-----
### 1. 패키지 관리 시스템
+ 패키지 : 소프트웨어의 실행 파일, 도큐먼트 파일, 설정 파일, 스크립트를 아카이브한 파일 하나
  * 리눅스에서 널리 사용되는 rpm, deb
  * .rpm : redhat 형식, CentOS, Fedora
  * .deb : Debian 형식, Debian GNU/Linux, Ubuntu
  * 레포지터리 : 패키지 파일을 모아서 배포하는 사이트
+ 패키지 단위로 소프트웨어를 설치, 삭제
+ 패키지 관리 시스템 CentOS-RPM, Debian-APT

dpkg| apt<br>(Advanced Packaging Tool)| aptitude
---|---|---
. |`CLI`만 제공<br> 여러가지 APT(cache, mark, get)에서 자주 사용한 옵션만 추출하여 사용하기 편하게 만든 것 | `GUI` `CLI` 모두 제공<br> apt-get을 모두 대체 가능
데비안 패키지 관리 시스템의 기초가 되는 소프트웨어, apt보다 낮은 수준에서 수행<br>* dpkg -l : 패키지 목록 보기<br>+ dpkg -L [패키지명] : 특정 패키지에 설치된 모든 파일 표시<br>+ dpkg -s [패키지명] : 주어진 패키지의 상태를 봄 |단순, 설치 삭제 > 보수, 유지(업데이트 등)이 어려움 | apt와 atp-cache를 통합하여 설치, 삭제 뿐만 아니라 쓰지 않는 패키지 삭제, 업데이트 자동화 등 패키지 관리가 자동화된 매니저
 + AppArmor
  * 시스템 관리자가 프로그램 프로파일별로 프로그램의 역량을 제한할 수 있게 해주는 리눅스 커널 보안 모듈
    - 네트워크 엑세스 권한
    - raw 소켓 액세스 권한
    - 파일의 읽기, 쓰기, 실행 권한 
### 2. virtual macine
+ virtual macine
 * 물리적 하드웨어가 없어도 논리적으로 컴퓨터를 구성하여 다양한 운영체제를 한개의 하드웨어로 사용이 가능하게 함.
 * 물리적 하드웨어가 없어 비용 절감, 속도, 이전 속도 등에 장점을 가진다.
 * Hypervisor(born2beroot 과제로 치면 VirtualBox)가 설치되는 물리 하드웨어를 Host, Hypervisor에서 리소스를 사용하는 VM들을 Guest라고 함.
+ Linux 설치
 * 다양한 배포판이 있는데 그 중 Debian을 사용
 * Debian 배포판 : Debian 팀에 의한 오픈소스. CentOS에 비해 설치 및 구성이 쉽고 업데이트 및 보수유지가 빠르다는 장점이 있음.
 * 레드햇 계열 CentOS : 기업이 만들어서 배포, 유지보수가 느리고 업데이트가 거의 되지 않지만 보안에서 비교적 안전하다는 장점.

### 3. LVM(Logical Volume Management)
 + logical volume을 효율적이고 유연하게 관리하기 위한 커널의 한 부분이자 프로그램
 + why logical volume?
   * 리눅스 안에서는 하나의 디스크를 여러 파티션으로 분할해 파일 시스템을 사용해 특정 디렉토리와 연결시켜 사용함.
   * 이 때 하드디스크를 파티션으로 나눌 경우 물리적인 개념이 강해서 고정적인 용량으로 사용하게 됨. 따라서 공간 효율성이 낮음
   * *LVM은 파티션을 논리적인 개념인 logical volume으로 나눠 더 유동적으로 디스크의 용량을 관리할 수 있음*
   * LVM이 없다면 리눅스에서 디스크를 사용하는 방법은 디스크->파티션->파일시스템이지만 LVM을 적용하면 볼륨단위로 저장장치를 관리할 수 있고 물리 디스크를 볼륨그룹으로 묶어 이를 다시 논리그룹으로 나누어 파일 시스템을 만든다.(디스크->파티션->볼륨그룹->논리그룹->파일시스템)
   * ***물리적 공간인 `disk`를 논리적 공간의 최소단위인 `PE`로 나누고 분산된 `PE`들을 모아서 `LV`를 구성***
----
###  4. Linux Debian 배포판 구현

#### 1) sudo 설치 및 설정
+ sudo : SuperUser Do : 유닉스 및 유닉스 계열 운영체제에서 슈퍼유저로서 프로그램을 구동할 수 있게 함.
+ su : Substitute user - 현 사용자를 로그아웃 하지 않고 다른 사용자 권한을 얻음. : `su -`를 이용하면 root 계정으로 로그인 가능
+ '-' : -l 또는 --login과 동일 : 사용하면 환경변수가 정리되고 로그인한 사용자의 홈으로 이동함
+ `apt install sudo` 를 이용하여 설치
> sudoers파일 : sudo 명령어를 사용할 수 있는 계정 관리 설정 파일
> > 설정파일이므로 /etc 폴더 아래에 존재
> > 기본적으로 /etc/sudoers 파일은 쓰기권한을 가지고 있지 않아서 별도의 *visudo* 명령어를 통해 파일을 편집하게 됨
+ 'visudo' : sudoers 파일을 편집하기 위한 명령어
  * secure_path에 :/snap/bin 추가
    - *Secure Path*는 `sudo` 명령 실행시 현재 계정의 쉘이 아닌 가상의 새로운 쉘을 생성하여 명령을 실행함 -> 이때 명령을 찾을 경로를 나열한 환경변수 PATH가 secure_path인 것
    - secure_path는 트로이목마에 대한 1차적 방어기능을 할 수 있음. (현재 계정 PATH에 악의적 경로가 포함된 경우 전체 시스템 해킹을 방지하려는 목적, sudo에는 포함되어있지 않으니까!)
  * 추가 사항
  * ![image](https://user-images.githubusercontent.com/39961274/190332837-92aa4254-74a6-49e8-bd90-cb9858233e54.png)
    - passwd_tries : sudo 인증 시 비밀번호가 틀렸을 때 제공하는 기회의 횟수 설정
    - badpass_message : sudo 권한 사용 중 비밀번호가 틀렸을 때 나오는 오류 메세지 설정
    - log_input, log_output : sudo 권한을 이용하여 수행한 명령어의 입출력을 모두 기록
    - iolog_dir : log가 기록될 위치 설정
    - requiretty : TTY 모드에서만 실행될 수 있도록 설정
    - > TTY (teletyperwriter) : 리눅스 디바이스 드라이브 중 콘솔이나 터미널을 의미. 현 프로젝트에서는 가상 환경의 터미널이라고 할 수 있음.
   * vi 사용법 - 입력 -> `ctrl + x` -> y -> filename에서 tmp를 지우고 저장
   * sudo 로그 확인 : `sudo ls /var/log/sudo/`

#### 2) group 설정
 + `groupadd` : group을 추가함
 + 'usermod' : 사용자 계정에 관련된 다양한 정보 변경 : 관리자만 사용 가능 / 이미 존재하는 사용자에 대해서 처리 가능
   * `usermod -l <새로운계정><기존계정>
     - 로그인 아이디 변경
     - 홈, 디렉토리명도 함께 변경해주기 위하여 -d(사용자의 홈 디렉토리 변경), -m(홈 디렉토리 변경 시 기존에 사용하던 파일 및 디렉토리도 함께 변경) 옵션 함께 사용
   * ***`hostnamectl set-hostname <HOST_NAME>`***
   * `usermode -aG <그룹명><사용자명>`
      - 해당 그룹에 유저를 추가
      - 그룹이 여러 개인 경우 공백없이 콤마(,)로 구분하면 됨
      - a 옵션은 G옵션과 같이 사용하는 옵션 / 기존의 2차 그룹 이외에 추가로 2차 그룹을 지정할 때 사용 / a 옵션 없이 G만 사용시 gid 그룹을 제외하고 명령어에 없는 그룹에서는 전부 탈퇴처리
   * `usermode -g <그룹명><사용자명>`
      - 그룹이 primary group이 되게 함
   * 그룹 및 유저 추가 및 삭제 `adduser <사용자명>` deluser <사용자명>` `addgroup <그룹명>` `delgroup <그룹명>`
      - adduser와 useradd의 차이
      - useradd 는 low lever utility로 계정 생성시에 필요한 모든 설정을 명시해야 하며 홈 디렉토리를 자동으로 생성하지 않는다.
      - adduser 는 configuration information in /etc/adduser.conf에 의해서 계정을 생성하고 홈디렉토리를 자동으로 생성한다.
   * 유저가 속한 그룹 확인  : `groups <사용자명>`
   * 그룹에 속한 사용자 확인 : `getent group <그룹명>`
   * 그룹 리스트 확인 `cat /etc/group`

#### 3) user 설정
 + `libpam-pwquality` : 패스워드 설정 강화를 위한 pam 모듈
   * *pam(Pluggable Authentication Modules)* : 리눅스 시스템에서 사용하는 인증 모듈로써 응용 프로그램에 대한 사용자 권한을 제어함
   * pam 모듈을 사용하면 직접 인증로직을 구현하지 않아도 되며 passwd파일 등 시스템 파일을 직접 열람하지 않아 안전하게 시스템을 운영할 수 있다.
 + `vi /etc/pam.d/common-password`
 + `vi /etc/login.defs`
   * 비밀번호 정책 모듈 파일 : 파일을 수정하여 정책을 수정할 수 있음.
   * ```c
     $ sudo vi /etc/login.defs
 
     PASS_MAX_DAYS 30 // 30일 후 만료
     PASS_MIN_DAYS 2  // 최소 사용기간 2일
     PASS_WARN_AGE 7  // 7일전에 경고 보내기
     PASS_MIN_LEN 10  // 최소 10글자 이상

     $ sudo apt install libpam-pwquality // 패키지 설치
     $ sudo vi /etc/pam.d/common-password // 이 파일에서 비밀번호 정책 수정

     /// common-passwrod 파일 수정
     retry=3 // 암호 재입력은 최대 3회까지
     minlen=10 // 최소 길이 10
     difok=7 // 기존 패스워드와 달라야 하는 문자 수 7
     maxrepeat=3 // 동일한 문자를 반복 가능한 최대 횟수 3
     ucredit=-1 // 대문자 한개 이상 포함
     lcredit=-1 // 소문자 한개 이상 포함
     dcredit=-1 // digit 한개 이상 포함
     reject_username // username이 그대로 혹은 reversed 된 문자는 패스워드로 사용 불가
     enforce_for_root // root 계정도 위의 정책들 적용
      ```
#### 4) partition 확인
  + `lsblk` partition 정보를 확인할 수 있다.

### 5) SSH
 + `systemctl status ssh`로 ssh 작동 여부 확인

### ^) UWF
 + 방화벽
 + ufw status verbose
