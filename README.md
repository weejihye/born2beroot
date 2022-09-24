# born2beroot

-----
### 1. 패키지 관리 시스템
+ 패키지 : 소프트웨어의 실행 파일, 도큐먼트 파일, 설정 파일, 스크립트를 아카이브한 파일 하나
  * 리눅스에서 널리 사용되는 rpm, deb
  * .rpm : redhat 형식, CentOS, Fedora
  * .deb : Debian 형식, Debian GNU/Linux, Ubuntu
  * 레포지터리 : 패키지 파일을 모아서 배포하는 사이트
+ 패키지 단위로 소프트웨어를 설치, 삭제
+ 패키지 관리 시스템 CentOS-RPM, Debian-APT *(요즘에는 apt가 rpm패키지에도 적용가능하다고 함!)*

dpkg| apt<br>(Advanced Packaging Tool)| aptitude
---|---|---
. |`CLI`만 제공<br> 여러가지 APT(cache, mark, get)에서 자주 사용한 옵션만 추출하여 사용하기 편하게 만든 것<br>다른 high-level 매니저에서 사용될 수 있음. 패키지 삭제시 사용되지 않은 패키지까지 같이 삭제하려면 `auto-remove`, `apt-get autoremove` 명시가 필요함. | high-level 대화형 패키지 매니저<br>`GUI` `CLI` 모두 제공<br> apt-get을 모두 대체 가능.<br> aptitute는 설치, 제거, 업데이트 과정에서 충돌이 있는 경우 다른 대안을 제시함.<br> 
데비안 패키지 관리 시스템의 기초가 되는 소프트웨어, apt보다 낮은 수준에서 수행<br>* dpkg -l : 패키지 목록 보기<br>+ dpkg -L [패키지명] : 특정 패키지에 설치된 모든 파일 표시<br>+ dpkg -s [패키지명] : 주어진 패키지의 상태를 봄 |단순, 설치 삭제 > 보수, 유지(업데이트 등)이 어려움 | apt와 atp-cache를 통합하여 설치, 삭제 뿐만 아니라 쓰지 않는 패키지 삭제, 업데이트 자동화 등 패키지 관리가 자동화된 매니저

### 2. AppArmor
  * 노벨에서 만든 보안 솔루션 == 리눅스 보안 모듈로 오픈소스.
  * `aa-enabled` 명령어를 통해 활성화 여부 확인 가능
  * `aa-status`로 apparmor가 있는지 
  * 시스템 관리자가 프로그램 프로파일별로 프로그램의 역량을 제한할 수 있게 해주는 리눅스 커널 보안 모듈
  * 강제적 접근 통제 (Mandatory Access Control, MAC)을 제공함으로써 전통적인 유닉스 임의적 접근 통제(Discretionary Access Control, DAC) 모델을 지원

DAC(임의적 접근 통제) | MAC(강제적 접근 통제)
---|---
\* 시스템 객체에 대한 접근을 사용자나 그룹의 신분을 기준으로 제한<br> \* 사용자나 그룹이 객체의 소유자라면 다른 주체에 대해 이 객체에 대한 접근 권한을 설정할 수 있음<br> \* 사용자가 임의로 접근 권한을 지정하므로 사용자의 권한을 탈취당하면 사용자가 소유하고 있는 모든 객체 접근 권한을 가질 수 있게 되는 치명적인 문제가 있음. | \* 미리 정해진 정책과 보안 등급에 의거하여 주체에게 허용된 접근 권한과 객체에게 부여된 허용 등급을 비교하여 접근을 통제<br> \* 높은 보안을 요구하는 정보는 낮은 보안 수준의 주체가 접근할 수 없으며 소유자라고 해도 정책에 어긋나면 객체에 접근할 수 없으므로 강력한 보안을 제공함
  * 프로파일의 특징
    - profile은 텍스트 파일로 구성됨
    - 주석을 지원
    - 파일의 경로를 지정할 때 glob 패턴(*.log와 같은)을 사용할 수 있음.(와일드카드같이)
    - read, write, memory map, file locking, create hard ling 등 파일에 대한 다양한 접근 제어
    - 네트워크 접근 제어
    - capability에 대한 제어
    - #include를 이요하여 외부 프로파일 사용 가능
    - /etc/apparmor.d 디렉토리에 저장되어 있음.
  * enforce모드는 허가되지 않는 파일에 접근하는 것을 거부. complain 모드는 어플리케이션이 해야할 행동이 아닌 다른 행동을 하면 로그를 남김.
  * 보안설정을 일일이 하지 않고 텍스트로 저장해놓고 일괄적으로 사용한다고 생각하면 편할듯.
  * [참고 yeji.log](https://velog.io/@kyj93790/2.-Apparmor)
### 3. virtual macine
+ virtual macine
 * 물리적 하드웨어가 없어도 논리적으로 컴퓨터를 구성하여 다양한 운영체제를 한개의 하드웨어로 사용이 가능하게 함.
 * 물리적 하드웨어가 없어 비용 절감, 속도, 이전 속도 등에 장점을 가진다.
 
 * vm에 두가지 방식이 있는데 네이티브형, 호스트형
   + 네이티브 형은 하이퍼바이저가 하드웨어를 직접 제어하는 방식 : 효율적이지만 여러 하드웨어 드라이버를 직접 세팅해야 해서 설치가 어려움
   + 호스트 형은 하이퍼바이저가 소프트웨어처럼 호스트OS위에 작동 : VM내부의 게스트 OS에 하드웨어자원을 에뮬레이트하는 방식으로 게스트 OS에 대한 제약이 없음 
 
 * 물리적 하드웨어가 없어 비용 절감, 속도, 이전 속도 등에 장점을 가진다
 * Hypervisor(born2beroot 과제로 치면 VirtualBox)가 설치되는 물리 하드웨어를 Host, Hypervisor에서 리소스를 사용하는 VM들을 Guest라고 함.
+ Linux 설치
 * 다양한 배포판이 있는데 그 중 Debian을 사용
 * Debian 배포판 : Debian 팀에 의한 오픈소스. CentOS에 비해 설치 및 구성이 쉽고 업데이트 및 보수유지가 빠르다는 장점이 있음.
 * 레드햇 계열 CentOS : 기업이 만들어서 배포, 유지보수가 느리고 업데이트가 거의 되지 않지만 보안에서 비교적 안전하다는 장점.

### 4. LVM(Logical Volume Management)
 + logical volume을 효율적이고 유연하게 관리하기 위한 커널의 한 부분이자 프로그램
 + why logical volume?
   * 리눅스 안에서는 하나의 디스크를 여러 파티션으로 분할해 파일 시스템을 사용해 특정 디렉토리와 연결시켜 사용함.
   * 이 때 하드디스크를 파티션으로 나눌 경우 물리적인 개념이 강해서 고정적인 용량으로 사용하게 됨. 따라서 공간 효율성이 낮음
   * *LVM은 파티션을 논리적인 개념인 logical volume으로 나눠 더 유동적으로 디스크의 용량을 관리할 수 있음*
   * LVM이 없다면 리눅스에서 디스크를 사용하는 방법은 디스크->파티션->파일시스템이지만 LVM을 적용하면 볼륨단위로 저장장치를 관리할 수 있고 물리 디스크를 볼륨그룹으로 묶어 이를 다시 논리그룹으로 나누어 파일 시스템을 만든다.(디스크->파티션->볼륨그룹->논리그룹->파일시스템)
     - 파티션 : 하나의 하드디스크에 대해 (물리적으로) 영역을 나누는 것. fdisk로 파티션 설정이 가능함.
     - 물리볼륨 : `PV` 물리볼륨은 각가의 파티션을 LVM으로 사용하기 위하여 형식을 변환시킨 것 (ex) /dev/hda1 등
     - 논리볼륨 : `LV`, 여기서부터 사용자가 이용, 마운터 포인터로 사용할 실질적 파티션, 크기 확장 축소가 가능하여 유연하게 사용할 수 있음!
     - 볼륨그룹 : `VG`, PV로 되어 있는 파티션을 그룹으로 설정하는 것 (ex) /dev/sda1/ or /dev/sda1/ + /dev/sda2/
     - 물리적 범위 : `PE(Physical Extent)` LVM이 물리적 저장공간을 가르키는 기본 단위. 4MB
     - 논리적 범위 : `LE(Logical Extent)`  LVM이 논리적 저장공간을 가리키는 단위. 기본단위는 물리적 범위와 동일.
     - VGDA : (Volume Group Descriptor Area) 볼륨그룹의 모든 정보가 기록되는 부분. VG의 이름, 상태, 속해있는 PV, LV, PE, LE들의 할당 상태 등을 저장. VGDA는 각 물리볼륨의 처음 부분에 저장.
   * ***물리적 공간인 `disk`를 논리적 공간의 최소단위인 `PE`로 나누고 분산된 `PE`들을 모아서 `LV`를 구성***
   [참고1 -r0k's l0g](https://padawanr0k.github.io/posts/ft_seoul/born2beroot/01/index/)
----
###  5. Linux Debian 배포판 구현

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
      + 유닉스 시스템의 기본적인 이용방법은 유닉스가 인스톨된 호스트에 네트워크를 통해서 다른 컴퓨터가 접근하여 작업을 수행하는 것. 이 때 사용자로부터 명령을 받아 전달하는 역할을 콘솔이 담당함
      + 콘솔은 컴퓨터를 조작할 때 사용하는 장치임
        + 터미널, tty 포함
        + 실제 물리적 장치가 아니므로 커널에서 터미널을 에뮬레이션 함.
        + 문자를 치면 자동으로 전신 부호로 번역되어 송신되고, 수신측에서는 반대로 수신된 전신부호가 문자로 번역되어 출력됨.
   * ~~vi 사용법 - 입력 -> `ctrl + x` -> y -> filename에서 tmp를 지우고 저장~~ 그냥 `apt install`로 vim을 설치하자... 
   * sudo 로그 확인 : `sudo ls /var/log/sudo/`
   * 각 Defaults 뒤에 의미하는 바는 다음과 같다.
     + env_reset: HOME, LOGNAME, PATH, SHELL, TERM, USER을 제외한 모든 환경 변수를 reset
     + mail_badpass: 잘못된 패스워드로 sudo 실행 시, 지정된 메일로 보고
     + secure_path: sudo 명령은 현재 계정의 쉘이 아닌 가상 쉘을 생성하고 그 안에서 실행된다. 이 때, 이 가상 쉘의 환경변수 PATH의 값을 secure_path 옵션을 통해 지정한다.


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
     
     $ passwd -e [user_name] // 패스워드 강제 만료시키기
     $ chage -m 2 -M 30 -W 7 [user_name] // 이전 계정에 새로 설정한 정책 적용하기 
        > chage: 시스템 보안을 위해 사용자 패스워드의 만기일을 설정하너가 변경하는 명령어
        > -m: 패스워드 변경 후 다시 변경할 수 있는 최소 날짜 설정 옵션
        > -M: 패스워드가 유효한 최대 날짜 설정 옵션
        > -W: 경고 메세지 기간 설정 옵션
     $ sudo vi /etc/shadow // 계정들에 따른 패스워드 정책 상황 확인하기

      ```
#### 4) partition 확인
  + `lsblk` partition 정보를 확인할 수 있다.

### 5) SSH (Secure Shell Protocol)
 + 원격 호스트에 접속하기 위해 사용되는 보안 프로토콜
 + 작동원리 : 클라이언트와 호스트가 각각 키를 보유하고 이를 활용하여 주고 받는 데이터를 암호화.
 + 방식
   1. `ssh-keygen` 명령어를 사용하여 한쌍의 key 생성
   2. 호스트 또는 클라이언트가 1개의 쌍을 가진 키를 사용하여 연결
     + `id_rsa.pub`(public key)
     + `id_rsa` (private key)
   3. 서버는 public key로 만들어진 랜덤한 값을 생성하고 클라이언트에 보냄 (public key로 랜덤 256bit 문자열을 암호화)
   4. 클라이언트는 랜덤한 값을 private key를 이용해 암호화하여 서버로 전송
   5. 서버에서는 받은 값을 유저의 public key로 복호화
   6. 복호화한 값을 클라이언트에 전달, 인증 확인
   7. 인증되었다면, 세션키 교환 (두개의 키로 한가지 결과가 나와야 함.)
   
 + 짱구(서버)가 철수가 알려준 레시피(퍼블릭키)대로 호빵(랜덤값)을 만든다.
 + 호빵을 철수(클라이언트)에게 보내면 철수는 그 호빵 재료를 가지고 다시 자신만의 레시피(프라이빗키)로 찐빵을 만들어서 짱구에게 돌려보낸다.
 + 짱구가 찐빵을 해체해보고 레시피가 철수의 것이라는 것을 확인한다.
 + 철수를 집으로 들여보내준다.
 
 + `systemctl status ssh`로 ssh 작동 여부 확인

> DHCP 끄기<br>
[참고 yeji.log](https://velog.io/@kyj93790/Born2beroot-4.-Vim-%EC%84%A4%EC%B9%98-%EB%B0%8F-SSH-%EC%84%A4%EC%A0%95)
💡 DHCP란?<br>
호스트의 IP 주소와 각종 TCP/IP 프로토콜의 기본 설정을 클라이언트에게 자동적으로 제공해주는 프로토콜<br>
네트워크 안에 컴퓨터에 자동으로 네임 서버 주소, IP주소, 게이트웨이 주소를 할당해주는 것<br>
해당 클라이언트에게 일정 기간 임대를 하는 동적 주소 할당 프로토콜<br>

장점
PC 수가 많거나 PC 자체 변동사항이 많은 경우 IP 설정이 자동으로 되므로 효율적
IP를 자동으로 할당하므로 IP 충돌을 막을 수 있음
단점
DHCP 서버에 의존되므로 서버가 다운되면 IP 할당이 제대로 이루어지지 않는다

```
sudo vim /etc/ssh/sshd_config // ssh config 파일 ssh demone 의 줄임말 demone은 기다리는 쪽(서버측)
ip addr // netmask 확인용 8->255.0.0.0
ip route // gateway 확인용
hostname -I // ip 주소 확인용

address 가상머신ip주소
netmask 위참고
gateway 위참고
```


### 6) UWF (Uncomplicated Firewall)
 + 방화벽 : 미리 정의된 보안 규칙에 기반하여 들어오고 나가는 네트워크 트래픽을 모니터링하고 제어하는 네트워크 보안 시스템, 신뢰할 수 있는 내부 네트워크, 신뢰할 수 없는 외부 네트워크 간의 장벽을 구성
 + UFW
   - 데비안 계열 및 다양한 리눅스 환경에서 작동되는 사용하기 쉬운 방화벽 관리 프로그램
   - 방화벽은 netfilter를 사용하여 filtering을 수행하는데 리눅스에서 가장 많이 사용하는 netfilter가 iptable임.
   - 근데 iptable은 너무 기본 모듈이어서 절차상 번거로운면이 존재하여 iptable의 작업을 간편화해준 것이 UFW.
     + iptable : 시스템관리자가 리눅스 커널 방화벽이 제공하는 테이블과 그것들을 저장하는 체인, 규칙들을 구성할 수 있게 해주는 사용자 공간 응용프로그램
 + ufw status 로 포트 확인 가능
 + 외부와의 연결은 자유롭되 내부로의 연결은 제한 - 네트워크 보호, 망 관리에 대한 보증
   * 주요 기능
      - 접근제어 (리스트를 통해서 관리)
      - 사용자 인증
      - 주소 변환 (인터넷 사용시 추가의 공인 내부망의 사설 io 주소를 공인 ip 주소로 자동 변환하여 인터넷에 연결)
      - 감사 기록 (시도된 모든 접근에 대한 출발지, 목적지, 서비스 종류, 포트번호, 사용자id, 사용내역 등의 감사 기록을 남김)
      - 사설가상망을 위한 데이터암호화(VPN)
   * 방화벽 사용 이유
      - 보안이 필요한 네트워크의 통로를 단일화하여 관리, 외부의 불법침입으로부터 내부의 정보자산을 보호
   * 방화벽 단점
      - 바이러스와 같은 맬웨어 형태로 존재하는 위협과 방화벽을 경유하지 않는 접근의 경우 방어가 어렵다.
  + 명령어
    * `ufw status verbose`
    * `ufw enable`
    * `ufw allow <포트번호>`

### 7) crontab
  + cron : 특정한 시간 또는 시간마다 어떤 작업을 자동으로 수행하게 하는 명령어
  + crontab : cron 작업을 설정하는 파일 -> /etc/crontab 파일에 설정된 것을 읽어 작업을 수행한다.
    * 참고

      - /usr/sbin/anacron  : cron과 같이 동작하는 같이 동작하는 프로그램으로 서버가 일정시간 중지되었을 때에도 작업이 실행되는 것을 보장하기 위해 실행되는 도구
      - /etc/cron.daily, /etc/cron.weekly, /etc/cron.monthly : 이름처럼 주기적으로 실행할 내용을 시스템 크론 설정 디렉토리에 넣어 작동
      - /var/log/cron : 크론 실행내용 기록
   + monitoring.sh
     * #Architecture : 운영체제 및 커널 버전의 아키텍처
       - uname -a : 시스템 정보 출력
     * #CPU physical : 물리적 프로세서의 수
       - nproc : print the number of processing units available (man 참고)
     * #vCPU : 가상 프로세서 개수(쓰레드)
       - vCPU는 가상 머신 또는 서버가 가상 머신에 대해 파티션되지 않은 경우 실제 프로세서 코어에 지정된 가상 코어
       - /proc/cpuinfo : 이 때 나오는 processor의 number 0은 개수가 아닌 id임!!! 그래서 line 개수를 세는 것
     * #Memory Usage (서버에서 현재 사용 가능한 ram 및 사용률)
       - free 메모리 사용량을 보여줌
       - -m megabyte 단위로 보여줌
       - grep Mem : swap 부분이랑 같이 보여지기 때문에 memory 부분만 끄집어 내고
       - awk로 필요한 필드들 이용해서 솎아낸 값을 출력
         + 이 때 %.2f는 double로 소수 2자리까지 출력하라는 말. printf의 보너스 옵션을 생각합시다.
     * #Disk Usage (서버에서 현재 사용 가능한 메모리 및 사용률)
       - df 명령을 사용하면 리눅스 시스템 전체의 (마운트 된) 디스크 사용량을 확인할 수 있습니다.
       - 파일시스템, 디스크 크기, 사용량, 여유공간, 사용률, 마운트지점 순으로 나타납니다. [참고 - 빌노트](https://withcoding.com/104)
       - df -BG G바이트 단위로 단위까지 표시
     * #CPU load (cpu 사용량)
       - mpstat : Report processors related statistics.
       - sysstat 패키지 설치하여 사용
       - 마지막 idle이 사용가능 공간이므로 빼서 현재 load를 계산
     * #Last boot 
       - who 명령어와 -b(boot)의 조합
     * #LVM use
       - greater than
       - [] 사이는 반드시 띄어쓰기
       - then 을 사용하여 결과 행동
       - ;을 사용하여 행동 추가
       - fi로 종료
     * #Connections TCP (네트워크 상태 확인)
       - ss -tunplsudo 프로그램 실행된 명령 수
       - usermod -aG systemd-journal <사용자이름> 명령어를 통해 journalctl 명령어를 사용할 준비
       - journalctl을 사용하면 systemd-journald 데몬이 수집한 모든 로그 정보를 볼 수 있다.
       - journalctl을 사용하여 특정 로그를 보고싶다면 _COMM=<특정> 옵션을 추가하면 된다.
       [참고 Hans.log](https://velog.io/@tmdgks2222/42seoul-born2beroot-cron-monitoring.sh)
     
   
 __________________________________

 # BONUS PART
 웹서버, database 서버를 활용하여 wordpress 블로그를 구성하는 과제
 ## 1. 웹서버 : 웹브라우저와 같은 클라이언트로부터 HTTP 요청을 받아들이고, 이를 HTML 문서와 같은 정적 페이지로 처리해 반환하는 프로그램
 lighttpd는 apachi보다 훨씬 적은 자원으로 빠른 속도를 지원한다.
 
 ## 2. CGI : 웹서버와 외부 프로그램을 연결해주는 표준화된 프로토콜
   1) 웹서버가 처리할 수 없는 정보가 요청되면 C, PHP, Python 등의 동적인 작업이 가능한 외부프로그램을 호출하고 반환되는 HTML을 브라우저로 전송하게 됨.
   2) fastCGI : 보통 CGI는 하나의 요청에 하나의 프로세스를 생성하여 작업하므로 많은 요청이 있으면 서버에 부하가 생기는데 fastcgi는 하나의 프로세스로 다중의 요청들을 처리하여 프로세스를 생성하고 제거하는 부하를 경감함.

## 3. database : 관계형 데이터베이스 관리 시스템. mariadb 무료 사용 가능, MySQL 호환 가능. 가볍고 빠름. 설치, 유저 등록에 편함. 마리아 DB용 매니저 프로그램도 가볍고 사용하기 간편
