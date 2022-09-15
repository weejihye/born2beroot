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
  * apt (Advanced Packaging Tool) - 단순, 설치 삭제 > 보수, 유지(업데이트 등)이 어려움
  * dpkg : 데비안 패키지 관리 시스템의 기초가 되는 소프트웨어, apt보다 낮은 수준에서 수행
   + dpkg -l : 패키지 목록 보기
     - dpkg -L [패키지명] : 특정 패키지에 설치된 모든 파일 표시
     - dpkg -s [패키지명] : 주어진 패키지의 상태를 봄
  * aptitude - apt와 atp-cache를 통합하여 설치, 삭제 뿐만 아니라 쓰지 않는 패키지 삭제, 업데이트 자동화 등 패키지 관리가 자동화된 매니저
 
### 2. virtual macine
+ virtual macine
 * 물리적 하드웨어가 없어도 논리적으로 컴퓨터를 구성하여 다양한 운영체제를 한개의 하드웨어로 사용이 가능하게 함.
 * 물리적 하드웨어가 없어 구성 후 배포가 쉬운 장점
+ Linux 설치
 * 다양한 배포판이 있는데 그 중 Debian을 사용
 * Debian 배포판 : Debian 팀에 의한 오픈소스. CentOS에 비해 설치 및 구성이 쉽고 업데이트 및 보수유지가 빠르다는 장점이 있음.
 * 레드햇 계열 CentOS : 기업이 만들어서 배포, 유지보수가 느리고 업데이트가 거의 되지 않지만 보안에서 비교적 안전하다는 장점.
----
###  3. Linux Debian 배포판 구현

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
