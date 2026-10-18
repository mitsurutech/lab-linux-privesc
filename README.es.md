# Laboratorio de escalada de privilegios en Linux: un binario setuid, una regla de sudo y una tarea de cron modificable

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml)

Un **laboratorio de escalada de privilegios en Linux** listo para usar en clase, de la [plataforma de aprendizaje MitsuruTech](https://mitsurutech.co.uk/es/learning), donde las escuelas y los centros dan a cada alumno sus propias máquinas y su terminal en el navegador. Cada alumno recibe su propia máquina virtual Ubuntu, con una segunda cuenta de pocos privilegios ya creada, `intern`, y tres errores corrientes de administrador sembrados para que los encuentre: un programa auxiliar con el bit setuid que nunca debió tener, una regla de sudo que permite a `intern` ejecutar un editor como root sin contraseña y un script de mantenimiento que el cron de root ejecuta cada minuto y en el que cualquiera puede escribir. La tarea consiste en usar uno de los tres para llegar a ser root, leer `/root/flag.txt` y redactar lo que se usó.

Es un ejercicio de seguridad, así que la regla va primero: ataca solo tu propia máquina virtual. No tiene ruta a la máquina de ningún otro alumno ni ruta a internet más allá del archivo de paquetes de Ubuntu, PyPI, npm y GitHub. No apliques nunca lo aprendido aquí a una máquina que no sea la tuya: según la Computer Misuse Act 1990 del Reino Unido, hacerlo sin permiso es un delito.

## El laboratorio

| Archivo | Qué hace el alumno | Página de ayuda |
| --- | --- | --- |
| [`linux-privesc.lab.yaml`](linux-privesc.lab.yaml) | Cambiar a la cuenta de pocos privilegios `intern`, encontrar una de las tres debilidades sembradas, usarla para llegar a root y demostrarlo | [Laboratorio de escalada de privilegios en Linux](https://mitsurutech.co.uk/es/help/lab-linux-privesc) |

La preparación deja en la carpeta personal del alumno un README que dice la regla, nombra la segunda cuenta y su contraseña, y dice que root ha dejado una vía de entrada. No da ningún paso de explotación.

**No tiene script de calificación, y es a propósito.** La propia cuenta del alumno en la máquina virtual tiene derechos de administrador, así que cualquier archivo que un script pudiera leer como prueba, el alumno podría escribirlo en una línea sin usar ninguna de las tres debilidades: lo probamos y una comprobación pasó. Una nota fácil de falsificar es peor que ninguna, así que el trabajo es un breve informe escrito, qué debilidad, qué comandos y qué decía la bandera, que lee el profesor. No se pide corregir las debilidades después, y un profesor puede ponerlo como trabajo adicional.

## Qué hay en el archivo

`linux-privesc.lab.yaml` es una **definición de laboratorio de máquina virtual** completa, exactamente lo que ejecuta la plataforma:

| Clave | Qué es |
| --- | --- |
| `cpus`, `memory`, `disk` | el tamaño de la máquina virtual del alumno: 1 CPU, 1024 MB y 6 GB |
| `open` | los puertos que se abren en el navegador: ninguno, el laboratorio es la terminal |
| `setup` | un script que se ejecuta una vez, como root, la primera vez que arranca la máquina virtual: crea la cuenta `intern`, escribe la bandera, siembra las tres debilidades y escribe el README |

## Usarlo en la plataforma

**Para ponerlo como trabajo a una clase:**

1. En tu módulo, añade una tarea y elige la tarjeta **Laboratorio de VM Ubuntu**.
2. Pulsa el ejemplo para partir de él, o pega el contenido de `linux-privesc.lab.yaml`. Usar un ejemplo sustituye el texto, y Deshacer recupera el tuyo.
3. Publica y prueba primero tu propia copia. Cada alumno recibe una máquina virtual propia, que se conserva entre sesiones.
4. Asígnala como cualquier tarea y, en sus ajustes de entrega, elige Documentos para que el alumno pueda entregar un informe escrito. Léelo como cualquier entrega: nada en la máquina lo califica.

La [guía de laboratorios de VM Ubuntu](https://mitsurutech.co.uk/es/help/labs) del manual y su [página de laboratorios de ejemplo](https://mitsurutech.co.uk/es/help/sample-labs) cubren el resto. ¿Aún no tienes una cuenta de centro? [El primer año es gratis](https://mitsurutech.co.uk/es/pricing) hasta 50 alumnos, y [registrarse](https://mitsurutech.co.uk/es/signup) solo pide una dirección de correo electrónico.

## Comprobar el archivo

En cada push, `.github/workflows/check.yml` comprueba que el archivo es un YAML válido con las claves de arriba y que `setup` está dentro del límite de 16 KB de la plataforma. No arranca una máquina virtual: eso se hace en las propias máquinas de la plataforma.

## Lo que no funciona en un laboratorio

La máquina virtual llega al archivo de paquetes de Ubuntu, PyPI, npm y GitHub, y a nada más, así que al otro lado de este laboratorio no hay nada que atacar ni una segunda máquina desde la que atacar. Es una sola máquina virtual con una cuenta adicional.

## Acerca de

Hecho por [Mitsuru Technologies](https://mitsurutech.co.uk) para la [plataforma de aprendizaje MitsuruTech](https://mitsurutech.co.uk/es/learning): Python, Java, Go, Node, SQL y Linux en el navegador para escuelas y centros, con laboratorios de contenedores, laboratorios de máquina virtual, aplicaciones web desplegadas desde GitHub y cuestionarios que se califican solos. Prueba código sin cuenta en el [entorno de pruebas](https://mitsurutech.co.uk/es/playground).

Más laboratorios del mismo sitio: [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-juice-shop](https://github.com/mitsurutech/lab-juice-shop), [lab-dvwa](https://github.com/mitsurutech/lab-dvwa), [lab-webgoat](https://github.com/mitsurutech/lab-webgoat), [lab-mysql-phpmyadmin](https://github.com/mitsurutech/lab-mysql-phpmyadmin) y [lab-sample](https://github.com/mitsurutech/lab-sample).

## Licencia

Esta obra está bajo licencia [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Puedes compartirla y adaptarla para aprender y enseñar, si citas a Mitsuru Technologies Ltd, no la usas comercialmente y compartes lo que hagas con ella bajo la misma licencia. El texto completo está en el archivo `LICENSE`.

Todo lo que instala la preparación, `cron` y `vim`, procede del propio archivo de paquetes de Ubuntu cuando arranca la máquina virtual y conserva la licencia de su propio proyecto; nada de ello se distribuye aquí.
