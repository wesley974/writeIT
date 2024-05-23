# Windows Firewall Analyser
## Script powershell

```powershell
# Définir le chemin du journal de pare-feu
$logFilePath = "C:\Windows\System32\LogFiles\Firewall\pfirewall.log"

# Vérifier si le fichier de log existe
if (Test-Path $logFilePath) {
    Write-Host "Surveillance du fichier de log : $logFilePath"
} else {
    Write-Host "Le fichier de log n'existe pas : $logFilePath"
    exit
}

# Lire le fichier de log en temps réel et filtrer les paquets bloqués
Get-Content $logFilePath -Wait | Where-Object { $_ -match "DROP" } | ForEach-Object {
    $logEntry = $_
    $fields = $logEntry -split "\s+"
    $timestamp = $fields[0] + " " + $fields[1]
    $action = $fields[2]
    $protocol = $fields[3]
    $srcIP = $fields[4]
    $destIP = $fields[5]
    $srcPort = $fields[6]
    $destPort = $fields[7]
    Write-Host "[$timestamp] Paquet bloqué - Action: $action, Protocole: $protocol, Src: $srcIP:$srcPort, Dest: $destIP:$destPort"
}
```

