<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Random dãy nhị phân 10 bit</title>

<style>
body {
    font-family: Arial, sans-serif;
    min-height: 100vh;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(135deg, #ffe259, #ffa751);
}

.container {
    padding: 30px 40px;
    border-radius: 16px;
    background: linear-gradient(135deg, #ff1a1a, #ff004c);
    border: 3px solid #ff0033;
    box-shadow: 0 0 25px rgba(255,0,60,0.7);
    text-align: center;
    color: #fff;
}

.group {
    display: flex;
    justify-content: center;
    gap: 6px;
    margin-bottom: 20px;
}

.bit {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.bit span {
    font-size: 12px;
    margin-bottom: 4px;
}

.input-bit,
.result-bit {
    width: 44px;
    height: 44px;
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    border-radius: 8px;
    border: 2px solid #d2691e;
}

.input-bit { background: #ffe0b2; }
.result-bit { background: #f5deb3; }

button {
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    border-radius: 6px;
    border: none;
    cursor: pointer;
    margin: 5px;
    background: #f1f1f1;
    transition: 
        box-shadow 0.3s ease,
        transform 0.2s ease,
        background 0.3s ease;
}

/* Chỉ lung linh khi focus */
button:focus {
    outline: none;
    background: linear-gradient(135deg, #ffe259, #ffa751);
    box-shadow:
        0 0 6px rgba(255, 200, 0, 0.8),
        0 0 14px rgba(255, 180, 0, 0.6),
        0 0 28px rgba(255, 150, 0, 0.4);
    transform: scale(1.05);
}

.glow {
    background: linear-gradient(135deg, #ffeb3b, #ffc107);
    color: #000;
    animation: yellowPulse 1.4s infinite alternate;
}

@keyframes yellowPulse {
    from { box-shadow: 0 0 6px rgba(255,215,0,0.6); }
    to   { box-shadow: 0 0 16px rgba(255,215,0,1); }
}

#processBtn {
    background: linear-gradient(135deg, #ff3b3b, #ff8a00);
    color: #fff;
}

.reset {
    background: linear-gradient(135deg, #00c6ff, #0072ff);
    color: #fff;
    box-shadow: 0 0 8px rgba(0,198,255,0.7);
    animation: resetGlow 1.6s infinite alternate;
}

@keyframes resetGlow {
    from {
        box-shadow:
            0 0 6px rgba(0,198,255,0.6),
            0 0 12px rgba(0,114,255,0.6);
        transform: scale(1);
    }
    to {
        box-shadow:
            0 0 14px rgba(0,198,255,1),
            0 0 28px rgba(0,114,255,1);
        transform: scale(1.04);
    }
}
</style>
</head>

<body>

<div class="container">
    <h3>Nhập 10 bit</h3>

    <div class="group">
        <div class="bit"><span>0</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>1</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>2</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>3</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>4</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>5</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>6</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>7</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>8</span><input class="input-bit" maxlength="1"></div>
        <div class="bit"><span>9</span><input class="input-bit" maxlength="1"></div>
    </div>

    <div class="group">
        <button id="processBtn">Bùm Bùm Bùm</button>
        <button class="reset" id="resetBtn">Reset</button>
    </div>

    <h3>Kết quả</h3>

    <div class="group">
    <div class="bit"><span>0</span><input class="result-bit" readonly></div>
    <div class="bit"><span>1</span><input class="result-bit" readonly></div>
    <div class="bit"><span>2</span><input class="result-bit" readonly></div>
    <div class="bit"><span>3</span><input class="result-bit" readonly></div>
    <div class="bit"><span>4</span><input class="result-bit" readonly></div>

    <div class="bit"><span>5</span><input class="result-bit glow" readonly></div>
    <div class="bit"><span>6</span><input class="result-bit glow" readonly></div>
    <div class="bit"><span>7</span><input class="result-bit glow" readonly></div>
    <div class="bit"><span>8</span><input class="result-bit glow" readonly></div>
    <div class="bit"><span>9</span><input class="result-bit glow" readonly></div>
</div>
</div>

<script>
const inputs = document.querySelectorAll('.input-bit');
const results = document.querySelectorAll('.result-bit');
const processBtn = document.getElementById('processBtn');
const resetBtn = document.getElementById('resetBtn');

// Shuffle
function shuffle(arr) {
    for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
    }
}

// Dịch trái input 1–9, để trống ô 9
function shiftInputsLeft() {
    for (let i = 0; i < 9; i++) {
        inputs[i].value = inputs[i + 1].value;
    }
    inputs[9].value = '';
    inputs[9].focus();
}

// Bùm
function handleBoom() {
    const bits = [];

    for (let i = 0; i < 10; i++) {
        const v = inputs[i].value;
        if (v !== '0' && v !== '1') {
            alert('Nhập đủ 10 bit nhị phân (0 hoặc 1)');
            return;
        }
        bits.push(v);
    }

    shuffle(bits);
    results.forEach((el, i) => el.value = bits[i]);

    shiftInputsLeft();
}

processBtn.addEventListener('click', handleBoom);

resetBtn.addEventListener('click', () => {
    inputs.forEach(i => i.value = '');
    results.forEach(r => r.value = '');
    inputs[0].focus();
});

// Auto focus + kiểm tra nhập
inputs.forEach((input, idx) => {
    input.addEventListener('input', () => {
        if (input.value !== '0' && input.value !== '1') {
            input.value = '';
            return;
        }
       if (idx < 9) {
            inputs[idx + 1].focus();
        } else {
            processBtn.focus(); // 👈 nhập xong ô 9 → nhảy vào nút Bùm
        }    });
});

window.onload = () => inputs[0].focus();
</script>

</body>
</html>

