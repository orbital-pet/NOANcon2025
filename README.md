# NOAN con 2025 in jeju
- "NOANcon" is a 24-hour conference in Jeju where 33 NOAN citizen speakers offer a sharper, clearer perspective on the current era & AI.
- 데놀이 맥주라면 노안은 소주, 막걸리
- AI 시대 노안 개발자의 생존기

## 관련 링크
- 페이스북 이벤트 : https://www.facebook.com/share/1C2vSnzNXb
- 공식홈피 베타용 : https://orbital-pet.diginori.com/conf/noan (누구라도 본 API 등 이용해 새로 만들어 주셔도 됩니다)

## 발표자 목록 (`speakers_list.json`) 활용 및 수정 안내

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/615db7b2-c675-45cd-90e0-84ab3fe9a9ad" />

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

## `speakers_list.json` 파일 유효성 검사

`speakers_list.json` 파일이 올바른 JSON 형식을 따르는지 확인하는 것은 중요합니다. 형식이 잘못된 경우, 이 파일을 사용하는 애플리케이션에서 오류가 발생할 수 있습니다.

### 1. `jq` 사용 (권장)

`jq`는 커맨드 라인에서 JSON을 처리하는 경량하고 유연한 도구입니다. `jq`를 사용하여 파일의 유효성을 검사하는 가장 간단한 방법은 다음과 같습니다.

```bash
jq . speakers_list.json
```

*   **성공**: 파일이 유효한 JSON이라면, `jq`는 내용을 예쁘게 포맷하여 출력하고 종료 코드 0을 반환합니다.
*   **실패**: 파일에 오류가 있다면, `jq`는 오류 메시지를 출력하고 0이 아닌 종료 코드를 반환합니다.

이 방법은 `speakers_list.json`을 수정하고 난 후 간단하게 유효성을 확인할 때 유용합니다.

### 2. Python 사용

Python의 내장 `json` 모듈을 사용하여 유효성 검사 스크립트를 작성할 수 있습니다.

```python
import json

def validate_json(file_path):
    try:
        with open(file_path, 'r', encoding='utf-8') as f:
            json.load(f)
        print(f"'{file_path}' is a valid JSON file.")
        return True
    except FileNotFoundError:
        print(f"Error: '{file_path}' not found.")
        return False
    except json.JSONDecodeError as e:
        print(f"Error: Invalid JSON in '{file_path}': {e}")
        return False

# speakers_list.json 파일 유효성 검사
validate_json('speakers_list.json')
```

## 기여 방법 (Contribution Guide)

이 프로젝트에 기여하고 싶으시다면, 다음 절차를 따라주세요.

1.  이 리포지토리를 **Fork**합니다.
2.  자신만의 브랜치를 생성합니다 (`git checkout -b feature/AmazingFeature`).
3.  변경 사항을 커밋합니다 (`git commit -m 'Add some AmazingFeature'`).
4.  브랜치에 푸시합니다 (`git push origin feature/AmazingFeature`).
5.  **Pull Request**를 생성합니다.

모든 기여를 환영합니다!
