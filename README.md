# English
영어학습플래너
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>매일 영어 성장 플래너 (월-금)</title>
    <style>
        :root {
            --bg-color: #f4f7f6;
            --card-bg: #ffffff;
            --text-main: #2c3e50;
            --accent: #3498db;
            --accent-light: #e8f4fd;
            --border: #e2e8f0;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }
        .container {
            width: 100%;
            max-width: 500px;
            background: var(--card-bg);
            padding: 25px;
            border-radius: 16px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }
        h1 {
            font-size: 20px;
            text-align: center;
            margin-bottom: 5px;
            color: #1a202c;
        }
        .date-display {
            text-align: center;
            font-size: 14px;
            color: #718096;
            margin-bottom: 20px;
        }
        .quote-box {
            background-color: var(--accent-light);
            border-left: 4px solid var(--accent);
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 25px;
            font-size: 14px;
            line-height: 1.6;
        }
        .quote-title {
            font-weight: bold;
            color: var(--accent);
            margin-bottom: 5px;
        }
        .routine-section {
            margin-bottom: 20px;
        }
        .section-title {
            font-size: 15px;
            font-weight: bold;
            margin-bottom: 10px;
            color: #4a5568;
            border-bottom: 1px solid var(--border);
            padding-bottom: 5px;
        }
        .todo-item {
            display: flex;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px dashed var(--border);
        }
        .todo-item:last-child {
            border-bottom: none;
        }
        .todo-item input[type="checkbox"] {
            width: 18px;
            height: 18px;
            margin-right: 12px;
            cursor: pointer;
        }
        .todo-text {
            font-size: 15px;
        }
        .time-tag {
            font-size: 12px;
            background: #edf2f7;
            padding: 2px 6px;
            border-radius: 4px;
            color: #4a5568;
            margin-left: auto;
        }
        .weekend-message {
            text-align: center;
            padding: 40px 20px;
            color: #718096;
            font-size: 15px;
            line-height: 1.6;
        }
    </style>
</head>
<body>

<div class="container">
    <div id="planner-content">
        </div>
</div>

<script>
    // 10일 주기 동기부여 명언 리스트 (짐퀵, 뇌가소성 이론 기반)
    const quotes = [
        { title: "Day 1: 뇌가소성의 힘", text: "뇌는 고정된 벽돌이 아니라 플라스틱과 같습니다. 새로운 언어를 학습하는 순간, 당신의 뇌세포는 새로운 지도를 그리며 물리적으로 변화합니다." },
        { title: "Day 2: 짐 퀵의 한계 초월", text: "당신의 기억력이나 지능은 정해져 있지 않습니다. '못한다'는 내면의 목소리를 지우고 '어떻게 할 수 있을까?'라는 질문으로 뇌를 깨우세요." },
        { title: "Day 3: 미세한 습관의 반복", text: "뇌가소성은 단 한 번의 강한 자극이 아니라, 매일 반복되는 미세한 자극에 더 강력하게 반응합니다. 매일 10분의 독서가 뇌를 바꿉니다." },
        { title: "Day 4: 디지털 치매 극복", text: "스마트폰에 의존할수록 뇌의 근육은 약해집니다. 오늘 외우는 로마서 한 구절이 당신의 뇌 근육을 지키는 가장 강력한 웨이트 트레이닝입니다." },
        { title: "Day 5: 뇌의 주파수 제어", text: "몰입할 때 우리 뇌는 알파파 상태가 됩니다. 쉐도잉을 시작하기 전 깊은 호흡을 3번 하세요. 뇌가 가장 학습하기 좋은 상태로 전환됩니다." },
        { title: "Day 6: 신경 연결망(Synapse) 확장", text: "외국어 통역과 쉐도잉은 좌뇌와 우뇌를 동시에 자극하는 최고의 고강도 뇌 운동입니다. 매일 새로운 신경망이 빛의 속도로 연결되고 있습니다." },
        { title: "Day 7: 짐 퀵의 FASTER 법칙", text: "새로운 지식을 얻고 싶다면 가르치듯 배워보세요. 오늘 학습한 영어 표현을 누군가에게 설명한다고 상상할 때 뇌의 기억 저장률은 극대화됩니다." },
        { title: "Day 8: 성인 뇌가소성의 진실", text: "과거 과학자들은 성인이 되면 뇌 세포 성장이 멈춘다고 믿었지만 틀렸습니다. 죽을 때까지 새로운 언어와 지식을 배우면 뉴런은 계속 생성됩니다." },
        { title: "Day 9: 몰입(Flow)의 조건", text: "너무 쉬우면 지루하고 너무 어려우면 포기하게 됩니다. ITC 단계별 수업처럼 자신의 수준보다 약간 높은 과제(Challenge)를 만날 때 뇌는 가장 빠르게 성장합니다." },
        { title: "Day 10: 당신은 당신의 생각보다 똑똑하다", text: "뇌는 상상과 현실을 구분하지 못합니다. 유창하게 통역하고 설교를 쉐도잉하는 당신의 모습을 생생하게 상상하세요. 뇌는 그 모습을 따라갑니다." }
    ];

    function initPlanner() {
        const now = new Date();
        const dayOfWeek = now.getDay(); // 0: 일, 1: 월, ..., 6: 토
        const plannerContent = document.getElementById('planner-content');

        // 주말인 경우 (토요일=6, 일요일=0)
        if (dayOfWeek === 0 || dayOfWeek === 6) {
            plannerContent.innerHTML = `
                <h1>주말 충전 모드 🌿</h1>
                <div class="date-display">${now.toLocaleDateString('ko-KR', { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' })}</div>
                <div class="weekend-message">
                    월요일부터 금요일까지 치열하게 달린 당신의 뇌에게 휴식을 선물하세요.<br>
                    주말의 충분한 수면과 휴식은 평일에 학습한 내용을 장기 기억으로 저장하는 필수 과정입니다.
                </div>
            `;
            return;
        }

        // 평일인 경우 (월-금)
        const dayTimestamp = Math.floor(now.getTime() / (1000 * 60 * 60 * 24));
        const quoteIndex = dayTimestamp % quotes.length;
        const todayQuote = quotes[quoteIndex];

        // 격일 루틴 구분 (홀수날/짝수날)
        const isEvenDay = now.getDate() % 2 === 0;

        let routineHTML = `
            <h1>Core 영어 루틴 플래너</h1>
            <div class="date-display">${now.toLocaleDateString('ko-KR', { year: 'numeric', month: 'long', day: 'numeric', weekday: 'long' })}</div>
            
            <div class="quote-box">
                <div class="quote-title">${todayQuote.title}</div>
                <div>"${todayQuote.text}"</div>
            </div>

            <div class="routine-section">
                <div class="section-title">🌅 아침 루틴 (뇌 깨우기)</div>
                <div class="todo-item">
                    <input type="checkbox" id="todo1">
                    <label class="todo-text" for="todo1">정철영어 로마서 암기 및 누적 복습</label>
                    <span class="time-tag">10분</span>
                </div>
            </div>
        `;

        if (isEvenDay) {
            routineHTML += `
                <div class="routine-section">
                    <div class="section-title">📚 코스 A: 다독 & 심화 트레이닝 데이</div>
                    <div class="todo-item">
                        <input type="checkbox" id="todo2">
                        <label class="todo-text" for="todo2">링큐(LingQ) 원서 독서</label>
                        <span class="time-tag">10분</span>
                    </div>
                    <div class="todo-item">
                        <input type="checkbox" id="todo3">
                        <label class="todo-text" for="todo3">리딩앤(Reading Gate) 다독</label>
                        <span class="time-tag">10분</span>
                    </div>
                    <div class="todo-item">
                        <input type="checkbox" id="todo4">
                        <label class="todo-text" for="todo4">설교 및 전문 자료 쉐도잉/영상 통역</label>
                        <span class="time-tag">40분</span>
                    </div>
                </div>
            `;
        } else {
            routineHTML += `
                <div class="routine-section">
                    <div class="section-title">⚡ 코스 B: ITC 하이퍼 몰입 데이</div>
                    <div class="todo-item">
                        <input type="checkbox" id="todo2">
                        <label class="todo-text" for="todo2">ITC 영어 온라인 수업 (9-12단계)</label>
                        <span class="time-tag">40분</span>
                    </div>
                    <div class="todo-item">
                        <input type="checkbox" id="todo3">
                        <label class="todo-text" for="todo3">ITC 수업 직후 스피킹 쉐도잉 복습</label>
                        <span class="time-tag">10분</span>
                    </div>
                </div>
            `;
        }

        routineHTML += `
            <div class="routine-section">
                <div class="section-title">🧸 Kids 루틴 (상시)</div>
                <div class="todo-item">
                    <input type="checkbox" id="todo-kids">
                    <label class="todo-text" for="todo-kids">칸아카데미 키즈 (2세 단계~)</label>
                    <span class="time-tag">15분</span>
                </div>
            </div>
        `;

        plannerContent.innerHTML = routineHTML;
    }

    window.onload = initPlanner;
</script>

</body>
</html>
