# body-
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lumière Wellness</title>
    <!-- 読み込みエラーを防ぐため、標準的なライブラリに統一 -->
    <script src="https://unpkg.com/react@17/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital@0;1&family=Jost:wght@300;400;500&display=swap" rel="stylesheet">
    <style>
        :root {
          --ink: #1a1118; --parchment: #faf6f0; --blush: #f2e8e0;
          --rose: #c97a7a; --roseDark: #a85a5a; --gold: #c9a96e;
          --sage: #8aab96; --text: #3a2a32; --textMid: #7a6070;
          --textFaint: #b0a0a8; --border: rgba(201,169,110,0.2);
          --cardBg: rgba(255,252,248,0.92); --ff: 'Playfair Display', serif; --fs: 'Jost', sans-serif;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { background: var(--ink); font-family: var(--fs); background-color: var(--parchment); }
        .lm { max-width: 430px; margin: 0 auto; min-height: 100vh; background: var(--parchment); color: var(--text); padding: 20px; position: relative; }
        .hero { padding: 30px 10px; position: relative; }
        .hero-eyebrow { font-size: 9px; letter-spacing: 0.35em; text-transform: uppercase; color: var(--gold); margin-bottom: 8px; }
        .hero-name { font-family: var(--ff); font-size: 42px; font-weight: 300; color: var(--ink); }
        .hero-name em { font-style: italic; color: var(--roseDark); }
        .hero-tagline { font-size: 11px; color: var(--textMid); margin-top: 10px; letter-spacing: 0.05em; }
        .nav-strip { display: flex; overflow-x: auto; gap: 10px; margin: 20px 0; padding-bottom: 5px; }
        .nav-btn { padding: 8px 12px; background: none; border: none; border-bottom: 2px solid transparent; font-size: 11px; color: var(--textFaint); cursor: pointer; white-space: nowrap; }
        .nav-btn.on { color: var(--roseDark); border-bottom-color: var(--rose); font-weight: bold; }
        .card { background: var(--cardBg); border: 1px solid var(--border); border-radius: 6px; padding: 18px; margin-bottom: 15px; }
        .stats3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        .stat-c { background: var(--blush); padding: 12px 5px; text-align: center; border-radius: 4px; }
        .stat-n { font-size: 22px; font-weight: bold; color: var(--roseDark); }
        .stat-l { font-size: 9px; color: var(--textMid); margin-top: 4px; }
        .lbl { display: block; font-size: 10px; color: var(--textMid); margin-bottom: 5px; }
        .inp { width: 100%; padding: 10px; background: var(--blush); border: 1px solid var(--border); border-radius: 4px; margin-bottom: 10px; outline: none; }
        .btn-primary { width: 100%; padding: 12px; background: var(--roseDark); border: none; color: white; border-radius: 4px; cursor: pointer; font-size: 12px; letter-spacing: 0.1em; }
        .btn-sm { padding: 4px 8px; background: var(--blush); border: 1px solid var(--rose); color: var(--roseDark); border-radius: 3px; font-size: 11px; cursor: pointer; }
        .fi { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid var(--blush); font-size: 13px; }
        .ai-wrap { background: white; border: 1px solid var(--border); border-radius: 6px; overflow: hidden; }
        .ai-msgs { padding: 15px; min-height: 160px; max-height: 250px; overflow-y: auto; font-size: 13px; }
        .msg-a { background: var(--blush); padding: 8px 12px; border-radius: 4px; margin-bottom: 8px; inline-size: fit-content; max-width: 90%; }
        .msg-u { background: var(--roseDark); color: white; padding: 8px 12px; border-radius: 4px; margin-bottom: 8px; margin-left: auto; text-align: right; width: fit-content; max-width: 90%; }
        .ai-inp-row { display: flex; border-top: 1px solid var(--border); }
        .ai-inp { flex: 1; padding: 10px; border: none; outline: none; }
        .ai-send { padding: 10px 15px; background: var(--roseDark); color: white; border: none; cursor: pointer; }
        .bmi-big { text-align: center; font-size: 32px; color: var(--roseDark); font-family: var(--ff); padding: 10px; }
        .footer { text-align: center; font-size: 10px; color: var(--textFaint); margin-top: 30px; padding-bottom: 20px; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;

        const QUICK = [
          { name: "白米 (茶碗1杯)", cal: 252 },
          { name: "目玉焼き", cal: 91 },
          { name: "バナナ", cal: 86 },
          { name: "サラダチキン", cal: 105 }
        ];

        function Lumiere() {
          const [tab, setTab] = useState("today");
          const [foods, setFoods] = useState([]);
          const [foodName, setFoodName] = useState("");
          const [foodCal, setFoodCal] = useState("");
          const [steps, setSteps] = useState(0);
          const [stepsInput, setStepsInput] = useState("");
          const [weight, setWeight] = useState("56.0");
          const [weightInput, setWeightInput] = useState("");
          const [mood, setMood] = useState("未記録");
          const [msgs, setMsgs] = useState([{ role: "ai", text: "こんにちは ✨ 今日の体調や食事を教えてくださいね。" }]);
          const [aiInput, setAiInput] = useState("");

          const totalCal = foods.reduce((s, f) => s + f.cal, 0);
          const calorieGoal = 1600;
          const remaining = Math.max(0, calorieGoal - totalCal);
          const stepKcal = Math.round(steps * 0.04 * (parseFloat(weight) / 60));

          const addFood = (name, cal) => {
            if (!name || !cal) return;
            setFoods([...foods, { name, cal: parseInt(cal), id: Date.now() }]);
            setFoodName(""); setFoodCal("");
          };

          const sendAI = () => {
            if (!aiInput.trim()) return;
            setMsgs([...msgs, 
              { role: "user", text: aiInput },
              { role: "ai", text: "素晴らしい記録ですね！その調子で心地よい毎日を過ごしましょう 🌸" }
            ]);
            setAiInput("");
          };

          return (
            <div className="lm">
              <div className="hero">
                <div className="hero-eyebrow">Wellness Journal</div>
                <div className="hero-name">Lu<em>mière</em></div>
                <div className="hero-tagline">内側から輝く、あなたのために</div>
              </div>

              <nav className="nav-strip">
                <button className={`nav-btn ${tab==="today"?"on":""}`} onClick={()=>setTab("today")}>Today</button>
                <button className={`nav-btn ${tab==="food"?"on":""}`} onClick={()=>setTab("food")}>Food</button>
                <button className={`nav-btn ${tab==="steps"?"on":""}`} onClick={()=>setTab("steps")}>Steps</button>
                <button className={`nav-btn ${tab==="weight"?"on":""}`} onClick={()=>setTab("weight")}>Weight</button>
                <button className={`nav-btn ${tab==="ai"?"on":""}`} onClick={()=>setTab("ai")}>AI Coach</button>
              </nav>

              {tab === "today" && (
                <div>
                  <div className="card">
                    <div className="stats3">
                      <div className="stat-c"><div className="stat-n">{totalCal}</div><div className="stat-l">摂取 (kcal)</div></div>
                      <div className="stat-c"><div className="stat-n">{remaining}</div><div className="stat-l">残り (kcal)</div></div>
                      <div className="stat-c"><div className="stat-n">{stepKcal}</div><div className="stat-l">消費 (kcal)</div></div>
                    </div>
                  </div>
                  <div className="card">
                    <p style={{fontSize:"14px", color:"var(--textMid)"}}>今日の気分: <strong>{mood}</strong></p>
                    <p style={{fontSize:"14px", color:"var(--textMid)", marginTop:"8px"}}>現在の体重: <strong>{weight} kg</strong></p>
                    <p style={{fontSize:"14px", color:"var(--textMid)", marginTop:"8px"}}>今日の歩数: <strong>{steps.toLocaleString()} 歩</strong></p>
                  </div>
                </div>
              )}

              {tab === "food" && (
                <div>
                  <div className="card">
                    <label className="lbl">食べ物の名前</label>
                    <input className="inp" value={foodName} onChange={e=>setFoodName(e.target.value)} placeholder="例：サラダ" />
                    <label className="lbl">カロリー (kcal)</label>
                    <input className="inp" type="number" value={foodCal} onChange={e=>setFoodCal(e.target.value)} placeholder="0" />
                    <button className="btn-primary" onClick={()=>addFood(foodName, foodCal)}>追加する</button>
                  </div>
                  <div className="card">
                    <h3>クイック追加</h3>
                    {QUICK.map(q => (
                      <div key={q.name} className="fi">
                        <span>{q.name} ({q.cal}kcal)</span>
                        <button className="btn-sm" onClick={()=>addFood(q.name, q.cal)}>+ Add</button>
                      </div>
                    ))}
                  </div>
                </div>
              )}

              {tab === "steps" && (
                <div className="card">
                  <label className="lbl">本日の歩数を入力</label>
                  <input className="inp" type="number" value={stepsInput} onChange={e=>setStepsInput(e.target.value)} placeholder="例: 6000" />
                  <button className="btn-primary" onClick={()=>{ if(stepsInput){setSteps(parseInt(stepsInput)); setStepsInput("");} }}>歩数を保存</button>
                </div>
              )}

              {tab === "weight" && (
                <div>
                  <div className="card">
                    <label className="lbl">現在の体重 (kg)</label>
                    <input className="inp" type="number" step="0.1" value={weightInput} onChange={e=>setWeightInput(e.target.value)} placeholder="56.0" />
                    <button className="btn-primary" onClick={()=>{ if(weightInput){setWeight(weightInput); setWeightInput("");} }}>体重を保存</button>
                  </div>
                  <div className="card">
                    <div className="bmi-big">{weight} kg</div>
                  </div>
                </div>
              )}

              {tab === "ai" && (
                <div className="ai-wrap">
                  <div className="ai-msgs">
                    {msgs.map((m, i) => (
                      <div key={i} className={m.role === "user" ? "msg-u" : "msg-a"}>{m.text}</div>
                    ))}
                  </div>
                  <div className="ai-inp-row">
                    <input className="ai-inp" value={aiInput} onChange={e=>setAiInput(e.target.value)} placeholder="メッセージを入力..." />
                    <button className="ai-send" onClick={sendAI}>送信</button>
                  </div>
                </div>
              )}

              <div className="footer">Lumière Wellness · アカウント不要</div>
            </div>
          );
        }

        ReactDOM.render(<Lumiere />, document.getElementById('root'));
    </script>
</body>
</html>