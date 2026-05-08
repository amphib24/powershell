## PowerShell Port - Scanner Top 50 Common TCP Ports

## Description
This is a PowerShell script I wrote during a live lab during my Pentest+ studies to gain a better understanding of how port scanners work under the hood. The port scanner scans the top 50 commonly used TCP ports on a target host to identify 
open service and help assess potential attack surface exposure.
This was designed for educational purposes only and is not intended for illegal or malicious use of any kind.

## Features
   - Scans top 50 common TCP ports
   - Identifies open ports on a target host
   - Displays real-time scan results
   - Lightweight PowerShell-based implementation


```powershell
#set top 50 ports(to keep the scan short)

$ports = @(21,22,23,25,53,67,68,80,110,111,123,
135,139,143,161,389,443,445,465,587,636,993,995,1080,1433,1521,1723,3306,3389,5432,5900,6379,8000,8080,8443,8888,9000,10000,11211,27017,50000,49152,49153,49154,49155,49156)

# set target and create array for scan results
$ip = Read-Host -Prompt "Enter the IP address you want to scan"
$openPortResults = @()



foreach($port in $ports){
   try{
      #creates a new TCP client
      $client = [System.Net.Sockets.TcpClient]::new()

      #try and connect asynchronously
      $connectionAttempt = $tcp.ConnectAsync($ip, $port)
     
      #set time for waiting for response
      if ($connectionAttempt.Wait(500)) {
         if ($client.Connected) {
             $openPortsResults += $port
             }
      }
      $client.Close()
  }
  catch{Write-Host "$port failed to connect"
  }
}

#print results
if ($openPortsResults.Count -gt 0){
    Write-Host "Open Ports on ${ip}:"
    $openPortsResults | Sort-Object
}
else{
   Write-Host "Looks like no ports are open!"
}
```
## Detection Considerations
From a defensive standpoint, repeated port scanning activity may indicate:
   - Reconnaissance attempts
   - Automated scanning tools
   - Early-stage intrusion activity
This activity can be monitored and/ or detected using firewall logs, IDS/IPS, or SIEM solutions.
   
