
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Program Copy Website</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #f2f2f2;
    padding: 20px;
}

h1 {
    text-align: center;
}

.program {
    background: #1e1e1e;
    color: #fff;
    border-radius: 8px;
    margin-bottom: 20px;
    padding: 15px;
    position: relative;
}

.program-title {
    font-size: 18px;
    margin-bottom: 10px;
    color: #4CAF50;
}

.copy-btn {
    position: absolute;
    top: 15px;
    right: 15px;
    background: #4CAF50;
    color: white;
    border: none;
    padding: 6px 12px;
    cursor: pointer;
    border-radius: 4px;
}

.copy-btn:hover {
    background: #45a049;
}

pre {
    margin: 0;
    overflow-x: auto;
}

code {
    font-family: Consolas, monospace;
    font-size: 14px;
}
</style>
</head>

<body>

<h1>Programming Examples</h1>

<!-- ===== PROGRAM 1 ===== -->
<div class="program">
    <div class="program-title">Program 1: Hello World</div>
    <button class="copy-btn" onclick="copyCode('code1', this)">Copy</button>
    <pre><code id="code1">
// Add your program code here
printf("Hello World");
    </code></pre>
</div>

<!-- ===== PROGRAM 2 ===== -->
<div class="program">
    <div class="program-title">Program 2: Addition</div>
    <button class="copy-btn" onclick="copyCode('code2', this)">Copy</button>
    <pre><code id="code2">
// Add your program code here
int a = 5, b = 10;
printf("%d", a + b);
    </code></pre>
</div>

<!-- ===== PROGRAMS 3 to 19 ===== -->
<!-- Just change program name and code -->

<div class="program"><div class="program-title">Program 3</div>
<button class="copy-btn" onclick="copyCode('code3', this)">Copy</button>
<pre><code id="code3">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 4</div>
<button class="copy-btn" onclick="copyCode('code4', this)">Copy</button>
<pre><code id="code4">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 5</div>
<button class="copy-btn" onclick="copyCode('code5', this)">Copy</button>
<pre><code id="code5">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 6</div>
<button class="copy-btn" onclick="copyCode('code6', this)">Copy</button>
<pre><code id="code6">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 7</div>
<button class="copy-btn" onclick="copyCode('code7', this)">Copy</button>
<pre><code id="code7">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 8</div>
<button class="copy-btn" onclick="copyCode('code8', this)">Copy</button>
<pre><code id="code8">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 9</div>
<button class="copy-btn" onclick="copyCode('code9', this)">Copy</button>
<pre><code id="code9">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 10</div>
<button class="copy-btn" onclick="copyCode('code10', this)">Copy</button>
<pre><code id="code10">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 11</div>
<button class="copy-btn" onclick="copyCode('code11', this)">Copy</button>
<pre><code id="code11">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 12</div>
<button class="copy-btn" onclick="copyCode('code12', this)">Copy</button>
<pre><code id="code12">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 13</div>
<button class="copy-btn" onclick="copyCode('code13', this)">Copy</button>
<pre><code id="code13">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 14</div>
<button class="copy-btn" onclick="copyCode('code14', this)">Copy</button>
<pre><code id="code14">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 15</div>
<button class="copy-btn" onclick="copyCode('code15', this)">Copy</button>
<pre><code id="code15">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 16</div>
<button class="copy-btn" onclick="copyCode('code16', this)">Copy</button>
<pre><code id="code16">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 17</div>
<button class="copy-btn" onclick="copyCode('code17', this)">Copy</button>
<pre><code id="code17">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 18</div>
<button class="copy-btn" onclick="copyCode('code18', this)">Copy</button>
<pre><code id="code18">// Your code here</code></pre></div>

<div class="program"><div class="program-title">Program 19</div>
<button class="copy-btn" onclick="copyCode('code19', this)">Copy</button>
<pre><code id="code19">// Your code here</code></pre></div>

<script>
function copyCode(codeId, button) {
    const codeText = document.getElementById(codeId).innerText;
    navigator.clipboard.writeText(codeText);
    button.innerText = "Copied!";
    setTimeout(() => button.innerText = "Copy", 2000);
}
</script>

</body>
</html>
