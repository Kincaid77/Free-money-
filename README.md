# Free-money-
Free money site
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Play Games and Win Money</title>
<style>
body { font-family: Arial; background: #f4f7f9; margin: 0; text-align: center; }
header { background: #4caf50; color: white; padding: 14px; font-size: 20px; font-weight:700; }
.topbar { display:flex; justify-content:space-between; align-items:center; padding:10px 18px; background:#fff; box-shadow:0 2px 6px rgba(0,0,0,0.1); display:none; }
.balance { font-weight:700; }
.actions { display:flex; gap:8px; }
button { padding:8px 14px; border-radius:6px; border:none; cursor:pointer; }
button.primary { background:#4caf50; color:white; }
button.black { background:#000; color:white; }
button.ghost { background:transparent; border:1px solid #ccc; color:#333; }
.container { padding:18px; max-width:760px; margin:auto; }
input { padding:10px; margin-bottom:10px; width:100%; border:1px solid #ccc; border-radius:6px; box-sizing:border-box; }
.muted { color:#666; font-size:13px; }
.gameCard { display:flex; align-items:center; gap:16px; padding:12px; background:#fff; border-radius:8px; box-shadow:0 6px 18px rgba(0,0,0,0.04); margin-bottom:12px; flex-direction:column; }
.previewPencil { width:50px; height:50px; background: linear-gradient(to bottom, #f7df9f 0%, #e0a25a 70%, #2b2b2b 100%); border:1px solid #3b2b1b; border-radius:3px; display:flex; align-items:flex-end; justify-content:center; margin:auto; }
.previewCarBoard { width:160px; height:80px; display:flex; justify-content:center; align-items:center; gap:12px; }
.previewCar { width:50px; height:30px; border-radius:8px; color:#fff; font-weight:700; display:flex; align-items:center; justify-content:center; }
.previewRed { background:#d9534f; }
.previewBlue { background:#337ab7; }
#backBtn { display:none; position:fixed; left:14px; top:14px; z-index:60; border:none; padding:8px 12px; border-radius:6px; background:#000; color:#fff; cursor:pointer; }

#board { width:250px; height:250px; margin:24px auto; border-radius:50%; background: conic-gradient(green 0deg 180deg, red 180deg 360deg); position: relative; display:none; box-shadow:0 6px 18px rgba(0,0,0,0.12); }
#pencil { width:12px; height:100px; background: linear-gradient(to bottom, #f7df9f 0%, #e0a25a 70%, #2b2b2b 100%); border:2px solid #3b2b1b; position:absolute; top:50%; left:50%; transform-origin:50% 88%; transform:translate(-50%,-50%) rotate(0deg); border-radius:3px; display:flex; align-items:flex-end; }
#spinBtn { display:none; margin:12px auto; }

#carRaceBoard { width:100%; height:200px; background:#eee; margin-top:20px; position:relative; display:none; border-radius:8px; box-shadow:0 6px 18px rgba(0,0,0,0.06); overflow:hidden; }
.lane { position:absolute; width:100%; height:60px; border-bottom:2px dashed rgba(0,0,0,0.08); left:0; }
.l1 { top:40px; }
.l2 { top:120px; }
#finishLine { position:absolute; width:6px; height:100%; background:black; right:20px; top:0; z-index:10; }
.car { width:50px; height:30px; position:absolute; border-radius:6px; top:0; left:10px; z-index:20; display:flex; align-items:center; justify-content:center; color:#fff; font-weight:700; }
#car1 { background:#d9534f; top:46px; left:6px; }
#car2 { background:#337ab7; top:126px; left:6px; }
#raceButtons { margin-top:12px; display:none; }

#withdrawScreen, #depositScreen { display:none; margin-top:20px; background:#fff; padding:18px; border-radius:8px; box-shadow:0 6px 16px rgba(0,0,0,0.08); }
.smallNote { font-size:13px; color:#555; margin-bottom:10px; }
</style>
</head>
<body>

<header>Play Games and Win Money</header>

<div class="topbar" id="topbar">
  <div class="balance" id="balanceDisplay">Balance: ₦0.00</div>
  <div class="actions">
    <button id="depositBtn" class="primary" onclick="openDeposit()">Deposit</button>
    <button id="withdrawBtn" class="black" onclick="openWithdraw()">Withdraw</button>
  </div>
</div>

<button id="backBtn" onclick="backToDashboard()">Back</button>

<div class="container">

  <!-- REGISTER -->
  <div id="registerDiv">
    <h2>Create account</h2>
    <input id="regEmail" type="email" placeholder="Email">
    <input id="regPass" type="password" placeholder="Password (more than 6 characters)">
    <div>
      <button class="primary" onclick="register()">Register</button>
      <button class="ghost" onclick="showLogin()">Go to Login</button>
    </div>
  </div>

  <!-- LOGIN -->
  <div id="loginDiv" style="display:none;">
    <h2>Login</h2>
    <input id="logEmail" type="email" placeholder="Email">
    <input id="logPass" type="password" placeholder="Password">
    <div>
      <button class="primary" onclick="login()">Login</button>
      <button class="ghost" onclick="showRegister()">Back to Register</button>
    </div>
  </div>

  <!-- DASHBOARD -->
  <div id="dashboard" style="display:none; text-align:center;">
    <p class="muted">Choose a game to play. Minimum ₦50 deposit required.</p>

    <!-- Spin Pencil Preview -->
    <div class="gameCard" id="spinPreview">
      <div class="previewPencil"></div>
      <input id="spinStake" type="number" placeholder="Enter stake amount">
      <button class="primary" onclick="openSpinGame()">PLAY</button>
    </div>

    <hr>

    <!-- Car Race Preview -->
    <div class="gameCard" id="carPreview">
      <div class="previewCarBoard">
        <div class="previewCar previewRed">R</div>
        <div class="previewCar previewBlue">B</div>
      </div>
      <input id="carStake" type="number" placeholder="Enter stake amount">
      <button class="primary" onclick="openCarRace()">PLAY</button>
    </div>

  </div>

  <!-- SPIN GAME -->
  <div id="board">
    <div id="pencil"></div>
  </div>
  <div>
    <button id="spinBtn" class="primary" onclick="spinPencil()">Spin</button>
  </div>

  <!-- CAR RACE -->
  <div id="carRaceBoard">
    <div class="lane l1"></div>
    <div class="lane l2"></div>
    <div id="finishLine"></div>
    <div id="car1" class="car">R</div>
    <div id="car2" class="car">B</div>
  </div>
  <div id="raceButtons">
    <button class="primary" onclick="race('red')">Red</button>
    <button class="primary" onclick="race('blue')">Blue</button>
  </div>

  <!-- WITHDRAW -->
  <div id="withdrawScreen">
    <h2>Withdraw Money</h2>
    <input id="accNum" type="text" placeholder="Account number">
    <input id="bankName" type="text" placeholder="Bank name">
    <button class="primary" onclick="confirmWithdraw()">Confirm</button>
    <p id="withdrawMessage" class="muted"></p>
  </div>

  <!-- DEPOSIT -->
  <div id="depositScreen">
    <h2>Deposit Money</h2>
    <input id="depositAmount" type="number" placeholder="Enter amount (₦100-₦1000)">
    <button class="primary" onclick="depositEnter()">ENTER</button>
    <div id="depositInstruction" style="display:none;">
      <p class="muted"><strong>8026735608 — OPay</strong></p>
      <p class="muted">After payment press "I have already made payment".</p>
      <button class="primary" onclick="confirmDeposit()">I have already made payment</button>
    </div>
    <p id="depositMessage" class="muted"></p>
  </div>

</div>

<script>
let users = {};
let currentUser = null;
let balance = 0;

// Register/Login
function showLogin(){ document.getElementById('registerDiv').style.display='none'; document.getElementById('loginDiv').style.display='block'; }
function showRegister(){ document.getElementById('registerDiv').style.display='block'; document.getElementById('loginDiv').style.display='none'; }
function updateBalanceDisplay(){ document.getElementById('balanceDisplay').innerText='Balance: ₦'+(balance?balance.toFixed(2):'0.00'); }

function register(){
  const email=document.getElementById('regEmail').value.trim();
  const pass=document.getElementById('regPass').value.trim();
  if(!email||!pass){ alert("Fill all fields"); return;}
  if(pass.length<=6){ alert("Password > 6 chars"); return;}
  if(users[email]){ alert("This Gmail is already in use!"); return;}
  users[email]={password:pass,balance:0};
  alert("Registration successful. Please login");
  showLogin();
}

function login(){
  const email=document.getElementById('logEmail').value.trim();
  const pass=document.getElementById('logPass').value.trim();
  if(!users[email]){ alert("No account"); return;}
  if(users[email].password!==pass){ alert("Wrong password"); return;}
  currentUser=email;
  balance=users[currentUser].balance;

  document.getElementById('loginDiv').style.display='none';
  document.getElementById('registerDiv').style.display='none';
  document.getElementById('dashboard').style.display='block';
  document.getElementById('topbar').style.display='flex';
  document.getElementById('backBtn').style.display='none';
  hideScreens();
  updateBalanceDisplay();
}

// Hide all special screens
function hideScreens(){
  document.getElementById('depositScreen').style.display='none';
  document.getElementById('withdrawScreen').style.display='none';
  document.getElementById('board').style.display='none';
  document.getElementById('spinBtn').style.display='none';
  document.getElementById('carRaceBoard').style.display='none';
  document.getElementById('raceButtons').style.display='none';
  document.getElementById('depositInstruction').style.display='none';
  document.getElementById('withdrawMessage').innerText='';
}

// Deposit flow
function openDeposit(){
  document.getElementById('dashboard').style.display='none';
  hideScreens();
  document.getElementById('depositScreen').style.display='block';
  document.getElementById('backBtn').style.display='inline-block';
}
function depositEnter(){
  const amt=parseInt(document.getElementById('depositAmount').value);
  if(isNaN(amt)||amt<100||amt>1000){ alert("Enter 100-1000"); return;}
  document.getElementById('depositInstruction').style.display='block';
}
function confirmDeposit(){
  const amt=parseInt(document.getElementById('depositAmount').value);
  balance+=amt;
  users[currentUser].balance=balance;
  updateBalanceDisplay();
  alert("Deposit confirmed! Balance updated.");
  backToDashboard();
}

// Withdraw flow
function openWithdraw(){
  document.getElementById('dashboard').style.display='none';
  hideScreens();
  document.getElementById('withdrawScreen').style.display='block';
  document.getElementById('backBtn').style.display='inline-block';
}
function confirmWithdraw(){ document.getElementById('withdrawMessage').innerText='Your money is processing...'; }

// Back button
function backToDashboard(){
  hideScreens();
  document.getElementById('dashboard').style.display='block';
  document.getElementById('backBtn').style.display='none';
}

// Spin Pencil Game
function openSpinGame(){
  const stake=parseInt(document.getElementById('spinStake').value);
  if(isNaN(stake)||stake<50){ alert("Enter minimum ₦50 stake"); return;}
  if(stake>balance){ alert("Insufficient balance!"); return;}
  window.currentSpinStake=stake;

  document.getElementById('dashboard').style.display='none';
  hideScreens();
  document.getElementById('board').style.display='block';
  document.getElementById('spinBtn').style.display='block';
  document.getElementById('backBtn').style.display='inline-block';
}
function spinPencil(){
  const pencil = document.getElementById('pencil');
  const randomDeg=Math.floor(Math.random()*360);
  pencil.style.transition="transform 2s ease-out";
  pencil.style.transform=`translate(-50%,-50%) rotate(${randomDeg}deg)`;
  setTimeout(()=>{
    let win=false;
    if(randomDeg>=0 && randomDeg<180) win=true; // green half
    if(win){
      balance+=window.currentSpinStake*2;
      alert("You won! +₦"+window.currentSpinStake*2);
    }else{
      balance-=window.currentSpinStake;
      alert("Try again! -₦"+window.currentSpinStake);
    }
    users[currentUser].balance=balance;
    updateBalanceDisplay();
  },2000);
}

// Car Race Game
function openCarRace(){
  const stake=parseInt(document.getElementById('carStake').value);
  if(isNaN(stake)||stake<50){ alert("Enter minimum ₦50 stake"); return;}
  if(stake>balance){ alert("Insufficient balance!"); return;}
  window.currentCarStake=stake;

  document.getElementById('dashboard').style.display='none';
  hideScreens();
  document.getElementById('carRaceBoard').style.display='block';
  document.getElementById('raceButtons').style.display='block';
  document.getElementById('backBtn').style.display='inline-block';
}
function race(color){
  const car1=document.getElementById('car1');
  const car2=document.getElementById('car2');
  car1.style.left="10px"; car2.style.left="10px";
  const finish=300;
  let car1Speed=Math.random()*3+2;
  let car2Speed=Math.random()*3+2;
  let car1Pos=10, car2Pos=10;
  const interval=setInterval(()=>{
    car1Pos+=car1Speed; car2Pos+=car2Speed;
    car1.style.left=car1Pos+"px"; car2.style.left=car2Pos+"px";
    if(car1Pos>=finish || car2Pos>=finish){
      clearInterval(interval);
      let winner=(car1Pos>=finish && car1Pos>car2Pos)?"red":(car2Pos>=finish && car2Pos>car1Pos)?"blue":"tie";
      if(winner===color){
        balance+=window.currentCarStake*2;
        alert("You won! +₦"+window.currentCarStake*2);
      }else{
        balance-=window.currentCarStake;
        alert("Try again! -₦"+window.currentCarStake);
      }
      users[currentUser].balance=balance;
      updateBalanceDisplay();
    }
  },50);
}
</script>

</body>
</html>
