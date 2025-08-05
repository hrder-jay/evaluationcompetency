
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title>성과평가면담 역량 진단</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body { font-family: 'Segoe UI', sans-serif; margin: 20px; }
h1 { text-align: center; }
.question { margin-bottom: 15px; }
button { padding: 8px 16px; margin: 5px; cursor: pointer; border: none; border-radius: 5px; }
.btn-yes { background-color: #eee; color: #000; }
.btn-no { background-color: #eee; color: #000; }
.btn-yes.active { background-color: #2196F3; color: white; }
.btn-no.active { background-color: #2196F3; color: white; }
#result { display: none; text-align: center; margin-top: 20px; }
canvas { max-width: 100%; }
.feedback { margin-top: 20px; padding: 15px; border: 1px solid #ddd; border-radius: 8px; text-align: left; background: #fafafa; }
.feedback h3 { margin-top: 0; }
</style>
</head>
<body>

<h1>성과평가면담 역량 진단</h1>

<div id="quiz">
  <!-- 질문 시작 -->
  <div class="question" data-category="평가제도">
    1. 우리 조직의 평가 제도에 대해 명확히 알고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="평가제도">
    2. 우리 팀의 평가 배분 비율은 알고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="평가제도">
    3. 회사의 평가 프로세스를 명확히 인지하고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>

  <div class="question" data-category="목표이해">
    4. 수립된 팀의 목표를 기억하고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="목표이해">
    5. 팀원 개인별 목표를 알고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="목표이해">
    6. 개인 KPI 달성률을 파악하고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>

  <div class="question" data-category="팀원이해">
    7. 팀원의 역량(리더십/업무)에 대해 측정이 되어 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="팀원이해">
    8. 팀원 업무의 목적과 활동을 알고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="팀원이해">
    9. 팀원의 평소 업무·의사소통 스타일을 알고 있나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>

  <div class="question" data-category="면담스킬">
    10. 면담 시작 전 목적을 명확히 설정하나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="면담스킬">
    11. 팀원의 예상 반응을 예측하고 면담하나요?<br>
    <button class="btn-yes" onclick="answer(this,1)">Yes</button>
    <button class="btn-no" onclick="answer(this,0)">No</button>
  </div>
  <div class="question" data-category="면담스킬">
    12. 저항이 두려워 면담을 피한 적이 있나요? (Yes=부족)<br>
    <button class="btn-yes" onclick="answer(this,0)">Yes</button>
    <button class="btn-no" onclick="answer(this,1)">No</button>
  </div>

  <button onclick="showResult()">결과 보기</button>
</div>

<div id="result">
  <h2>진단 결과</h2>
  <canvas id="chart"></canvas>
  <div class="feedback" id="feedbackText"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
let scores = { "평가제도":0, "목표이해":0, "팀원이해":0, "면담스킬":0 };

function answer(btn, val){
  let category = btn.parentElement.getAttribute("data-category");
  let yesBtn = btn.parentElement.querySelector(".btn-yes");
  let noBtn = btn.parentElement.querySelector(".btn-no");

  yesBtn.classList.remove("active");
  noBtn.classList.remove("active");

  if (btn.classList.contains("btn-yes")) {
    yesBtn.classList.add("active");
  } else {
    noBtn.classList.add("active");
  }

  btn.parentElement.answered = true;
  scores[category] = (val === 1) ? (scores[category] + 1) : scores[category];
}

function showResult(){
  let answeredCount = 0;
  document.querySelectorAll(".question").forEach(q=>{
    if(q.answered) answeredCount++;
  });
  if(answeredCount < 12){
    alert("모든 문항에 답해주세요.");
    return;
  }

  document.getElementById("quiz").style.display = "none";
  document.getElementById("result").style.display = "block";

  let ctx = document.getElementById('chart').getContext('2d');
  let maxScore = 3;
  new Chart(ctx, {
    type: 'radar',
    data: {
      labels: Object.keys(scores),
      datasets: [{
        label: 'Yes 응답 수',
        data: Object.values(scores),
        fill: true,
        borderColor: 'rgb(54, 162, 235)',
        backgroundColor: 'rgba(54, 162, 235, 0.2)'
      }]
    },
    options: {
      scales: {
        r: { min: 0, max: maxScore, ticks: { stepSize: 1 } }
      }
    }
  });

  let weakest = Object.keys(scores).reduce((a,b) => scores[a] < scores[b] ? a : b);
  let feedback = {
    "평가제도": `<h3>공정상실형</h3><p>“평가의 기본은 회사의 평가제도에 기반한다.”를 잊지 마세요...</p>`,
    "목표이해": `<h3>목표기억상실형</h3><p>목표는 반드시 달성해야 할 지표입니다...</p>`,
    "팀원이해": `<h3>방목위임형</h3><p>평소 팀원에게 관심을 가지세요...</p>`,
    "면담스킬": `<h3>착한사람증후군</h3><p>면담 목적을 분명히 하세요...</p>`
  };

  document.getElementById("feedbackText").innerHTML = feedback[weakest];
}
</script>

</body>
</html>
