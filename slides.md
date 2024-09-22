---
marp: true
# @auto-scaling: true
theme: default
class: invert
---


# Powershell
## Introducción a Powershell en Windows

---

# Diferencias básicas

* Powershell utiliza **objetos** en lugar de **bloques de texto** como salida

* Es un shell más potente, moderno y versátil que cmd

---

# Ejemplo de objetos

```powershell
PS C:\Users\Sergi> $PSVersion

Name                           Value
----                           -----
PSVersion                      5.1.22621.4111
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.22621.4111
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```
Utilizando el carácter punto (.) podemos obtener uno de sus atributos individualmente

```powershell
PS C:\Users\Sergi> $PSVersionTable.PSEdition
Desktop
```

---

# cmdlets

* Los **cmdlets** son los comandos de powershell
* Su nombre siempre sigue el formato Verbo-Sustantivo  (Verb-Noun)

## Verbos más comúnes

* **Get** Para obtener un valor
* **Start** Para ejecutar/iniciar un proceso
* **Out** Para mostrar información
* **Stop** Para detener un proceso
* **Set** Para establecer un valor
* **New** Para crear un objeto

---

# Verbos

Podemos obtener todos los verbos disponibles con el comando ```Get-Verb```

--- 

# Ejemplos de cmdlets

Siguiendo la estructura Verbo-Sustantivo, podemos cambiar y obtener la ubicación actual con el sustantivo **Location**

**Equivalente a hacer cd** 
```powershell
PS C:\Users\Sergi> Set-Location Music
PS C:\Users\Sergi\Music>
```

**Equivalente a hacer cd sin argumentos**
```powershell
PS C:\Users\Sergi\Music> Get-Location

Path
----
C:\Users\Sergi\Music
```

---

# Comandos de ayuda

**Get-Help** permite buscar información sobre un comando o tema específico


Explica las distintas maneras de usar el comando
```powershell
PS C:\Users\Sergi> Get-Help 
```

Busca todos los comandos que comienzan por **Get**
```powershell
PS C:\Users\Sergi> Get-Help "Get*"
```

El siguiente comando abrirá en el navegador web la información sobre el comando Out-String
```powershell
PS C:\Users\Sergi> Get-Help Out-String -Online
```

---

# Parámetros

De modo similar a cmd, los cmdlets aceptan modificadores en el comportamiento del comando

* Los parámetros en Powershell deben ponerse explícitamente

--- 

# Ejemplo de parámetros

El sustantivo ```Process``` es utilizado para gestionar los procesos del sistema. 

* Para el ejemplo, debemos abrir el bloc de notas (llamado Notepad en inglés)

**Comando para listar los procesos**

Dado que queremos **obtener** la lista de procesos, utilizaremos el verbo **Stop**

```powershell
PS C:\Users\Sergi> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    469      32    30712      25696      12,83  13000   8 Agent
    145       9     2612       8644              1084   0 AggregatorHost
    372      22    13216      26624       0,00  14944   8 ApplicationFrameHost
...
```
---

Dado que la lista es muy grande, utilizaremos el parámetro ```-Name``` para filtrar por nombre:

```powershell
PS C:\Users\Sergi> Get-Process -Name Notepad

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   1159      47    77168      92568       0,14  24264   8 Notepad
```

Si queremos cerrar el bloc de notas desde powershell, podemos usar el verbo **Stop**

```powershell
PS C:\Users\Sergi> Stop-Process -Name Notepad
```


---
# CommonParameters

Existen parámetros comúnes a todos los comandos existentes [CommonParameters](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_commonparameters?view=powershell-7.4)

## Ejemplo

Almacenar la ubicación actual en la variable *ubicacion*
```powershell
PS C:\Users\Sergi> Get-Location -OutVariable ubicacion
```

```powershell
PS C:\Users\Sergi> $ubicacion 
C:\Users\Sergi
```

---

# Comandos básicos

---

# Creación de ficheros y directorios

Crear un directorio

```powershell
PS C:\Users\Sergi> New-Item 'C:\Users\Sergi\Carpeta' -ItemType Directory 
```

Crear un fichero

```powershell
PS C:\Users\Sergi> New-Item 'C:\Users\Sergi\Carpeta' -ItemType File 
```

---
# Item

El sustantivo Item hace referencia a ficheros y directorios del árbol

* Se pueden aplicar los siguientes verbos: 

    * **Copy** para copiar
    * **Remove** para borrar
    * **Clear** para eliminar el contenido
    * **Move** para mover de ubicacion
    * **Rename** para cambiar de ubicacion

---

# Listar y leer

Listar el contenido de un directorio

```powershell
PS C:\Users\Sergi> Get-Dir C:\Users\Sergi\carpeta
```

Listar el contenido de un directorio

```powershell
PS C:\Users\Sergi> Get-Content hola.txt
```

---

# Escribir en un fichero

```powershell
PS C:\Users\Sergi> Write-Output "Hola" | Out-File "hola.txt"
PS C:\Users\Sergi> Get-Content "hola.txt"
Hola
```


--- 
# Ejemplos más complejos

De momento hemos visto las mismas funcionalidades que en cmd solo que más complicado

El siguiente ejemplo muestra la ventaja de utilizar powershell

---

Inicialmente tenemos el siguiente fichero

```powershell
PS C:\Users\Sergi> Get-Content hola.txt
Hola
```

El comando ```Get-Content``` no ha devuelto texto, ha devuelto un objeto del tipo String

```powershell
PS C:\Users\Sergi> (Get-Content hola.txt).GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     String                                   System.Object
```
Dado que todos los resultados son objetos, podemos aplicar métodos.

En el ejemplo anterior hemos obtenido el tipo del objeto devuelto por ```Get-Content```

---

# Get-Member

Para ver todos los métodos y atributos de cualquier objeto/resultado, podemos utilizar el comando ```Get-Member```


Guarda el contenido del fichero en una variable
```powershell
PS C:\Users\Sergi> Get-Content hola.txt -OutVariable contenidoFichero
```

Podemos obtener todos su miembros gracias al comando ```Get-Member```

```powershell
PS C:\Users\Sergi> $contenidoFichero | Get-Member
```

---

El uso de sus miembros nos permite modificar el objeto resultado

Obtener la longitud en carácteres
```powershell
PS C:\Users\Sergi> $contenidoFichero.Length
4
```


Convertir a mayúscula
```powershell
PS C:\Users\Sergi> $contenidoFichero.ToUpper()
HOLA
```
Este cambio no modifica el contenido del fichero ni de la variable. Tan solo devuelve otro objeto


---


# Ejemplo: Modificar un fichero

Vamos a escribir un comando en powershell que sobreescriba en mayúsculas todo el contenido de un fichero

Inicialmente tenemos el siguiente fichero:

```powershell
PS C:\Users\Sergi> Get-Content .\fichero_prueba.txt
Implantacion de sistemas operativos
Administracion de Sistemas Informaticos y Redes
```

--- 

Necesitamos usar el miembro ```ToUpper()``` para convertir el contenido a mayúsculas y posteriormente sobreescribir el fichero

## Resumen

* El método ```ToUpper()``` generará un objeto nuevo con el contenido en mayúscula

* El comando ```Out-File``` escribe contenido en un fichero

---

1. Insertamos el contenido del fichero en una variable

```powershell
PS C:\Users\Sergi> Get-Content .\fichero_prueba.txt -OutVariable contenido
```

2. Modificamos la variable con el contenido de la misma en mayúscula

```powershell
PS C:\Users\Sergi> $contenido = $contenido.ToUpper()
```

3. Escribimos el valor de la variable en el fichero

```powershell
PS C:\Users\Sergi> $contenido | Out-File .\fichero_prueba.txt
```

--- 

# Ejemplo en clase: Añadir el número de lineas

```powershell
PS C:\Users\Sergi> $lineas=(Get-Content .\fichero_prueba.txt).Length
PS C:\Users\Sergi> Write-Output "El fichero tiene $lineas lineas" | Out-File .\fichero_prueba.txt -Append
```


**¿Por qué Length me da el núemro de lineas en este caso?**

```powershell
PS C:\Users\Sergi> (Get-Content .\fichero_prueba.txt).GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Object[]                                 System.Array

PS C:\Users\Sergi> $contenido[0]
IMPLANTACION DE SISTEMAS OPERATIVOS
PS C:\Users\Sergi> $contenido[1]
ADMINISTRACION DE SISTEMAS INFORMATICOS Y REDES

```