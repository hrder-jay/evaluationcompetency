<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>성과평가면담 역량 진단</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://fonts.googleapis.com/css2?family=Nanum+Gothic&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Nanum Gothic', sans-serif;
      margin: 20px;
      background: #f5f7fa;
    }
    h1 {
      text-align: center;
      color: #333;
      margin-bottom: 20px;
    }
    .question {
      display: flex;
      align-items: flex-start;
      background: #fff;
      border-radius: 12px;
      padding: 15px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.05);
      margin-bottom: 15px;
      transition: transform 0.2s;
    }
    .question:hover {
      transform: translateY(-2px);
    }
    .question img.icon {
      width: 24px;
      height: 24px;
      margin-right: 10px;
      margin-top: 2px;
    }
    .question .text {
      flex: 1;
    }
    button {
      padding: 8px 16px;
      margin: 5px;
      border: none;
      border-radius: 5px;
      font-size: 14px;
      cursor: pointer;
      transition: background 0.3s, transform 0.1s;
    }
    .btn-yes { background: #e0f2f1; color: #00695c; }
    .btn-no  { background: #ffebee; color: #c62828; }
    .btn-yes.active { background: #26a69a; color: #fff; }
    .btn-no.active  { background: #e53935; color: #fff; }
    button:active { transform: scale(0.95); }

    #result {
      display: none;
      margin-top: 20px;
    }
    canvas {
      max-width: 100%;
      margin: 20px auto;
    }

    .feedback-container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
    }
    .feedback-card {
      flex: 1 1 300px;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      overflow-y: auto;
      max-height: 400px;
      color: #fff;
      padding: 20px;
    }
    .feedback-card h3 {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-top: 0;
    }
    .feedback-card img {
      width: 40px;
      height: 40px;
    }
    .type-평가제도 { background: linear-gradient(135deg,#1565c0,#1e88e5); }
    .type-목표이해 { background: linear-gradient(135deg,#ef6c00,#f57c00); }
    .type-팀원이해 { background: linear-gradient(135deg,#2e7d32,#43a047); }
    .type-면담스킬 { background: linear-gradient(135deg,#6a1b9a,#8e24aa); }
  </style>
</head>
<body>

  <h1>성과평가면담 역량 진단</h1>

  <div id="quiz">
    <!-- 평가제도 -->
    <div class="question" data-category="평가제도">
      <img class="icon" src="https://img.icons8.com/color/24/settings.png" alt="평가제도"/>
      <div class="text">
        <strong>평가제도 이해</strong><br>
        1. 우리 조직의 평가 제도에 대해 명확히 알고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="평가제도">
      <img class="icon" src="https://img.icons8.com/color/24/settings.png" alt="평가제도"/>
      <div class="text">
        2. 우리 팀의 평가 배분 비율은 알고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="평가제도">
      <img class="icon" src="https://img.icons8.com/color/24/settings.png" alt="평가제도"/>
      <div class="text">
        3. 회사의 평가 프로세스를 명확히 인지하고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>

    <!-- 목표이해 -->
    <div class="question" data-category="목표이해">
      <img class="icon" src="https://img.icons8.com/color/24/target.png" alt="목표이해"/>
      <div class="text">
        <strong>목표 이해</strong><br>
        4. 수립된 팀의 목표를 기억하고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="목표이해">
      <img class="icon" src="https://img.icons8.com/color/24/target.png" alt="목표이해"/>
      <div class="text">
        5. 팀원 개인별 목표를 알고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="목표이해">
      <img class="icon" src="https://img.icons8.com/color/24/target.png" alt="목표이해"/>
      <div class="text">
        6. 개인 KPI 달성률을 파악하고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>

    <!-- 팀원이해 -->
    <div class="question" data-category="팀원이해">
      <img class="icon" src="https://img.icons8.com/color/24/people.png" alt="팀원이해"/>
      <div class="text">
        <strong>팀원 이해</strong><br>
        7. 팀원의 역량(리더십/업무)에 대해 측정이 되어 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="팀원이해">
      <img class="icon" src="https://img.icons8.com/color/24/people.png" alt="팀원이해"/>
      <div class="text">
        8. 팀원 업무의 목적과 활동을 알고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="팀원이해">
      <img class="icon" src="https://img.icons8.com/color/24/people.png" alt="팀원이해"/>
      <div class="text">
        9. 팀원의 평소 업무·의사소통 스타일을 알고 있나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>

    <!-- 면담스킬 -->
    <div class="question" data-category="면담스킬">
      <img class="icon" src="https://img.icons8.com/color/24/chat.png" alt="면담스킬"/>
      <div class="text">
        <strong>면담 스킬</strong><br>
        10. 면담 시작 전 목적을 명확히 설정하나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="면담스킬">
      <img class="icon" src="https://img.icons8.com/color/24/chat.png" alt="면담스킬"/>
      <div class="text">
        11. 팀원의 예상 반응을 예측하고 면담하나요?<br>
        <button class="btn-yes" onclick="answer(this,1)">Yes</button>
        <button class="btn-no" onclick="answer(this,0)">No</button>
      </div>
    </div>
    <div class="question" data-category="면담스킬">
      <img class="icon" src="https://img.icons8.com/color/24/chat.png" alt="면담스킬"/>
      <div class="text">
        12. 저항이 두려워 면담을 피한 적이 있나요?<br>
        <button class="btn-yes" onclick="answer(this,0)">Yes</button>
        <button class="btn-no" onclick="answer(this,1)">No</button>
      </div>
    </div>

    <button onclick="showResult()">결과 보기</button>
  </div>

  <div id="result">
    <h2>진단 결과</h2>
    <canvas id="chart"></canvas>
    <div class="feedback-container" id="feedbackText"></div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script>
    let scores = { "평가제도":0, "목표이해":0, "팀원이해":0, "면담스킬":0 };

    function answer(btn, val) {
      // 변경: .closest('.question')로 올바른 question 요소 선택
      const questionEl = btn.closest('.question');
      const category = questionEl.getAttribute("data-category");
      const yesBtn = questionEl.querySelector(".btn-yes");
      const noBtn  = questionEl.querySelector(".btn-no");

      yesBtn.classList.remove("active");
      noBtn.classList.remove("active");
      if (btn.classList.contains("btn-yes")) {
        yesBtn.classList.add("active");
      } else {
        noBtn.classList.add("active");
      }

      // 변경: questionEl에 직접 answered 플래그 기록
      questionEl.answered = true;
      scores[category] += val;
    }

    function showResult() {
      const allQuestions = document.querySelectorAll(".question");
      const answeredCount = [...allQuestions].filter(q => q.answered).length;
      if (answeredCount < allQuestions.length) {
        alert("모든 문항에 답해주세요.");
        return;
      }

      document.getElementById("quiz").style.display = "none";
      document.getElementById("result").style.display = "block";

      // 방사형 그래프
      const ctx = document.getElementById('chart').getContext('2d');
      new Chart(ctx, {
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

      // 가장 낮은 2개 유형 선택
      const minScore = Math.min(...Object.values(scores));
      const lows = Object.keys(scores).filter(k => scores[k] === minScore);
      let selected = [];
      if (lows.length <= 2) {
        selected = lows;
      } else {
        while (selected.length < 2) {
          const pick = lows[Math.floor(Math.random() * lows.length)];
          if (!selected.includes(pick)) selected.push(pick);
        }
      }

      // PDF 전문 피드백 데이터 (생략 없이 그대로 사용)
      const feedbackData = {
        "평가제도": {
          title: "공정상실형",
          icon: "https://img.icons8.com/color/48/scales.png",
          color: "type-평가제도",
          text: `<b>공정상실형에서 벗어나기 위한 면담 Tip</b><br><br>
1. “평가의 기본은 회사의 평가제도에 기반한다.”를 잊지 마세요.<br>
- 승진대상자, 자녀의 진학, 가정의 형편 등 우리는 팀원들의 여러가지 개인 사정을 마주합니다.<br>
- 조직과 팀 문화에 따라 일부 개인 사가 용인되더라도, 지속되면 다른 팀원들의 불만으로 이어집니다.<br>
- 승진 대상자이기 때문에 저성과자가 A등급을 받으면, 이후 아무도 열심히 하지 않을 수 있습니다.<br><br>
- 명확한 평가제도 이해는 팀원에게 공정함을 인식시키는 최고의 수단입니다.<br>
- 목표 달성 정도에 따른 배분 시 ‘냉정’ 아닌 ‘공정’하다는 인식을 줍니다.<br>
- 절대평가·상대평가 구분을 명확히 하고, 향후 발전 방안을 제시해야 합니다.<br><br>
2. 정(情)에 휘둘리면 팀원들에게 정(釘) 맞습니다.`
        },
        "목표이해": {
          title: "목표기억상실형",
          icon: "https://img.icons8.com/color/48/target.png",
          color: "type-목표이해",
          text: `<b>목표기억상실형에서 벗어나기 위한 면담 Tip</b><br><br>
1. 목표는 반드시 달성해야 할 지표입니다.<br>
- 팀원들은 KPI 달성을 위해 1년 내내 노력합니다.<br>
- 리더는 방향 제시와 상시 면담을 통해 팀을 이끌어야 합니다.<br>
- 관리가 없으면 팀원은 책임을 리더에게 전가합니다.<br><br>
- 목표 달성은 팀원의 열정보다 앞서야 합니다.<br>
- 정성적 요소 개입은 가능해도, 평가 기준은 흔들어선 안 됩니다.<br><br>
2. ‘팀원들 KPI가 뭐였지?’ 유형을 주의하세요.`
        },
        "팀원이해": {
          title: "방목위임형",
          icon: "https://img.icons8.com/color/48/conference.png",
          color: "type-팀원이해",
          text: `<b>방목위임형에서 벗어나기 위한 면담 Tip</b><br><br>
1. 평소 팀원에게 관심을 가져야 합니다.<br>
- 말투·행동을 모르면, 일시적 태도로 평가합니다.<br>
- ‘지금’ ‘이 자리’ 행동만 평가하면 면담이 실패할 수 있습니다.<br><br>
- 평가면담은 단순 통보 자리가 아닙니다.<br>
- 뛰어난 역량은 강화, 부족한 부분은 교정 피드백을 제공합니다.<br><br>
2. 팀원 성향에 맞춘 면담을 진행하세요.`
        },
        "면담스킬": {
          title: "착한사람증후군",
          icon: "https://img.icons8.com/color/48/chat.png",
          color: "type-면담스킬",
          text: `<b>착한사람증후군에서 벗어나기 위한 면담 Tip</b><br><br>
1. 면담 목적을 명확히 설정하세요.<br>
- 예상 반응 준비 없이 시작하면 기준이 흔들립니다.<br>
- 저항을 두려워해 피하면 리더십이 약해집니다.<br><br>
2. 반복 면담은 상처만 남길 수 있습니다.<br>
- 팀원의 예상 반응을 학습하고 단호하게 진행하세요.`
        }
      };

      const container = document.getElementById("feedbackText");
      container.innerHTML = "";
      selected.forEach(key => {
        const f = feedbackData[key];
        const card = document.createElement("div");
        card.className = `feedback-card ${f.color}`;
        card.innerHTML = `<h3><img src="${f.icon}"/>${f.title}</h3><p>${f.text}</p>`;
        container.appendChild(card);
      });
    }
  </script>

</body>
</html>
