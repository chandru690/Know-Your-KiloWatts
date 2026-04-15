```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Know Your Kilowatts – Smart Energy Advisor</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Syne:wght@700;800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box;}
:root{
  --bg:#07080f;
  --surface:#0e1020;
  --card:#151828;
  --border:#1e2340;
  --accent:#f5c842;
  --accent2:#3b82f6;
  --accent3:#10b981;
  --danger:#ef4444;
  --text:#eef0f8;
  --muted:#6b7280;
  --font-head:'Syne',sans-serif;
  --font-body:'Space Grotesk',sans-serif;
}
body{font-family:var(--font-body);background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;}
body::before{content:'';position:fixed;inset:0;background-image:radial-gradient(circle at 20% 80%,rgba(59,130,246,.08) 0%,transparent 50%),radial-gradient(circle at 80% 20%,rgba(245,200,66,.06) 0%,transparent 50%);pointer-events:none;z-index:0;}
::-webkit-scrollbar{width:4px;}
::-webkit-scrollbar-thumb{background:var(--accent);border-radius:4px;}
h1,h2,h3{font-family:var(--font-head);}
.card{background:var(--card);border:1px solid var(--border);border-radius:16px;padding:20px;}
.card-sm{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:14px;}
.btn{display:inline-flex;align-items:center;gap:8px;padding:10px 20px;border-radius:10px;font-family:var(--font-body);font-size:14px;font-weight:500;cursor:pointer;border:none;transition:all .2s;}
.btn-primary{background:var(--accent);color:#000;}
.btn-primary:hover{background:#f0b800;transform:translateY(-1px);}
.btn-ghost{background:transparent;color:var(--text);border:1px solid var(--border);}
.btn-ghost:hover{background:var(--surface);}
.btn-full{width:100%;justify-content:center;padding:14px;}
input[type=text],input[type=number],select{
  width:100%;padding:11px 14px;background:var(--surface);border:1px solid var(--border);
  border-radius:10px;color:var(--text);font-family:var(--font-body);font-size:14px;
  outline:none;transition:border .2s;
}
input:focus,select:focus{border-color:var(--accent);}
select option{background:var(--card);}
label{display:block;font-size:12px;color:var(--muted);margin-bottom:6px;text-transform:uppercase;letter-spacing:.05em;}
.chip{display:inline-flex;align-items:center;gap:6px;padding:8px 14px;border-radius:8px;font-size:13px;font-weight:500;cursor:pointer;border:1px solid var(--border);background:var(--surface);color:var(--muted);transition:all .2s;user-select:none;}
.chip.on{background:rgba(245,200,66,.12);border-color:var(--accent);color:var(--accent);}
.chip:hover{border-color:var(--accent);color:var(--text);}
.spec-row{display:flex;align-items:center;gap:10px;padding:10px 14px;background:var(--surface);border:1px solid var(--border);border-radius:10px;margin-bottom:8px;}
.spec-row select{flex:1;background:var(--card);padding:6px 10px;}
.spec-label{font-size:13px;color:var(--text);white-space:nowrap;min-width:90px;}
.bar-track{height:6px;background:var(--border);border-radius:99px;overflow:hidden;}
.bar-fill{height:100%;border-radius:99px;transition:width .6s ease;}
@keyframes fadeUp{from{opacity:0;transform:translateY(24px);}to{opacity:1;transform:translateY(0);}}
@keyframes spin{to{transform:rotate(360deg);}}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:.4;}}
@keyframes ripple{0%{transform:scale(1);opacity:.7;}100%{transform:scale(2.2);opacity:0;}}
.fade-up{animation:fadeUp .4s ease both;}
.spin{animation:spin 1s linear infinite;}
.pulse{animation:pulse 1.4s ease infinite;}
.badge{display:inline-block;padding:3px 10px;border-radius:99px;font-size:11px;font-weight:600;}
.badge-gold{background:rgba(245,200,66,.15);color:var(--accent);}
.badge-blue{background:rgba(59,130,246,.15);color:#60a5fa;}
.badge-green{background:rgba(16,185,129,.15);color:#34d399;}
.badge-red{background:rgba(239,68,68,.15);color:#f87171;}
.metric{text-align:center;padding:16px 10px;}
.metric-val{font-family:var(--font-head);font-size:26px;font-weight:700;line-height:1;}
.metric-label{font-size:11px;color:var(--muted);margin-top:4px;text-transform:uppercase;letter-spacing:.06em;}
.toast{position:fixed;bottom:24px;right:24px;z-index:9999;animation:fadeUp .3s ease;}
@media(max-width:600px){.toast{right:12px;left:12px;bottom:12px;}}
.mic-btn{width:96px;height:96px;border-radius:50%;display:flex;align-items:center;justify-content:center;cursor:pointer;position:relative;border:none;background:var(--accent);transition:all .2s;}
.mic-btn:hover{transform:scale(1.04);}
.mic-ripple{position:absolute;inset:0;border-radius:50%;border:2px solid var(--accent);animation:ripple 1.5s ease infinite;}
.mic-btn.listening{background:#ef4444;}
.mic-btn.listening .mic-ripple{border-color:#ef4444;}
.steps{display:flex;gap:6px;margin-bottom:24px;}
.step-dot{width:24px;height:4px;border-radius:99px;background:var(--border);transition:all .3s;}
.step-dot.active{background:var(--accent);width:40px;}
.step-dot.done{background:var(--accent3);}
.nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:32px;padding:0 0 16px;border-bottom:1px solid var(--border);}
.sug-card{display:flex;gap:12px;padding:14px;background:var(--surface);border:1px solid var(--border);border-radius:12px;margin-bottom:10px;transition:all .2s;}
.sug-card:hover{border-color:var(--accent);transform:translateY(-2px);}
.sug-icon{width:40px;height:40px;border-radius:10px;background:rgba(245,200,66,.1);display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0;}
.sug-body{flex:1;}
.sug-title{font-weight:600;font-size:14px;margin-bottom:2px;}
.sug-desc{font-size:12px;color:var(--muted);line-height:1.5;}
.sug-footer{display:flex;align-items:center;justify-content:space-between;margin-top:6px;}
.transcript-box{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:14px;min-height:60px;font-size:14px;color:var(--text);line-height:1.6;margin-top:12px;}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
.grid3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;}
.grid4{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;}
@media(max-width:600px){.grid2,.grid3,.grid4{grid-template-columns:1fr 1fr;}}
@media(max-width:400px){.grid3,.grid4{grid-template-columns:1fr 1fr;}}
.container{max-width:680px;margin:0 auto;padding:24px 16px;position:relative;z-index:1;}
.container-wide{max-width:900px;margin:0 auto;padding:24px 16px;position:relative;z-index:1;}
.chart-wrap{position:relative;height:220px;}
.num-row{display:flex;gap:8px;align-items:center;}
.num-row input{max-width:120px;}
.divider{border:none;border-top:1px solid var(--border);margin:20px 0;}
.grade{font-family:var(--font-head);font-size:36px;font-weight:800;}
.grade.A{color:var(--accent3);}
.grade.B{color:#60a5fa;}
.grade.C{color:var(--accent);}
.grade.D{color:#fb923c;}
.grade.E{color:var(--danger);}
</style>
</head>
<body>
<div id="root"></div>
<script>
// Original TNEB tariff (simple slab rates) - unchanged
const TNEB = {
  bill(units){
    let energy = 0, fixed = 0;
    if(units <= 100){ energy = units * 1.5; fixed = 25; }
    else if(units <= 200){ energy = 100*1.5 + (units-100)*3.0; fixed = 50; }
    else if(units <= 500){ energy = 100*1.5 + 100*3.0 + (units-200)*4.5; fixed = 70; }
    else { energy = 100*1.5 + 100*3.0 + 300*4.5 + (units-500)*6.5; fixed = 90; }
    return Math.round(energy + fixed);
  },
  units(bill){
    if(bill <= 175){ return Math.round(Math.max(0,(bill-25)/1.5)); }
    if(bill <= 500){ return Math.round(100 + (bill-50-150)/3.0); }
    if(bill <= 1820){ return Math.round(200 + (bill-70-300-150)/4.5); }
    return Math.round(500 + (bill-90-300-150-1350)/6.5);
  },
  rate(units){
    if(units<=100) return 1.5;
    if(units<=200) return 3.0;
    if(units<=500) return 4.5;
    return 6.5;
  }
};
const APPLIANCES = {
  ac:{
    label:'Air Conditioner', icon:'❄️',
    specs:[
      {key:'ton', label:'Capacity', options:['0.75 Ton','1 Ton','1.5 Ton','2 Ton','2.5 Ton'], watts:[700,900,1400,1800,2200]},
      {key:'star', label:'Star Rating', options:['1 Star','2 Star','3 Star','4 Star','5 Star'], factors:[1.0,0.92,0.84,0.76,0.68]},
    ],
    defaultHours:8, defaultDays:25,
    calcUnits(spec,hrs,days){ return (APPLIANCES.ac.specs[0].watts[spec.ton||1]*APPLIANCES.ac.specs[1].factors[spec.star||2]*hrs*days)/1000; }
  },
  fridge:{
    label:'Refrigerator', icon:'🧊',
    specs:[
      {key:'liter', label:'Capacity', options:['150L','200L','250L','300L','350L','400L+'], watts:[30,40,50,60,75,90]},
      {key:'star', label:'Star Rating', options:['1 Star','2 Star','3 Star','4 Star','5 Star'], factors:[1.0,0.88,0.76,0.65,0.55]},
    ],
    defaultHours:24, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.fridge.specs[0].watts[spec.liter||1]*APPLIANCES.fridge.specs[1].factors[spec.star||2]*hrs*days)/1000; }
  },
  waterHeater:{
    label:'Water Heater (Geyser)', icon:'🚿',
    specs:[
      {key:'liter', label:'Capacity', options:['6L','10L','15L','25L'], watts:[2000,2000,2000,3000]},
      {key:'type', label:'Type', options:['Instant','Storage'], factors:[1.0,0.7]},
    ],
    defaultHours:0.5, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.waterHeater.specs[0].watts[spec.liter||0]*APPLIANCES.waterHeater.specs[1].factors[spec.type||1]*hrs*days)/1000; }
  },
  washingMachine:{
    label:'Washing Machine', icon:'👕',
    specs:[
      {key:'type', label:'Type', options:['Semi-Auto','Top Load','Front Load'], watts:[400,500,800]},
      {key:'loads', label:'Loads/week', options:['1','2','3','4','5'], vals:[1,2,3,4,5]},
    ],
    defaultHours:1, defaultDays:30,
    calcUnits(spec,hrs,days){ 
      let loadsPerMonth = (APPLIANCES.washingMachine.specs[1].vals[spec.loads||1]||2)*4.3;
      return (APPLIANCES.washingMachine.specs[0].watts[spec.type||0]*1.0*loadsPerMonth)/1000;
    }
  },
  evCharger:{
    label:'EV Charger', icon:'⚡',
    specs:[
      {key:'kw', label:'Charger (kW)', options:['1.5 kW (2W)','3.3 kW (Car)','7.2 kW (Fast)'], watts:[1500,3300,7200]},
      {key:'hrs', label:'Charge hrs/day', options:['1 hr','2 hrs','3 hrs','4 hrs','5 hrs'], vals:[1,2,3,4,5]},
    ],
    defaultHours:2, defaultDays:25,
    calcUnits(spec,hrs,days){ return (APPLIANCES.evCharger.specs[0].watts[spec.kw||0]/1000)*(APPLIANCES.evCharger.specs[1].vals[spec.hrs||1]||2)*days; }
  },
  tv:{
    label:'Television', icon:'📺',
    specs:[
      {key:'size', label:'Screen Size', options:['24"','32"','40"','43"','55"','65"'], watts:[25,40,60,70,100,130]},
      {key:'type', label:'Panel Type', options:['LED','OLED/QLED'], factors:[1.0,1.3]},
    ],
    defaultHours:5, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.tv.specs[0].watts[spec.size||2]*APPLIANCES.tv.specs[1].factors[spec.type||0]*hrs*days)/1000; }
  },
  microwave:{
    label:'Microwave / OTG', icon:'📡',
    specs:[
      {key:'type', label:'Type', options:['Solo 20L','Grill 28L','Convection 30L'], watts:[800,900,1200]},
    ],
    defaultHours:0.25, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.microwave.specs[0].watts[spec.type||0]*hrs*days)/1000; }
  },
  induction:{
    label:'Induction Cooktop', icon:'🍳',
    specs:[
      {key:'watt', label:'Power', options:['1200W','1600W','2000W'], watts:[1200,1600,2000]},
    ],
    defaultHours:1.5, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.induction.specs[0].watts[spec.watt||1]*hrs*days)/1000; }
  },
  fans:{
    label:'Ceiling Fans', icon:'🌀',
    specs:[
      {key:'count', label:'No. of Fans', options:['1','2','3','4','5','6'], vals:[1,2,3,4,5,6]},
      {key:'type', label:'Type', options:['Regular (75W)','BLDC (28W)'], watts:[75,28]},
    ],
    defaultHours:12, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.fans.specs[1].watts[spec.type||0]*APPLIANCES.fans.specs[0].vals[spec.count||1]*hrs*days)/1000; }
  },
  lights:{
    label:'Lights / LED Bulbs', icon:'💡',
    specs:[
      {key:'count', label:'No. of Bulbs', options:['2','4','6','8','10','12'], vals:[2,4,6,8,10,12]},
      {key:'watt', label:'Watts each', options:['5W','7W','9W','12W','15W'], watts:[5,7,9,12,15]},
    ],
    defaultHours:6, defaultDays:30,
    calcUnits(spec,hrs,days){ return (APPLIANCES.lights.specs[1].watts[spec.watt||1]*APPLIANCES.lights.specs[0].vals[spec.count||2]*hrs*days)/1000; }
  },
};
const TIPS = {
  ac:{icon:'❄️', title:'AC Optimization',
    gen:(u,spec)=>`Your ${['0.75T','1T','1.5T','2T','2.5T'][spec?.ton??1]} AC uses ~${Math.round(u)} units/month. Each 1°C above 24°C saves 6%. Use 26°C + fan.`,
    act:'Set AC to 26°C, enable sleep mode, clean filter monthly.',
    savePct:18},
  fridge:{icon:'🧊', title:'Fridge Efficiency',
    gen:(u,spec)=>`Your ${['150L','200L','250L','300L','350L','400L+'][spec?.liter??1]} fridge uses ~${Math.round(u)} units/month. ${['1★','2★','3★','4★','5★'][spec?.star??1]} rated.`,
    act:'Set fridge to 4°C, freezer -15°C. Keep coils dust-free. Don\'t overfill.',
    savePct:12},
  waterHeater:{icon:'🚿', title:'Geyser Savings',
    gen:(u,spec)=>`Geyser uses ~${Math.round(u)} units/month. Schedule to off-peak hours.`,
    act:'Heat water 5:30–6:30 AM only. Turn off when not in use. Install timer.',
    savePct:30},
  washingMachine:{icon:'👕', title:'Wash Smarter',
    gen:(u,spec)=>`Washing machine uses ~${Math.round(u)} units/month.`,
    act:'Use cold water, full loads, eco mode. Saves 40% energy.',
    savePct:22},
  evCharger:{icon:'⚡', title:'EV Night Charging',
    gen:(u,spec)=>`EV charger uses ~${Math.round(u)} units/month. Shift to 10PM–6AM for ₹${Math.round(u*0.5)} savings.`,
    act:'Set EV timer to charge after 10 PM. Use scheduled charging.',
    savePct:15},
  fans:{icon:'🌀', title:'BLDC Fan Upgrade',
    gen:(u,spec)=>`Fans use ~${Math.round(u)} units/month. BLDC fans use 28W vs 75W.`,
    act:'Upgrade to BLDC fans — saves up to 65% fan energy.',
    savePct:35},
  lights:{icon:'💡', title:'LED Upgrade',
    gen:(u,spec)=>`Lights use ~${Math.round(u)} units/month.`,
    act:'Switch to BEE 5-star LED bulbs. Use motion sensors.',
    savePct:40},
};
let S = {
  view: 'home',
  step: 1,
  formData: { billAmount: '', people: '', houseType: '', location: 'urban' },
  selectedApps: [],
  appSpecs: {},
  appHours: {},
  analysis: null,
  voiceState: 'idle',
  voiceText: '',
};
function calculateUnits(selectedApps, appSpecs, appHours){
  let breakdown = {};
  let total = 0;
  for(let appId of selectedApps){
    let def = APPLIANCES[appId];
    if(!def) continue;
    let spec = appSpecs[appId] || {};
    let hrs = appHours[appId] ?? def.defaultHours;
    let days = def.defaultDays || 30;
    let units = def.calcUnits(spec, hrs, days);
    breakdown[appId] = Math.round(units * 10) / 10;
    total += units;
  }
  return { breakdown, total: Math.round(total * 10) / 10 };
}
function runAnalysis(){
  let {formData, selectedApps, appSpecs, appHours} = S;
  let inputBill = parseFloat(formData.billAmount) || 0;
  let people = parseInt(formData.people) || 3;
  let {breakdown, total: calcUnits} = calculateUnits(selectedApps, appSpecs, appHours);
  let billUnits = inputBill ? TNEB.units(inputBill) : 0;
  let estUnits, estBill;
  if(inputBill && calcUnits > 10){
    estUnits = Math.round(billUnits * 0.6 + calcUnits * 0.4);
    estBill = inputBill;
  } else if(inputBill){
    estUnits = billUnits;
    estBill = inputBill;
  } else {
    estUnits = calcUnits;
    estBill = TNEB.bill(calcUnits);
  }
  let scale = calcUnits > 0 ? estUnits / calcUnits : 1;
  let scaledBreakdown = {};
  for(let k in breakdown) scaledBreakdown[k] = Math.round(breakdown[k] * scale * 10)/10;
  let perCap = Math.round(estUnits / people);
  let grade;
  if(perCap < 60) grade = 'A+';
  else if(perCap < 100) grade = 'A';
  else if(perCap < 150) grade = 'B';
  else if(perCap < 200) grade = 'C';
  else if(perCap < 280) grade = 'D';
  else grade = 'E';
  let peerAvg = 110;
  let peerPct = Math.round((perCap / peerAvg) * 100);
  let peerLabel = peerPct < 80 ? 'Efficient' : peerPct < 115 ? 'Average' : 'Above Average';
  let savings = [];
  for(let appId of selectedApps){
    let tip = TIPS[appId];
    let u = scaledBreakdown[appId] || 0;
    if(tip && u > 2){
      let saveUnits = Math.round(u * tip.savePct / 100);
      let saveRs = TNEB.bill(estUnits) - TNEB.bill(estUnits - saveUnits);
      savings.push({
        appId, icon:tip.icon, title:tip.title,
        desc: tip.gen(u, appSpecs[appId]),
        act: tip.act,
        saveUnits, saveRs: Math.max(saveRs, 0),
        savePct: tip.savePct,
      });
    }
  }
  savings.sort((a,b)=>b.saveRs - a.saveRs);
  let totalSaveUnits = savings.reduce((s,x)=>s+x.saveUnits,0);
  let optimisedUnits = Math.max(estUnits - Math.round(totalSaveUnits*0.7), estUnits * 0.55);
  let optimisedBill = TNEB.bill(optimisedUnits);
  let nextMonthUnits = Math.round(estUnits * 1.04);
  let nextMonthBill = TNEB.bill(nextMonthUnits);
  let co2kg = Math.round(estUnits * 0.71);
  let treeDays = Math.round(co2kg / 0.022);
  return {
    estUnits, estBill, people, perCap, grade,
    breakdown: scaledBreakdown, totalSaveUnits,
    optimisedUnits, optimisedBill,
    nextMonthUnits, nextMonthBill,
    savings: savings.slice(0,6),
    peerPct, peerLabel, co2kg, treeDays,
    houseType: formData.houseType,
    marginalRate: TNEB.rate(estUnits),
  };
}
function parseVoice(txt){
  let t = txt.toLowerCase();
  let result = {};
  let billM = t.match(/(?:bill|amount|pay|paid|rs|rupees|₹)\s*(?:is|of|around|about)?\s*(\d{3,5})/);
  if(billM) result.billAmount = billM[1];
  let pplM = t.match(/(\d+)\s*(?:people|persons?|members?|family members?)/);
  if(pplM) result.people = pplM[1];
  if(t.match(/\bindependent\b|villa|bungalow/)) result.houseType = 'independent';
  else if(t.match(/3\s*bhk/)) result.houseType = '3bhk';
  else if(t.match(/2\s*bhk/)) result.houseType = '2bhk';
  else if(t.match(/1\s*bhk/)) result.houseType = '1bhk';
  let apps = [];
  let specs = {};
  if(t.match(/\bac\b|air.?con/)){
    apps.push('ac');
    specs.ac = {};
    let tonM = t.match(/(\d+(?:\.\d+)?)\s*ton/);
    if(tonM){
      let ton = parseFloat(tonM[1]);
      specs.ac.ton = ton<=0.75?0:ton<=1?1:ton<=1.5?2:ton<=2?3:4;
    }
    let starM = t.match(/(\d)\s*star/);
    if(starM) specs.ac.star = Math.min(4, Math.max(0, parseInt(starM[1])-1));
  }
  if(t.match(/fridge|refrigerator/)){
    apps.push('fridge');
    specs.fridge = {};
    let starM = t.match(/(\d)\s*star/);
    if(starM) specs.fridge.star = Math.min(4, Math.max(0, parseInt(starM[1])-1));
    let litM = t.match(/(\d{2,3})\s*(?:liter|litre|l\b)/);
    if(litM){
      let l = parseInt(litM[1]);
      specs.fridge.liter = l<=150?0:l<=200?1:l<=250?2:l<=300?3:l<=350?4:5;
    }
  }
  if(t.match(/geyser|water.?heater/)){
    apps.push('waterHeater');
    specs.waterHeater = {};
    let litM = t.match(/(\d+)\s*(?:liter|litre|l)/);
    if(litM){
      let l = parseInt(litM[1]);
      specs.waterHeater.liter = l<=6?0:l<=10?1:l<=15?2:3;
    }
  }
  if(t.match(/washing.?machine|washer/)) apps.push('washingMachine');
  if(t.match(/\btv\b|television/)){
    apps.push('tv');
    specs.tv = {};
    let inchM = t.match(/(\d{2})\s*(?:inch|")/);
    if(inchM){
      let i = parseInt(inchM[1]);
      specs.tv.size = i<=24?0:i<=32?1:i<=40?2:i<=43?3:i<=55?4:5;
    }
  }
  if(t.match(/\bev\b|electric.?vehicle|electric.?car/)) apps.push('evCharger');
  if(t.match(/fan/)){
    apps.push('fans');
    specs.fans = {};
    let cntM = t.match(/(\d+)\s*fan/);
    if(cntM) specs.fans.count = Math.min(5, Math.max(0, parseInt(cntM[1])-1));
  }
  if(t.match(/induction/)) apps.push('induction');
  return {formData: result, apps, specs};
}
function $(id){ return document.getElementById(id); }
function mount(html){ $('root').innerHTML = html; }
function showToast(msg, sub, type='success'){
  let colors = {success:'var(--accent3)', warning:'var(--accent)', error:'var(--danger)'};
  let t = document.createElement('div');
  t.className = 'toast';
  t.innerHTML = `<div style="background:var(--card);border:1px solid var(--border);border-radius:12px;padding:14px 16px;max-width:320px;display:flex;gap:10px;align-items:flex-start;">
    <div style="width:8px;height:8px;border-radius:50%;background:${colors[type]||colors.success};margin-top:5px;flex-shrink:0;"></div>
    <div><div style="font-weight:600;font-size:13px;">${msg}</div><div style="font-size:12px;color:var(--muted);margin-top:2px;">${sub}</div></div>
    <button onclick="this.closest('.toast').remove()" style="background:none;border:none;color:var(--muted);cursor:pointer;font-size:16px;line-height:1;margin-left:auto;">×</button>
  </div>`;
  document.body.appendChild(t);
  setTimeout(()=>t.remove(), 5000);
}
function renderHome(){
  mount(`
  <div class="container" style="min-height:100vh;display:flex;flex-direction:column;justify-content:center;">
    <div class="fade-up" style="text-align:center;margin-bottom:40px;">
      <div style="display:inline-flex;align-items:center;gap:8px;background:rgba(245,200,66,.1);border:1px solid rgba(245,200,66,.2);border-radius:99px;padding:6px 14px;margin-bottom:20px;">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="${getComputedStyle(document.documentElement).getPropertyValue('--accent').trim()}" xmlns="http://www.w3.org/2000/svg"><path d="M13 3L4 14h7l-2 7 9-11h-7l2-7z"/></svg>
        <span style="font-size:12px;color:var(--accent);font-weight:600;">TNEB Precision Engine</span>
      </div>
      <h1 style="font-size:clamp(36px,8vw,56px);font-weight:800;line-height:1;margin-bottom:12px;">Know Your <span style="color:var(--accent);">Kilowatts</span></h1>
      <p style="color:var(--muted);font-size:15px;max-width:360px;margin:0 auto;line-height:1.6;">Know exactly what every appliance costs. Save real money with precise TNEB tariff calculations.</p>
    </div>
    <div style="display:flex;flex-direction:column;gap:12px;" class="fade-up">
      <div onclick="goForm()" style="cursor:pointer;padding:20px;background:var(--card);border:1px solid var(--border);border-radius:16px;display:flex;align-items:center;gap:16px;transition:all .2s;" onmouseover="this.style.borderColor='var(--accent)'" onmouseout="this.style.borderColor='var(--border)'">
        <div style="width:48px;height:48px;border-radius:12px;background:rgba(245,200,66,.12);display:flex;align-items:center;justify-content:center;font-size:22px;flex-shrink:0;">📋</div>
        <div style="flex:1;">
          <div style="font-weight:700;font-size:15px;">Guided Setup</div>
          <div style="font-size:12px;color:var(--muted);margin-top:2px;">Enter appliance details for the most accurate analysis</div>
        </div>
        <div style="color:var(--accent);font-size:20px;">→</div>
      </div>
      <div onclick="goVoice()" style="cursor:pointer;padding:20px;background:var(--card);border:1px solid var(--border);border-radius:16px;display:flex;align-items:center;gap:16px;transition:all .2s;" onmouseover="this.style.borderColor='var(--accent3)'" onmouseout="this.style.borderColor='var(--border)'">
        <div style="width:48px;height:48px;border-radius:12px;background:rgba(16,185,129,.12);display:flex;align-items:center;justify-content:center;font-size:22px;flex-shrink:0;">🎤</div>
        <div style="flex:1;">
          <div style="font-weight:700;font-size:15px;">Voice AI</div>
          <div style="font-size:12px;color:var(--muted);margin-top:2px;">Speak naturally — "2BHK, 1.5 ton 3-star AC, 250L 4-star fridge"</div>
        </div>
        <div style="color:var(--accent3);font-size:20px;">→</div>
      </div>
    </div>
    <p style="text-align:center;font-size:11px;color:var(--muted);margin-top:32px;">Tamil Nadu TNEB Domestic Tariff 2024 · Calculations updated</p>
  </div>`);
}
function goForm(){ S.view='form'; S.step=1; renderForm(); }
function renderForm(){
  if(S.step===1) renderStep1();
  else if(S.step===2) renderStep2();
  else if(S.step===3) renderStep3();
}
function stepDots(current, total){
  return Array.from({length:total}, (_,i)=>`<div class="step-dot ${i<current-1?'done':i===current-1?'active':''}"></div>`).join('');
}
function renderStep1(){
  let d = S.formData;
  mount(`
  <div class="container" style="padding-top:40px;">
    <div class="nav">
      <button class="btn btn-ghost" style="padding:8px 12px;" onclick="S.view='home';renderHome()">← Back</button>
      <div class="steps">${stepDots(1,3)}</div>
      <span style="font-size:12px;color:var(--muted);">1 of 3</span>
    </div>
    <h2 style="font-size:24px;margin-bottom:6px;">Your Home</h2>
    <p style="color:var(--muted);font-size:13px;margin-bottom:28px;">Basic details to calibrate the tariff model</p>
    <div style="display:flex;flex-direction:column;gap:16px;" class="fade-up">
      <div>
        <label>Monthly EB Bill (₹) — optional if you select appliances</label>
        <input type="number" placeholder="e.g. 2400" value="${d.billAmount||''}" oninput="S.formData.billAmount=this.value">
        <div style="font-size:11px;color:var(--muted);margin-top:5px;">Leave blank to calculate from appliances below</div>
      </div>
      <div>
        <label>People in Household</label>
        <input type="number" placeholder="e.g. 4" min="1" max="20" value="${d.people||''}" oninput="S.formData.people=this.value">
      </div>
      <div>
        <label>House Type</label>
        <div class="grid2" style="gap:8px;margin-top:2px;">
          ${['1bhk','2bhk','3bhk','independent'].map((t,i)=>{
            let labels = ['1 BHK 🏢','2 BHK 🏘️','3 BHK 🏠','Independent 🏡'];
            let sel = S.formData.houseType===t;
            return `<button class="chip ${sel?'on':''}" onclick="S.formData.houseType='${t}';renderStep1()" style="justify-content:center;padding:12px;">${labels[i]}</button>`;
          }).join('')}
        </div>
      </div>
      <div>
        <label>Area Type</label>
        <div class="grid2" style="gap:8px;margin-top:2px;">
          ${['urban','rural'].map(t=>{
            let labels={urban:'Urban 🏙️',rural:'Rural 🌾'};
            let sel = S.formData.location===t;
            return `<button class="chip ${sel?'on':''}" onclick="S.formData.location='${t}';renderStep1()" style="justify-content:center;padding:12px;">${labels[t]}</button>`;
          }).join('')}
        </div>
      </div>
      <button class="btn btn-primary btn-full" onclick="validateStep1()">Continue → Appliances</button>
    </div>
  </div>`);
}
function validateStep1(){
  let d = S.formData;
  if(!d.people){ showToast('Missing info', 'Please enter number of people', 'warning'); return; }
  if(!d.houseType){ showToast('Missing info', 'Please select house type', 'warning'); return; }
  S.step=2; renderStep2();
}
function renderStep2(){
  let appList = Object.keys(APPLIANCES);
  mount(`
  <div class="container" style="padding-top:40px;">
    <div class="nav">
      <button class="btn btn-ghost" style="padding:8px 12px;" onclick="S.step=1;renderStep1()">← Back</button>
      <div class="steps">${stepDots(2,3)}</div>
      <span style="font-size:12px;color:var(--muted);">2 of 3</span>
    </div>
    <h2 style="font-size:24px;margin-bottom:6px;">Appliances</h2>
    <p style="color:var(--muted);font-size:13px;margin-bottom:24px;">Select all that you regularly use</p>
    <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:28px;" class="fade-up" id="appGrid">
      ${appList.map(id=>{
        let app = APPLIANCES[id];
        let on = S.selectedApps.includes(id);
        return `<button class="chip ${on?'on':''}" id="chip-${id}" onclick="toggleApp('${id}')" style="font-size:13px;padding:10px 14px;">${app.icon} ${app.label}</button>`;
      }).join('')}
    </div>
    <div style="background:rgba(245,200,66,.06);border:1px solid rgba(245,200,66,.15);border-radius:12px;padding:12px 14px;margin-bottom:20px;font-size:12px;color:var(--muted);">
      💡 Select all appliances — we'll ask for their specs next to calculate accurate usage.
    </div>
    <button class="btn btn-primary btn-full" onclick="validateStep2()">Continue → Appliance Details</button>
  </div>`);
}
function toggleApp(id){
  if(S.selectedApps.includes(id)){
    S.selectedApps = S.selectedApps.filter(a=>a!==id);
    if(S.appSpecs[id]) delete S.appSpecs[id];
  } else {
    S.selectedApps.push(id);
    S.appSpecs[id] = {};
  }
  let chip = $(`chip-${id}`);
  if(chip) chip.className = `chip ${S.selectedApps.includes(id)?'on':''}`;
}
function validateStep2(){
  if(S.selectedApps.length===0 && !S.formData.billAmount){
    showToast('No appliances', 'Select at least one appliance or enter bill amount', 'warning');
    return;
  }
  S.step=3; renderStep3();
}
function renderStep3(){
  if(S.selectedApps.length===0){ runAndShow(); return; }
  let rows = S.selectedApps.map(id=>{
    let app = APPLIANCES[id];
    let spec = S.appSpecs[id] || {};
    let specHtml = app.specs.map((s,si)=>`
      <div style="flex:1;min-width:120px;">
        <div style="font-size:11px;color:var(--muted);margin-bottom:4px;">${s.label}</div>
        <select onchange="setSpec('${id}','${s.key}',parseInt(this.value))">
          ${s.options.map((opt,oi)=>`<option value="${oi}" ${spec[s.key]===oi?'selected':''}>${opt}</option>`).join('')}
        </select>
      </div>`).join('');
    let hrs = S.appHours[id] ?? app.defaultHours;
    return `
    <div class="card-sm" style="margin-bottom:10px;">
      <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
        <span style="font-size:18px;">${app.icon}</span>
        <span style="font-weight:600;font-size:14px;">${app.label}</span>
      </div>
      <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:10px;">${specHtml}</div>
      <div style="display:flex;align-items:center;gap:10px;">
        <div style="font-size:11px;color:var(--muted);white-space:nowrap;">Daily use (hrs):</div>
        <input type="number" step="0.25" min="0.25" max="24" value="${hrs}" style="max-width:80px;padding:6px 10px;font-size:13px;" onchange="S.appHours['${id}']=parseFloat(this.value)">
        <div style="font-size:11px;color:var(--muted);">hrs/day</div>
      </div>
    </div>`;
  }).join('');
  mount(`
  <div class="container" style="padding-top:40px;padding-bottom:40px;">
    <div class="nav">
      <button class="btn btn-ghost" style="padding:8px 12px;" onclick="S.step=2;renderStep2()">← Back</button>
      <div class="steps">${stepDots(3,3)}</div>
      <span style="font-size:12px;color:var(--muted);">3 of 3</span>
    </div>
    <h2 style="font-size:24px;margin-bottom:6px;">Appliance Details</h2>
    <p style="color:var(--muted);font-size:13px;margin-bottom:20px;">Accurate specs = accurate bill prediction</p>
    <div class="fade-up">${rows}</div>
    <button class="btn btn-primary btn-full" style="margin-top:16px;" onclick="runAndShow()">🔍 Analyse My Bill</button>
  </div>`);
}
function setSpec(id, key, val){
  if(!S.appSpecs[id]) S.appSpecs[id] = {};
  S.appSpecs[id][key] = val;
}
function runAndShow(){
  mount(`<div style="min-height:100vh;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:20px;">
    <div style="width:56px;height:56px;border:3px solid var(--border);border-top-color:var(--accent);border-radius:50%;" class="spin"></div>
    <div style="text-align:center;">
      <div style="font-family:var(--font-head);font-size:18px;margin-bottom:6px;">Crunching numbers…</div>
      <div style="font-size:13px;color:var(--muted);">Applying TNEB 2024 tariff slabs</div>
    </div>
  </div>`);
  setTimeout(()=>{
    S.analysis = runAnalysis();
    S.view = 'dash';
    renderDash();
    setTimeout(()=>showToast('Analysis ready','Your personalised energy report is here','success'),400);
  }, 1400);
}
function renderDash(){
  let A = S.analysis;
  if(!A){ S.view='home'; renderHome(); return; }
  let breakdownEntries = Object.entries(A.breakdown).filter(([k,v])=>v>0.5);
  let labels = breakdownEntries.map(([k])=>APPLIANCES[k]?.label||k);
  let data   = breakdownEntries.map(([k,v])=>v);
  let palette = ['#f5c842','#3b82f6','#10b981','#f87171','#a78bfa','#fb923c','#34d399','#60a5fa','#fbbf24','#f472b6'];
  let gradeColor = {A:getComputedStyle(document.documentElement).getPropertyValue('--accent3').trim(),'A+':'#34d399',B:'#60a5fa',C:'#f5c842',D:'#fb923c',E:'#ef4444'};
  let gc = gradeColor[A.grade] || '#f5c842';
  let sugHtml = A.savings.map(s=>`
    <div class="sug-card">
      <div class="sug-icon">${s.icon}</div>
      <div class="sug-body">
        <div class="sug-title">${s.title}</div>
        <div class="sug-desc">${s.desc}</div>
        <div class="sug-footer">
          <span style="color:var(--accent3);font-size:12px;font-weight:600;">Save ₹${s.saveRs}/month · ${s.savePct}% reduction</span>
          <button onclick="showToast('Tip applied','${s.act.replace(/'/g,"\\'")}','success')" style="font-size:11px;color:var(--accent);background:none;border:none;cursor:pointer;font-family:var(--font-body);">Apply →</button>
        </div>
        <div style="font-size:11px;color:var(--muted);margin-top:4px;font-style:italic;">${s.act}</div>
      </div>
    </div>`).join('');
  let tableRows = breakdownEntries.map(([k,v],i)=>`
    <tr>
      <td style="padding:8px 0;font-size:13px;">${APPLIANCES[k]?.icon||''} ${APPLIANCES[k]?.label||k}</td>
      <td style="padding:8px 0;font-size:13px;text-align:right;font-weight:600;">${v} units</td>
      <td style="padding:8px 0;font-size:13px;text-align:right;color:var(--accent);">₹${TNEB.bill(v)}</td>
      <td style="padding:8px 4px;">
        <div class="bar-track" style="width:80px;">
          <div class="bar-fill" style="width:${Math.min(100,v/A.estUnits*100)}%;background:${palette[i%palette.length]};"></div>
        </div>
      </td>
    </tr>`).join('');
  mount(`
  <div class="container-wide" style="padding-top:32px;padding-bottom:60px;">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:28px;">
      <div style="display:flex;align-items:center;gap:8px;">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="#f5c842"><path d="M13 3L4 14h7l-2 7 9-11h-7l2-7z"/></svg>
        <span style="font-family:var(--font-head);font-size:20px;font-weight:800;">Know Your Kilowatts</span>
        <span class="badge badge-gold" style="margin-left:4px;">${{home:'',form:'',voice:'Voice'}[S.view]||'Guided'}</span>
      </div>
      <button class="btn btn-ghost" style="font-size:12px;padding:8px 14px;" onclick="S.view='home';S.analysis=null;S.selectedApps=[];S.appSpecs={};S.appHours={};S.formData={billAmount:'',people:'',houseType:'',location:'urban'};renderHome()">New Analysis</button>
    </div>
    <div style="background:linear-gradient(135deg,rgba(245,200,66,.12),rgba(59,130,246,.08));border:1px solid rgba(245,200,66,.2);border-radius:20px;padding:24px;margin-bottom:20px;" class="fade-up">
      <div style="font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;margin-bottom:8px;">Estimated Monthly Consumption</div>
      <div style="font-family:var(--font-head);font-size:42px;font-weight:800;line-height:1;">${A.estUnits} <span style="font-size:20px;color:var(--muted);">units</span></div>
      <div style="font-size:28px;font-weight:700;color:var(--accent);margin-top:4px;">₹${A.estBill.toLocaleString('en-IN')}</div>
      <div style="font-size:12px;color:var(--muted);margin-top:6px;">TNEB tariff · ${A.marginalRate}/unit marginal rate · ${A.perCap} units/person/month</div>
    </div>
    <div class="grid4" style="margin-bottom:20px;" class="fade-up">
      <div class="card-sm metric">
        <div class="grade ${A.grade}" style="font-size:32px;">${A.grade}</div>
        <div class="metric-label">Efficiency Grade</div>
      </div>
      <div class="card-sm metric">
        <div class="metric-val" style="color:${A.peerPct<100?'var(--accent3)':'var(--danger)'};">${A.peerPct}%</div>
        <div class="metric-label">vs. avg Indian home</div>
      </div>
      <div class="card-sm metric">
        <div class="metric-val">${A.co2kg}</div>
        <div class="metric-label">kg CO₂/month</div>
      </div>
      <div class="card-sm metric">
        <div class="metric-val" style="color:var(--accent3);">₹${(A.estBill - A.optimisedBill).toLocaleString('en-IN')}</div>
        <div class="metric-label">Max savings/month</div>
      </div>
    </div>
    <div class="grid2" style="margin-bottom:20px;align-items:start;">
      <div class="card">
        <div style="font-weight:700;font-size:15px;margin-bottom:16px;">Consumption Breakdown</div>
        <div class="chart-wrap"><canvas id="donut" role="img" aria-label="Appliance consumption breakdown chart"></canvas></div>
      </div>
      <div class="card">
        <div style="font-weight:700;font-size:15px;margin-bottom:12px;">Unit × Cost Table</div>
        <table style="width:100%;border-collapse:collapse;">
          <thead><tr style="border-bottom:1px solid var(--border);">
            <th style="text-align:left;font-size:11px;color:var(--muted);padding:6px 0;font-weight:500;">Appliance</th>
            <th style="text-align:right;font-size:11px;color:var(--muted);padding:6px 0;font-weight:500;">Units</th>
            <th style="text-align:right;font-size:11px;color:var(--muted);padding:6px 0;font-weight:500;">Cost</th>
            <th style="font-size:11px;color:var(--muted);padding:6px 4px;font-weight:500;">Share</th>
          </tr></thead>
          <tbody>${tableRows}</tbody>
          <tfoot><tr style="border-top:1px solid var(--border);">
            <td style="padding:8px 0;font-weight:700;font-size:13px;">Total</td>
            <td style="text-align:right;font-weight:700;font-size:13px;padding:8px 0;">${A.estUnits}</td>
            <td style="text-align:right;font-weight:700;font-size:13px;color:var(--accent);padding:8px 0;">₹${A.estBill.toLocaleString('en-IN')}</td>
            <td></td>
          </tfoot>
        </table>
      </div>
    </div>
    <div class="card" style="margin-bottom:20px;">
      <div style="font-weight:700;font-size:15px;margin-bottom:16px;">💰 Bill Forecast</div>
      <div class="grid3">
        <div class="card-sm">
          <div style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.06em;margin-bottom:6px;">Current Month</div>
          <div style="font-family:var(--font-head);font-size:26px;font-weight:700;">₹${A.estBill.toLocaleString('en-IN')}</div>
          <div style="font-size:11px;color:var(--muted);margin-top:2px;">${A.estUnits} units · ₹${A.marginalRate}/unit</div>
        </div>
        <div class="card-sm" style="border-color:rgba(239,68,68,.3);">
          <div style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.06em;margin-bottom:6px;">Next Month (trend)</div>
          <div style="font-family:var(--font-head);font-size:26px;font-weight:700;color:#f87171;">₹${A.nextMonthBill.toLocaleString('en-IN')}</div>
          <div style="font-size:11px;color:var(--muted);margin-top:2px;">${A.nextMonthUnits} units · +4% trend</div>
        </div>
        <div class="card-sm" style="border-color:rgba(16,185,129,.3);">
          <div style="font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.06em;margin-bottom:6px;">With All Tips Applied</div>
          <div style="font-family:var(--font-head);font-size:26px;font-weight:700;color:var(--accent3);">₹${A.optimisedBill.toLocaleString('en-IN')}</div>
          <div style="font-size:11px;color:var(--accent3);margin-top:2px;">Save ₹${(A.estBill-A.optimisedBill).toLocaleString('en-IN')}/month</div>
        </div>
      </div>
    </div>
    <div class="card" style="margin-bottom:20px;">
      <div style="font-weight:700;font-size:15px;margin-bottom:14px;">📊 TNEB Tariff Slab Analysis</div>
      ${renderSlabAnalysis(A.estUnits)}
    </div>
    ${A.savings.length>0?`
    <div style="margin-bottom:20px;">
      <div style="font-weight:700;font-size:15px;margin-bottom:14px;">🤖 Personalised Savings Tips</div>
      ${sugHtml}
    </div>`:''}
    <div class="card-sm" style="display:flex;gap:14px;align-items:center;">
      <div style="font-size:28px;">🌳</div>
      <div>
        <div style="font-weight:600;font-size:14px;">Carbon Footprint</div>
        <div style="font-size:12px;color:var(--muted);margin-top:2px;">Your home emits ~${A.co2kg} kg CO₂/month (${Math.round(A.co2kg/12)} kg/day). That equals ${A.treeDays} tree-days to offset.</div>
      </div>
    </div>
  </div>`);
  setTimeout(()=>{
    let ctx = $('donut')?.getContext('2d');
    if(!ctx) return;
    new Chart(ctx, {
      type:'doughnut',
      data:{ labels, datasets:[{ data, backgroundColor: palette.slice(0,data.length), borderWidth:0, hoverOffset:6 }] },
      options:{
        responsive:true, maintainAspectRatio:false, cutout:'65%',
        plugins:{
          legend:{ position:'bottom', labels:{ color:'#6b7280', font:{size:11,family:'Space Grotesk'}, padding:12, usePointStyle:true, pointStyle:'rect' } },
          tooltip:{ callbacks:{ label: ctx=>`${ctx.label}: ${ctx.raw} units (₹${TNEB.bill(ctx.raw)})` } }
        }
      }
    });
  }, 100);
}
function renderSlabAnalysis(units){
  let slabs = [
    { label:'0–100 units', rate:'₹1.50/unit', limit:100, color:'#34d399' },
    { label:'101–200 units', rate:'₹3.00/unit', limit:200, color:'#60a5fa' },
    { label:'201–500 units', rate:'₹4.50/unit', limit:500, color:'#fbbf24' },
    { label:'501+ units', rate:'₹6.50/unit', limit:Infinity, color:'#f87171' },
  ];
  return slabs.map(s=>{
    let inSlab = units >= (s.limit===100?0:s.limit===200?101:s.limit===500?201:501) && units <= s.limit;
    return `<div style="display:flex;align-items:center;gap:12px;padding:8px 0;border-bottom:1px solid var(--border);">
      <div style="width:10px;height:10px;border-radius:50%;background:${s.color};flex-shrink:0;"></div>
      <div style="flex:1;font-size:13px;">${s.label}</div>
      <div style="font-size:13px;color:var(--muted);">${s.rate}</div>
      ${inSlab?`<span class="badge" style="background:rgba(245,200,66,.12);color:var(--accent);">YOU ARE HERE</span>`:''}
    </div>`;
  }).join('');
}
function goVoice(){ S.view='voice'; S.voiceState='idle'; renderVoice(); }
function renderVoice(){
  mount(`
  <div class="container" style="min-height:100vh;display:flex;flex-direction:column;justify-content:center;padding-top:40px;">
    <button class="btn btn-ghost" style="align-self:flex-start;padding:8px 12px;margin-bottom:32px;" onclick="S.view='home';renderHome()">← Back</button>
    <div style="text-align:center;margin-bottom:32px;" class="fade-up">
      <div style="position:relative;display:inline-block;margin-bottom:20px;">
        ${S.voiceState==='listening'?'<div class="mic-ripple"></div><div class="mic-ripple" style="animation-delay:.4s"></div>':''}
        <button class="mic-btn ${S.voiceState==='listening'?'listening':''}" id="micBtn" onclick="handleMic()">
          <svg width="36" height="36" fill="${S.voiceState==='listening'?'white':'#000'}" viewBox="0 0 24 24"><path d="M12 14c1.66 0 3-1.34 3-3V5c0-1.66-1.34-3-3-3S9 3.34 9 5v6c0 1.66 1.34 3 3 3zm5.3-3c0 3-2.54 5.1-5.3 5.1S6.7 14 6.7 11H5c0 3.41 2.72 6.23 6 6.72V21h2v-3.28c3.28-.48 6-3.3 6-6.72h-1.7z"/></svg>
        </button>
      </div>
      <h2 style="font-size:22px;margin-bottom:8px;">${S.voiceState==='idle'?'Tap to speak':S.voiceState==='listening'?'Listening…':'Processing…'}</h2>
      <p style="color:var(--muted);font-size:13px;max-width:320px;margin:0 auto;line-height:1.6;">
        Try: <em>"2BHK, 3 people, bill 2400, 1.5 ton 3-star AC, 250L 4-star fridge, geyser, washing machine"</em>
      </p>
    </div>
    ${S.voiceText?`<div class="transcript-box fade-up"><span style="font-size:11px;color:var(--muted);display:block;margin-bottom:6px;">You said:</span>"${S.voiceText}"</div>`:''}
    <div style="margin-top:20px;background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:14px;">
      <div style="font-size:12px;font-weight:600;margin-bottom:10px;">Voice tip examples:</div>
      ${[
        '"2BHK, 4 people, AC 1 ton 5 star, fridge 300 litre 4 star"',
        '"Independent house, bill 4500, geyser 15 litre, 3 fans"',
        '"1BHK, 2 people, bill 1800, fridge and TV 43 inch"',
      ].map(e=>`<div style="font-size:12px;color:var(--muted);padding:4px 0;border-bottom:1px solid var(--border);">${e}</div>`).join('')}
    </div>
  </div>`);
}
let recognition = null;
function handleMic(){
  if(S.voiceState==='listening'){
    if(recognition) recognition.stop();
    S.voiceState='idle';
    renderVoice();
    return;
  }
  if(!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)){
    showToast('Not supported','Use Chrome or Edge for voice input','warning');
    return;
  }
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  recognition = new SR();
  recognition.lang = 'en-IN';
  recognition.continuous = false;
  recognition.interimResults = true;
  recognition.onstart = ()=>{ S.voiceState='listening'; renderVoice(); };
  recognition.onresult = (e)=>{
    let interim = '', final = '';
    for(let r of e.results){ if(r.isFinal) final += r[0].transcript; else interim += r[0].transcript; }
    S.voiceText = final || interim;
    if($('transcriptBox')) $('transcriptBox').textContent = '"'+S.voiceText+'"';
  };
  recognition.onend = ()=>{
    if(!S.voiceText){ S.voiceState='idle'; renderVoice(); return; }
    S.voiceState='processing';
    renderVoice();
    setTimeout(()=>{
      let parsed = parseVoice(S.voiceText);
      if(parsed.formData.billAmount) S.formData.billAmount = parsed.formData.billAmount;
      if(parsed.formData.people) S.formData.people = parsed.formData.people;
      if(parsed.formData.houseType) S.formData.houseType = parsed.formData.houseType;
      S.selectedApps = parsed.apps.length > 0 ? parsed.apps : S.selectedApps;
      for(let k in parsed.specs) S.appSpecs[k] = {...(S.appSpecs[k]||{}), ...parsed.specs[k]};
      if(parsed.apps.length>0 || S.formData.billAmount){
        if(!S.formData.people) S.formData.people = '3';
        if(!S.formData.houseType) S.formData.houseType = '2bhk';
        runAndShow();
      } else {
        showToast('Could not parse','Please try speaking more clearly or use the form','warning');
        S.voiceState='idle'; renderVoice();
      }
    }, 600);
  };
  recognition.onerror = (e)=>{
    S.voiceState='idle';
    showToast('Voice error',e.error==='not-allowed'?'Microphone permission denied':e.error,'error');
    renderVoice();
  };
  recognition.start();
}
renderHome();
</script>
</body>
</html>
```
