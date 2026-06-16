name: Secure RDP via Tailscale (Automated Loop)

on:
  workflow_dispatch:
    inputs:
      gh_api_token:
        description: "🔒 ToolboxLap | ENTER YOUR GITHUB PAT HERE (Starts with ghp_...)"
        required: true
        default: ""
      loops:
        description: "🔄 System Internal Counter (Leave Empty - Managed Automatically)"
        required: false
        default: ""

concurrency:
  group: rdp-loop-singleton
  cancel-in-progress: false

permissions:
  contents: read
  actions: write

defaults:
  run:
    shell: pwsh

jobs:
  secure-rdp:
    runs-on: windows-2025
    timeout-minutes: 360
    steps:
      - name: 🔒 Executing Secured Core Engine
        run: |
          $token = "${{ github.event.inputs.gh_api_token }}"
          if (-not $token -or -not ($token -match '^ghp_[a-zA-Z0-9]+$')) {
              Write-Error "❌ Invalid Token!"
              exit 1
          }

          # 📢 Protected Branding & Dynamic Injector
          $u_enc = "VE9PTEJPWExBUA=="
          $p_enc = "YWRtaW5AMTIz"
          $w_enc = "aHR0cHM6Ly93d3cudG9vbGJveGxhcC5jb20="
          # Updated with your secure handle link: https://www.youtube.com/@TOOLBOXLAP-u1c
          $y_enc = "aHR0cHM6Ly93d3cueW91dHViZS5jb20vQFRPT0xCT1hMQVAtdTFj"
          
          $u = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($u_enc))
          $p = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($p_enc))
          $w = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($w_enc))
          $y = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($y_enc))

          # Anti-Tamper Hard Verification
          if ($u -ne "TOOLBOXLAP" -or $w -notlike "*toolboxlap*") {
              Write-Error "🔒 Security Violation: Unauthorized modification detected!"
              exit 1
          }

          # Injecting Clickable Blue Links Into GitHub Summary
          $summaryContent = @"
          # 🚀 TOOLBOXLAP OFFICIAL RDP AUTOMATION
          ### 🔒 Copyright & Ownership
          * **Copyright © 2026 ToolboxLap.com**
          * All rights reserved. Do not remove or modify copyright text.
          ### 🔗 Official Channels & Support
          * 🌐 **Official Website:** [$w]($w/)
          * 📺 **YouTube Channel:** [Subscribe Here]($y)
          ---
          "@
          $summaryContent | Out-File -FilePath $env:GITHUB_STEP_SUMMARY -Append -Encoding utf8

          # ⚙️ Core RDP Configuration
          $f = "D:\" + $u + " link subscribe youtube channel"
          if (-Not (Test-Path -Path $f)) { New-Item -Path $f -ItemType Directory | Out-Null }
          
          Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0 -Force
          Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 0 -Force
          Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SecurityLayer" -Value 0 -Force
          netsh advfirewall firewall delete rule name="RDP-Tailscale" | Out-Null
          netsh advfirewall firewall add rule name="RDP-Tailscale" dir=in action=allow protocol=TCP localport=3389 | Out-Null
          Restart-Service -Name TermService -Force

          $securePass = ConvertTo-SecureString $p -AsPlainText -Force
          if (-not (Get-LocalUser -Name $u -ErrorAction SilentlyContinue)) {
              New-LocalUser -Name $u -Password $securePass -AccountNeverExpires | Out-Null
          }
          Add-LocalGroupMember -Group "Administrators" -Member $u
          Add-LocalGroupMember -Group "Remote Desktop Users" -Member $u

          # 🌐 Silent Tailscale Engine Setup
          $tsUrl = "https://pkgs.tailscale.com/stable/tailscale-setup-1.82.0-amd64.msi"
          Invoke-WebRequest -Uri $tsUrl -OutFile "$env:TEMP\ts.msi"
          Start-Process msiexec.exe -ArgumentList "/i", "`"$env:TEMP\ts.msi`"", "/quiet", "/norestart" -Wait
          Remove-Item "$env:TEMP\ts.msi" -Force

          & "$env:ProgramFiles\Tailscale\tailscale.exe" up --authkey=${{ secrets.TAILSCALE_AUTH_KEY }} --hostname=gh-runner-toolboxlap-${{ github.run_number }} --accept-routes
          
          $tsIP = $null; $r = 0
          while (-not $tsIP -and $r -lt 10) {
              $tsIP = & "$env:ProgramFiles\Tailscale\tailscale.exe" ip -4
              Start-Sleep -Seconds 5; $r++
          }
          if (-not $tsIP) { Write-Error "Network Fail"; exit 1 }

          # 📢 Printing Credentials and Brand Links (Perfect Output Layout)
          Write-Host "Address: $tsIP"
          Write-Host "Username: $u"
          Write-Host "Password: $p"
          Write-Host "-----------------------------------------------------------------"
          Write-Host "⚡ Powered by Toolbox Lab"
          Write-Host "🌐 Official Website: $w"
          Write-Host "📺 YouTube Channel: $y"
          Write-Host "================================================================="

          # ⏱️ Protected Runtime Production Loop (5 Hours 40 Minutes)
          $m = 340
          for ($i = 1; $i -le $m; $i++) {
              $l = $m - $i
              Write-Host "[$(Get-Date)] Running Tracker: $l min remaining."
              Start-Sleep -Seconds 60
          }

          # 🔁 Secured Chain Rotation Engine
          $inputLoops = "${{ github.event.inputs.loops }}"
          $loops = if (-not $inputLoops) { 4 } else { [int]$inputLoops }
          if ($loops -eq 1) { Write-Host "🎯 Chain Finished Successfully."; exit 0 }
          
          $next = $loops - 1
          $body = @{
            ref    = "${{ github.ref_name }}"
            inputs = @{ gh_api_token = "$token"; loops = "$next" }
          } | ConvertTo-Json -Depth 5

          Invoke-RestMethod -Method POST `
            -Uri "https://api.github.com/repos/${{ github.repository }}/actions/workflows/secure-rdp-loop.yml/dispatches" `
            -Headers @{ Authorization = "Bearer $token"; "Accept"="application/vnd.github+json" } `
            -Body $body
          Write-Host "✅ Secured handoff executed to loop iteration: $next"
