<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>성과평가면담 역량 진단</title>
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@400;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #f0f2f5;
      --card-bg: #fff;
      --primary: #3b82f6;
      --success: #10b981;
      --error: #ef4444;
      --shadow: rgba(0,0,0,0.1);
    }
    * { box-sizing:border-box; margin:0; padding:0 }
    body {
      font-family:'Pretendard',sans-serif;
      background:var(--bg);
      color:#333;
    }
    .container {
      max-width: 600px;
      margin: 30px auto;
      padding: 0 15px;
    }
    h1 {
      text-align:center;
      margin-bottom:20px;
      font-size:1.8rem;
      font-weight:600;
    }
    #progress {
      width:100%; height:8px;
      background:#ddd; border-radius:4px;
      overflow:hidden; margin-bottom:20px;
    }
    #progress span {
      display:block; height:100%;
      background:var(--primary);
      width:0; transition:width .3s;
    }
    .question {
      display:flex; align-items:flex-start;
      background:var(--card-bg);
      border-radius:12px;
      padding:15px;
      box-shadow:0 4px 12px var(--shadow);
      margin-bottom:15px;
      transition:transform .2s,box-shadow .2s;
    }
    .question:hover {
      transform:translateY(-3px);
      box-shadow:0 6px 16px var(--shadow);
    }
    .question img.icon {
      width:24px; height:24px;
      margin-right:10px; margin-top:4px;
    }
    .question .text strong {
      color:var(--primary);
      display:block; margin-bottom:5px;
      font-weight:600;
    }
    button {
      padding:8px 16px;
      margin:5px 5px 5px 0;
      border:none; border-radius:6px;
      font-size:14px; cursor:pointer;
      transition:transform .1s;
    }
    .btn-yes { background:var(--success); color:#fff; }
    .btn-no  { background:var(--error); color:#fff; }
    button:disabled { opacity:.5; cursor:not-allowed }
    button:active { transform:scale(.95) }

    #result { display:none; margin-top:30px; text-align:center }
    #result h2 { margin-bottom:20px; font-weight:600; color:var(--primary) }
    canvas { max-width:100%; margin:0 auto }

    .feedback-container {
      display:flex; flex-wrap:wrap;
      gap:20px; justify-content:center;
      margin-top:30px;
    }
    .feedback-card {
      flex:1 1 260px;
      background:var(--card-bg);
      border-radius:12px;
      box-shadow:0 4px 12px var(--shadow);
      overflow-y:auto; max-height:380px;
      padding:20px; position:relative;
      transition:transform .2s,box-shadow .2s;
    }
    .feedback-card:hover {
      transform:translateY(-3px);
      box-shadow:0 6px 16px var(--shadow);
    }
    .feedback-card h3 {
      display:flex; align-items:center; gap:10px;
      margin-bottom:15px; font-size:1.2rem; font-weight:600;
    }
    .feedback-card img { width:36px; height:36px }
    .type-평가제도 { border-top:6px solid #1565c0 }
    .type-목표이해 { border-top:6px solid #ef6c00 }
    .type-팀원이해 { border-top:6px solid #2e7d32 }
    .type-면담스킬 { border-top:6px solid #6a1b9a }

    #showBtn {
      display:block; width:100%; background:var(--primary);
      color:#fff; font-weight:600; margin-top:10px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>성과평가면담 역량 진단</h1>
    <div id="progress"><span></span></div>

    <div id="quiz">
      <!-- 평가제도 -->
      <div class="question" data-category="평가제도">
        <img class="icon" src="https://img.icons8.com/color/24/settings.png" alt="평가제도"/>
        <div class="text">
          <strong>평가제도 이해</strong>
          1. 우리 조직의 평가 제도에 대해 명확히 알고 있나요?
          <button class="btn-yes" onclick="answer(this,1)">Yes</button>
          <button class="btn-no" onclick="answer(this,0)">No</button>
        </div>
      </div>
      <!-- (나머지 11문항 동일 패턴) -->

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
    const score = { "평가제도":0, "목표이해":0, "팀원이해":0, "면담스킬":0 };
    const total = 12;
    let answered = 0;

    function updateProgress(){
      document.querySelector('#progress span').style.width = (answered/total*100)+'%';
    }

    function answer(btn,val){
      const q=btn.closest('.question');
      if(!q.dataset.answered){
        answered++;
        q.dataset.answered='true';
        updateProgress();
      }
      const yes=q.querySelector('.btn-yes'),
            no =q.querySelector('.btn-no');
      yes.classList.remove('active'); no.classList.remove('active');
      btn.classList.contains('btn-yes')?yes.classList.add('active'):no.classList.add('active');
      score[q.dataset.category]+=val;
    }

    function showResult(){
      if(answered<total) return alert("모든 문항에 답해주세요.");
      document.getElementById('quiz').style.display='none';
      document.getElementById('result').style.display='block';

      // 그래프
      new Chart(document.getElementById('chart').getContext('2d'),{
        type:'radar',
        data:{
          labels:Object.keys(score),
          datasets:[{
            label:'Yes 응답 수',
            data:Object.values(score),
            fill:true,
            borderColor:'rgb(54,162,235)',
            backgroundColor:'rgba(54,162,235,0.2)'
          }]
        },
        options:{ scales:{ r:{ min:0,max:3,ticks:{ stepSize:1 } } } }
      });

      // 최저 2개
      const mn=Math.min(...Object.values(score));
      const lows=Object.keys(score).filter(k=>score[k]===mn);
      const sel=lows.length<=2?lows:(()=>{
        const a=[];
        while(a.length<2){
          const c=lows[Math.floor(Math.random()*lows.length)];
          if(!a.includes(c)) a.push(c);
        }
        return a;
      })();

      // 전문 피드백
      const feedbackData = {
        "평가제도": {
          title:"공정상실형",
          icon:"https://img.icons8.com/color/48/scales.png",
          color:"type-평가제도",
          text:`
<b>공정상실형에서 벗어나기 위한 면담 Tip</b><br>
1. “평가의 기본은 회사의 평가제도에 기반한다.”를 잊지 마세요.<br>
- 승진대상자, 자녀의 진학, 가정의 형편 등…<br>
- 조직과 팀의 문화에 따라…<br>
- 승진대상자가 A등급을 받으면…<br>
<br>
- 명확한 평가제도 이해는…<br>
- 목표 달성 배분 시…<br>
- 절대평가·상대평가 구분…<br>
<br>
2. 정(情)에 휘둘리면 팀원들에게 정(釘) 맞습니다.
`
        },
        "목표이해": {
          title:"목표기억상실형",
          icon:"https://img.icons8.com/color/48/target.png",
          color:"type-목표이해",
          text:`
<b>목표기억상실형에서 벗어나기 위한 면담 Tip</b><br>
1. 목표는 반드시 달성해야 할 지표입니다.<br>
- 팀원들은 KPI 달성을 위해 노력합니다.<br>
- 리더는 방향 제시와 상시 면담을…<br>
- 과정 관리 부족 시…<br>
<br>
- 목표 달성은 열정보다…<br>
- 정성적 개입 가능하나…<br>
2. “팀원들 KPI가 뭐였지?” 유형 주의.
`
        },
        "팀원이해": {
          title:"방목위임형",
          icon:"https://img.icons8.com/color/48/conference.png",
          color:"type-팀원이해",
          text:`
<b>방목위임형에서 벗어나기 위한 면담 Tip</b><br>
1. 평소 팀원에게 관심을…<br>
- 말투·행동 파악 부족 시…<br>
- “지금” 행동만 평가 시…<br>
<br>
- 평가면담은 통보 자리가…<br>
- 뛰어난 점은 강화, 부족한 점은…<br>
2. 성향에 맞춘 면담을.
`
        },
        "면담스킬": {
          title:"착한사람증후군",
          icon:"https://img.icons8.com/color/48/chat.png",
          color:"type-면담스킬",
          text:`
<b>착한사람증후군에서 벗어나기 위한 면담 Tip</b><br>
1. 면담 목적을 명확히…<br>
- 예상 반응 준비 없이…<br>
- 저항 두려워 시…<br>
<br>
2. 반복 면담은 상처만…<br>
- 예상 반응 학습 후 단호하게…
`
        }
      };

      const wrap = document.getElementById('feedbackText');
      wrap.innerHTML = '';
      sel.forEach(k=>{
        const f = feedbackData[k];
        const d = document.createElement('div');
        d.className = `feedback-card ${f.color}`;
        d.innerHTML = `<h3><img src="${f.icon}"/>${f.title}</h3><p>${f.text}</p>`;
        wrap.appendChild(d);
      });
    }
  </script>
</body>
</html>
