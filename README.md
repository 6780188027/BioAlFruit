<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Fermentation Lab Simulator</title>

  <style>
    *{
      margin:0;
      padding:0;
      box-sizing:border-box;
      font-family: "Prompt", sans-serif;
    }

    body{
      background: linear-gradient(135deg,#fef6e4,#ffd6a5);
      min-height:100vh;
      overflow-x:hidden;
      color:#333;
    }

    .container{
      width:95%;
      max-width:1200px;
      margin:auto;
      padding:30px 0;
    }

    .title{
      text-align:center;
      margin-bottom:20px;
    }

    .title h1{
      font-size:3rem;
      color:#7b2d26;
    }

    .title p{
      margin-top:10px;
      font-size:1.1rem;
      color:#5c4033;
    }

    .game-area{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap:25px;
      margin-top:30px;
    }

    .panel{
      background:white;
      border-radius:25px;
      padding:25px;
      box-shadow:0 10px 25px rgba(0,0,0,0.1);
    }

    h2{
      margin-bottom:15px;
      color:#8d5524;
    }

    .ingredient{
      margin-bottom:20px;
    }

    label{
      display:block;
      margin-bottom:8px;
      font-weight:bold;
    }

    select, input[type=range]{
      width:100%;
      padding:10px;
      border-radius:10px;
      border:none;
      background:#f3f3f3;
    }

    .btn{
      width:100%;
      margin-top:20px;
      padding:15px;
      border:none;
      border-radius:15px;
      background:#ff7b54;
      color:white;
      font-size:1.1rem;
      cursor:pointer;
      transition:0.3s;
      font-weight:bold;
    }

    .btn:hover{
      background:#ff5722;
      transform:scale(1.03);
    }

    .lab{
      position:relative;
      height:400px;
      display:flex;
      align-items:center;
      justify-content:center;
      flex-direction:column;
    }

    .flask{
      width:180px;
      height:250px;
      border:8px solid #555;
      border-top:none;
      border-radius:0 0 80px 80px;
      position:relative;
      overflow:hidden;
      background:white;
    }

    .liquid{
      position:absolute;
      bottom:0;
      width:100%;
      height:30%;
      background:orange;
      transition:2s;
    }

    .bubble{
      position:absolute;
      bottom:20px;
      width:15px;
      height:15px;
      background:white;
      border-radius:50%;
      opacity:0.7;
      animation: rise 4s infinite;
    }

    @keyframes rise{
      0%{
        transform:translateY(0);
        opacity:0.7;
      }
      100%{
        transform:translateY(-220px);
        opacity:0;
      }
    }

    .result-box{
      margin-top:25px;
      padding:20px;
      border-radius:15px;
      background:#fff3cd;
      min-height:120px;
      line-height:1.7;
    }

    .score{
      margin-top:15px;
      font-size:1.4rem;
      font-weight:bold;
      color:#b22222;
    }

    .meter{
      margin-top:15px;
      height:25px;
      background:#ddd;
      border-radius:20px;
      overflow:hidden;
    }

    .meter-fill{
      height:100%;
      width:0%;
      background:linear-gradient(90deg,#ff9800,#ff5722);
      transition:1s;
    }

    .info{
      margin-top:20px;
      background:#e8f5e9;
      padding:15px;
      border-radius:15px;
      line-height:1.8;
    }

    .footer{
      text-align:center;
      margin-top:30px;
      color:#555;
    }

    @media(max-width:900px){
      .game-area{
        grid-template-columns:1fr;
      }
    }

  </style>
</head>
<body>

  <div class="container">

    <div class="title">
      <h1>🍇 Fermentation Lab Simulator</h1>
      <p>
        เกมจำลองกระบวนการหมักแอลกอฮอล์จากน้ำผลไม้และยีสต์
        สำหรับวิชาชีววิทยา เรื่อง "กระบวนการหมัก"
      </p>
    </div>

    <div class="game-area">

      <!-- LEFT -->
      <div class="panel">

        <h2>🧪 เลือกส่วนผสม</h2>

        <div class="ingredient">
          <label>🍎 ชนิดของน้ำผลไม้</label>
          <select id="fruit">
            <option value="apple">น้ำแอปเปิล</option>
            <option value="grape">น้ำองุ่น</option>
            <option value="orange">น้ำส้ม</option>
            <option value="pineapple">น้ำสับปะรด</option>
          </select>
        </div>

        <div class="ingredient">
          <label>🍬 ปริมาณน้ำตาล</label>
          <input type="range" id="sugar" min="1" max="10" value="5">
          <p id="sugarValue">5</p>
        </div>

        <div class="ingredient">
          <label>🌡️ อุณหภูมิ</label>
          <input type="range" id="temp" min="10" max="45" value="30">
          <p id="tempValue">30°C</p>
        </div>

        <div class="ingredient">
          <label>🦠 ปริมาณยีสต์</label>
          <input type="range" id="yeast" min="1" max="10" value="5">
          <p id="yeastValue">5</p>
        </div>

        <button class="btn" onclick="startFermentation()">
          ▶ เริ่มการหมัก
        </button>

        <div class="info">
          <strong>📚 ความรู้ชีววิทยา:</strong><br>
          ยีสต์จะเปลี่ยนน้ำตาลให้เป็นแอลกอฮอล์และก๊าซคาร์บอนไดออกไซด์
          ผ่านกระบวนการ "Fermentation"
        </div>

      </div>

      <!-- RIGHT -->
      <div class="panel">

        <h2>⚗️ ห้องปฏิบัติการหมัก</h2>

        <div class="lab">

          <div class="flask">
            <div class="liquid" id="liquid"></div>

            <div class="bubble" style="left:30px;"></div>
            <div class="bubble" style="left:80px; animation-delay:1s;"></div>
            <div class="bubble" style="left:120px; animation-delay:2s;"></div>

          </div>

          <div class="meter">
            <div class="meter-fill" id="meterFill"></div>
          </div>

        </div>

        <div class="result-box" id="result">
          รอเริ่มการทดลอง...
        </div>

        <div class="score" id="score"></div>

      </div>

    </div>

    <div class="footer">
      Developed for Biology Learning Game 🎮
    </div>

  </div>

  <script>

    const sugar = document.getElementById("sugar");
    const temp = document.getElementById("temp");
    const yeast = document.getElementById("yeast");

    sugar.oninput = () => {
      document.getElementById("sugarValue").innerText = sugar.value;
    }

    temp.oninput = () => {
      document.getElementById("tempValue").innerText = temp.value + "°C";
    }

    yeast.oninput = () => {
      document.getElementById("yeastValue").innerText = yeast.value;
    }

    function startFermentation(){

      let fruit = document.getElementById("fruit").value;
      let sugarVal = parseInt(sugar.value);
      let tempVal = parseInt(temp.value);
      let yeastVal = parseInt(yeast.value);

      let alcohol = 0;
      let quality = "";
      let explanation = "";

      // สูตรคำนวณง่าย ๆ
      alcohol = (sugarVal * 1.5) + (yeastVal);

      if(tempVal >= 25 && tempVal <= 35){
        alcohol += 5;
      }else{
        alcohol -= 3;
      }

      if(alcohol < 10){
        quality = "❌ การหมักไม่สมบูรณ์";
      }
      else if(alcohol < 18){
        quality = "🟡 การหมักระดับปานกลาง";
      }
      else{
        quality = "✅ การหมักสมบูรณ์";
      }

      // เปลี่ยนสีตามผลไม้
      let liquid = document.getElementById("liquid");

      if(fruit === "apple"){
        liquid.style.background = "#f4c542";
      }
      else if(fruit === "grape"){
        liquid.style.background = "#7b1fa2";
      }
      else if(fruit === "orange"){
        liquid.style.background = "#ff9800";
      }
      else{
        liquid.style.background = "#ffd54f";
      }

      liquid.style.height = (30 + alcohol) + "%";

      // meter
      document.getElementById("meterFill").style.width = alcohol * 4 + "%";

      explanation = `
        <strong>ผลการทดลอง</strong><br><br>

        🍷 ระดับแอลกอฮอล์ประมาณ: <strong>${alcohol.toFixed(1)}%</strong><br>
        🧪 สถานะการหมัก: <strong>${quality}</strong><br><br>

        🔬 ยีสต์ใช้ "น้ำตาล" เป็นแหล่งพลังงาน
        และเปลี่ยนเป็นแอลกอฮอล์ + ก๊าซคาร์บอนไดออกไซด์<br><br>

        🌡️ อุณหภูมิที่เหมาะสมช่วยให้ยีสต์ทำงานได้ดีขึ้น
      `;

      document.getElementById("result").innerHTML = explanation;

      // score
      let score = 0;

      if(tempVal >= 25 && tempVal <= 35){
        score += 40;
      }

      score += sugarVal * 3;
      score += yeastVal * 3;

      if(score > 100){
        score = 100;
      }

      document.getElementById("score").innerHTML =
      "🏆 คะแนนการทดลอง: " + score + "/100";

    }

  </script>

</body>
</html>
