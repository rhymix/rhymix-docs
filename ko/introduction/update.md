라이믹스 업데이트
-----------------

라이믹스는 활발하게 개발이 이루어지고 있으며 수시로 새 기능이 추가, 버그 수정, 보안패치 등이 이루어지므로
라이믹스를 사용하여 운영하시는 사이트는 자주 업데이트하실 준비가 되어 있어야 합니다.

라이믹스는 크게 두 가지 방법으로 업데이트할 수 있습니다.

### 새 버전으로 덮어씌우기

라이믹스 설치 방법과 동일합니다. 최신 버전을 다운로드하여 기존 사이트에 덮어씌웁니다.

모든 설정과 첨부파일 등은 `files` 폴더 및 DB에 저장되므로 라이믹스를 덮어씌워도 안전합니다.
(배포된 파일에는 `files` 폴더가 포함되어 있지 않습니다.)
각각의 폴더 내에 추가로 설치하신 모듈, 애드온, 스킨 등의 서드파티 자료도 마찬가지입니다.

단, 라이믹스에서 배포하는 파일을 수정하여 사용하고 계셨다면
이 방법으로 업데이트할 경우 모든 변경내역이 초기화될 수 있습니다.
변경분만 업데이트하고 싶으신 분은 아래에서 설명할 git 방식을 사용하시기 바랍니다.

### git으로 업데이트하기

라이믹스를 git으로 설치하셨다면 git으로 업데이트하실 수 있습니다.

#### 소스 수정이 없는 경우

`git status` 명령을 내렸을 때 "modified", "deleted" 등으로 나오는 것이 없다면 소스 수정이 없는 것입니다.
(추가한 파일, 서드파티 자료 등은 "Untracked files" 아래에 나옵니다. 이것은 문제가 되지 않습니다.)

소스 수정이 없다면 아래의 명령으로 간단하게 업데이트할 수 있습니다.

    git pull

단, 이 명령 사용시 현재 브랜치(master 또는 develop)만 업데이트되므로
브랜치를 변경할 경우 다시 업데이트해 주어야 합니다.

#### 소스 수정이 있는 경우 (간단한 수정)

`git status` 명령을 내렸을 때 "modified", "deleted" 등으로 나오는 파일이 있다면
해당 파일이 변경된 경우 업데이트시 충돌이 발생할 수 있습니다.
라이믹스는 수시로 변경되는 분량이 많기 때문에 어느 파일도 안전하지 않습니다!

변경 내역이 간단한 경우 `git stash`를 사용해서 소스 수정이 없는 상태로 임시 전환한 후,
업데이트를 마치고 기존의 변경 내역을 재적용하는 방법이 있습니다.

우선 아래의 명령으로 변경 내역을 따로 저장합니다.

    git stash

변경 내역을 따로 저장하고 나서 `git status` 명령을 내려보면 소스 수정이 없는 상태로 보일 것입니다.
이 때 아래의 명령으로 업데이트를 합니다.

    git pull

업데이트가 성공하면 아래의 명령으로 따로 저장해 두었던 변경 내역을 재적용합니다.

    git stash apply

충돌이 발생하지 않으면 이것으로 업데이트를 마칩니다.

만약 동일한 파일의 동일한 부분이 변경되었다면 이 단계에서 충돌이 발생할 것입니다.
이 경우 어느 파일이 충돌인지 화면에 표시되니, 즉시 해당 부분을 찾아서 수정해 주시기 바랍니다.
git에서 충돌 표시에 사용하는 
충돌하는 파일을 그대로 방치할 경우 백지현상 등 심각한 문제가 발생할 수 있습니다.

안전하게 업데이트가 끝났다면 아래의 명령으로 따로 저장해 두었던 변경 내역을 삭제합니다.

    git stash clear

#### 소스 수정이 있는 경우 (대폭 수정)

git은 대부분의 상황에서 커밋(commit)을 기준으로 변경 내역을 추적하기 때문에,
소스 수정 분량이 많은 경우 커밋을 하지 않는 stash 방식으로 변경 내역을 관리하기에는 한계가 있습니다.
이 때는 별도의 브랜치에서 변경 내역을 커밋한 후 공식 브랜치(master 또는 develop)와 merge하는 과정이 필요합니다.

우선 별도의 브랜치를 생성합니다. 브랜치 이름은 `mybranch` 대신 사이트 이름 등 쉽게 구분할 수 있는 것을 사용하십시오.

    git checkout -b mybranch

지금까지의 변경내역을 모두 커밋합니다. (아래의 예제는 서드파티 자료 등은 무시하고 코어 수정분만 커밋합니다.)

    git add -u .
	git commit -m "변경내역 커밋"

운영 도중에도 어느 정도 규모의 변경이 발생할 때마다 이렇게 커밋을 해주는 것이 좋습니다.
변경 내역 때문에 문제가 생겼을 때 쉽게 되돌릴 수 있으니까요.

변경내역을 커밋했으면 기존의 브랜치에 업데이트 내역을 받아옵니다. (현재 브랜치로 직접 받아오면 안됩니다.)

    git fetch origin master:master
	git fetch origin develop:develop

현재 브랜치는 건드리지 말고 master는 master로, develop은 develop으로 받아오라는 뜻입니다.
평소 master와 develop 중 하나만 사용하시는 경우 한쪽만 업데이트하셔도 무방합니다.

업데이트 정보를 받아왔으면 현재 브랜치와 merge합니다.

    git merge master

평소 develop 브랜치를 사용하시는 경우 이 명령에서 master 대신 develop을 사용하십시오.

충돌이 발생하지 않으면 이것으로 업데이트를 마칩니다.

만약 동일한 파일의 동일한 부분이 변경되었다면 이 단계에서 충돌이 발생할 것입니다.
이 경우 어느 파일이 충돌인지 화면에 표시되니, 즉시 해당 부분을 찾아서 수정해 주시기 바랍니다.

충돌이 발생하는 파일을 모두 수정한 후에는 다시 커밋을 해주어야 합니다.

    git add -u .
	git commit -m "업데이트"
