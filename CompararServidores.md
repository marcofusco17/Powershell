```Powershell
#Generamos el txt

foreach ($directorio In Get-ChildItem -name D:\PROYECTOS) {
 Write-host $directorio 
du -nobanner "$directorio" | Out-File -FilePath D:\ocupacion3\"$directorio".txt 
}

#Comparamos el txt con el txt_old y lo guardamos 

cmd /C "for %i in (C:\Users\MAFUSCO\Desktop\x) do fc *.txt *.txt_old %i"  >> C:\Users\MAFUSCO\Desktop\s\comparar.txt

#Borramos todos los txt_old
Remove-Item -force d:\ocupacion\*.txt_old

#Renombraomos todos  los txt  a txt_old

Get-ChildItem d:\ocupacion\*.txt | Rename-Item -NewName { $_.Name -replace '.txt','.txt_old' }
---------------------------------------------------------------------------------------------------------------------------------
                         OTRA MANERA DE HACER ESTE SCRIPT 
---------------------------------------------------------------------------------------------------------------------------------

Así estas controlado lo que ocupa cada carpeta todos los viernes, se mete todo en un excel y que nos calcule la diferencia.



$day = Get-Date -Format dd
$month = Get-Date -Format MM
$year = Get-Date -Format yyyy
$destino = "D:\ocupacion\"
$origen = "D:\PROYECTOS\"
$resultados = "D:\ocupacion\Comparativa\"
#generamos el txt
foreach ($directorio In Get-ChildItem -name $origen) {
#Write-host $directorio
Add-Content -Value "###########################################" -Path "$destino$directorio.txt"
Add-Content -Value "Timestamp $day/$month/$year" -Path "$destino$directorio.txt"
Add-Content -Value "" -Path "$destino$directorio.txt"
#du -nobanner "$origen$directorio" | Out-File -FilePath "$destino$directorio.txt" -append
$size = du -nobanner "$origen$directorio"
Add-Content -Value $size -Path "$destino$directorio.txt"
}
```

