# Windows Firewall Analyser
## Script powershell

```powershell
Need to correct this part
```

# Deploying a Static Website with Surge CLI on Windows
## Node.js

Step 1: Install Surge CLI

First, ensure you have [Node.js](https://nodejs.org/) installed. Then, open your command prompt and install Surge globally:

```bash
npm install -g surge
```
Step 2: Prepare Your HTML Folder

Create a folder named <b>html</b> on your desktop containing your website files:

```
C:\Users\YourName\Desktop\html
    ├── index.html
    ├── about.html
    └── styles.css
```

Step 3: Deploy Your Site

Navigate to your <b>html</b> folder in the command prompt:

```
cd C:\Users\YourName\Desktop\html
```

Deploy your site with Surge:

```
surge
```

Follow the instructions.

> Quickly, your static site will be online! 🚀

Step 4: Update Your Site

To update your site, modify the files in your <b>html</b> folder and run:

```
surge
```

Surge will update the deployment.

Step 5: Remove Your Site

To remove your site, run:

```
surge teardown example.surge.sh
```
Replace example.surge.sh with your domain.

# Telnet
## Script powershell

```powershell
function Test-Port {

  param (
    [string]$Computer,
    [int]$Port,
    [int]$Millisecond = 100,
    [boolean]$multiple = $false
  )

  $Test = New-Object -TypeName Net.Sockets.TcpClient

  try {

    # Attempting to connect with a timeout of 100 milliseconds

    $result = ($Test.BeginConnect($Computer, $Port, $null, $null)).AsyncWaitHandle.WaitOne($Millisecond)
    if ($result) {
      
      Write-Output "--> ${Computer}:${Port} is open"
      
      if ($multiple){
        return "GOOD"
      }

    }

  } finally {
    # Cleanup
    $Test.Close()
  }
}

function Test-TelnetRange {
  param (
    [string]$StartIP,
    [string]$EndIP,
    [int]$StartPort,
    [int]$EndPort
  )

  $currentIP = [System.Net.IPAddress]::Parse($StartIP).GetAddressBytes()
  $endIPBytes = [System.Net.IPAddress]::Parse($EndIP).GetAddressBytes()

  # Assuming comparison should be on the last byte of IPv4 addresses
  $startNumber = $currentIP[3]
  $endNumber = $endIPBytes[3]

  if ($startNumber -eq 0 -or $endNumber -eq 0){
    Write-Error("--> Zero is not allowed as last byte!")
    exit
  }

  if ($startNumber -gt 254 -or $endNumber -gt 254){
    Write-Error("--> 254 is the max for the last byte!")
    exit
  }

  if ($startNumber -gt $endNumber) {
      Write-Error("--> Start IP is higher than EndIP!")
      exit
  }

  for ($i = 0; $i -lt 3; $i++) {
    if ($currentIP[$i] -ne $endIPBytes[$i]) {
      Write-Error("--> Class C only!")
      exit
    }
  }

  while ($true) {
    $currentIPString = ($currentIP -join '.')
    $connectionResult = "BAD"

    # Ports stuff
    for ($port = $StartPort; $port -le $EndPort; $port++) {
      $connectionResult = Test-Port -Computer $currentIPString -Port $port -multiple $true
      if ($connectionResult -eq "GOOD") {
        Write-Output $connectionResult[0]
      }
    }

    # IP stuff
    if ($startNumber -eq $endnumber){
        exit
    }

    $startNumber++

    $currentIP[3] = $startNumber 

  }
}

# Sample, case 1
Test-Port -Computer "8.8.8.8" -Port 53

# Sample, case 2
Test-TelnetRange -StartIP "8.8.4.2" -EndIP "8.8.4.8" -StartPort 50 -EndPort 55
```







