<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>성과평가면담 역량 진단</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://fonts.googleapis.com/css2?family=Nanum+Gothic&display=swap" rel="stylesheet">
  <style>
    body { font-family:'Nanum Gothic',sans-serif; margin:20px; background:#f5f7fa; }
    h1 { text-align:center; color:#333; margin-bottom:20px; }
    .question { background:#fff; border-radius:12px; padding:15px; box-shadow:0 4px 12px rgba(0,0,0,0.05); margin-bottom:15px; transition:transform .2s; display:flex; align-items:flex-start; }
    .question:hover { transform:translateY(-2px); }
    .question img.icon { width:24px; height:24px; margin-right:10px; margin-top:2px; }
    .question .text { flex:1; }
    button { padding:8px 16px; margin:5px; border:none; border-radius:5px; cursor:pointer; transition:background .3s,transform .1s; }
    .btn-yes { background:#e0f2f1; color:#00695c; }
    .btn-no  { background:#ffebee; color:#c62828; }
    .btn-yes.active { background:#26a69a; color:#fff; }
    .btn-no.active  { background:#e53935; color:#fff; }
    button:active { transform:scale(.95); }
    #result { display:none; margin-top:20px; }
    canvas { max-width:100%; margin:20px auto; }
    .feedback-container { display:flex; flex-wrap:wrap; gap:20px; justify-content:center; }
    .feedback-card { flex:1 1 300px; background:#fff; border-radius:12px; box-shadow:0 4px 10px rgba(0,0,0,0.1); overflow-y:auto; max-height:400px; color:#fff; padding:20px; }
    .feedback-card h3 { display:flex; align-items:center; gap:10px; margin-top:0; }
    .feedback-card img { width:40px; height:40px; }
    .type-평가제도  { background:linear-gradient(135deg,#1565c0,#1e88e5); }
    .type-목표이해  { background:linear-gradient(135deg,#ef6c00,#f57c00); }
    .type-팀원이해  { background:linear-gradient(135deg,#2e7d32,#43a047); }
    .type-면담스킬  { background:linear-gradient(135deg,#6a1b9a,#8e24aa); }
  </style>
</head>
<body>

  <h1>성과평가면담 역량 진단</h1>
  <div id="quiz">
    <!-- (생략: 위에 예시처럼 12개 질문) -->
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
      const q = btn.closest('.question');
      const yes = q.querySelector('.btn-yes');
      const no  = q.querySelector('.btn-no');
      yes.classList.remove('active');
      no.classList.remove('active');
      if (btn.classList.contains('btn-yes')) yes.classList.add('active');
      else no.classList.add('active');
      scores[q.dataset.category] += val;
    }

    function showResult() {
      // .active가 붙은 버튼이 있는 문항만 센다
      const answeredCount = [...document.querySelectorAll('.question')].filter(q => {
        return q.querySelector('.btn-yes').classList.contains('active')
            || q.querySelector('.btn-no').classList.contains('active');
      }).length;
      if (answeredCount < 12) {
        return alert("모든 문항에 답해주세요.");
      }

      document.getElementById('quiz').style.display   = 'none';
      document.getElementById('result').style.display = 'block';

      // 방사형 그래프
      new Chart(document.getElementById('chart').getContext('2d'), {
        type:'radar',
        data:{
          labels: Object.keys(scores),
          datasets:[{
            label:'Yes 응답 수',
            data: Object.values(scores),
            fill:true,
            borderColor:'rgb(54,162,235)',
            backgroundColor:'rgba(54,162,235,0.2)'
          }]
        },
        options:{
          scales:{ r:{ min:0, max:3, ticks:{ stepSize:1 } } }
        }
      });

      // 가장 낮은 2개 영역 뽑기
      const min = Math.min(...Object.values(scores));
      const lows = Object.keys(scores).filter(k=>scores[k]===min);
      let sel = lows.length <= 2
              ? lows
              : (()=>{
                  const out=[]; 
                  while(out.length<2){
                    const p=lows[Math.floor(Math.random()*lows.length)];
                    if(!out.includes(p)) out.push(p);
                  }
                  return out;
                })();

      // 전문 피드백 데이터 (생략하지 않고 실제 넣어주세요)
      const feedbackData = {
        /* "평가제도":{title,icon,color,text}, ... */
      };

      const wrap = document.getElementById('feedbackText');
      wrap.innerHTML = '';
      sel.forEach(key=>{
        const f = feedbackData[key];
        const d = document.createElement('div');
        d.className = `feedback-card ${f.color}`;
        d.innerHTML = `<h3><img src="${f.icon}"/>${f.title}</h3><p>${f.text}</p>`;
        wrap.appendChild(d);
      });
    }
  </script>
</body>
</html>


      // PDF 전문 피드백
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
