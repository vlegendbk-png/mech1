<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ALP & Embedded C Code Reference</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;600&family=Syne:wght@400;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0c10;
    --surface: #111318;
    --surface2: #181b22;
    --border: #1e2230;
    --accent: #00e5ff;
    --accent2: #ff6b35;
    --text: #c9d1e0;
    --muted: #4a5568;
    --green: #39d98a;
    --tag-alp: #1a2e4a;
    --tag-c: #2a1a0e;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    min-height: 100vh;
    padding-bottom: 60px;
  }

  /* Noise overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
  }

  header {
    position: relative;
    padding: 52px 40px 36px;
    border-bottom: 1px solid var(--border);
    background: linear-gradient(135deg, #0d1117 0%, #111827 100%);
    overflow: hidden;
  }

  header::after {
    content: '';
    position: absolute;
    top: -60px; right: -80px;
    width: 320px; height: 320px;
    background: radial-gradient(circle, rgba(0,229,255,0.07) 0%, transparent 70%);
    pointer-events: none;
  }

  .header-tag {
    display: inline-block;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.18em;
    color: var(--accent);
    text-transform: uppercase;
    border: 1px solid rgba(0,229,255,0.25);
    padding: 4px 12px;
    border-radius: 2px;
    margin-bottom: 16px;
  }

  h1 {
    font-size: clamp(26px, 4vw, 44px);
    font-weight: 800;
    line-height: 1.1;
    letter-spacing: -0.02em;
    color: #f0f4ff;
  }

  h1 span { color: var(--accent); }

  .subtitle {
    margin-top: 10px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    color: var(--muted);
  }

  .stats {
    display: flex;
    gap: 28px;
    margin-top: 24px;
    flex-wrap: wrap;
  }

  .stat {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 13px;
    color: var(--muted);
    font-family: 'JetBrains Mono', monospace;
  }

  .stat span { color: var(--accent); font-weight: 600; }

  /* Filter bar */
  .filter-bar {
    display: flex;
    gap: 10px;
    padding: 20px 40px;
    border-bottom: 1px solid var(--border);
    background: var(--surface);
    flex-wrap: wrap;
    position: sticky;
    top: 0;
    z-index: 10;
  }

  .filter-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.1em;
    padding: 6px 14px;
    border-radius: 2px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.15s;
  }

  .filter-btn:hover, .filter-btn.active {
    background: var(--accent);
    border-color: var(--accent);
    color: #000;
  }

  /* Main grid */
  .container {
    max-width: 1100px;
    margin: 0 auto;
    padding: 40px 24px;
    position: relative;
    z-index: 1;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(480px, 1fr));
    gap: 20px;
  }

  /* Code card */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 6px;
    overflow: hidden;
    transition: border-color 0.2s, transform 0.2s;
    animation: fadeIn 0.4s ease both;
  }

  .card:hover {
    border-color: rgba(0,229,255,0.3);
    transform: translateY(-2px);
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(12px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 14px 18px;
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
    gap: 10px;
  }

  .card-title {
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.02em;
    color: #e2e8f0;
    flex: 1;
  }

  .card-num {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    background: rgba(0,229,255,0.08);
    border: 1px solid rgba(0,229,255,0.2);
    padding: 2px 8px;
    border-radius: 2px;
    white-space: nowrap;
  }

  .type-badge {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.1em;
    padding: 2px 8px;
    border-radius: 2px;
    white-space: nowrap;
  }

  .type-alp {
    background: var(--tag-alp);
    color: #60a5fa;
    border: 1px solid rgba(96,165,250,0.2);
  }

  .type-c {
    background: var(--tag-c);
    color: #fb923c;
    border: 1px solid rgba(251,146,60,0.2);
  }

  .code-wrap {
    position: relative;
    max-height: 340px;
    overflow-y: auto;
  }

  .code-wrap::-webkit-scrollbar { width: 5px; }
  .code-wrap::-webkit-scrollbar-track { background: var(--surface); }
  .code-wrap::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

  pre {
    padding: 18px 20px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    line-height: 1.75;
    color: #a8b4cc;
    white-space: pre;
    overflow-x: auto;
    tab-size: 4;
  }

  /* Syntax highlight colors */
  .kw  { color: #7dd3fc; }
  .cm  { color: #4a5568; font-style: italic; }
  .num { color: #86efac; }
  .str { color: #fcd34d; }
  .fn  { color: #c4b5fd; }
  .dir { color: #f9a8d4; }

  .card-footer {
    padding: 10px 18px;
    border-top: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: 8px;
  }

  .copy-btn {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.08em;
    padding: 7px 18px;
    border-radius: 3px;
    border: 1px solid rgba(0,229,255,0.35);
    background: rgba(0,229,255,0.05);
    color: var(--accent);
    cursor: pointer;
    transition: all 0.15s;
    display: flex;
    align-items: center;
    gap: 7px;
  }

  .copy-btn:hover {
    background: var(--accent);
    color: #000;
    border-color: var(--accent);
  }

  .copy-btn.copied {
    background: rgba(57,217,138,0.1);
    border-color: var(--green);
    color: var(--green);
  }

  .copy-btn svg { flex-shrink: 0; }

  /* Copy all bar */
  .copy-all-bar {
    text-align: center;
    padding: 16px;
    margin-top: 32px;
  }

  .copy-all-btn {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.08em;
    padding: 12px 36px;
    border-radius: 3px;
    border: 1px solid rgba(0,229,255,0.4);
    background: rgba(0,229,255,0.06);
    color: var(--accent);
    cursor: pointer;
    transition: all 0.2s;
    text-transform: uppercase;
  }

  .copy-all-btn:hover {
    background: var(--accent);
    color: #000;
  }

  @media (max-width: 600px) {
    header { padding: 32px 20px 24px; }
    .filter-bar { padding: 14px 20px; }
    .grid { grid-template-columns: 1fr; }
    .container { padding: 24px 14px; }
  }
</style>
</head>
<body>

<header>
  <div class="header-tag">ARM Assembly &amp; Embedded C</div>
  <h1>ALP Code <span>Reference</span></h1>
  <p class="subtitle">// 13 programs — LPC1768 / ARM Cortex-M3</p>
  <div class="stats">
    <div class="stat">Programs: <span>13</span></div>
    <div class="stat">ALP: <span>9</span></div>
    <div class="stat">Embedded C: <span>4</span></div>
  </div>
</header>

<div class="filter-bar">
  <button class="filter-btn active" onclick="filter('all', this)">ALL</button>
  <button class="filter-btn" onclick="filter('alp', this)">ALP ONLY</button>
  <button class="filter-btn" onclick="filter('c', this)">EMBEDDED C</button>
</div>

<div class="container">
  <div class="grid" id="grid"></div>
  <div class="copy-all-bar">
    <button class="copy-all-btn" onclick="copyAll()">⬡ Copy All Programs</button>
  </div>
</div>

<script>
const codes = [
  {
    num: "01",
    title: "Sum of First 10 Integer Numbers",
    type: "alp",
    code: ` area reg2, data, readwrite
result  dcd 00
 area reg1, code, readonly
 export __main
__main
 mov r0,#0
 mov r2,#9
 mov r1,#1
up add r0,r0,r1
 add r1,#01
 sub r2,#01
 cmp r2,#00
 bne up
 ldr r1,=result
 str r0,[r1]
here b here
 end`
  },
  {
    num: "02",
    title: "Multiply Two 16-bit Binary Numbers",
    type: "alp",
    code: ` area reg_1,data,readwrite
result dcd 00
 area reg_2,code,readonly
 export __main
__main
 ldr r2, =2_1010101111001101
 ldr r1, =2_0001001000110100
 mul r1,r1,r2
 ldr r2,=result
 str r1,[r2]
here b here
 end`
  },
  {
    num: "03",
    title: "Add Two 64-bit Binary Numbers",
    type: "alp",
    code: `  area reg_1,data,readwrite
result dcd 00
  area reg_3,code,readonly
 export __main
__main
 ldr r0,=0x1234E640
 ldr r1,=0x43210010
 ldr r2,=0x12348900 
 ldr r3,=0x43212102 
 add r4,r1,r3 
 add r5,r0,r2
here b here
 end`
  },
  {
    num: "04",
    title: "Factorial of a Given Number",
    type: "alp",
    code: `   area reg_2, data, readwrite
fact dcd 0
 area reg_3,code,readonly
 export __main
__main
  mov r1,#5  
  mov r2,#1
l1  mul r2,r2,r1
 sub r1,#1 
 cmp r1,#0
 bne l1
 ldr r3,=fact
 str r2,[r3]
here b here
 end`
  },
  {
    num: "05",
    title: "Add Array of 16-bit Numbers (32-bit Result)",
    type: "alp",
    code: ` area reg_1,data,readwrite
sum dcd 00
 area reg_3,code,readonly
 export __main
__main
 mov r0,#4
 mov r1,#0
 ldr r3,=array
l1 ldrh r2,[r3]
 add r1,r1,r2
 add r3,#2
 sub r0,#1
 cmp r0,#0
 bne l1
 ldr r5,=sum
 str r1,[r5]
here b here
array dcw 0x1111,0x2222,0x3333,0x4444
 end`
  },
  {
    num: "06",
    title: "Square of Number Using Lookup Table",
    type: "alp",
    code: `   area reg_1, data, readonly
table dcb 1,4,9,16,25,36,49,64,81,100
 area reg_2,data,readwrite
result dcb 0
 area reg_3,code,readonly
 export __main
__main
 ldr r2,=table
 mov r1,#8 
 add r2,r1
 ldrb r5,[r2]
 ldr r4,=result
 strb r5,[r4]
here b here
 end`
  },
  {
    num: "07",
    title: "Find Largest / Smallest Number in Array",
    type: "alp",
    code: ` area reg_2,code,readonly
array dcd 0x11111111,0x22222222,0x53333333,0x44444444
 area reg_3,code,readonly
 export __main
__main
 ldr r0,=array
 ldr r2,[r0]
 mov r4,#4
 mov r1,r2
l1  add r0,#4
 ldr r3,[r0]
 sub r4,#01
 cmp r4,#0
 beq here
 cmp r1,r3
 bge l1
 mov r1,r3 ;largest value in r1 reg
 b l1
here  b here
 end`
  },
  {
    num: "08",
    title: "Sort 32-bit Numbers (Ascending / Descending)",
    type: "alp",
    code: ` area reg_1,data,readwrite
data1 dcd 00
 area reg_3,code,readonly
 export __main
__main
 mov r3,#4
 ldr r0,=array
 ldr r1,=data1
l1 ldr r2,[r0]
 str r2,[r1]
 add r0,#4
 add r1,#4
 sub r3,#1
 cmp r3,#0
 bne l1
 mov r1,#4
 mov r2,#3
 mov r6,r1
 mov r5,#0
l4 ldr r0,=data1
 mov r1,r6
 add r5,#1
 sub r1,r1,r5
l3 ldr r3,[r0]
 add r0,#4
 ldr r4,[r0]
 cmp r3,r4
 ble l2
 str r4,[r0,#-4]
 str r3,[r0]
l2 sub r1,#1
 cmp r1,#0
 bne l3
 sub r2,#1
 cmp r2,#0
 bne l4
here b here 
array dcd 0x51111111,0x42222222,0x33333333,0x24444444
 end`
  },
  {
    num: "09",
    title: "Count Ones and Zeros in Two Memory Locations",
    type: "alp",
    code: ` area reg_1, data, readwrite
result dcd 0000
 area reg_2, code, readonly
 export __main
__main
 ldr r0,=data
 mov r1,#0
 mov r4,#2
l2 mov r3,#32
 ldr r5,[r0]
up lsls r5,#01
 bcc l1
 add r1,#01
l1 sub r3,#01
 cmp r3,#0
 bne up
 add r0,#4
 sub r4,#1
 cmp r4,#0
 bne l2
 ldr R0,=result
 str r1,[r0]
 mov r3,#64
 sub r3,r3,r1
 str r3,[r0,#4]
here b here
data dcd 0x12340000,0x24680000
 end`
  },
  {
    num: "10",
    title: "Stepper Motor Control (LPC1768)",
    type: "c",
    code: `#include <lpc17xx.h>
unsigned char i;

void delay_ms(unsigned int ms)
{
    unsigned int i, j;
    for(i = 0; i < ms; i++)
        for(j = 0; j < 20000; j++);
}

int main()
{
    LPC_GPIO2->FIODIR = 0x0F;
    while(1)
    {
        LPC_GPIO2->FIOPIN = 0X03;
        delay_ms(1);
        LPC_GPIO2->FIOPIN = 0X09;
        delay_ms(1);
        LPC_GPIO2->FIOPIN = 0X0C;
        delay_ms(1);
        LPC_GPIO2->FIOPIN = 0X06;
        delay_ms(1);
    }
}`
  },
  {
    num: "11a",
    title: "DAC Triangular Wave (LPC1768)",
    type: "c",
    code: `#include <lpc17xx.h>
void GPIOINIT(void);
unsigned int j = 0;

int main(void)
{
    LPC_PINCON->PINSEL1 |= (1 << 21);
    while(1)
    {
        for(j = 0; j < 1023; j++)
        {
            LPC_DAC->DACR = j << 6;
        }
        for(j = 1023; j > 0; j--)
        {
            LPC_DAC->DACR = j << 6;
        }
    }
}`
  },
  {
    num: "11b",
    title: "DAC Square Wave (LPC1768)",
    type: "c",
    code: `#include <lpc17xx.h>

void delayms(unsigned int ms)
{
    unsigned int i, j;
    for(i = 0; i < ms; i++)
        for(j = 0; j < 20000; j++);
}

int main(void)
{
    LPC_PINCON->PINSEL1 |= (1 << 21);
    while(1)
    {
        LPC_DAC->DACR = 0x3FF << 6;
        delayms(1);
        LPC_DAC->DACR = 0x00 << 6;
        delayms(1);
    }
}`
  },
  {
    num: "12",
    title: "7-Segment LED Display Counter",
    type: "c",
    code: `#include <lpc17xx.h>

void delay_ms(unsigned int);
void Disp6Digi(unsigned char);
int display(unsigned char);

unsigned char disp[16] = {
    0xC0, 0xF9, 0xA4, 0xB0, 0x99, 0x92,
    0x82, 0xF8, 0x80, 0x90, 0x88, 0x83,
    0xC6, 0xA1, 0x86, 0x8E
};
unsigned char count = 0;

int main()
{
    LPC_GPIO2->FIODIR = 7;
    while(1)
    {
        Disp6Digi(count);
        count++;
        if(count > 15) count = 0;
        delay_ms(50);
    }
}

void Disp6Digi(unsigned char Disp)
{
    unsigned int x;
    for(x = 0; x < 6; x++)
        display(disp[Disp]);
}

void delay_ms(unsigned int ms)
{
    unsigned int i, j;
    for(i = 0; i < ms; i++)
        for(j = 0; j < 10000; j++);
}`
  },
  {
    num: "13",
    title: "Buzzer Control with Button (LPC1768)",
    type: "c",
    code: `#include <LPC17xx.h>

void delay_ms(unsigned int ms)
{
    unsigned int i, j;
    for(i = 0; i < ms; i++)
        for(j = 0; j < 500; j++);
}

void GPIOConfig(void)
{
    LPC_GPIO2->FIODIR = 0xFB;
    LPC_GPIO2->FIOPIN = 0xFB;
}

int main(void)
{
    int cnt;
    GPIOConfig();
    while(1)
    {
        if((LPC_GPIO2->FIOPIN & 4) != 4)
        {
            while(((LPC_GPIO2->FIOPIN & 4) != 4));
            delay_ms(100);
            cnt++;
            if(cnt > 2) cnt = 1;
        }
        if(cnt == 1) LPC_GPIO2->FIOPIN = 0x10;
        if(cnt == 2)
        {
            LPC_GPIO2->FIOPIN = 0x20;
            delay_ms(1);
            LPC_GPIO2->FIOPIN = 0x00;
            delay_ms(1);
        }
    }
}`
  }
];

let activeFilter = 'all';

function buildCard(item, idx) {
  const card = document.createElement('div');
  card.className = 'card';
  card.dataset.type = item.type;
  card.style.animationDelay = (idx * 0.05) + 's';

  card.innerHTML = `
    <div class="card-header">
      <div class="card-num">#${item.num}</div>
      <div class="card-title">${item.title}</div>
      <div class="type-badge type-${item.type}">${item.type === 'alp' ? 'ALP' : 'C'}</div>
    </div>
    <div class="code-wrap">
      <pre>${escHtml(item.code)}</pre>
    </div>
    <div class="card-footer">
      <button class="copy-btn" onclick="copyCode(this, ${idx})">
        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/>
        </svg>
        Copy Code
      </button>
    </div>
  `;
  return card;
}

function escHtml(s) {
  return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

function renderGrid() {
  const grid = document.getElementById('grid');
  grid.innerHTML = '';
  codes.forEach((item, idx) => {
    if (activeFilter === 'all' || item.type === activeFilter) {
      grid.appendChild(buildCard(item, idx));
    }
  });
}

function filter(type, btn) {
  activeFilter = type;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  renderGrid();
}

function copyCode(btn, idx) {
  navigator.clipboard.writeText(codes[idx].code.trim()).then(() => {
    btn.classList.add('copied');
    btn.innerHTML = `
      <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
        <polyline points="20 6 9 17 4 12"/>
      </svg>
      Copied!`;
    setTimeout(() => {
      btn.classList.remove('copied');
      btn.innerHTML = `
        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/>
        </svg>
        Copy Code`;
    }, 2000);
  });
}

function copyAll() {
  const all = codes.map(c => `// ${c.num}. ${c.title}\n${c.code.trim()}`).join('\n\n' + '//'.padEnd(70, '/') + '\n\n');
  navigator.clipboard.writeText(all).then(() => {
    const btn = document.querySelector('.copy-all-btn');
    btn.textContent = '✓ All Copied!';
    setTimeout(() => btn.textContent = '⬡ Copy All Programs', 2500);
  });
}

renderGrid();
</script>
</body>
</html>
