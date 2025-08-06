<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>성과평가면담 역량 진단</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://fonts.googleapis.com/css2?family=Nanum+Gothic&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #f5f7fa;
      --card: #fff;
      --primary: #1565c0;
      --secondary: #ef6c00;
      --success: #2e7d32;
      --accent: #6a1b9a;
      --shadow: rgba(0,0,0,0.1);
      --btn-yes: #e0f2f1;
      --btn-no: #ffebee;
      --btn-yes-active: #26a69a;
      --btn-no-active: #e53935;
    }
    * { box-sizing:border-box; margin:0; padding:0; }
    body {
      background: var(--bg);
      font-family: 'Nanum Gothic', sans-serif;
      color: #333;
    }
    .container {
      max-width: 640px;
      margin: 30px auto;
      padding: 0 20px;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 1.8rem;
    }
    .question {
      display: flex;
      align-items: flex-start;
      background: var(--card);
      border-radius: 12px;
      padding: 15px;
      margin-bottom: 15px;
      box-shadow: 0 4px 12px var(--shadow);
      transition: transform .2s, box-shadow .2s;
    }
    .question:hover {
      transform: translateY(-3px);
      box-shadow: 0 6px 16px var(--shadow);
    }
    .question img.icon {
      width: 24px; height: 24px;
      margin-right: 10px; margin-top: 2px;
    }
    .question .text strong {
      display: block;
      margin-bottom: 5px;
      color: var(--primary);
      font-weight: bold;
    }
    button {
      padding: 8px 16px;
      margin: 5px 5px 5px 0;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: transform .1s;
      font-size: 0.9rem;
    }
    .btn-yes { background: var(--btn-yes); color: #00695c; }
    .btn-no  { background: var(--btn-no);  color: #c62828; }
    .btn-yes.active { background: var(--btn-yes-active); color: #fff; }
    .btn-no.active  { background: var(--btn-no-active);  color: #fff; }
    button:active { transform: scale(.95); }

    #showBtn {
      display: block;
      width: 100%;
      padding: 12px;
      background: var(--primary);
      color: #fff;
      font-weight: 600;
      border-radius: 6px;
      margin-top: 10px;
    }

    #result {
      display: none;
      text-align: center;
      margin-top: 30px;
    }
    #result h2 {
      color: var(--primary);
      margin-bottom: 20px;
    }
    canvas {
      max-width: 100%;
      margin: 0 auto 30px;
    }
    .feedback-container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
      margin-bottom: 40px;
    }
    .feedback-card {
      flex: 1 1 280px;
      background: var(--card);
      border-radius: 12px;
      overflow-y: auto;
      max-height: 360px;
      box-shadow: 0 4px 12px var(--shadow);
      transition: transform .2s, box-shadow .2s;
      color: #333;
      padding: 20px;
    }
    .feedback-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 6px 16px var(--shadow);
    }
    .feedback-card h3 {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 15px;
      font-size: 1.1rem;
    }
    .feedback-card img { width: 36px; height: 36px; }
    .type-평가제도  { border-top: 6px solid var(--primary); }
    .type-목표이해  { border-top: 6px solid var(--secondary); }
    .type-팀원이해  { border-top: 6px solid var(--success); }
    .type-면담스킬  { border-top: 6px solid var(--accent); }
  </style>
</head>
<body>

  <div class="container">
    <h1>성과평가면담 역량 진단</h1>

    <div id="quiz">
      <!-- 질문 12개 (평가제도, 목표이해, 팀원이해, 면담스킬) 동일 패턴으로 삽입 -->
      <!-- 예시 한 문항만 보여드립니다 -->
      <div class="question" data-category="평가제도">
        <img class="icon" src="https://img.icons8.com/color/24/settings.png" alt="평가제도">
        <div class="text">
          <strong>평가제도 이해</strong>
          1. 우리 조직의 평가 제도에 대해 명확히 알고 있나요?<br>
          <button class="btn-yes" onclick="answer(this,1)">Yes</button>
          <button class="btn-no"  onclick="answer(this,0)">No</button>
        </div>
      </div>
      <!-- 나머지 11문항 여기에 동일하게 추가 -->

      <button id="showBtn" onclick="showResult()">결과 보기</button>
    </div>

    <div id="result">
      <h2>진단 결과</h2>
      <canvas id="chart"></canvas>
      <div class="feedback-container" id="feedbackText"></div>
    </div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    const scores = { "평가제도":0, "목표이해":0, "팀원이해":0, "면담스킬":0 };
    let answeredCount = 0;

    function answer(btn, val) {
      const q = btn.closest('.question');
      if (!q.dataset.answered) {
        q.dataset.answered = 'true';
        answeredCount++;
      }
      const yes = q.querySelector('.btn-yes');
      const no  = q.querySelector('.btn-no');
      yes.classList.remove('active');
      no.classList.remove('active');
      btn.classList.add('active');
      scores[q.dataset.category] += val;
    }

    function showResult() {
      if (answeredCount < 12) {
        return alert("모든 문항에 답해주세요.");
      }
      document.getElementById('quiz').style.display   = 'none';
      document.getElementById('result').style.display = 'block';

      // 방사형 그래프
      new Chart(document.getElementById('chart').getContext('2d'), {
        type: 'radar',
        data: {
          labels: Object.keys(scores),
          datasets: [{
            label: 'Yes 응답 수',
            data: Object.values(scores),
            fill: true,
            borderColor: 'rgb(54,162,235)',
            backgroundColor: 'rgba(54,162,235,0.2)'
          }]
        },
        options: {
          scales: { r: { min: 0, max: 3, ticks: { stepSize: 1 } } }
        }
      });

      // 최저 2개 유형 선택
      const minScore = Math.min(...Object.values(scores));
      const lows = Object.keys(scores).filter(k => scores[k] === minScore);
      const selected = (lows.length <= 2) ? lows : (() => {
        const arr = [];
        while (arr.length < 2) {
          const pick = lows[Math.floor(Math.random() * lows.length)];
          if (!arr.includes(pick)) arr.push(pick);
        }
        return arr;
      })();

      // PDF 전문 피드백 데이터 (3~6페이지 전문 그대로)
      const feedbackData = {
        /* ...각 유형별 전문 피드백... */
      };

      const wrap = document.getElementById("feedbackText");
      wrap.innerHTML = "";
      selected.forEach(key => {
        const f = feedbackData[key];
        const card = document.createElement("div");
        card.className = `feedback-card ${f.color}`;
        card.innerHTML = `<h3><img src="${f.icon}"/>${f.title}</h3><p>${f.text}</p>`;
        wrap.appendChild(card);
      });
    }
  </script>
</body>
</html>
