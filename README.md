1. 애플 개발자 도구 및 라이브러리 설치 (애플 로그인 필요)

개발자 도구 설치  
https://developer.apple.com/download/all/?q=Command%20Line%20Tools%20for%20Xcode%2015.1

game-porting-toolkit 라이브러리 마운트  (설치가 아니고 추후 복사를 위한 마운트)
https://developer.apple.com/download/all/?q=Game%20Porting%20Toolkit%201.1

2. Homebrew x86_64 버전 설치

터미널창 에서 로제타 버전
softwareupdate --install-rosetta

터미널창에서 아키텍쳐는 X86으로 설정
arch -x86_64 zsh

터미널 언어는 영어로 설정
LANG=en_US.UTF-8

터미널에서 X64 홈브루 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

3. game-porting-toolkit 설치

아래 명령어를 입력하여 Apple Homebrew tab을 설치한다.
brew tap apple/apple http://github.com/apple/homebrew-apple

아래 명령어를 입력하여 game-porting-toolkit을 설치한다.
brew -v install apple/apple/game-porting-toolkit

(만약 설치 중간에 오류나면 아래 업데이트 명령어 )
brew update brew -v install apple/apple/game-porting-toolkit

4. Game Porting Tookit 연결

아래 명령어를 입력하여 Game Porting Tookit DMG가 /Volumes/Game Porting Toolkit-1.0에 탑재되어 있는지 확인 후 Game Porting Tookit 라이브러리 폴더를 Wine의 라이브러리 폴더로 복사한다.

$ ditto /Volumes/Game\ Porting\ Toolkit-1.0/lib/ `brew --prefix game-porting-toolkit`/lib/

아래 명령어를 입력하여 Game Porting Tookit DMG의 스크립트를 /usr/local/bin에 복사한다.

$ cp /Volumes/Game\ Porting\ Toolkit*/gameportingtoolkit* /usr/local/bin

5. Wine 설정
WINEPREFIX=~/Games `brew --prefix game-porting-toolkit`/bin/wine64 winecfg
WINEPREFIX=~/Games `brew --prefix game-porting-toolkit`/bin/wine64 reg add 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion' /v CurrentBuild /t REG_SZ /d 19042 /f
WINEPREFIX=~/Games `brew --prefix game-porting-toolkit`/bin/wine64 reg add 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion' /v CurrentBuildNumber /t REG_SZ /d 19042 /f
WINEPREFIX=~/Games `brew --prefix game-porting-toolkit`/bin/wineserver -k

6. Battle.net 설치 및 실행
gameportingtoolkit ~/Games ~/Downloads/Battle.net-Setup.exe
설치가 완료되면 Battle.net이 실행되니, 자동 로그인 설정을 체크하고 로그인하여 디아블로 4를 설치하면 된다.

7. 실행 스크립트
맥북을 재부팅 후, Windows용 Battle.net 및 디아블로 4를 실행하려면 터미널에 아래 스크립트를 입력하면 된다.

8. Battle.net 실행 스크립트
LANG=en_US.UTF-8
arch -x86_64 gameportingtoolkit-no-hud ~/Games 'C:\Program Files (x86)\Battle.net\Battle.net Launcher.exe'

참고자료
Apple Gamning Wiki - Game Porting Toolkit
How to install game-porting-toolkit
Playing Diablo IV on macOS
