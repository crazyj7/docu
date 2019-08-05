# Jupyter Notebook Keyboard

# 원격 접속
```
$jupyter notebook --generate-config
```
vi ~/.jupyter/jupyter_notebook_config.py
```
c.NotebookApp.ip = '*'
c.NotebookApp.password = u'sha1:bcd259ccf...<your hashed password here>'
c.NotebookApp.open_browser = False

# It is a good idea to set a known, fixed port for server access
c.NotebookApp.port = 9999
```
패스워드 설정 방법
```python
from notebook.auth import passwd
passwd('yourpassword')
```
출력된 결과를 위 설정 파일에 password에 설정한다.
jupyter 


# 단축키 
## 셀 모드
|Key|Description|
|---|----|
| Esc  or ^M | 셀모드 진입 (헤드가 파란색) |
| a | 위에 셀 추가  |
| b | 아래에 셀 추가  |
| x  or dd | 삭제|
| c | copy|
| p | paste|
| shift m | 아래셀과 merge|
| m | markdown으로 변경|
| y | code로 변경|
| ^ s or s| save |

## 코드 입력 모드 (edit)
|Key|Description|
|---|----|
| enter | 코드 입력로 진입 (헤드가 초록색)|
| shift enter | 선택 셀 실행하고 다음셀로 이동|
| ctrl enter | 선택 셀 실행만 |
| alt enter | 선택 셀 실행하고 다음에 빈셀 추가 |
| ctrl shift - | 현재위치에서 셀을 반으로 나누기 |
| ctrl / | 주석처리 |
| **TAB** | 메소드 목록 보기(.을 찍고 누름) |
| **SHIFT+TAB** | 메소드 parameter 보기 |
<!--stackedit_data:
eyJoaXN0b3J5IjpbODc3NzYwMzUwXX0=
-->