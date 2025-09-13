<h1>🪟 DWM Low PID Brute-Forcer</h1>

<p>This batch script repeatedly restarts <strong>Desktop Window Manager (<code>dwm.exe</code>)</strong> until it gets a <strong>PID below 1000</strong>.</p>

<p>Some tools or hacks work best (or only) when <code>dwm.exe</code> runs with a low PID. Since Windows doesn’t let you reserve or assign PIDs, this script brute-forces the outcome.</p>

<hr>

<h2>⚠️ Disclaimer</h2>

<ul>
  <li>This script kills and restarts <code>dwm.exe</code>, which controls your Windows UI.</li>
  <li>Use only if you understand the risk of screen flickers or temporary instability.</li>
  <li><strong>Run as Administrator.</strong></li>
</ul>

<hr>

<h2>📄 How It Works</h2>

<ol>
  <li>Kills <code>dwm.exe</code></li>
  <li>Restarts it</li>
  <li>Checks its PID</li>
  <li>Repeats every 10 seconds until the PID is between <code>1–999</code></li>
</ol>

<hr>

<h2>🧰 Requirements</h2>

<ul>
  <li>Windows 10 or 11</li>
  <li>Admin privileges</li>
  <li>Command Prompt (<code>.bat</code> file)</li>
</ul>

<hr>

<h2>💻 Script</h2>

<pre><code>@echo off
setlocal enabledelayedexpansion

echo Restarting Desktop Window Manager until PID &lt; 1000...

:RESTART
taskkill /f /im dwm.exe &gt;nul 2&gt;&amp;1
echo Waiting for DWM to stop...
timeout /t 5 /nobreak &gt;nul

start "" "%SystemRoot%\System32\dwm.exe"
echo Waiting for DWM to start...
timeout /t 5 /nobreak &gt;nul

for /f "tokens=2" %%a in ('tasklist /fi "imagename eq dwm.exe" ^| findstr "dwm.exe"') do (
    set PID=%%a
)

set PID=!PID:,=!
echo Found PID: !PID!

if !PID! lss 1000 (
    echo Success: dwm.exe is running with PID !PID!
    goto END
) else (
    echo PID !PID! is too high, retrying in 10 seconds...
    timeout /t 10 /nobreak &gt;nul
    goto RESTART
)

:END
pause
</code></pre>

<hr>

<h2>🕒 Typical Runtime</h2>

<p>⏱ 2–10 minutes depending on system load and timing.</p>
