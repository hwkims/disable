```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript 필수</title>
    <style>
        body {
            font-family: sans-serif;
            text-align: center; /* 내용 가운데 정렬 */
        }
        .warning {
            color: red;
            font-weight: bold;
            margin-bottom: 20px; /* 경고 메시지 아래 여백 */
        }
    </style>
</head>
<body>

    <script>
        // JavaScript 코드는 여기에 (이전 답변의 개발자 도구 감지 코드 등)
        // ... (이전 답변의 isDevToolsOpen, detectConsoleUsage, blockPage, checkDevTools 함수 등) ...

        // (이전 답변의 JavaScript 코드 전체를 여기에 붙여넣기 하세요)

        // 페이지를 이동시키기 직전에 경고창 띄우는 함수
        function redirectWithAlert(url, message) {
          alert(message); // 경고창 띄우기
          window.location.href = url; // 페이지 이동
        }

        // 주기적 검사 함수
        function checkDevTools() {
            if (isDevToolsOpen() || detectConsoleUsage()) {

              redirectWithAlert("https://example.com/blocked", "개발자 도구 사용이 감지되었습니다. 페이지를 이동합니다."); //경고+이동
              clearInterval(intervalId); // 검사 중지
            }
        }
         const intervalId = setInterval(checkDevTools, 500); // 0.5초마다

        // 이미지 크기 변화 감지 부분 (devtoolsImage 관련 코드 - 이전 답변 내용 그대로)
        let devtoolsImage = new Image();
        let imageDetectionTriggered = false;

        devtoolsImage.onload = function() {
             if (!imageDetectionTriggered && (devtoolsImage.width > 100 || devtoolsImage.height > 100)) {
                imageDetectionTriggered = true; // 한 번만 실행
                redirectWithAlert("https://example.com/blocked", "개발자 도구 사용이 감지되었습니다(이미지). 페이지를 이동합니다."); //경고+이동
             }
        }
        devtoolsImage.src = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8z8BQDwAEhQGAhKmMIQAAAABJRU5ErkJggg==";



    </script>

    <noscript>
        <div class="warning">
            <h1>JavaScript 비활성화됨</h1>
            <p>이 웹사이트는 JavaScript를 필요로 합니다. JavaScript를 활성화해주세요.</p>
        </div>
        <!-- meta refresh를 사용하여 리디렉션 + 경고 -->
        <meta http-equiv="refresh" content="5;url=https://example.com/noscript">
        <script>
            // <noscript> 태그 안에서도 JavaScript 실행 가능 (아이러니하지만, 일부 브라우저에서 동작)
            // 5초 후에 리디렉션되기 전에 경고창을 띄움
            setTimeout(function() {
                alert("JavaScript가 비활성화되어 있어 5초 후 다른 페이지로 이동합니다.");
            }, 0); // 바로 실행 (0ms 지연)
        </script>
    </noscript>
</body>
</html>
```

**설명:**

1.  **`redirectWithAlert(url, message)` 함수 추가:**
    *   `alert(message)`: 주어진 메시지로 경고창을 띄웁니다.
    *   `window.location.href = url`: 주어진 URL로 페이지를 이동시킵니다.

2.  **`checkDevTools` 함수 수정:**
    *   개발자 도구가 감지되면, `blockPage()` 대신 `redirectWithAlert()` 함수를 호출합니다.
    *   경고 메시지와 함께 사용자를 `https://example.com/blocked` 페이지로 이동시킵니다.

3. **이미지 감지 부분 수정**
    *   `blockPage` 호출 부분을 `redirectWithAlert`를 호출하는 것으로 수정했습니다.

4.  **`<noscript>` 태그:**
    *   `meta refresh` 태그를 사용하여 5초 후 `https://example.com/noscript` 페이지로 리디렉션합니다. (`content="5;url=..."`)
    *   `<noscript>` 태그 안에 `<script>` 태그를 추가했습니다.  *아이러니하게도*, `<noscript>` 태그 안에서도 JavaScript 코드가 실행되는 경우가 있습니다 (모든 브라우저에서 보장되지는 않음).
    *   `setTimeout` 함수를 사용하여, 리디렉션되기 *직전*에 경고창을 띄웁니다.  `setTimeout`의 지연 시간을 `0`으로 설정하여 최대한 빨리 경고창을 띄우도록 했습니다.

**개선된 점:**

*   JavaScript가 활성화된 상태에서 개발자 도구가 감지되면, 경고창을 띄운 *후* 다른 페이지로 이동합니다.
*   JavaScript가 비활성화된 경우에도, `meta refresh`로 리디렉션되기 *전*에 경고창을 띄웁니다 (가능한 경우).

**여전히 중요한 고려 사항:**

*   `<noscript>` 안의 JavaScript 코드는 모든 브라우저에서 보장되지 않습니다.  일부 브라우저에서는 `<noscript>` 안의 `<script>` 태그를 무시할 수 있습니다.
*   `meta refresh`를 사용한 리디렉션은 사용자 경험 측면에서 좋지 않습니다 (페이지 깜빡임, 뒤로 가기 문제 등).  가능하면 JavaScript를 사용하여 리디렉션하는 것이 더 좋습니다.
*  이전 답변에서 언급했던 우회 가능성, 오탐 가능성, 사용자 경험, 서버 측 검증 등은 여전히 유효합니다.

이 코드는 JavaScript 비활성화와 개발자 도구 사용 감지에 대해 좀 더 사용자 친화적인 방식으로 대응하려고 시도했지만, 여전히 완벽한 해결책은 아니며, 한계가 있다는 것을 기억해야 합니다.
