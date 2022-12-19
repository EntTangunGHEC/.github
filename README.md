# upload.yml

```
winscp.com /command "open sftp://$merged_by@103.12.248.203:223/ 
-privatekey=`"`"C:\actions_runner\$merged_by.ppk`"`"" 
"cd `"`"/K_DRV/${{ github.event.repository.name }}/$eachRemotePath/`"`"" 
"put `"`"$pwd\$localFilePath`"`""
```
1. WingFTP 인증
```
-privatekey=`"`"C:\actions_runner\$merged_by.ppk`"`""
```
Pull Request를 Merge(병합)하신 분의 GitHub 계정명을 받아오며, 

Runner서버 "C:\actions_runner" 폴더에 저장된 "GitHub계정명.ppk" 키파일을 사용하여 WingFTP 서버에 연결합니다.

2. WingFTP 경로 이동
```
"cd `"`"/K_DRV/${{ github.event.repository.name }}/$eachRemotePath/`"`""
```
WingFTP 서버의 저장소 경로로 이동합니다.  

`/K_DRV/` 최상위 경로는 수동으로 수정해주셔야 합니다.

`${{ github.event.repository.name }}` GitHub 저장소명을 받아옵니다.

$eachRemotePath은 GitHub 저장소 내부 경로를 받아오며, 만약 GitHub 경로가 WingFTP 서버 경로와 일치하지 않는다면 업로드가 불가합니다.

3.WingFTP 업로드
```
"put `"`"$pwd\$localFilePath`"`""
```
> GitHub 저장소파일을 WingFTP 서버에 업로드합니다.



# sync.yml
```
winscp.com /command "open sftp://$merged_by@103.12.248.203:223/ 
-privatekey=`"`"C:\actions_runner\$merged_by.ppk`"`"" 
"synchronize remote $pwd /K_DRV/${{ github.event.repository.name }}"
```
1. WingFTP 인증
```
-privatekey=`"`"C:\actions_runner\$merged_by.ppk`"`""
```
> Pull Request를 Merge(병합)하신 분의 GitHub 계정명을 받아오며, Runner서버 "C:\actions_runner" 폴더에 저장된 "GitHub계정명.ppk" 키파일을 사용하여 WingFTP 서버에 연결합니다.
2. 동기화
```
"synchronize remote $pwd /K_DRV/${{ github.event.repository.name }}"
```
> GitHub 저장소를 WingFTP 서버와 동기화합니다. (GitHub 저장소의 변경사항을 WingFTP 서버에 업로드합니다.)


# build.yml
Pull Request시 빌드가 자동으로 수행됩니다.

Push 시에도 빌드하실 경우 아래와 같이 수정하시면 됩니다.
```
on:
  # Triggers the workflow on push or pull request events
  push:
  pull_request:
```

Runner서버가 로컬 개발환경과 동일할때만 정상적으로 작동합니다.

Pull Request시 빌드 성공이 될때만 병합이 가능하게 할 경우 브랜치 보호 정책을 설정해야 합니다. 

저장소 Settings > Branch > Branch protection rules > Add rule > `Require status checks to pass before merging` 체크 > 아래 검색창에 Build 검색 후 적용

<img width="769" alt="image" src="https://git.krs.co.kr/storage/user/3/files/92a3ff77-494c-4b29-9d3c-8dc1a72878b4">


