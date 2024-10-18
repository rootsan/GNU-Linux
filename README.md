# Conocimientos básicos de GNU/Linux
Este grandioso sistema operativo se ha ganado su lugar por su constante innovación y crecimiento colaborativo,desde la comunidad, academía, desarrollo e investigación y hasta los sistemas más críticos de la iniciativa privada y entidades públicas (Gobierno) demanda más personal con habilidades para administrar la base instalada. En este espacio vamos a compartir conocimientos básicos y prácticas recomendadas para cada caso de uso. 
## Primeros consejos
- Preguntar a quién más sabe -> Y quién más sabe es el manual -> Leer y practicar es como más aprenderás.
- Equivocarse y corregir -> Aprender del error y evitar en producción -> Si te equivocas es también aprender las formas y cosas que en producción no debes hacer.
- Practicar en ambiente controlado -> Si aún no usas GNU/Linux puedes usar un sistema vivo live-usb, (live-cd edad antigua)
  - Personalmente me gusta Fedora con KDE (SPIN) https://fedoraproject.org/spins/kde/
  - Si la lap o pc tiene pocos recursos puedes usar https://fedoraproject.org/spins/lxde/
** Importante ** Debes iniciar tu lap o PC desde el dispositivo usb, esto carga al sistema en memoria sin instalar nada, al reiniciar o apagar todo lo que hayamos hecho se perderá, conservando así la integridad del sistema.
## Requisitos para la primer sesión
- Descargar alguna distro linux, si es de las dos opciones que cite mejor.
- instala UNetbootin del sitio oficial https://unetbootin.github.io/ (Si usas windows o MAC) y sigue las instrucciones para crear el usb-live.
  
## Primer sesión
Esta es una referencia o guía que a penas alcanza la categoría de básica, dicho esto usted tiene la desición de seguir o salir a buscar algo más.

Linux no ha dejado de crecer desde su nacimiento (1991) cuando sonaba "Bella Señora" de Emmanuel o "Burbujas de Amor" de Juan Luis Guerra y su 440, bueno lo importante de esta sección es hablar de manera 
muy general de este grandioso sistema operativo, partiendo de entender que es GNU/Linux y las Distribuciones Linux, que básicamente se componen de tres proyectos.

El primero de ellos el kernel/núcleo (corazón del sistema operativo) que es monolítico y está escrito en lenguaje “C”, con algunas porciones de código en lenguaje ensamblador para optimizar el manejo de 
memoria y procesos en arquitecturas de hardware específico, su diseño es modular lo que permite incorporar funcionalidades como componentes incluso de manera dinámica (evitando el reinicio la mayoría de las veces).

El uso de la memoria es una de las tantas buenas características que tiene este sistema operativo, utiliza memoria virtual para proporcionar 2 espacios de direcciones/regiones separadas que son espacio de usuario y 
espacio de kernel, brindándonos protección y aislamiento de memoria, y de hardware contra comportamientos de software erróneo o código malicioso.

En el espacio de kernel se ejecuta funciones del sistema operativo, extensiones y la mayoría de los controladores de dispositivos y en el espacio de usuario se ejecuta software de aplicaciones y algunos 
controladores direccionados por procesos, aquí la [dirección](https://en.wikipedia.org/wiki/User_space_and_kernel_space), por si te quieres profundizar en el tema.

El segundo proyecto es GNU que inició antes que linux (1984 para ser específico) con el objetivo de ser un sistema operativo completo de software libre, de esto surgió el movimiento de software libre, que busca
defender principalmente 4 libertades que son:
- La libertad de ejecutar el programa como lo desee, con cualquier propósito (libertad 0).
- La libertad de estudiar el funcionamiento del programa y modificarlo de modo que realice las tareas como usted desee (libertad 1). El acceso al código fuente es un prerrequisito para esto.
- La libertad de redistribuir copias para ayudar a los demás (libertad 2).
- La libertad de distribuir copias de sus versiones modificadas a otras personas (libertad 3). Al hacerlo da a toda la comunidad la oportunidad de beneficiarse de sus cambios. El acceso al código fuente es un 
prerrequisito para esto.

El tercero y último, muy importante también por la definición y visión que va más allá de las condiciones de distribución del software, son 10 sus principios por lo tanto les dejo la dirección donde podrán
[documentarse:](https://opensource.org/osd).

Los antecedentes históricos citados deben invitarlos a profundizar más en sus objetivos, momentos importantes y comprender detalladamente la importancia del modelo, para distinguir que no es un tema de gratuidad
sino de libertad, que está completamente abierto a crear un modelo de negocio sustentable e inclusivo, pueden enviarme sus preguntas comentarios posiblemente tarde en contestar.

Ahora hablemos del sistema de archivos y su jerarquía del sistema operativo, basado en una de mis distribuciones favoritas “Fedora Linux”, la jerarquía de directorios es muy grande para fines prácticos mencionare 
los más importantes dejando la dirección donde pueden profundizar al respecto.

Vamos con los directorios principales “core system directories”, en esta sección vamos a mencionar a:
- /boot, contiene archivos estáticos necesarios para iniciar el sistema, tamaño recomendado 1 GiB.
- /boot/efi, contiene archivos esenciales para el arranque para arquitecturas AMD64, Intel 64 y ARM 64 de bits basados en UEFI y el tamaño recomendado son 600 MiB. 
- /, El directorio raíz es el nivel superior de la estructura de directorios, tamaño recomendado 10 GiB, recomendado usar LVM y formato xfs.
- swap, espacio de intercambio asociado con la memoria RAM, recomiendo usar LVM, ya no se sigue la regla de otorgar el doble de la cantidad de memoria RAM, debe 
[consultar:](https://docs.redhat.com/es/documentation/red_hat_enterprise_linux/8/html/performing_an_advanced_rhel_installation/recommended-partitioning-scheme_partitioning-reference).

Para /boot y /boot/efi serán particiones primarias y sin cifrado, es altamente recomendable después de las dos particiones mencionadas en esta sección usar LVM (Logical Volume Manager) y cifrar a partir de aquí 
todos los dispositivos de bloque con LUKs2 (Linux Unified Key Setup Versión 2). “Si olvida la contraseña le deseó suerte al repetir el proceso de instalación y la pérdida de información, también puede no cifrar 
los dispositivos de bloque”. 

Entonces si nuestro sistema será para uso personal de escritorio deberá crear la partición /home, que contiene datos del usuario la cantidad recomendada será de acuerdo a sus necesidades pero sobre todo a la disponibilidad 
de su almacenamiento interno. 

Si su sistema será para un servidor WEB, Base de datos u otra aplicación utilizando el estándar de la distribución le sugiero seguir usando volúmenes lógicos (LVM) y asignar una partición a /var aquí se guardan las bitácoras
del sistema, datos variables de aplicaciones con una extensa jerarquía de subdirectorios.

Si hay aplicaciones desarrolladas en casa o de otro fabricante que no se apaguen al estándar de la distro agregue una partición /opt (optional) en donde podrá aislar y gestionar con menor riesgo para el sistema.

Vuelvo a mencionar que el tamaño de /home, /var y /opt serán basados en las necesidades y disponibilidad de su almacenamiento, pero sugiero por experiencia mantenerlo en un volumen lógico y con formato XFS. 

Mencionaré al directorio /etc por su importancia en la jerarquía porque aquí se almacena la mayoría de los archivos de configuración del sistema, no es necesario separarlo pero si saber que existe. 

Aquí más referencias para leer: 

- https://fedoraproject.org/wiki/Docs/Drafts/DirectoryStructure
- https://docs.redhat.com/es/documentation/red_hat_enterprise_linux/8/html/performing_an_advanced_rhel_installation/recommended-partitioning-scheme_partitioning-reference 
- https://es.wikipedia.org/wiki/Categor%C3%ADa:Sistemas_de_archivos_de_Linux <- Para entender el formato de los sistemas de archivos.

Por último y antes de ir a prepararse un café o tomar algo refrescante, para poner manos a la obra, mencionare al usuario root que es el todopoderoso al cual en la línea de comandos o consola lo distinguiremos por el símbolo de #, 
también tenemos al usuario normal, el cual vamos a distinguir por el símbolo $.

## Comandos (Guías)
Los comandos son la contracción de las palabras en inglés que describe la acción ejemplo cd (change directory). Su estructura es muy simple comando + [opción,argumentos], dos comandos mega importantes son
- man , páginas del manual, es el único ente que sabe todo acerca de un comando y acorde a la versión, ejemplos: $ man apropos o man -k "acción que quiere hacer"
- apropos "acción que quiere hacer".

Siga con esta guía: https://developers.redhat.com/cheat-sheets/linux-commands-cheat-sheet?extIdCarryOver=true&sc_cid=701f2000001Css5AAC&source=sso 


