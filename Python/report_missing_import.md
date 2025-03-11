# report missing import

## 발견

- vscode에 python 개발환경을 구축
- pip으로 fake-useragen package를 install
- python interpreter가 install된 package를 인식하지 못 함

## 해결

- 터미널에 `which python3`를 입력해 python의 경로 확인
- `shiht` + `cmd/ctrl` + p -> `python| select interpreter` 선택
- 제시된 interpreter 중 `which python3`에서 확인한 경로와 일치하는 Interpreter 선택
