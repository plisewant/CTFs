app.py코드를 잘 보면 맨 마지막 줄에 디버그 모드가 켜져있는 것을 볼 수 있음
그래서 잘못된 파일을 읽으면 디버그 모드가 켜짐

/console url에 가보면 명령어를 쓸 수 있는 창이 나온다
하지만 그걸 사용하기 위해서는 pin번호를 알아야 한다 

pin번호를 구하는 알고리즘이 

`username` (도커 파일에 나옴)

`modname` (그냥 flask.app임)

`getattr(app, '__name__', getattr (app .__ class__, '__name__'))` (그냥 Flask임)

`getattr(mod, '__file__', None)` (app.py의 절대 경로)

`uuid.getnode()` (그 pc의 mac주소 **/sys/class/net/<device id>/address** 주소 폴더에 가면 볼 수 있고 **<device id>** 부분이 정해져 있지는 않은데 보통 eth0 인가봄)

`get_machine_id()` (/etc/machine-id 파일의 값이나 /proc/sys/kernel/random/boot_id 의 파일 값 둘 중 하나인데 /etc/machine-id를 우선으로 봄 그리고 아마도 도커파일이 있으면 **/proc/self/cgroup**값도 더해줘야 한다고 함)

이런 값들을 모두 구해서 알고리즘을 구한 다음 3자리 숫자 3개가 -로 구분된 문자가 나오는데 그게 pin임
알고리즘은 /usr/local/lib/python3.8/site-packages/werkzeug/debug/**init**.py 파일을 읽어보면 알 수 있는데 
그 중 get_pin_and_cookie_name함수 부분을 잘 개조하면 pin을 만드는 코드를 만들 수 있다

그걸 이용해서 플라스크 디버그 모드의 콘솔창에 들어간 다음
명령어(알고보니 파이썬 코드를 쓸 수 있음)를 사용해 flag를 실행시켜 그 값을 얻는다
예시: os.popen('/flag').read()

답: DH{2517d2b288f2cdee289a14d3bacd1afc}


처음으로 4티어 문제를 풀어봄

놀라운건 거짓말 없이

```
#!/usr/bin/python3
from flask import Flask
import os

app = Flask(__name__)
app.secret_key = os.urandom(32)

@app.route('/')
def index():
	return 'Hello !'

@app.route('/<path:file>')
def file(file):
	return open(file).read()

app.run(host='0.0.0.0', port=8000, threaded=True, debug=True)
```

이게 끝임

그래서 좀 당황하긴 했는데 일단 수상한건 `/<path:file>` 뭔진 몰라도
그 밑 함수가 `file` 을 받아서 그 이름 파일을 읽어줌

좀 찾아 보니까 `<>` 안에 있는게 url이라고 하고 `path` 그니까 그 url이름을 `:file` 이라는 변수로 받아서 함수의 파라미터로 넘겨서 그 이름을 가진 파일을 읽는다는거 

한마디로 url에 있는 이름의 파일을 읽는다는거 

그래서 저게 문제의 핵심인 줄 알고 검색을 해봄 찾아보다 임의 파일 읽기 취약점 이라는 단어를 찾음

오 바로 검색 “플라스크 파일 읽기 취약점”

근데 보니까 임의 파일 읽기를 디버그 모드가 켜져있을 때 할 수 있다고 함

디버그 pin만 알면 시스템 커맨드를 쓸 수 있다네

까지 왔는데 pin번호를 몰라서 실패

어떻게 푸는지 대충 알기도 했고 푸는 방법이 그게 맞았고 풀이를 보기도 했는데 아니 이게 안되는거

일단 확실한건 pin번호를 구한 다음 코드를 잘 짜서 플래그를 구하면 되는데

그 과정이 완전 빡쌔다 무슨 폴더를 막 돌아야 하는데 폴더를 읽는 url에 필터가 없기 때문에 
어차피 url값의 이름의 폴더를 읽으니 url에 때려 박았는데 앞에 ../../../../가 생략되서 없는 폴더를 보개됌

결국 버프슈트로 해결

그래서 기본적으로 알고리즘에 들어가는 값을 다 찾고 봤더니 무슨 도커파일을 쓰면 cgroup폴더를 더 찾아야 한다고 함 전까지 시도한 pin번호는 그냥 헛수고 

그래서 그 값까지 넣어 보려고 하니까 어디까지 넣어야 하는지 에매함 그래서 ai의 말을 믿고 - 다음부분부터 해봤더니 또 실패 알고보니 계속 중복되는 값에 - 전 값도 들어감

그래서 그거까지 포함해서 돌렸더니 또 실패
그냥 gpt가 문제라는걸 깨달음 그래서 코드를 그냥 뜯어오기로 함 

근데 이 코드도 전체코드 중 일부라서 함수 부분에서 어디까지 잘라야 하는지 애매함
그래도 잘 잘랐다

그리고 pin을 돌려보니 바로 정답

그리고 커맨드를 적어 봤더니
알고보니 파이썬 코드를 적을 수 있는거였음
그래서 좀 고생좀 하다가 ai의 힘으로 풀었다