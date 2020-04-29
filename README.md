# Habilitar la virtualización en ACER E15-553-1786 para windows y linux

### El problema
* La serie de Acer E15 no tiene una opción para habilitar la virtualización amd-v desde el BIOS

### La solución
* Parchar la BIOS con las opciones de hipervirtualización activada. 
* Esto debe hacerse forzosamente desde un sistema windows, pero al hacerse se preservan los cambios en la configuración BIOS por lo que en Linux también estará activada la hipervirtualización.

### Procedimiento (uwu)
1. Apagar windows defender
    * Abrir una consola powershell con privilegios de administrador
        * Puedes hacerlo con (WINDOWS+X+A) o cualquier otro
    * Ejecutar
        * ``Set-MpPreference -DisableRealtimeMonitoring $true``
        * Nota, ya no funciona. Debe hacerse manualmente el apagar Windows Defender.
2. Descargar los archivos desde https://github.com/elcaza/acer_e15_amd-v/ 
3. Con powershell (Con privilegios de administrador) situarse en la carpeta donde está el archivo H2OUVE.exe
4. Sacar la configuración del BIOS actual
    * Posicionado en la ubicación de **H2OUVE.exe** ejecutar 
    * ``.\H2OUVE.exe -gv bios.txt``
    * Y eso nos entregará un **bios.txt** con la configuración de la BIOS
5. Modificamos el archivo **bios.txt** que ahora tenemos en la misma ruta. 
    * Para esto se recomienda usar un editor de texto como:
        * Visual Studio Code, Sublime Text, Atom ó Notepad ++
    * Buscamos la línea que diga **setup** (Ctrl+f) (o su equivalente para buscar) .
    * En mi caso (Pero va a diferir en cada máquina)
    * ``` powershell 
       [04B] "Setup"
        GUID: A04A27F4-DF00-4D42-B552-39511302113D
        Attributes: 0x7
        DataSize: 0x279
        Data:
    * Modificamos la línea **con terminación E0 de la sección setup**
        * [image] https://github.com/elcaza/acer-e15-amd-v/blob/master/images/Screenshot_2.png
        * En el penúltimo número que actualmente tiene un valor de **00** lo debemos sustituir por un **01**
    * Modificamos la línea **con terminación 140 de la sección setup**
        * [image] https://github.com/elcaza/acer-e15-amd-v/blob/master/images/Screenshot_3.png
        * En el antepenúltimo número que actualmente tiene un valor de **01** lo debemos sustituir por un **00**
    * Guardamos los cambios en el documento
6. Cargamos la nueva configuración en la BIOS
    * Desde el powershell de administrador ejecutamos 
    * ``.\H2OUVE.exe -sv bios.txt``
    * Nos mostrará en consola varios mensajes diciendo que ha tenido éxito el parche (Podría mandar uno que otro error del que no tenemos que preocuparnos)
7. Reiniciamos la computadora y disfrutamos de nuestra hipervirtualización activada
8. Activar nuevamente windows defender
    * `Set-MpPreference -DisableRealtimeMonitoring $false`
9. Recuerda siempre romper el sistema

### Preguntas frecuentes
* ¿En qué modelo se está probando?
    * ACER E15-553-1786 con windows 10
* ¿Es seguro?
    * Nada es seguro en este mundo, pero ha resultado efectivo siempre y cuando sigas las instrucciones

### Referencias y agradecimientos a
* https://www.youtube.com/watch?v=SVK943_upho
* https://howtoscomos.blogspot.com/2017/12/no-amd-v-on-a9-9410-acer-e5-523g-958x.html
* https://www.geektech.co.nz/how-to-unlock-the-nvme-performance-on-the-lenovo-y700
* https://www.bios-mods.com/forum/Thread-REQUEST-Acer-Aspire-E5-523-G-BIOS-Unlock?page=2