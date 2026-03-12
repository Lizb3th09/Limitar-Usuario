# Limitar-Usuario

## Equipo LCK


### Integrantes:

- Rosa Lizbeth Barcenas Mancilla
- Cristian Yahir Garcia  Hernandez
- Alberto Daiji Montes Kato

# Usuarios creados en el servidor: 

- LCK -> usuario  Administrador
- pako -> usuario  limitado

# Comandos Utilizados para limitar:

+ groups pako -> agregamos al usuario a un grupo
+ sudo visudo ->  permitimos oh negamos el uso de comandos para el bloqueo en escalamiento
+ sudo edquota pako -> para limitarle el uso de espacio que usara



## 1. Cuotas de Disco - Límite de Almacenamiento
Se implementaron cuotas para restringir la capacidad de escritura del usuario `pako`.

* **Instalación:** `sudo apt install quota`
* **Inicialización:** `sudo quotacheck -cum /`
* **Activación:** `sudo quotaon /`
* **Configuración aplicada (`sudo edquota pako`):**

- Editamos el limite de disco -> le dimos 10 gb ->  10,000,000

![WhatsApp Image 2026-03-11 at 2 36 57 PM (7)](https://github.com/user-attachments/assets/dd220fd4-1b8f-48a3-98f2-8b9d40088109)

---

## 2. Restricción de Privilegios 
Se modificó el archivo `/etc/sudoers` mediante `sudo visudo` para limitar las acciones de root que `pako` puede realizar.

### Comandos Permitidos
```bash
pako ALL=(root) /bin/systemctl status *, /bin/systemctl restart *

```

### Comandos Prohibidos (Escalamiento de privilegios bloqueado)

Se utilizó el operador `!` para denegar explícitamente el acceso a:

* **Shells:** `!/bin/su`, `!/bin/bash`, `!/bin/sh`
* **Gestión de paquetes:** `!/usr/bin/apt`, `!/usr/bin/apt-get`, `!/usr/bin/aptitude`
* **Editores y Sudo:** `!/usr/sbin/visudo`, `!/bin/nano`, `!/usr/bin/sudoedit`

---

## 3. Gestión de Usuarios y Permisos de Archivos

Acciones para aislar al usuario y proteger el código fuente.

* **Eliminar de administradores:** `sudo gpasswd -d pako sudo`
* **Protección de carpeta API:**
* `sudo chown -R apiuser:apiuser /home/lck/commando-api`

---

Al haber bloqueado `/bin/bash` y `/bin/sh` en el archivo `sudoers`, el usuario `pako` ya no podrá ejecutar scripts complejos con privilegios elevados, lo cual es una excelente medida de "hardened system".

