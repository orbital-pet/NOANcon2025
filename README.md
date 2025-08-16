# NOANcon2025 발표자 목록 (`speakers_list.json`) 활용 안내

이 문서는 `speakers_list.json` 파일의 구조를 설명하고, 다양한 프로그래밍 환경에서 이 파일을 읽고 활용하는 방법에 대한 예제 코드를 제공합니다.

## `speakers_list.json` 파일 구조

`speakers_list.json` 파일은 JSON 배열 형태로, 각 객체는 발표자의 정보를 담고 있습니다. 객체는 다음과 같은 키를 가집니다.

*   `이름`: 발표자의 이름 (문자열)
*   `소속`: 발표자의 소속 (문자열, 비어 있을 수 있음)
*   `주제`: 발표 주제 (문자열, 비어 있을 수 있음)

### 예시

```json
[
  {
    "이름": "박성철",
    "소속": "",
    "주제": ""
  },
  {
    "이름": "박조은",
    "소속": "",
    "주제": ""
  }
]
```

## 사용 예제

### 1. JavaScript (웹 브라우저)

웹 브라우저 환경에서는 `fetch` API를 사용하여 `speakers_list.json` 파일을 비동기적으로 불러올 수 있습니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>발표자 목록</title>
</head>
<body>
    <h1>발표자 목록</h1>
    <ul id="speakers"></ul>

    <script>
        fetch('speakers_list.json')
            .then(response => response.json())
            .then(data => {
                const speakersList = document.getElementById('speakers');
                data.forEach(speaker => {
                    const listItem = document.createElement('li');
                    listItem.textContent = `${speaker.이름} (${speaker.소속 || '소속 없음'}) - ${speaker.주제 || '주제 미정'}`;
                    speakersList.appendChild(listItem);
                });
            })
            .catch(error => console.error('Error loading speakers:', error));
    </script>
</body>
</html>
```

### 2. Node.js

Node.js 환경에서는 `fs` (File System) 모듈을 사용하여 동기적 또는 비동기적으로 파일을 읽을 수 있습니다.

#### 비동기 방식

```javascript
const fs = require('fs');

fs.readFile('speakers_list.json', 'utf8', (err, data) => {
    if (err) {
        console.error('파일을 읽는 중 오류 발생:', err);
        return;
    }
    const speakers = JSON.parse(data);
    console.log(speakers);
});
```

#### 동기 방식

```javascript
const fs = require('fs');

try {
    const data = fs.readFileSync('speakers_list.json', 'utf8');
    const speakers = JSON.parse(data);
    console.log(speakers);
} catch (err) {
    console.error('파일을 읽는 중 오류 발생:', err);
}
```

### 3. Python

Python에서는 내장 `json` 모듈을 사용하여 JSON 파일을 쉽게 파싱할 수 있습니다.

```python
import json

try:
    with open('speakers_list.json', 'r', encoding='utf-8') as f:
        speakers = json.load(f)
        for speaker in speakers:
            print(f"이름: {speaker['이름']}, 소속: {speaker.get('소속', 'N/A')}, 주제: {speaker.get('주제', 'N/A')}")
except FileNotFoundError:
    print("speakers_list.json 파일을 찾을 수 없습니다.")
except json.JSONDecodeError:
    print("JSON 파일 형식이 올바르지 않습니다.")

```

### 4. Shell (cURL & jq)

`curl`을 사용하여 원격 위치의 파일을 가져오거나 로컬 파일을 읽고, `jq`를 사용하여 JSON 데이터를 파싱하고 필터링할 수 있습니다. `jq`는 별도 설치가 필요할 수 있습니다.

```bash
# 모든 발표자 목록 출력 (pretty-print)
cat speakers_list.json | jq '.'

# 첫 번째 발표자의 이름만 출력
cat speakers_list.json | jq '.[0].이름'

# 모든 발표자의 이름과 주제만 출력
cat speakers_list.json | jq '.[] | {name: .이름, topic: .주제}'
```
