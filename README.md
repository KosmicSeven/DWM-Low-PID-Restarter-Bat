<h1>🪟 DWM Low PID Brute-Forcer</h1>

<p>This batch script repeatedly restarts <strong>Desktop Window Manager (<code>dwm.exe</code>)</strong> until it gets a <strong>PID below 1000</strong>.</p>

<hr>

<h2>🔄️ Run Script</h2>

<ol>
  <li><strong>Run as Administrator.</strong></li>
  <li>Let CMD run. The script logs each restart and PID attempt.</li>
  <li>It stops automatically once <code>dwm.exe</code> has a PID between <strong>1–999</strong>.</li>
</ol>

<hr>

<h2>📄 How It Works</h2>

<ol>
  <li>Kills <code>dwm.exe</code></li>
  <li>Restarts it</li>
  <li>Checks the new PID</li>
  <li>Repeats every 10 seconds until PID is <code>&lt; 1000</code></li>
</ol>

<hr>

<h2>⚠️ Disclaimer</h2>

<ul>
  <li>This script restarts <code>dwm.exe</code>, which manages the Windows desktop environment.</li>
  <li>You may experience flickering or momentary instability during the process.</li>
  <li>If <code>dwm.exe</code> fails to restart (unlikely), rebooting the system will restore it.</li>
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

<p>⏱ 2–10 minutes depending on PID assignment timing.</p>
