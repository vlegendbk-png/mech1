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

<!-- Program Template -->

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
MOV R2,#9
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

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>2. Multiply Two 16-bit Numbers</h3>
<pre>
AREA reg_1,DATA,READWRITE
result DCD 0
AREA reg_2,CODE,READONLY
EXPORT __main
__main
LDR R2, =2_1010101111001101
LDR R1, =2_0001001000110100
MUL R1,R1,R2
LDR R2,=result
STR R1,[R2]
HERE B HERE
END
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>3. Add Two 64-bit Numbers</h3>
<pre>
AREA reg_1,DATA,READWRITE
result DCD 0
AREA reg_3,CODE,READONLY
EXPORT __main
__main
LDR R0,=0x1234E640
LDR R1,=0x43210010
LDR R2,=0x12348900 
LDR R3,=0x43212102 
ADD R4,R1,R3 
ADD R5,R0,R2
HERE B HERE
END
</pre>
</div>

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
SUB R1,#1 
CMP R1,#0
BNE L1
LDR R3,=fact
STR R2,[R3]
HERE B HERE
END
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>5. Add Array (16-bit)</h3>
<pre>
AREA reg_1,DATA,READWRITE
sum DCD 0
AREA reg_3,CODE,READONLY
EXPORT __main
__main
MOV R0,#4
MOV R1,#0
LDR R3,=array
L1 LDRH R2,[R3]
ADD R1,R1,R2
ADD R3,#2
SUB R0,#1
CMP R0,#0
BNE L1
LDR R5,=sum
STR R1,[R5]
HERE B HERE
array DCW 0x1111,0x2222,0x3333,0x4444
END
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>6. Square using Lookup Table</h3>
<pre>
AREA reg_1,DATA,READONLY
table DCB 1,4,9,16,25,36,49,64,81,100
AREA reg_2,DATA,READWRITE
result DCB 0
AREA reg_3,CODE,READONLY
EXPORT __main
__main
LDR R2,=table
MOV R1,#8 
ADD R2,R1
LDRB R5,[R2]
LDR R4,=result
STRB R5,[R4]
HERE B HERE
END
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>7. Largest Number in Array</h3>
<pre>
AREA reg_2,CODE,READONLY
array DCD 0x11111111,0x22222222,0x53333333,0x44444444
AREA reg_3,CODE,READONLY
EXPORT __main
__main
LDR R0,=array
LDR R2,[R0]
MOV R4,#4
MOV R1,R2
L1 ADD R0,#4
LDR R3,[R0]
SUB R4,#1
CMP R4,#0
BEQ HERE
CMP R1,R3
BGE L1
MOV R1,R3
B L1
HERE B HERE
END
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>8. Sorting</h3>
<pre>
AREA reg_1,DATA,READWRITE
data1 DCD 0
AREA reg_3,CODE,READONLY
EXPORT __main
__main
MOV R3,#4
LDR R0,=array
LDR R1,=data1
L1 LDR R2,[R0]
STR R2,[R1]
ADD R0,#4
ADD R1,#4
SUB R3,#1
CMP R3,#0
BNE L1
HERE B HERE
array DCD 0x51111111,0x42222222,0x33333333,0x24444444
END
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>9. Count Ones & Zeros</h3>
<pre>
AREA reg_1,DATA,READWRITE
result DCD 0
AREA reg_2,CODE,READONLY
EXPORT __main
__main
LDR R0,=data
MOV R1,#0
MOV R4,#2
L2 MOV R3,#32
LDR R5,[R0]
UP LSLS R5,#1
BCC L1
ADD R1,#1
L1 SUB R3,#1
CMP R3,#0
BNE UP
ADD R0,#4
SUB R4,#1
CMP R4,#0
BNE L2
HERE B HERE
data DCD 0x12340000,0x24680000
END
</pre>
</div>

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
LPC_GPIO2-&gt;FIODIR = 0x0F;
while(1){
LPC_GPIO2-&gt;FIOPIN = 0X03;
delay_ms(1);
LPC_GPIO2-&gt;FIOPIN = 0X09;
delay_ms(1);
LPC_GPIO2-&gt;FIOPIN = 0X0C;
delay_ms(1);
LPC_GPIO2-&gt;FIOPIN = 0X06;
delay_ms(1);
}}
</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>11. DAC Triangle Wave</h3>
<pre>
  #include <lpc17xx.h>
void GPIOINIT(void);
unsigned int j=0;
int main(void)
{
LPC_PINCON->PINSEL1 |= (1<<21);
while(1)
{
for (j=0;j<1023;j++)
{
LPC_DAC->DACR = j<<6;
}
for (j=1023;j>0;j--)
{
LPC_DAC->DACR = j<<6;
}}}


</pre>
</div>



<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>12. DAC Square Wave</h3>
<pre>
#include <lpc17xx.h>
void delayms(unsigned int);
int main(void)
{
LPC_PINCON->PINSEL1 |= (1<<21);
while(1)
{
LPC_DAC->DACR = 0x3FF<<6;
delayms(1);
LPC_DAC->DACR = 0x00<<6;
delayms(1);
}}
void delayms(unsigned int ms)
{
unsigned int i,j;
for (i=0;i<ms;i++)
for(j=0;j<20000;j++);
}


</pre>
</div>

<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>13. Buzzer</h3>
<pre>
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
<div class="program">
<button onclick="copyCode(this)">Copy</button>
<h3>14.LED</h3>
<pre>
#include <lpc17xx.h>
void delay_ms(unsigned int);
void convert_display(unsigned int);
void Disp6Digi (unsigned char);
int display(unsigned char);
unsigned char disp[16]
={0xC0,0xF9,0xA4,0xB0,0x99,0x92,0x82,0xF8,0x80,0x90,0x88,0x83,0xC6,0xA1,0x86,0x8E};
unsigned char count = 0;
int main()
{
LPC_GPIO2->FIODIR = 7;
while(1)
{
Disp6Digi(count);
count++;
if(count>15) count=0;
delay_ms(50);
}}
void Disp6Digi (unsigned char Disp)
{
unsigned int x;
for (x=0;x<6;x++)
{
display(disp[Disp]);
}}
void delay_ms(unsigned int ms)
{
unsigned int i,j;
for(i=0;i<ms;i++)
for(j=0;j<10000;j++);
}

</pre>
</div>

<script>
function copyCode(btn){
    const code = btn.nextElementSibling.nextElementSibling.innerText;
    navigator.clipboard.writeText(code);
    alert("Copied!");
}

function copyAll(){
    const text = document.getElementById("allPrograms").innerText;
    navigator.clipboard.writeText(text);
    alert("All programs copied!");
}
</script>

</body>
</html>
