# Pasos para crear un Template de Windows 10 para VM

Se debe partir de una instalación limpia de Windows 10. Ademas debe estar actualizada a la fecha.

## Antes de comenzar
#### Paso 1
Para que la herramienta funcione se debe ejecutar el siguiente comando. Este comando hace que se borren las aplicaciones conflictivas del Widnows. Se debe abrir el PowerShell de Windows 10 como administrador.
```
# Import-Module appx
# Import-Module dism
```
#### Paso 2: Luego desinstalaremos todas las aplicaciones Appx sobrantes ejecutando el siguiente comando:
```
Get-AppxPackage | Remove-AppxPackage
```
>**Nota:** Durante las tareas de desinstalación aparecerán algunos errores debido a que algunas Apps como podría ser Edge, Framework o Cortana no pueden ser desinstaladas. Sin embargo, terminado el proceso ya no aparecerá el mensaje de error al  ejecutar Sysprep /Generalize.

## Ahora si, manos a la obra. Windows 10: Cambiar SID con SYSPREP.
El comando sysprep /generalize eliminará información única de nuestra instalación de Windows y nos permitirá que podamos desplegar la imagen sin problemas en tantos equipos distintos como queramos.

Cualquier método que usemos para el traslado de una imagen de Windows a un equipo nuevo, bien sea mediante la creación de imagen completa, la duplicación de disco duro u otro cualquiera deberemos preparar previamente el sistema operativo mediante el modificador /generalize.

### Limitacines de SYSREP
Según Microsoft, podemos ejecutar Sysprep tantas veces como sea necesario para conseguir crear y configurar nuestra instalación de Windows. Sin embargo, sólo podemos restablecer tres veces la activación de Windows.
* Sólo podemos usar la versión de Sysprep que viene instalada con la imagen de Windows que queremos configurar. Sysprep se instalará por defecto en el directorio %WINDIR%\system32\sysprep.
* Sysprep no puede ser utilizado en instalaciones de Windows que sean del tipo de actualización. Solo podremos ejecutar Sysprep en instalaciones de sistemas operativos limpias.
* Sólo  ejecutaremos Sysprep si el equipo es miembro de un grupo de trabajo, no debemos usar Sysprep en equipos que formen parte de un dominio de Active Directory. Si el equipo está unido a un dominio de Active Directory, Sysprep podría eliminarlo del dominio.
* Si ejecutamos Sysprep en una partición con el sistema con archivos NTFS que contenga archivos o carpetas cifrados, los datos que contengan dichas carpetas quedarán completamente ilegibles e irrecuperables.

#### Paso 1:
Para empezar, descargaremos la herramienta PsGetsid.exe de Sysinternals, para ver en SID original de nuestro equipo, podemos descargar PsGetsid en el enlace siguiente:
https://technet.microsoft.com/en-us/sysinternals/bb897417.aspx

#### Paso 2:
Una vez descargado el archivo PsTools, lo descomprimiremos en nuestro equipo y desde una ventana de CMD ejecutaremos:
```
PsGetsid.exe
```
Nos aparecerá una nueva ventana emergente con los términos de la licencia de usuario final, pulsaremos el botón Aceptar para aceptar los términos y cerrar la ventana.

Seguidamente nos aparecerá en pantalla el nombre de nuestro equipo y el SID.

#### Paso 3:
Cambiaremos al directorio c:\windows\system32\sysprep.

Ejecutaremos el comando:
```
Sysprep 
```
Nos aparecerá la ventana principal del producto. 
En el menú desplegable de la sección Acción de limpieza del sistema, dejaremos la opción que viene seleccionada por defecto `Iniciar la configuración (OOBE) del sistema`, pero marcaremos la opción Generalizar que viene desmarcada en la configuración original.


