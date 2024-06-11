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

That's it! Your static site is now online!

# Telnet
## Script powershell

```powershell
# Fonction pour tester la connexion Telnet à une IP et un port spécifiques en utilisant Test-Port
function Test-Port {
  param (
    [string]$Computer,
    [int]$Port,
    [int]$Millisecond = 100,
    [string]$type
  )

  # Initialiser l'objet
  $Test = New-Object -TypeName Net.Sockets.TcpClient

  try {
    # Tenter la connexion avec un timeout de 100 millisecondes
    $result = ($Test.BeginConnect($Computer, $Port, $null, $null)).AsyncWaitHandle.WaitOne($Millisecond)
    if ($result) {
      Write-Output "--> ${Computer}:${Port} is open"
      if ($type){
        return "GOOD"
      }
    }
  } finally {
    # Cleanup
    $Test.Close()
  }
}

# Fonction pour tester une plage d'IP et une plage de ports en utilisant Test-Port
function Test-TelnetRange {
  param (
    [string]$StartIP,
    [string]$EndIP,
    [int]$StartPort,
    [int]$EndPort
  )

  $currentIP = [System.Net.IPAddress]::Parse($StartIP).GetAddressBytes()
  $endIP = [System.Net.IPAddress]::Parse($EndIP).GetAddressBytes()
  $connectedOnce = $false

  while ($true) {
    $currentIPString = ($currentIP -join '.')
    $connectionResult = "BAD"

    for ($port = $StartPort; $port -le $EndPort; $port++) {
      $connectionResult = Test-Port -Computer $currentIPString -Port $port -type 'any'
      if ($connectionResult -eq "GOOD") {
        $connectedOnce = $true
        Write-Output $connectionResult[0]
        break
      }
    }

    # Exit loop if no connection after the first successful one
    if ($connectedOnce -and !$allMax) {
      break
    }

    # Increment IP address
    $carry = $false
    for ($i = $currentIP.Length - 1; $i -ge 0; $i--) {
      if ($currentIP[$i] -lt 254) {
        $currentIP[$i]++
        $carry = $false
        break
      } else {
        $currentIP[$i] = 0
        $carry = $true
      }
    }

    # Exit loop if we overflowed (reached end IP)
    if ($carry) {
      break
    }
  }
}

# Exemple d'utilisation - cas 1
Test-TelnetRange -StartIP "8.8.4.1" -EndIP "8.8.4.5" -StartPort 50 -EndPort 53

# Exemple d'utilisation - cas 2
Test-Port -Computer "8.8.8.8" -Port 53
```







