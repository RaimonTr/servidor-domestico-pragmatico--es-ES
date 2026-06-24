# Servidor doméstico pragmático
## Parte I — Red local

**Estado:** Parte I independiente. Montaje y uso del servidor dentro de la red local. Incluye los capítulos 1–27.

---

# Antes de empezar

Primera frase: Este manual se ha elaborado con ayuda de IA generativa. Ni lo oculto ni lo vendo como trabajo puramente humano. La IA ha estado como apoyo para explicar conceptos, proponer comandos, revisar errores y ordenar documentación, pero el texto del manual es mío casi al 99%. 

También una IA puede equivocarse con absoluta seguridad aparente. Por eso, cualquier comando que afecte a discos, usuarios, permisos, redes, seguridad o copias de seguridad debe entenderse antes de ejecutarse, que es lo que he intentado hacer yo.

La responsabilidad de ejecutar algo en un ordenador real sigue siendo humana. Eres muy libre de quemar este manual si no te sientes cómodo con ello.

Segunda frase: He intentado mantener un tono muy desenfadado en la escritura de este manual, y un porcentaje no medible de palabras malsonantes.

Pero también hay partes muy serias, porque un servidor, básicamente, es:

- 20% montaje
- 80% mantenimiento

Tercera frase: Has de divertirte. Si no disfrutas con la Terminal o lanzando comandos raros que casi no entiendes y que te has pegado 2 horas para más o menos comprenderlos y no te pica la curiosidad, mal vamos.

En este manual vamos a dedicar casi todo nuestro tiempo al 20%. El otro 80% es más complicado y deberás aprenderlo con el tiempo. Actualizar, revisar, hacer copias de seguridad, comprobar que todo sigue funcionando y arreglar las cosas cuando dejan de funcionar forman parte del trabajo real.

Nadie vende duros por cuatro pesetas. Nadie regala nada. Este manual puede ayudarte a salir de muchos servicios propietarios y de pago, pero eso NO IMPLICA que la alternativa doméstica vaya a ser más barata, más sencilla o más cómoda. Posiblemente ocurra justo lo contrario.

Lo que sí serás es soberano de tus datos. ¿Cuánto vale eso?

También obtendrás algo difícil de comprar: control. Tus fotos estarán donde tú decidas, tu música estará donde tú decidas y tus copias de seguridad estarán donde tú decidas. Y cuando algo falle, sabrás dónde mirar porque el sistema será tuyo. ¿Vale la pena? No lo sé. Cada persona tendrá que responder esa pregunta por sí misma. Pero después de terminar este manual, al menos podrás decidir con conocimiento de causa.

# Empecemos

Este libro cuenta cómo se montó un servidor doméstico real por necesidades muy concretas y sin tener ni p*** idea de montar un servidor.

La idea inicial era sencilla: un servidor para almacenar y ver desde cualquiera de mis dispositivos:

- fotos, y verlas en cualquiera de mis dispositivos (y salir de Google Fotos);
- escuchar mi música;
- tener mis películas y series;
- hacer copias de seguridad;
- depender un poco menos de los GAFAM.

No pretende ser un curso completo de Linux y no pretende convertirte en administrador de sistemas (a ratos puede que te sientas Neo, pero necesitas ser viejo para pillar eso).

Es simplemente la documentación, explicada con calma, de un proyecto que me ha solucionado problemas concretos y que sigue funcionando.

El objetivo es que cualquier persona curiosa pueda reproducirlo y volver a montarlo cuando quiera. Es un manual que empiezo a escribir en mayo 2026 que es cuando más o menos he terminado de hacer que el servidor funcione. La fecha no es que sea muy importante, pero, por si acaso, no uses este manual en el año 2150 que igual, y sólo igual, ya no funciona.

Muy importante:

Yo soy de ciencias, pero no estudié nada relacionado con informática/computadores.

No hace falta ser programador.

No hace falta haber usado Linux antes.

Sólo hace falta una cosa:

**No tener miedo a abrir una Terminal y escribir comandos.**

Una Terminal te da acceso directo al sistema operativo. Dicho rápido: es una forma de darle órdenes al ordenador sin pasar por ventanas, iconos y botones.

---

# ¿Para quién está escrito?

Especialmente dirigido a chavales, adolescentes, profes de instituto... y para viejales como yo que no queremos que nos expliquen el spin de un electrón.

Todo el montaje descrito en este libro se realizó desde un MacBook.

Llevo más de veinte años sin utilizar Windows de forma habitual, así que no puedo garantizar que los mismos pasos funcionen igual allí (que ni de risa funcionarán, aunque probablemente haya formas de adaptarlos).

Probablemente muchas partes también puedan hacerse desde Linux sin apenas cambios; de hecho, quien ya use Linux sabrá muchas cosas mejor que yo.

Este libro está escrito para alguien que quiera aprender mientras construye.

Y da igual si tiene 12 o 75 años. Yo estoy más cerca de la parte derecha de la frase.

---

# Una advertencia importante

A lo largo del libro aparecerán palabras raras.

SSH.

Docker.

Tailscale.

systemd.

No te preocupes si no sabes qué significan (yo no tenía ni idea). Cuando aparezcan por primera vez ya veremos cómo lo explicamos como si todos tuviéramos 12 años.

La idea es aprender cada herramienta justo cuando haga falta.

No antes.

**No pongas nombre a una cosa antes de que el lector sienta la necesidad de esa cosa.**
  "–Multitud de palabras inusuales: Esternocleidomastoideo no es una palabra que deba colarse en una novela de fantasía. Y guiverno no tiene por qué insertarse en una novela de intriga médica. De nada.". (https://gabriellaliteraria.com/tu-libro-por-la-ventana/)

---

# ¿Qué vamos a construir?

```text
Tu ordenador
      │
      ├──────── controla ────────┐
      │                          │
      ▼                          ▼

              Ubuntu Server

      ├── Fotos
      ├── Música
      ├── Películas
      ├── Copias de seguridad
      └── Acceso remoto privado
```

Todavía no importa cómo se llaman las cosas. Primero intentaremos entender qué problema resuelve qué, y después aprenderemos sus nombres.

---

# Esto tampoco va de informática

O, por lo menos, no sólo.

También va de tener una idea, buscar información, leer documentación.

Probar y equivocarse; arreglar lo que has roto y acabar construyendo algo útil con tus propias manos.

Si al terminar sabes montar un servidor, estupendo.

Pero si además descubres que aprender tecnología puede ser divertido, entonces este manual te habrá sido de un poco más de utilidad que montar simplemente el server.

---

# ¿Qué piezas forman nuestro servidor?

## Ubuntu Server

Es el armario sobre el que construiremos toda la casa.

## Docker

Cada aplicación vive en su propio cajón para no molestar a las demás.

Esos cajones, en este ámbito, se llaman contenedores:

## Immich

Nuestro álbum de fotos privado.

## Navidrome

Nuestro Spotify particular.

## Jellyfin

Nuestro pequeño Netflix.

## Tailscale

Crea una pequeña red privada entre nuestros dispositivos para poder entrar al servidor de casa desde cualquier lugar sin que nadie más pueda hacerlo (siempre que tengas cobertura de red, milagros a Lourdes).

---

# ¿Cómo trabaja todo esto junto?

```text
MacBook (tu ordenador personal desde donde quieras controlar el servidor)
   │
   ▼
Ubuntu Server
   │
   ├── Docker
   │     ├── Immich
   │     ├── Navidrome
   │     └── Jellyfin
   │
   └──  Tailscale
   
```

Puede parecer complicado, pero en realidad son varias herramientas pequeñas trabajando juntas.

Las iremos montando una a una.

---

# ¿Por qué he usado un portátil viejo?

Tenía un portátil relativamente moderno parado, consume poco y tiene batería incorporada, lo que me sirve como SAI (Sistema de Alimentación Ininterrumpida: vaya, que si se va la electricidad de casa tengo 5 o 6 horas de batería).

También hace poco ruido y la carga de la CPU es de risa, por debajo del 1% si no estoy haciendo nada.

También viene con pantalla y teclado, por si un día pasa algo raro. Está guay ser un admin ("hola, soy administrador de un servidor y molo mogollón), qué, como sabe todo el mundo, ligan tutiplén, pero como era mi primer servidor, preferí "por si acaso" disponer de un teclado y una pantalla a la que poder golpear cabreado. Si nunca has golpeado un ordenador, jamás serás un admin de verdad, lo siento.

Así que parte del objetivo de este proyecto no es tener el servidor perfecto. Es tener uno útil.

---

# Una última idea

La informática no consiste sólo en usar programas.

También consiste en decidir quién controla tus datos, dónde viven y qué pasa si un día desaparece el servicio que usas.

Y, sobre todo, consiste en divertirse, buscar información y aplicar lo aprendido para resolver problemas propios.

---

> El mejor sistema no es el más rápido.
>
> El mejor sistema es el que casi no necesita ejecutarse.

---

## Nota sobre marcas

Los nombres comerciales y marcas mencionados en este manual pertenecen a sus respectivos propietarios.

¡Já!, antes os he dicho que íbamos al meollo, y, releyendo todo esto unos días después, me doy cuenta que me enrollo como una persiana. Venga, vamos a darle a la chicha.

Fin de la intro.


---

# Capítulo 1. Vamos a instalar Ubuntu.

Lo único importante es que vamos a descargar una ISO de Ubuntu Server (sin interfaz gráfica, uuuuuh, miedito) y convertirla en un pendrive desde el que arrancará nuestro futuro servidor. Normalmente la puedes encontrar en https://ubuntu.com/download/server. 

Sin liarnos con el tema, necesitamos la versión server, y, si puede ser, LTS (Long Term Support).

Server significa servidor (oh, ¡chorprecha!), aunque eso tampoco aclara mucho.

Existe una versión de Ubuntu con ventanas, iconos, navegador y escritorio, pero nosotros no queremos eso, nuestro ordenador va a pasar la mayor parte del tiempo guardando fotos, música y películas, así que no necesita dibujar ventanitas.

Necesita trabajar y por eso elegimos Ubuntu Server. Si puede ser, elegiremos una con servicio de soporte largo (LTS). 

## ¿Qué necesito?

```text
- Un ordenador viejo (portátil o sobremesa; Publicidad gratuita que no me pagan: a mí me molan mogollón el Zero o el One de www.slimbook.es).
- Un pendrive de 8 GB o más.
- Otro ordenador desde el que preparar la instalación.
- Un rato libre.
```

Y ahora viene una buena noticia.

Si no tienes un ordenador viejo, no corras a comprar uno.

Pregunta a tus padres, a tus tíos, a tus abuelos, a ese amigo de la familia que guarda cualquier cosa "por si acaso".

La cantidad de ordenadores perfectamente válidos para aprender que están cogiendo polvo (un saludo a México y Argentina) es increíble.

De hecho, una de las cosas más bonitas de este proyecto es darle una segunda vida a una máquina que alguien ya daba por acabada.

Además, un servidor no necesita tanta potencia como la mayoría de la gente cree: Un Windows 11 recién arrancado suele tener decenas de procesos funcionando aunque no estés haciendo nada, Ubuntu Server, sin interfaz gráfica, arranca prácticamente con lo imprescindible.

No te obsesiones con los números porque dependen del ordenador, pero como orientación suele ser normal encontrarse algo parecido a esto:

```text
Windows 11:
CPU en reposo:   2% - 8%
RAM utilizada:   3 - 5 GB

Ubuntu Server (sin GUI):
CPU en reposo:   0% - 1%
RAM utilizada:   500 MB - 1 GB
```

Eso significa que un portátil que parece viejo para Windows puede seguir teniendo muchísima vida como servidor doméstico.

# Capítulo 2. Preparando el pendrive

Existen varios programas,

Rufus.

Ventoy.

Y unos cuantos más.

Yo utilicé Balena Etcher porque era sencillo y funcionó a la primera.

Como este libro trata de montar un servidor y no de discutir en foros, vamos a usar ése.

## Descargando Balena Etcher

Abre el navegador.

Busca simplemente:

```text
balena etcher
```

Entra en la página oficial y descárgalo; se instala como cualquier otro programa, sin misterios.

## Grabando la ISO

Cuando abras Balena Etcher verás tres pasos.

### 1. Selecciona el archivo.

Escoge la ISO de Ubuntu Server que acabas de descargar.

### 2. Selecciona el pendrive.

Todo lo que haya dentro desaparecerá.

Comprueba dos veces que has elegido el correcto. Perdón, hazlo 3 veces, o mejor, cómprate un pendrive nuevo sólo para ISO's.

No hay premio por hacerlo rápido.

### 3. Pulsa el botón.

Dependiendo del pendrive y del ordenador tardará unos minutos y cuando termine tendrás el pendrive de instalación completo. Acabas de fabricar el disco de instalación de tu futuro servidor. Blue or red, Neo.

---

# Capítulo 3. Primer arranque

Hasta ahora todo lo hemos hecho desde nuestro ordenador habitual y ahora toca tocar el futuro servidor.

Apaga lo que vaya a ser tu servidor, conecta el pendrive y vuelve a encenderlo. Ahora viene una cosa fácil pero que a mí me amargó un par de minutos de mi vida.

## ¿Qué es eso del Boot Menu?

Cuando un ordenador arranca tiene que decidir desde dónde cargar el sistema operativo, lo que normalmente hace desde el disco duro.

Pero claro, nosotros queremos que, esta vez, arranque desde el USB, y cada fabricante usa una tecla distinta:

F12.

F9.

ESC.

Supr.

Si no funciona a la primera, busca en Internet:

```text
boot menu + modelo de tu portátil
```

Por ejemplo:

```text
boot menu Lenovo T480
```

o

```text
boot menu HP ProBook 450
```

La informática también consiste en saber buscar. Y en perder tiempo en cosas que deberían ser homogéneas y que no hagan perder tiempo al usuario, pero quién dijo "fácil".

## El momento de la verdad

Con el Boot Menu abierto, selecciona el pendrive. Si todo ha ido bien, aparecerá la pantalla de instalación de Ubuntu Server.

---

# Capítulo 4. Instalando Ubuntu Server

Aquí conviene no perder el norte, porque el instalador de Ubuntu puede enseñar más pantallas de las que realmente necesitamos para este proyecto.

Este manual no va de aprender todas las opciones posibles de Ubuntu, ni de convertirnos en expertos en particiones, cifrado, LVM, arranques duales, discos empresariales o arquitecturas de centro de datos. Todo eso existe, todo eso puede ser interesante, y todo eso puede aprenderse más adelante si te pica la curiosidad.

Pero hoy no estamos montando un banco, estamos montando un servidor doméstico de forma SENCILLA:

```text
disco completo usado por Ubuntu
Sin cifrado
Ubuntu Server instalado
usuario creado
contraseña creada
SSH activado
```

Con eso basta para empezar.

## La instalación, resumida. Más abajo más extendida.

Cuando arranques desde el pendrive, Ubuntu te irá preguntando cosas. Algunas pantallas parecen importantes sólo porque tienen muchas palabras, pero en realidad la mayoría se pueden aceptar con la opción que propone el instalador.

La idea general es ésta:

```text
Idioma:
  el que uses normalmente o el que te apetezca

Teclado:
  Spanish si usas teclado español

Red:
  si tienes cable Ethernet, normalmente se detecta sola

Proxy:
  vacío

Mirror:
  el que proponga Ubuntu

Disco:
  usar el disco completo

Cifrado:
  no

Particiones raras:
  no

Snaps adicionales:
  no hace falta instalar nada aquí
```

Y ya está.

Podríamos explicar cada pantalla durante veinte páginas, pero nos estaríamos desviando del objetivo. Si algún día quieres estudiar particiones, cifrado completo de disco o instalaciones avanzadas, perfecto. Para este proyecto, lo que queremos es una máquina simple, limpia y fácil de entender.

## El disco: sin florituras

Cuando el instalador pregunte qué hacer con el disco, elegiremos la opción sencilla: usar el disco completo.

Esto significa que Ubuntu borrará lo que haya en ese ordenador y usará el disco para sí mismo. Por eso hemos insistido antes en que no hagas esto sobre un ordenador que tenga datos que quieras guardar, o que al menos hayas guardado antes en otro sitio.

## Nada de cifrado

Ubuntu puede ofrecer cifrado del disco.

Nosotros no lo vamos a activar.

No porque el cifrado sea malo, sino porque añade una capa más de complejidad, y este servidor está pensado para vivir en casa, arrancar solo después de un corte de luz y ser fácil de recuperar si pasa algo.

Un servidor doméstico que no arranca solito porque está esperando una contraseña de cifrado en una pantalla que nadie está mirando puede ser muy seguro, sí, pero también puede ser un coñazo.

Así que, para este montaje:

```text
sin cifrado
sin LVM complicado
sin particiones raras
```

Primero vamos a conseguir que funcione. Luego ya habrá tiempo de ponerse fino si hace falta.

## Usuario, nombre del servidor y contraseña

Esta parte sí importa.

En algún momento Ubuntu te pedirá crear el usuario principal y dar nombre a la máquina. Verás campos parecidos a estos:

```text
Your name:
Your server's name:
Pick a username:
Choose a password:
Confirm your password:
```

Traducido a español normal:

```text
Tu nombre:
Nombre del servidor:
Nombre de usuario:
Contraseña:
Repite la contraseña:
```

El nombre del servidor es cómo se llamará la máquina dentro de tu red. En mi caso, el servidor se llamó:

```text
serviplan
```

El usuario principal fue:

```text
servi
```

Tú puedes poner otros nombres, pero mi recomendación es que sean cortos, sin espacios, sin acentos y fáciles de escribir en una Terminal.

Ejemplos razonables:

```text
servidor
servi
server
usuario
Skynet
```

Ejemplos que yo evitaría:

```text
Administrador Supremo
ElPutoAmo
José María
usuario del servidor
```

No porque no tengan gracia, sino porque luego la Terminal no entiende tus ganas de literatura.

La contraseña debe ser suficientemente buena como para no dar vergüenza. No pongas `1234`, `password`, `ubuntu` ni `servidor`. Este ordenador acabará guardando fotos, música, datos y copias. No lo protejas con la contraseña de una maleta barata.

Y, sobre todo, apunta lo que pongas. USA UN MALDITO GESTOR DE CONTRASEÑAS, ALELAO, QUE ESTAMOS EN 2026 (eso hoy). Por cierto, ¿sabes que en tu server podrás montarte tu propio gestor de contraseñas sin depender de nadie?

No "ya me acordaré".

Apúntalo.

En una libreta, en tu gestor de contraseñas o donde guardes las cosas importantes. Dentro de seis meses no querrás estar delante de la pantalla negra preguntándote si el usuario era `server`, `ubuntu`, `admin`, `pepe` o `servi`.

## La casilla importante: SSH

Durante la instalación aparecerá una opción parecida a esta:

```text
Install OpenSSH server
```

Márcala. SSH es, explicado en una frase, una forma de conectarte a otro ordenador a través de Internet, bien fibra/cable, bien teléfono móvil. Dicho todavía más claro: nos permitirá controlar el servidor desde el ordenador personal sin tener que estar sentados delante y sin levantarnos del sofá.

Si no la marcas, no se acaba el mundo, pero nos complicaremos tontamente la vida, así que márcala.

## No instales extras

El instalador puede ofrecerte paquetes adicionales, snaps o herramientas de servidor.

No instales nada.

Ni Docker.

Ni Kubernetes.

Ni bases de datos.

Nada.

Ya instalaremos nosotros lo que necesitemos, cuando lo necesitemos y entendiendo para qué sirve. Ahora queremos un Ubuntu Server limpio:

```text
sistema base
usuario
contraseña
SSH
```

Nada más.

## Reiniciar

Cuando Ubuntu termine de instalarse, te pedirá reiniciar. Acepta. En algún momento te dirá que retires el pendrive, así que quítalo y pulsa ENTER o "Continuar" o lo que aparezca.

Si todo ha ido bien, el portátil ya no arrancará el instalador. Arrancará Ubuntu Server desde el disco interno y verás una pantalla negra pidiendo usuario y contraseña.

Esa pantalla negra puede parecer poca cosa, pero es una victoria estupenda, porque hace un rato tenías un portátil viejo y ahora tienes un servidor Ubuntu recién instalado.

---

# Capítulo 5. Primer arranque: la pantalla negra

Después del reinicio, lo normal es que veas algo parecido a esto:

```text
serviplan login:
```

Es normal que la primera vez parezca que has viajado a 1987. Pero no pasa nada. Un servidor no necesita parecer bonito. Necesita funcionar.

Escribe tu usuario y pulsa ENTER. Después escribe tu contraseña y vuelve a pulsar ENTER.

En entornos Linux cuando escribes la contraseña, normalmente no verás nada en pantalla. Ni asteriscos. Ni puntitos. Nada. No está roto, sólo pulsa ENTER después de la contraseña y accederás al servidor.

## El primer comando

Vamos a comprobar dónde estamos.

**Ejecutar en tu servidor:**

```bash
clear

hostname
whoami
pwd
```

Esto no cambia nada. Sólo pregunta cosas.

`hostname` muestra el nombre del servidor.

`whoami` muestra el usuario con el que has entrado.

`pwd` muestra en qué carpeta estás.

Si ves algo parecido a esto:

```text
serviplan
servi
/home/servi
```

vas bien, ya estás dentro.

## Qué significa estar dentro

Estar dentro no significa que estemos usando una app. Significa que estamos hablando directamente con el sistema operativo.

Cuando escribimos una orden, Ubuntu la ejecuta. Si la orden pregunta algo, Ubuntu responde. Si la orden cambia algo, Ubuntu lo cambia.

Por eso iremos con calma, pero no por miedo ni respeto, un servidor es una herramienta poderosa, y una herramienta poderosa conviene entenderla antes de ponerse a aporrearla como si fuera una piedra ( un gran poder conlleva etc etc etc).

---

# Capítulo 6. Primer contacto desde el ordenador personal

Hasta ahora hemos usado la pantalla y el teclado del propio servidor. Eso está bien para instalar, pero no queremos trabajar así todos los días.

La gracia de un servidor es que pueda estar en otra habitación, y que podamos controlarlo desde nuestro ordenador normal.

Aquí aparece por fin SSH.

Ya no como palabra rara, sino como solución a un problema real.

El problema es éste:

```text
Tengo un servidor en casa.
No quiero sentarme delante de él cada vez que quiera hacer algo.
Quiero controlarlo desde mi MacBook.
```

La solución se llama SSH (Copio y pego: SSH (Secure Shell o Intérprete de Órdenes Seguro) es un protocolo de red criptográfico que permite a los usuarios acceder, controlar y administrar servidores o dispositivos remotos de forma totalmente segura a través de una red abierta e insegura).

## Averiguar la IP del servidor

Para entrar desde el Mac necesitamos saber la dirección IP del servidor dentro de la red de casa.

La IP es como la dirección postal del ordenador dentro de la red.

Desde el servidor, ejecuta:

**Ejecutar en tu servidor**

```bash
clear

ip -br addr
```

Verás varias líneas. Alguna tendrá una IP parecida a:

```text
192.168.1.99
```

o:

```text
192.168.1.41
```

o cualquier número parecido que empiece por `192.168.1.x` o `192.168.0.x`.

Esa es la IP local del servidor, la apuntamos. En mi caso salió la IP

```text
192.168.1.99
```

## Entrar desde el Mac

Ahora ve al MacBook, abre Terminal y prueba:

**Ejecutar en MAC:**

```bash
clear

ssh servi@192.168.1.99
```

Cambia `servi` por el usuario que creaste antes al instalar Ubuntu Server y `192.168.1.99` por la IP real de tu servidor.

La primera vez puede aparecer un aviso largo diciendo que no conoce ese ordenador y preguntando si quieres continuar, lo normal.

Es tu ordenador personal diciendo:

> "Oye, nunca he hablado con esta máquina. ¿Seguro que quieres seguir?"

Escribe:

```text
yes
```

y pulsa ENTER.

Después te pedirá la contraseña del usuario del servidor.

Si todo va bien, acabarás dentro del servidor desde el ordenador personal, y esto sí que es un momento importante, acabas de controlar un ordenador desde otro ordenador sin pantalla conectada, ni ratón ni levantarte del sofá.

Eso es SSH.

## Salir del servidor

Para salir de la conexión SSH hacia tu servidor escribe:

```bash
clear

exit
```

Nota: Lo de las instrucciones en líneas separadas como las que acabas de ver es el equivalente a hacer "clear; exit", una orden se ejecuta después de otra. Lo del "clear" (limpiar pantalla) es una manía mía para que no se me acumulen las respuestas de la Terminal y no me líe.

Volverás a la Terminal de tu ordenador, no has apagado el servidor, sólo has cerrado la conexión.

Es como colgar una llamada.

---

# Capítulo 7. Actualizar Ubuntu

Si quieres actualizar Ubuntu Server antes de empezar con la fiesta:

```bash
sudo apt update && sudo apt upgrade
```

# Capítulo 8. Fijar una idea importante: LAN, IP y router

En este punto conviene entender una cosa antes de seguir:

Tu servidor vive en tu casa. Tu Mac vive en tu casa. Y tu router es el aparato que los conecta.

Cuando tu ordenador personal habla con el servidor usando una IP tipo:

```text
192.168.1.99
```

no está saliendo a Internet, está hablando dentro de tu red local y a eso se le llama LAN (Local Area Network, Red de Área Local).

No hace falta aprender redes enteras ahora. Sólo esta idea:

```text
LAN = red de casa
Internet = fuera de casa
```

Mientras estemos usando la IP local del servidor, estamos trabajando dentro de casa.

## ¿Por qué importa la IP?

Porque si hoy el servidor tiene una IP y mañana el router le da otra, los comandos dejarán de funcionar hasta que averigües la nueva.

Por eso, más adelante conviene reservar una IP fija para el servidor en el sistema DHCP del router. Esto suena complicado pero es una cosa necesaria. Aquí la dificultad reside en tu router y tu Operador de Internet. 

En nuestro montaje acabamos usando:

```text
192.168.1.99
```

Pero cada casa es distinta.

No vamos a entrar ahora en la configuración del router, porque cada modelo tiene sus menús y sus rarezas. De momento basta con saber esto:

> Si el servidor cambia de IP, no se ha roto. Sólo tienes que encontrar su nueva dirección. Escribe de nuevo en el servidor:

```text
ip -br addr
```
---

# Capítulo 9. Preparar unas carpetas básicas

Antes de instalar servicios como Immich, Navidrome o Jellyfin, vamos a preparar un poco la casa, sólo lo justo.

La idea es separar:

```text
programas y configuraciones
datos multimedia
scripts
logs
```

En nuestro servidor real usamos rutas como estas:

```text
/home/servi/docker
/home/servi/scripts
/home/servi/logs
/srv/media
/srv/media/music
/srv/media/movies
```

No hace falta que entiendas todavía cada una.

La idea es simple:

`/home/servi` será la zona del usuario.

`/srv/media` será la zona donde vivan los archivos multimedia.

Ahora vamos a empezar en serio a usar la Terminal, bien con destino de tus instrucciones a tu ordenador personal, bien a tu servidor.

Nota: Yo acabé un poco cansado de Terminal.app y me descargué iTerm2.app desde

(https://iterm2.com/)

Permite tener siultáneamente 2 ventanas de terminal contiguas (Cmd+D), así que en la de la izquierda trabajo con mi ordenador personal y en la de la derecha con el servidor. Puedes alternar entre ellas pulsando Cmd+option+fecha izquierda/derecha. También sirve para almacenar claves SSH y otras cosillas de las que hablaremos en una segunda parte de este manual.

## Crear carpetas

**Ejecutar en SERVIPLAN:**

Recuerda que ya no tienes que estar sentado delante del servidor, basta con entrar desde la terminal: 
```bash
ssh servi@192.168.1.39
```
Recuerda cambiar el usuario "servi" y la ip por los tuyos. Una vez conectado a tu servidor, ejecuta:

```bash
clear

mkdir -p ~/docker
mkdir -p ~/scripts
mkdir -p ~/logs
sudo mkdir -p /srv/media/music
sudo mkdir -p /srv/media/movies
sudo mkdir -p /srv/media/tv
```

El comando `mkdir` crea carpetas.

La opción `-p` significa que no proteste si alguna ya existe.

Usamos `sudo` para crear carpetas dentro de `/srv`, porque esa zona pertenece al sistema.

Ahora hacemos que nuestro usuario pueda trabajar en `/srv/media`:

**Ejecutar en SERVIPLAN**

```bash
clear

sudo chown -R "$USER:$USER" /srv/media
```

`chown` cambia el propietario de una carpeta.

Traducido:

> "Haz que esta carpeta sea de mi usuario."

Esto nos evitará muchos dolores de cabeza cuando empecemos a copiar música o películas.

## Comprobar

**Ejecutar en SERVIPLAN**

```bash
clear

ls -lah ~
ls -lah /srv/media
```

Si ves las carpetas, vamos bien.

No hemos instalado ningún servicio todavía.

Sólo hemos preparado el terreno.

---

# Capítulo 10. Instalar herramientas básicas

Antes de llegar a Docker y a los servicios, vamos a instalar unas herramientas que usaremos una y otra vez.

No son emocionantes, ni tienen lucecitas, pero hacen falta.

## Instalar paquetes útiles

**Ejecutar en SERVIPLAN**

```bash
clear

sudo apt install -y curl wget git ca-certificates gnupg
```

Traducción rápida:

`curl` y `wget` descargan cosas desde Internet.

`git` sirve para trabajar con repositorios de código y configuraciones.

`ca-certificates` ayuda a que las conexiones HTTPS sean fiables.

`gnupg` se usa para verificar claves de paquetes.

No hace falta memorizarlo, simplemente son herramientas básicas de taller que es como tener destornillador, cinta aislante y una linterna antes de abrir un armario.

---

# Capítulo 11. Docker: la estantería donde pondremos los servicios

Falta lo importante: que el servidor haga cosas útiles.

Un servidor doméstico no se monta para mirar una pantalla negra. Se monta para guardar fotos, escuchar música, ver películas, hacer copias de seguridad y depender un poco menos de servicios externos.

Para hacer eso vamos a instalar varias aplicaciones.

Podríamos instalar cada aplicación directamente sobre Ubuntu, una detrás de otra, como quien va metiendo cacharros en un cajón. El problema es que al cabo de unos meses nadie se acuerda de qué instaló, dónde lo instaló, qué archivo tocó ni qué dependencia rompió otra dependencia.

Y ahí empieza el festival: por eso vamos a usar Docker.

Docker nos permite ejecutar aplicaciones dentro de contenedores. Un contenedor es una especie de cajón donde vive una aplicación con muchas de las piezas que necesita para funcionar.

Immich vivirá en su cajón.

Navidrome vivirá en su cajón.

Jellyfin vivirá en su cajón.

Y cada uno podrá tener sus propios archivos, sus propias carpetas y sus propias necesidades sin mezclarse con las demás.

No es magia: es orden. Y un servidor doméstico, aunque sea pequeño y esté hecho con un portátil viejo, necesita orden.

## Qué problema resuelve Docker

Imagina que instalas una aplicación y necesita una versión concreta de una aplicación.

Luego instalas otra aplicación y necesita otra versión distinta.

Luego actualizas una cosa y se rompe otra.

Luego intentas arreglarlo y ya no sabes qué tocaste.

Docker no elimina todos los problemas, porque nada elimina todos los problemas, pero sí ayuda a que las aplicaciones estén más separadas entre sí.

La idea básica es ésta:

```text
Ubuntu Server
      │
      ├── Contenedor de Immich
      ├── Contenedor de Navidrome
      ├── Contenedor de Jellyfin
      └── Otros contenedores
```

Ubuntu sigue siendo la base, pero Docker es la estantería. Los contenedores son las cajas y las aplicaciones viven dentro de esas cajas.

## Qué necesitamos conseguir

Antes de instalar servicios de verdad, necesitamos tener esto funcionando:

```text
Docker instalado
Docker Compose instalado
usuario con permiso para usar Docker
carpetas ordenadas para cada servicio
```

Cuando eso esté listo, podremos levantar aplicaciones con archivos llamados `docker-compose.yml`.

Ese nombre parece feo.

Lo es.

Pero nos va a salvar la vida más de una vez.

---

# Capítulo 12. Instalar Docker sin inventar recetas

Aquí vamos a hacer una cosa importante: seguir la documentación oficial.

No porque sea más elegante, sino porque Docker puede cambiar con el tiempo. Una receta copiada de un blog viejo puede funcionar hoy y fallar mañana. Cuando instales una pieza importante del servidor, acostúmbrate a buscar la documentación oficial.

La página oficial de Docker para Ubuntu es:

```text
https://docs.docker.com/engine/install/ubuntu/
```

Si un día los comandos de este manual no funcionan, no significa necesariamente que seas idiota ni que el ordenador esté poseído. Puede significar que Docker cambió algo.

Primero insultas un poco al ordenador, porque somos humanos, después miras la documentación oficial.

## Instalar dependencias, cosas que necesitamos

Primero instalamos unas piezas que Ubuntu necesita para añadir repositorios externos y comprobar paquetes firmados.

**Ejecutar en SERVIPLAN**

```bash
clear

sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
```

`ca-certificates` ayuda a que Ubuntu confíe en conexiones seguras.

`curl` sirve para descargar contenido desde Internet.

`gnupg` sirve para trabajar con claves y firmas.

No hace falta memorizarlo. Son herramientas de taller.

## Añadir la clave oficial de Docker

Los paquetes de Docker van firmados. Eso permite que Ubuntu compruebe que lo que instala viene realmente de Docker y no de un señor raro con bigote y una web cutre.

**Ejecutar en SERVIPLAN**

```bash
clear

sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Este bloque parece más raro de lo que realmente es: primero crea una carpeta para guardar claves y luego descarga la clave oficial de Docker.

Finalmente ajusta permisos para que Ubuntu pueda leerla.

## Añadir el repositorio de Docker

Ahora le decimos a Ubuntu dónde debe buscar los paquetes de Docker.

**Ejecutar en SERVIPLAN**

```bash
clear

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Traducido:

> "Ubuntu, además de tus repositorios normales, a partir de ahora también puedes buscar Docker en el repositorio oficial de Docker."

## Instalar Docker

Ahora sí instalamos Docker.

**Ejecutar en SERVIPLAN**

```bash
clear

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Cuando termine, comprobamos:

**Ejecutar en SERVIPLAN**

```bash
clear

docker --version
docker compose version
```

Si ambos comandos muestran una versión, Docker está instalado.

Todavía no hemos montado ningún servicio, pero ya tenemos la estantería.

## Permitir usar Docker sin sudo

Docker necesita permisos especiales. Podríamos escribir `sudo docker` cada vez, pero sería un coñazo y acabaríamos mezclando permisos sin necesidad.

La forma habitual es añadir nuestro usuario al grupo `docker`.

**Ejecutar en SERVIPLAN**

```bash
clear

sudo usermod -aG docker "$USER"
```

Ahora hay que cerrar la sesión SSH y volver a entrar para que el cambio se aplique.

**Ejecutar en SERVIPLAN**

```bash
clear

exit
```

Desde el Mac volvemos a entrar por LAN:

**Ejecutar en MAC**

```bash
clear

ssh servi@192.168.1.99
```

Cambia `servi` y `192.168.1.99` por tu usuario y tu IP si son distintos.

Comprobamos que Docker responde sin `sudo`:

**Ejecutar en SERVIPLAN**

```bash
clear

docker ps
```

Si no da error, perfecto.

Si da error de permisos, sal de SSH y vuelve a entrar. Si sigue fallando, reinicia el servidor y prueba otra vez.

---

# Capítulo 13. Docker Compose: los comandos se olvidan, los archivos se guardan

Ya tenemos Docker instalado. Ahora aparece otra pieza: Docker Compose, que sirve para describir servicios en un archivo de texto.

Ese archivo suele llamarse:

```text
docker-compose.yml
```

Un `docker-compose.yml` no es un hechizo ni un programa secreto. Tampoco es una cosa que sólo puedan tocar administradores de sistemas con barba, camiseta negra y blog friki.

Es un archivo de texto con instrucciones.

Ahí le decimos a Docker cosas como:

```text
quiero este servicio
quiero esta imagen
quiero estos puertos
quiero estas carpetas
quiero que arranque solo
```

## Ver uno de verdad

Antes de seguir, mira este ejemplo:

```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

No hace falta que lo entiendas entero.

Sólo mira la forma: parece una lista porque es una lista.

Le está diciendo a Docker que cree un servicio llamado `web`, que use una imagen llamada `nginx` y que conecte el puerto `8080` del servidor con el puerto `80` del contenedor.

Ahora mismo no importa Nginx.

Sólo quiero que veas que un `docker-compose.yml` es texto ordenado y eso es importante porque el texto se guarda.

Los comandos sueltos se olvidan; un archivo se puede copiar, revisar, respaldar y usar de nuevo dentro de dos años.

## Por qué esto importa

Si mañana el servidor muere y tienes tus archivos `docker-compose.yml` guardados, puedes reconstruir mucho más rápido.

No tendrás que recordar cada comando ni tendrás que preguntar a tu yo del pasado qué demonios hiciste.

El archivo lo contará por ti.

Por eso Docker Compose encaja tan bien con la idea de este manual: montar algo útil, pero también poder volver a montarlo.

---

# Capítulo 14. Nuestra carpeta Docker

En capítulos anteriores creamos una carpeta:

```text
/home/servi/docker
```

No fue un capricho.

Ahí guardaremos las instrucciones de nuestros servicios.

La estructura será sencilla:

```text
/home/servi/docker
│
├── immich
├── navidrome
├── jellyfin
└── ...
```

Cada servicio tendrá su propia carpeta.

Dentro de cada carpeta estará su `docker-compose.yml` y, si hace falta, sus archivos de configuración.

Esto no es obligatorio. Docker no te obliga a organizarte así. Pero hacerlo desde el principio evita muchos dolores de cabeza.

Un servidor doméstico desordenado acaba convirtiéndose en un trastero, y un trastero puede ser útil, pero encontrar algo ahí suele ser una mierda.

## Comprobar la carpeta

**Ejecutar en SERVIPLAN**

```bash
clear

ls -lah ~/docker
```

Si la carpeta existe, seguimos y si no existe, la creamos:

**Ejecutar en SERVIPLAN**

```bash
clear

mkdir -p ~/docker
```

Ya está, no hace falta hacer nada más por ahora.

---

# Capítulo 15. El primer mueble será Immich

Si tuviera que elegir una sola aplicación para justificar todo este proyecto, probablemente elegiría Immich, porque las fotos son recuerdos y los recuerdos merecen vivir en un sitio que controles tú.

No significa que Google Photos sea el demonio (ejem), tampoco significa que todo el mundo tenga que montar un servidor, pero hay posibilidades más allá de ellos:

- Puedes tener tus fotos en tu propia máquina.
- Puedes verlas desde el móvil.
- Puedes organizarlas.
- Puedes hacer copias.

Y puedes dejar de depender exclusivamente de que una empresa mantenga un servicio, cambie precios, cambie condiciones o decida que tu cuenta le importa entre cero y nada.

Eso es lo que vamos a montar primero.

## Recordatorio: Qué queremos conseguir

El objetivo de los próximos capítulos es éste:

```text
Servidor Ubuntu
      ↓
Docker
      ↓
Immich
      ↓
Navegador del Mac
```

Cuando terminemos, deberías poder abrir Immich desde el Mac escribiendo algo como:

```text
http://192.168.1.99:2283
```

Y eso será una victoria real, ni una idea ni teoría geopolítica: una app en tu propio servidor.
---

# Capítulo 16. Instalar Immich

Vamos a instalar Immich usando Docker Compose.

No vamos a inventar el archivo a mano si Immich ya proporciona uno oficial.

Esa es otra regla importante: cuando un proyecto mantiene una forma oficial de instalación, empieza por ahí. Ya habrá tiempo de personalizar cuando sepas qué estás haciendo.

## Crear la carpeta de Immich

Nuestros servicios vivirán en `~/docker`.

Creamos la carpeta de Immich y entramos en ella.

**Ejecutar en SERVIPLAN**

```bash
clear

mkdir -p ~/docker/immich
cd ~/docker/immich
```

A partir de ahora, todo lo que hagamos con Immich ocurrirá dentro de esta carpeta.

## Descargar los archivos oficiales

Immich publica los archivos necesarios para instalarlo con Docker Compose.

Los descargamos:

**Ejecutar en SERVIPLAN**

```bash
clear

wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
```

Ahora deberíamos tener dos archivos:

```text
docker-compose.yml
.env
```

El `docker-compose.yml` describe los contenedores que necesita Immich.

El `.env` guarda valores de configuración.

Dicho en cristiano:

```text
docker-compose.yml = qué piezas se montan
.env                = con qué valores se montan
```

Comprobamos:

**Ejecutar en SERVIPLAN**

```bash
clear

ls -lah
```

Deberías ver los dos archivos.

Si están, seguimos.

---

# Capítulo 17. Configurar Immich sin volverse loco

Antes de arrancar Immich, vamos a tocar el archivo `.env`.

No vamos a desmontarlo entero.

No vamos a convertir esto en una oposición.

Sólo vamos a revisar las variables importantes.

Abrimos el archivo:

**Ejecutar en SERVIPLAN**

```bash
clear

nano .env
```

Dentro busca, como mínimo, estas variables o algo muy parecido:

```text
UPLOAD_LOCATION=
DB_PASSWORD=
```

La primera indica dónde se guardarán las fotos.

La segunda es la contraseña interna de la base de datos.

## Dónde guardar las fotos

Una opción sencilla para empezar es:

```text
UPLOAD_LOCATION=./library
```

Eso significa:

> "Guarda la biblioteca dentro de una carpeta llamada `library`, al lado del `docker-compose.yml`."

Es fácil de entender, fácil de respaldar y fácil de encontrar.

Más adelante podrías mover la biblioteca a otra ruta si tienes discos separados o necesidades especiales. Pero ahora mismo queremos que funcione, no ganar un campeonato de arquitectura de almacenamiento.

## Contraseña de base de datos

Busca `DB_PASSWORD`.

Cuando termines en `nano`:

```text
CTRL + O  guardar
ENTER     confirmar
CTRL + X  salir
```

Sí, `nano` tiene una estética de nave soviética.

Pero funciona.

Pon una contraseña seria.

No uses:

```text
password
immich
1234
servidor
```

Esto no es una contraseña que vayas a escribir cada día, pero sigue siendo una contraseña, así que no la publiques ni la pegues en capturas ni la metas en un manual que vayas a enviar a nadie.

Recuerda: GESTOR DE CONTRASEÑAS, que hay muchos, de pago y de no tan pago.

---

# Capítulo 18. Arrancar Immich

Ya tenemos Docker.

Ya tenemos la carpeta.

Ya tenemos el `docker-compose.yml`.

Ya tenemos el `.env`.

Ahora levantamos Immich.

**Ejecutar en SERVIPLAN**

```bash
clear

cd ~/docker/immich
docker compose up -d
```

La primera vez tardará.

Docker tiene que descargar varias imágenes. Piensa en esas imágenes como cajas ya preparadas con piezas dentro.

Una para Immich.

Otra para la base de datos.

Otra para Redis.

Otra para machine learning.

No hace falta entender todos esos nombres ahora. Lo importante es que Docker está montando el servicio.

## Comprobar que los contenedores están vivos

Cuando termine, ejecuta:

**Ejecutar en SERVIPLAN**

```bash
clear

cd ~/docker/immich
docker compose ps
```

Deberías ver varios servicios en estado `running`, `healthy` o parecido.

Si alguno tarda un poco en ponerse sano, no entres en pánico. Algunos contenedores necesitan unos segundos o minutos para arrancar del todo.

## Probar desde el servidor

**Ejecutar en SERVIPLAN**

```bash
clear

curl -I http://localhost:2283
```

Si responde algo parecido a `HTTP/1.1 200 OK`, Immich está vivo.

Puede que la respuesta exacta cambie según versiones futuras.

La idea importante es que no debe decir que no puede conectar.

---

# Capítulo 19. Abrir Immich desde el Mac

Ahora viene una pequeña recompensa.

Desde el Mac, abre el navegador y entra en:

```text
http://192.168.1.99:2283
```

Cambia la IP por la de tu servidor si es distinta. Si todo ha ido bien, verás la pantalla inicial de Immich y aquí conviene parar un momento; hace un rato tenías un portátil viejo y ahora tienes un servidor Ubuntu con una aplicación real funcionando en Docker.

Enhorabuena, tienes una aplicación corriendo en una máquina que has montado tú.

## Crear el usuario inicial

Immich te pedirá crear el primer usuario, que será el usuario administrador. Pon un correo y una contraseña que vayas a recordar o guardar bien.

No hace falta que sea un correo de verdad si sólo vas a usarlo en casa, pero lo normal es poner uno que reconozcas.

A partir de aquí ya puedes entrar en Immich, aunque no hayamos importado todavía fotos.

Tampoco hemos configurado el móvil ni gestionado backups, peroooooo el primer mueble ya está dentro de la casa.

---

# Capítulo 20. Instalar la app móvil de Immich

Immich empieza a tener sentido de verdad cuando lo ves desde el móvil.

Porque las fotos normalmente las ves en el móvil.

Así que ve a la Store de aplicaciones e instala la app de Immich en tu teléfono.

Cuando la abras, te pedirá la dirección del servidor.

Dentro de casa (LAN) será algo como:

```text
http://192.168.1.99:2283
```

Cambia la IP por la tuya.

Después inicia sesión con el usuario que creaste en Immich.

## Probar con una foto

No empieces subiendo toda tu vida, empieza sólo con una, que para hacer pruebas te sobra: la subes.

Vas al navegador del Mac y refrescas Immich, compruebas que aparece.

```text
móvil
  ↓
servidor de casa
  ↓
biblioteca privada
```
---

# Capítulo 21. Qué hemos conseguido con Immich

Antes de correr al siguiente servicio, conviene mirar lo que acabamos de hacer:

- Hemos instalado Docker.
- Hemos descargado los archivos oficiales de Immich.
- Hemos configurado dónde se guardan las fotos.
- Hemos arrancado varios contenedores.
- Hemos abierto Immich desde el Mac.
- Hemos creado el primer usuario.
- Hemos probado la app móvil.

Eso ya es un servidor doméstico haciendo algo útil.

No completo, no perfecto, pero útil.

Y eso era exactamente lo que buscábamos.

## Punto de control

Antes de seguir, deberíamos tener esto:

```text
Docker instalado
docker compose funcionando
~/docker/immich creado
docker-compose.yml descargado
.env configurado
Immich arrancado
http://IP_DEL_SERVIDOR:2283 funcionando desde el Mac
usuario inicial creado
app móvil conectada
primera foto de prueba subida
```

Si todo eso está bien, podemos hablar de una parte fundamental: migrar las fotos que ya tienes en Google Photos, porque si no puedes sacar tus recuerdos de allí, esto se queda a medias.

---

# Capítulo 22. Migrar tus fotos desde Google Photos

Immich ya funciona. Estupendo, maravilloso.

Pero ahora viene la pregunta importante:

> ¿Y cómo saco mis fotos de Google Photos para meterlas aquí?

Porque si no podemos migrar las fotos, todo esto se queda en una demo bonita.

Un servidor de fotos privado sirve de poco si tus recuerdos siguen encerrados en Google Photos.

Así que vamos a hablar de la migración.

## Primero: Google Takeout

Google permite exportar tus datos desde una herramienta llamada Google Takeout.

La idea es sencilla:

```text
entras en Google Takeout
eliges Google Photos
pides una exportación (en este caso sólo de los datos Google Photos)
Google prepara unos archivos .zip
los descargas
```

No suele ser inmediato. A veces tarda minutos, horas o días. Depende de cuántas fotos tengas.

Cuando termine, tendrás uno o varios archivos `.zip`.

Esos archivos contienen tus fotos y también muchos archivos raros de metadatos.

No te asustes.

Google Photos no exporta las cosas como una carpeta limpita y preciosa.

Las exporta como buenamente le da la gana para hacerte lo más amargo posible salir de su corralito.

Por eso necesitamos una herramienta que entienda ese desastre.

## immich-go

La herramienta que vamos a usar se llama:

```text
immich-go
```

immich-go está pensada, entre otras cosas, para subir archivos de Google Photos Takeout a Immich.

Dicho en castellano normal:

```text
Google Photos exporta tus fotos en ZIPs raros.
immich-go lee esos ZIPs.
immich-go sube las fotos a Immich.
```

No forma parte obligatoria de la instalación de Immich:

- Primero montas Immich.
- Compruebas que funciona.
- Creas tu usuario.
- Pruebas que puedes entrar desde el navegador.
- Y después usas immich-go para migrar las fotos.

Ese orden es importante.

No intentes mudarte a una casa antes de comprobar que existe la casa.

## Crear una API key en Immich

Para que immich-go pueda subir fotos, Immich necesita darle permiso.

Ese permiso se da con una API key, que es una llave criptográfica, ni una llave física ni una llave de texto.

Sirve para que un programa pueda hablar con otro sin que tú tengas que escribir la contraseña cada dos minutos.

En Immich, entra con tu usuario administrador y busca la sección de API keys.

Crea una nueva, cópiala y guárdala con cuidado:

- No la pegues en un documento público.
- No la subas a GitHub.
- No la metas en un manual.
- No la mandes por correo.
- No la pongas en una captura.

Esa llave permite subir cosas a tu Immich, así que trátala como una contraseña.

## Dónde ejecutaremos immich-go

Puedes ejecutar immich-go desde el Mac o desde el servidor.

En este manual vamos a elegir un camino concreto para no marear:

```text
Ejecutaremos immich-go desde el Mac.
```

¿Por qué desde el Mac?

Porque lo normal es que descargues los ZIP de Google Takeout en tu ordenador personal.

Así evitamos explicar ahora cómo mover archivos enormes al servidor antes de importarlos.

La ruta será ésta:

```text
Google Takeout
     ↓
ZIPs descargados en el Mac
     ↓
immich-go ejecutado desde el Mac
     ↓
Immich en el servidor
```

Más adelante podrías hacerlo desde el servidor si lo prefieres.

Pero para este manual, camino simple:

```text
Mac → Immich
```

## Preparar una carpeta para los ZIP de Takeout

En el Mac, crea una carpeta para dejar los ZIP de Google Takeout.

**Ejecutar en MAC:**

```bash
clear

mkdir -p ~/Downloads/takeout-google-photos
open ~/Downloads/takeout-google-photos
```

El primer comando crea la carpeta.

El segundo la abre en Finder.

Ahora arrastra dentro de esa carpeta los archivos `.zip` que te haya dado Google Takeout.

La idea será tener algo parecido a esto:

```text
~/Downloads/takeout-google-photos/takeout-001.zip
~/Downloads/takeout-google-photos/takeout-002.zip
~/Downloads/takeout-google-photos/takeout-003.zip
```

Si tus archivos tienen otros nombres, no pasa nada.

Lo importante es que estén todos juntos y que sepas dónde están.

## Comprobar que los ZIP están ahí

Antes de instalar o ejecutar nada más, comprueba que el Mac ve los ZIP.

**Ejecutar en MAC:**

```bash
clear

ls -lh ~/Downloads/takeout-google-photos/*.zip
```

Si aparecen archivos `.zip`, vamos bien.

Si aparece un error parecido a:

```text
No such file or directory
```

significa una de estas tres cosas:

```text
la carpeta está vacía
los ZIP están en otra carpeta
los archivos no terminan en .zip
```

No sigas hasta que este comando muestre los ZIP.

## Instalar immich-go en el Mac

Aquí hay que tener cuidado: las formas de instalar immich-go pueden cambiar con el tiempo.

La opción más prudente es ir a la página oficial del proyecto y descargar la versión correspondiente a tu sistema.

Busca:

```text
immich-go GitHub releases
```

Descarga la versión para tu sistema:

```text
macOS Apple Silicon si usas un Mac moderno con chip M1, M2, M3 o posterior
macOS Intel si usas un Mac antiguo con procesador Intel
Linux si lo ejecutas desde un servidor Linux
Windows si estás en Windows
```

En este manual seguimos con Mac.

Después de descargarlo, deja el ejecutable en `Downloads` con este nombre exacto:

```text
immich-go
```

Es decir, queremos acabar con algo así:

```text
~/Downloads/immich-go
```

Si el archivo descargado tiene otro nombre, cámbiaselo desde Finder o desde Terminal.

Por ejemplo, si el archivo estuviera en `Downloads` y se llamara `immich-go-darwin-arm64`, podrías hacer esto:

**Ejecutar en MAC:**

```bash
clear

mv ~/Downloads/immich-go-darwin-arm64 ~/Downloads/immich-go
```

No copies este comando a ciegas si tu archivo tiene otro nombre.

Primero mira cómo se llama realmente.

**Ejecutar en MAC:**

```bash
clear

ls -lh ~/Downloads | grep immich
```

Cuando veas el nombre real, si hace falta, lo renombras.

## Dar permiso de ejecución

En Mac o Linux puede que tengas que dar permiso de ejecución al archivo.

**Ejecutar en MAC:**

```bash
clear

chmod +x ~/Downloads/immich-go
```

Ahora comprueba que responde:

**Ejecutar en MAC:**

```bash
clear

~/Downloads/immich-go --help
```

Si aparece ayuda en pantalla, vamos bien.

Si aparece algo parecido a:

```text
permission denied
```

revisa el paso de `chmod +x`.

Si macOS se queja de que no puede abrirlo porque viene de Internet, tendrás que autorizarlo desde los ajustes de seguridad de macOS o abrirlo manualmente confirmando que confías en él.

No descargues ejecutables de sitios raros.

Descárgalo del proyecto oficial.

## Probar conexión con Immich

Antes de subir fotos, comprueba que desde el Mac puedes abrir Immich en el navegador:

```text
http://192.168.1.99:2283
```

Cambia la IP por la de tu servidor.

También puedes comprobarlo desde Terminal.

**Ejecutar en MAC:**

```bash
clear

curl -I http://192.168.1.99:2283
```

Si responde algo parecido a `HTTP/1.1 200 OK`, el Mac puede hablar con Immich.

Si no responde, immich-go tampoco va a poder subir nada.

Primero arregla la conexión con Immich, después seguimos.

## Preparar una prueba pequeña

No empieces importando 200 GB a lo loco, primero crea una carpeta de prueba:

**Ejecutar en MAC:**

```bash
clear

mkdir -p ~/Downloads/takeout-prueba
open ~/Downloads/takeout-prueba
```

Ahora copia dentro **un solo ZIP pequeño** de Google Takeout.

Puedes hacerlo arrastrando el archivo desde Finder.

O puedes copiar el primer ZIP que encuentre Terminal en la carpeta grande.

**Ejecutar en MAC:**

```bash
clear

cp "$(ls ~/Downloads/takeout-google-photos/*.zip | head -n 1)" ~/Downloads/takeout-prueba/
ls -lh ~/Downloads/takeout-prueba/*.zip
```

Si el último comando muestra un ZIP, la prueba está preparada.

## Ejecutar la prueba pequeña

Ahora ejecuta immich-go contra esa carpeta de prueba.

**Ejecutar en MAC:**

```bash
clear

~/Downloads/immich-go upload from-google-photos \
  --server=http://192.168.1.99:2283 \
  --api-key=TU_API_KEY \
  ~/Downloads/takeout-prueba/*.zip
```

Cambia:

```text
192.168.1.99
```

por la IP de tu servidor.

Cambia:

```text
TU_API_KEY
```

por la API key que has creado en Immich.

No publiques ese comando con la API key real.

Después entra en Immich desde el navegador y comprueba:

```text
las fotos aparecen
las fechas tienen sentido
no has duplicado todo como un animal
```

Cuando una prueba pequeña funciona, entonces ya lanzas la importación grande.

Esto no es cobardía.

Es sentido común.

## Ejecutar la importación grande

Cuando la prueba pequeña haya ido bien, ejecuta immich-go contra todos los ZIP de Takeout.

**Ejecutar en MAC:**

```bash
clear

~/Downloads/immich-go upload from-google-photos \
  --server=http://192.168.1.99:2283 \
  --api-key=TU_API_KEY \
  ~/Downloads/takeout-google-photos/*.zip
```

Otra vez:

```text
192.168.1.99 = IP de tu servidor
TU_API_KEY   = API key de Immich
```

Si tienes muchos ZIP, esto puede tardar bastante.

No cierres el Mac a mitad.

No lo mandes a dormir.

No lo ejecutes justo antes de irte con el portátil bajo el brazo.

## Si algo falla

Si falla, revisa en este orden:

```text
¿Immich abre desde el navegador del Mac?
¿La IP del servidor es correcta?
¿El puerto 2283 es correcto?
¿La API key está bien copiada?
¿Los ZIP existen en la ruta indicada?
¿immich-go responde con --help?
```

Comandos útiles para comprobarlo:

**Ejecutar en MAC:**

```bash
clear

curl -I http://192.168.1.99:2283
ls -lh ~/Downloads/immich-go
~/Downloads/immich-go --help
ls -lh ~/Downloads/takeout-google-photos/*.zip
```

No cambies diez cosas a la vez.

Cambia una.

Prueba.

Mira el error.

Y sólo después cambia otra.

Eso no garantiza que no te enfades.

Pero al menos te enfadarás con método.

## Qué hemos conseguido

Con Immich e immich-go ya tenemos una vía real para salir de Google Photos:

```text
Google Photos
     ↓
Google Takeout
     ↓
immich-go
     ↓
Immich en casa
```

Este capítulo es importante.

No estamos instalando una aplicación por postureo.

Estamos recuperando control sobre nuestras fotos.

A partir de ahora, el camino se repite bastante: crear carpetas, preparar un `docker-compose.yml`, levantar el servicio —música, vídeos, contraseñas, lo que quieras— y probarlo en el navegador.

---

# Capítulo 23. Navidrome: nuestro Spotify particular

Ya tenemos una carpeta para música:

```text
/srv/media/music
```

Ahora vamos a instalar Navidrome.

Navidrome es un servidor de música.

Tú dejas tus discos en `/srv/media/music` y Navidrome los organiza para que puedas escucharlos desde el navegador o desde una app compatible.

No es Spotify ni lo pretende ni tiene el catálogo de Spotify.

No te recomienda la canción de moda.

Pero reproduce tu música.

La tuya, la que tú tienes, y eso tiene su gracia.

## Antes de instalar: dónde va la música

Para que Navidrome encuentre música, los archivos tienen que acabar dentro de esta carpeta del servidor:

```text
/srv/media/music
```

Cómo copies esos archivos hasta ahí puede variar.

Puedes usar un disco USB, puedes copiarlos desde otro ordenador, puedes usar una herramienta gráfica o puedes usar comandos. Este manual no va a convertir la copia de archivos en otro proyecto paralelo.

La idea importante es esta:

```text
música en /srv/media/music
      ↓
Navidrome la lee
      ↓
la escuchas desde navegador o app
```

Si ahora no tienes música preparada, no pasa nada. Instala Navidrome primero. Ya llenarás la estantería después.

## Crear carpeta de Navidrome

**Ejecutar en SERVIPLAN**

```bash
clear

mkdir -p ~/docker/navidrome/data
cd ~/docker/navidrome
```

## Crear docker-compose.yml

Abrimos el archivo:

**Ejecutar en SERVIPLAN**

```bash
clear

nano docker-compose.yml
```

Pega esto:

```yaml
services:
  navidrome:
    image: deluan/navidrome:latest
    container_name: navidrome
    user: "1000:1000"
    ports:
      - "4533:4533"
    restart: unless-stopped
    environment:
      ND_SCANSCHEDULE: "1h"
      ND_LOGLEVEL: "info"
      ND_SESSIONTIMEOUT: "24h"
      ND_BASEURL: ""
    volumes:
      - "./data:/data"
      - "/srv/media/music:/music:ro"
```

Guarda y sal.

Fíjate en esta línea:

```yaml
- "/srv/media/music:/music:ro"
```

Fuera del contenedor, en Ubuntu, la música vive en:

```text
/srv/media/music
```

Dentro del contenedor, Navidrome la ve como:

```text
/music
```

Y el `ro` significa `read only`, sólo lectura.

Navidrome puede leer la música, pero no debería ponerse a modificar tu biblioteca como Pedro por su casa.

## Arrancar Navidrome

**Ejecutar en SERVIPLAN**

```bash
clear

cd ~/docker/navidrome
docker compose up -d
```

Comprobamos:

**Ejecutar en SERVIPLAN**

```bash
clear

docker ps | grep navidrome
```

## Abrir desde el Mac

En el navegador del Mac:

```text
http://192.168.1.99:4533
```

Cambia la IP por la de tu servidor.

La primera vez crearás el usuario administrador.

Después, Navidrome escaneará la música que pongas en:

```text
/srv/media/music
```

Si todavía no hay música, no pasa nada.

Ya llegará.

Lo importante es que el mueble está montado.

Ahora sólo falta llenarlo.

---

# Capítulo 24. Jellyfin: nuestro pequeño videoclub

Jellyfin hará con las películas algo parecido a lo que Navidrome hace con la música.

Tú dejas vídeos en:

```text
/srv/media/movies
```

y Jellyfin los organiza para verlos desde el navegador, la tele, el móvil o una app compatible.

No estamos montando Netflix.

Estamos montando una forma cómoda de ver nuestros propios vídeos desde el servidor.

## Antes de instalar: dónde van los vídeos

Para películas usaremos:

```text
/srv/media/movies
```

Para series usaremos:

```text
/srv/media/tv
```

Otra vez, el método exacto para copiar archivos no es lo central del capítulo.

Lo central es entender la estructura:

```text
/srv/media/movies  → películas
/srv/media/tv      → series
```

Si más adelante quieres una forma cómoda de abrir carpetas del servidor desde el Mac, existe Samba. Pero no lo necesitamos para entender Jellyfin ni para terminar este manual.

Samba se queda para el capítulo final de cosas opcionales.

## Crear carpeta de Jellyfin

**Ejecutar en SERVIPLAN**

```bash
clear

mkdir -p ~/docker/jellyfin/config
mkdir -p ~/docker/jellyfin/cache
cd ~/docker/jellyfin
```

## Crear docker-compose.yml

**Ejecutar en SERVIPLAN**

```bash
clear

nano docker-compose.yml
```

Pega:

```yaml
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    user: "1000:1000"
    ports:
      - "8096:8096"
    volumes:
      - "./config:/config"
      - "./cache:/cache"
      - "/srv/media/movies:/media/movies:ro"
      - "/srv/media/tv:/media/tv:ro"
    restart: unless-stopped
```

Guarda y sal.

## Arrancar Jellyfin

**Ejecutar en SERVIPLAN**

```bash
clear

cd ~/docker/jellyfin
docker compose up -d
```

Comprobamos:

**Ejecutar en SERVIPLAN**

```bash
clear

docker ps | grep jellyfin
```

## Abrir desde el Mac

En el navegador:

```text
http://192.168.1.99:8096
```

La primera vez Jellyfin abrirá su asistente inicial.

Crearás usuario.

Elegirás idioma.

Y añadirás bibliotecas.

Para películas, la ruta dentro de Jellyfin será:

```text
/media/movies
```

Para series:

```text
/media/tv
```

No uses `/srv/media/movies` dentro de Jellyfin.

Dentro del contenedor, esa carpeta se llama `/media/movies`.

Esto parece una tontería, pero es una de esas cosas que al principio vuelven loco a cualquiera.

Fuera del contenedor:

```text
/srv/media/movies
```

Dentro del contenedor:

```text
/media/movies
```

Misma carpeta real.

Nombre distinto según desde dónde la mires.

Bienvenido a Docker.

---

# Capítulo 25. Punto de control: ya hay muebles

Llegados aquí, nuestro servidor ya no es una idea.

Deberíamos tener:

```text
Immich funcionando en http://IP_DEL_SERVIDOR:2283
Navidrome funcionando en http://IP_DEL_SERVIDOR:4533
Jellyfin funcionando en http://IP_DEL_SERVIDOR:8096
```

Y además:

```text
Google Photos puede migrarse hacia Immich con immich-go
la música vive en /srv/media/music
las películas viven en /srv/media/movies
las series viven en /srv/media/tv
```

Ahora sí empieza a parecer un servidor doméstico.

No perfecto.

No blindado.

No terminado.

Pero vivo.

Y vivo significa que ya puedes usarlo.

El siguiente paso será hacer algo que mucha gente deja para el final y que debería estar presente desde el principio:

copias de seguridad.

Porque un servidor sin backup es una forma elegante de pedirle una hostia al destino.

---

# Capítulo 26. Copias de seguridad: porque Murphy existe

Ya tenemos servicios funcionando.

Eso está muy bien.

Pero ahora toca hablar de algo menos divertido: las copias de seguridad.

Y sí, da pereza.

A mí también me la daba.

Hasta que entiendes una verdad muy sencilla:

> Todo disco muere.

Da igual que sea SSD.

Da igual que sea HDD.

Da igual que sea caro.

Da igual que tenga una marca estupenda.

Tarde o temprano todos mueren.

La única incógnita es cuándo.

Por eso un servidor sin copias de seguridad no es un servidor.

Es una apuesta.

Y las apuestas suelen acabar regular.

## La regla 3-2-1

Existe una regla bastante conocida para copias de seguridad.

Se llama regla 3-2-1.

Dice algo así:

```text
3 copias de los datos
2 tipos de almacenamiento diferentes
1 copia fuera de casa
```

No es una ley física.

No vendrá la policía de los backups si no la cumples.

Pero es una referencia útil.

## Qué merece copia

No todo merece el mismo esfuerzo.

Las fotos sí.

La documentación importante sí.

Las contraseñas y archivos críticos, por supuesto.

La música, quizá sí.

Las películas, no necesariamente.

Si pierdes una película, fastidia.

Si pierdes fotos familiares, te acuerdas de todos los santos.

Por eso conviene clasificar:

```text
Crítico:
  fotos
  documentos importantes
  contraseñas
  configuración del servidor

Importante:
  música

Poco importante:
  películas
  descargas temporales
  basura acumulada
```

Un servidor doméstico no consiste en copiar absolutamente todo a todas partes.

Consiste en pensar qué importa de verdad.

## Este manual no va a montar un sistema de backup perfecto

Podríamos dedicar otros veinte capítulos a backups.

Podríamos hablar de discos externos.

Podríamos hablar de nubes cifradas.

Podríamos hablar de rclone, Borg, Restic, snapshots, S3 y mil cosas más.

Pero entonces este manual dejaría de ser un manual base y se convertiría en una oposición a administrador de sistemas con café frío.

Así que aquí nos quedamos con la idea principal:

```text
identifica qué importa
haz copia de lo importante
comprueba que puedes recuperar algo
no confíes en una sola máquina
```

Lo demás existe.

Lo veremos al final como ideas para quien quiera aburrirse voluntariamente.

---

# Capítulo 27. Verificar backups: confiar está bien, comprobar está mejor

Hacer backups está bien.

Pero comprobarlos es mejor.

Porque hay pocas sensaciones más ridículas que descubrir que llevas meses haciendo copias inútiles.

Un backup que nunca has probado es una promesa.

No una garantía.

Por eso conviene tener alguna forma de verificar que las copias existen, que tienen tamaño razonable y que no fallaron silenciosamente.

No hace falta montar un sistema de control de la NASA.

Pero sí conviene revisar.

## Qué comprobar

Como mínimo:

```text
la copia existe
hay archivos recientes
el tamaño parece razonable
puedes abrir o recuperar algún archivo
sabes dónde están las claves si la copia está cifrada
```

Si ayer tenías 30 GB de fotos y hoy tu backup ocupa 12 KB, algo huele a muerto.

## Documentar también es parte del backup

Hay otra copia que mucha gente olvida: la documentación.

Si mañana el servidor muere, no basta con tener archivos.

También necesitas saber:

```text
qué instalaste
qué carpetas usaste
qué puertos abriste
qué usuarios creaste
qué contraseñas o claves necesitas
qué servicios eran importantes
```

Un backup sin instrucciones puede ser un puzzle.

Y un puzzle, cuando estás cansado y cabreado porque algo se ha roto, es una maravilla de la literatura fantástica.

---

---

# Cierre de la Parte I

Hasta aquí, el servidor está pensado para funcionar dentro de tu red local.

Ya tienes una base útil:

- Ubuntu Server instalado.
- Acceso por SSH desde el Mac.
- Docker funcionando.
- Immich para fotos.
- Navidrome para música.
- Jellyfin para vídeos.
- Una idea clara de qué debes respaldar y cómo comprobar que tus copias existen.

No abras puertos del router ni expongas estos servicios directamente a Internet.

El acceso desde fuera de casa, Tailscale, mantenimiento avanzado, recuperación y automatización se tratarán aparte.

Fin de la Parte I.
