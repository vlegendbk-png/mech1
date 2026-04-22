<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ALP & Embedded Programs</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #f4f6f8;
    padding: 20px;
}
h1 {
    text-align: center;
}
.program {
    background: #1e1e1e;
    color: #dcdcdc;
    padding: 15px;
    margin: 20px 0;
    border-radius: 10px;
    position: relative;
}
button {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 5px 10px;
    border: none;
    background: #4CAF50;
    color: white;
    cursor: pointer;
    border-radius: 5px;
}
button:hover {
    background: #45a049;
}
.copy-all {
    display: block;
    margin: 20px auto;
    padding: 10px 20px;
}
pre {
    white-space: pre-wrap;
}
</style>
</head>

<body>

<h1>ALP & Embedded Programs</h1>
<button class="copy-all" onclick="copyAll()">Copy All Programs</button>

<div id="allPrograms">

<!-- Program 1 -->
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>1. Sum of First 10 Integers</h3>
<pre>
AREA reg2, DATA, READWRITE
result DCD 0
AREA reg1, CODE, READONLY
EXPORT __main
__main
MOV R0,#0
MOV R2,#10
MOV R1,#1
UP ADD R0,R0,R1
ADD R1,#1
SUB R2,#1
CMP R2,#0
BNE UP
LDR R1,=result
STR R0,[R1]
HERE B HERE
END
</pre>
</div>

<!-- Program 2 FIXED -->
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>2. Multiply Two Numbers</h3>
<pre>
AREA reg_1,DATA,READWRITE
result DCD 0
AREA reg_2,CODE,READONLY
EXPORT __main
__main
LDR R2, =0xABCD
LDR R1, =0x1234
MUL R1,R1,R2
LDR R2,=result
STR R1,[R2]
HERE B HERE
END
</pre>
</div>

<!-- Program 4 FIXED -->
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>4. Factorial</h3>
<pre>
AREA reg_2,DATA,READWRITE
fact DCD 0
AREA reg_3,CODE,READONLY
EXPORT __main
__main
MOV R1,#5  
MOV R2,#1
L1 MUL R2,R2,R1
SUB R1,R1,#1 
CMP R1,#0
BNE L1
LDR R3,=fact
STR R2,[R3]
HERE B HERE
END
</pre>
</div>

<!-- Program 10 -->
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>10. Stepper Motor</h3>
<pre>
#include &lt;lpc17xx.h&gt;

void delay_ms(unsigned int ms){
    unsigned int i,j;
    for(i=0;i&lt;ms;i++)
        for(j=0;j&lt;20000;j++);
}

int main(){
    LPC_GPIO2->FIODIR = 0x0F;

    while(1){
        LPC_GPIO2->FIOPIN = 0x03;
        delay_ms(1);
        LPC_GPIO2->FIOPIN = 0x09;
        delay_ms(1);
        LPC_GPIO2->FIOPIN = 0x0C;
        delay_ms(1);
        LPC_GPIO2->FIOPIN = 0x06;
        delay_ms(1);
    }
}
</pre>
</div>

<!-- Program 11 FIXED -->
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>11. DAC Triangle Wave</h3>
<pre>
#include &lt;lpc17xx.h&gt;

int main(void)
{
    unsigned int j;
    LPC_PINCON->PINSEL1 |= (1<<21);

    while(1)
    {
        for (j=0;j<1023;j++)
            LPC_DAC->DACR = j<<6;

        for (j=1023;j>0;j--)
            LPC_DAC->DACR = j<<6;
    }
}
</pre>
</div>

<!-- Program 13 FIXED -->
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>13. Buzzer</h3>
<pre>
#include &lt;LPC17xx.h&gt;

void delay_ms(unsigned int ms)
{
    unsigned int i,j;
    for(i=0;i&lt;ms;i++)
        for(j=0;j&lt;500;j++);
}

void GPIOConfig(void)
{
    LPC_GPIO2->FIODIR = 0xFB;
    LPC_GPIO2->FIOPIN = 0xFB;
}

int main(void)
{
    int cnt = 0;
    GPIOConfig();

    while(1)
    {
        if((LPC_GPIO2->FIOPIN & 4) != 4)
        {
            while((LPC_GPIO2->FIOPIN & 4) != 4);
            delay_ms(100);
            cnt++;
            if(cnt>2) cnt=1;
        }

        if(cnt==1) LPC_GPIO2->FIOPIN=0x10;

        if(cnt==2)
        {
            LPC_GPIO2->FIOPIN=0x20;
            delay_ms(1);
            LPC_GPIO2->FIOPIN=0x00;
            delay_ms(1);
        }
    }
}
_____BUZZER---
#include<LPC17xx.h>
void delay_ms(unsigned int ms)
{
unsigned int i,j;
for(i=0;i<ms;i++)
for(j=0;j<500;j++);
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
if((LPC_GPIO2->FIOPIN&4)!=4)
{
while(((LPC_GPIO2->FIOPIN&4)!=4));
delay_ms(100);
cnt++;
if(cnt>2) cnt=1;
}
if(cnt==1) LPC_GPIO2->FIOPIN=0x10;
if(cnt==2)
{LPC_GPIO2->FIOPIN=0x20;
delay_ms(1);
LPC_GPIO2->FIOPIN=0x00;
delay_ms(1);
}
}
}


</pre>
</div>

</div>

<script>
function copyCode(btn){
    const code = btn.parentElement.querySelector("pre").textContent;
    navigator.clipboard.writeText(code)
        .then(() => alert("Copied!"));
}

function copyAll(){
    const text = document.getElementById("allPrograms").textContent;
    navigator.clipboard.writeText(text)
        .then(() => alert("All programs copied!"));
}
</script>

</body>
</html>
