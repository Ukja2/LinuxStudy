##  Linux 기본 명령어

### ⚙️ 1. 파일 및 디렉토리 관리
###  `ls` : 디렉토리 내용 목록 보기
현재 디렉토리 안에 있는 파일이나 폴더들을 보여줌

`ls` : 현재 디렉토리의 파일과 폴더 목록을 나열
`ls -l` : 파일의 세부 정보까지 나열
`ls -a` : 숨겨진 파일까지 모두 나열
`ls -lh` : 사람이 읽기 좋은 형식으로 파일 크기 표시

### `cd` : 디렉토리 이동
현재 작업 중인 디렉토리를 바꾸는 명령어

`cd /home/user` : 지정된 경로로 이동
`cd ..` : 상위 디렉토리로 이동
`cd ~` : 홈 디렉토리로 이동
`cd` : 아무 옵션 없이 입력하면 기본적으로 홈 디렉토리로 이동

### `pwd` : 현재 작업 중인 디렉토리 경로 출력
pwd는 현재 내가 있는 디렉토리의 경로를 알려주는 명령어

`pwd` : 현재 작업 중인 디렉토리 경로 출력 (예: /home/user)

### `cp` : 파일 복사
cp는 파일이나 디렉토리를 다른 위치로 복사하는 명령어

`-r` : 디렉토리 복사 시 필요 (디렉토리 안의 파일과 하위 디렉토리까지 복사)
`-i` : 덮어쓰기 전에 사용자에게 확인을 요구

`cp file1.txt /backup/` : file1.txt를 /backup/ 디렉토리로 복사
`cp -r /home/user /backup/`: /home/user 디렉토리와 그 안의 모든 파일을 /backup/로 복사

###  `mv` : 파일/디렉토리 이동 또는 이름 변경
mv는 파일이나 디렉토리를 이동하거나 이름을 변경하는 명령어

`-i` : 덮어쓰기 전에 사용자에게 확인을 요구

`mv file1.txt /backup/` : file1.txt를 /backup/ 디렉토리로 이동
`mv oldname.txt newname.txt` : oldname.txt 파일의 이름을 newname.txt로 변경

### `rm` : 파일 삭제
rm은 파일을 삭제하는 명령어

`-r` : 디렉토리와 그 안의 파일을 재귀적으로 삭제
`-f` : 강제로 삭제 (파일이 없더라도 경고 없이 삭제)

`rm file1.txt` : file1.txt 파일 삭제
`rm -r directory/` : directory 디렉토리와 그 안의 모든 파일 삭제
`rm -f file1.txt` : 경고 없이 강제로 file1.txt 삭제

### `mkdir` : 디렉토리 생성
새로운 디렉토리를 생성하는 명령어

`-p` : 부모 디렉토리까지 함께 생성 ( 경로에 부모 디렉토리가 없으면 부모 디렉토리까지 한 번에 생성 )

`mkdir my_folder` : my_folder라는 새 디렉토리 생성
`mkdir -p projects/webapp/assets` : projects/webapp/assets 경로에 필요한 모든 디렉토리 생성

### `touch` : 빈 파일 생성 또는 수정 시간 갱신
새로운 파일을 생성하거나 기존 파일의 수정 시간을 갱신하는 명령어

`touch myfile.txt` : myfile.txt라는 빈 파일을 생성하거나 기존 파일의 수정 시간을 갱신

### `rmdir` : 빈 디렉토리 삭제
비어 있는 디렉토리만 삭제할 수 있는 명령어

`-p` : 부모 디렉토리까지 삭제
빈 디렉토리를 삭제하면서, 그 부모 디렉토리가 비어 있으면 부모 디렉토리까지 삭제

`rmdir my_folder` : my_folder라는 빈 디렉토리 삭제
`rmdir -p projects/webapp/assets` : assets, webapp, projects가 모두 비어 있으면 한 번에 부모 디렉토리까지 삭제

---

## 🔍 2.파일 내용 조회

### `cat` : 파일 내용 출력
- 파일의 내용을 전체 출력
- 내용이 길면 한 번에 다 나와서 화면을 넘기기 어려울 수 있음

`cat myfile.txt` : myfile.txt 파일 내용을 화면에 한 번에 출력

![alt text](<스크린샷 2025-02-17 14.44.08.png>)

### `more` : 파일 내용 한 페이지씩 출력

- 파일을 한 페이지씩, 화면을 넘겨서 볼 수 있는 명령어
- 파일 내용이 길 때 유용
  
`more myfile.txt` : myfile.txt 파일을 한 페이지씩 출력하면서 볼 수 있음

### `less` : 파일 내용 스크롤하며 출력
- more와 비슷하지만 더 많은 기능 제공
- less는 양 방향 스크롤이 가능 (위로, 아래로)

`less myfile.txt` : myfile.txt 파일을 스크롤하며 내용 확인, q를 눌러 종료

### `head` : 파일의 처음 10줄 출력
- 파일의 앞부분 10줄만 출력
- 출력할 줄 수는 `-n` 옵션으로 변경 가능
`head myfile.txt` :  myfile.txt 파일의 처음 10줄을 출력
`head -n 20 myfile.txt` : myfile.txt 파일의 처음 20줄을 출력

### `tail` : 파일의 마지막 10줄 출력
- 파일의 뒷부분 10줄만 출력
- 출력할 줄 수는 `-n` 옵션으로 변경 가능
- `-f` 옵션을 사용하면 파일 내용의 변화를 실시간으로 추적 가능
`tail myfile.txt` : myfile.txt 파일의 마지막 10줄을 출력
`tail -n 20 myfile.txt` : myfile.txt 파일의 마지막 20줄을 출력
`tail -f myfile.txt` : 파일이 업데이트될 때마다 마지막 줄을 계속 출력 (로그 파일 등 실시간 모니터링 시 유용)

### `grep` : 파일에서 특정 문자열 검색
- 파일 내에서 특정 문자열을 검색하고 해당 부분을 출력
- 대소문자 구분 없이 검색하려면 `-i` 옵션 사용
  
`grep "hello" myfile.txt` : myfile.txt 파일에서 hello라는 문자열을 찾고 출력
`grep -i "hello" myfile.txt` : hello를 대소문자 구분 없이 검색
`grep -r "hello"` : 현재 디렉토리와 하위 디렉토리에서 hello 검색

---
