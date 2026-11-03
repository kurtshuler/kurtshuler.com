---
title: "Sustituir el Livebox de Orange España por una ONT Huawei HG8310M"
slug: "orange-livebox-to-huawei-hg8310m-ont"
date: 2026-11-02
lastmod: 2026-11-03
draft: false
# Staging: reachable by URL, hidden from lists, feeds and sitemap. Remove this block to publish.
build:
  list: never
  render: always
sitemap:
  disable: true
description: "Cómo quitar el Livebox 6 de Orange España de tu red y conectar tu propio router directamente a la fibra con una ONT Huawei HG8310M de 15 euros. Qué copiar del Livebox, el ajuste que me hizo perder un intento de registro y los números de antes y de después."
summary: "Saqué el Livebox de Orange de mi armario de red y puse en su lugar una ONT Huawei HG8310M de 15 euros, con un gateway UniFi haciendo de router. Aquí tienes lo que hay que copiar del Livebox antes de desenchufarlo, el ajuste de autenticación que me costó un intento de registro y los números de antes y de después."
tags: ["fiber", "orange", "unifi", "ont", "livebox"]
categories: ["Homelab"]
series: ["Orange España fiber"]
showTableOfContents: true
showHero: false
---

Durante tres años el Livebox 6 de Orange España estuvo en una balda de mi armario de red haciendo exactamente una cosa útil: convertir luz en Ethernet. Todo lo demás que hacía estorbaba. Era un segundo router delante de mi gateway UniFi UDM Pro Max (doble NAT), emitía Wi-Fi quisiera yo o no, y una semana de agosto lo pillé anunciando una copia de mi SSID de IoT fuera de mi cortafuegos. El 2 de noviembre de 2026 le quité la fibra y la enchufé a una Huawei HG8310M de segunda mano, un terminal óptico de red (ONT) de 15 euros sin router dentro. Pasaron 42 minutos entre el primer ping perdido y una IP pública funcionando, y el Livebox está ahora en un cajón.

Esta entrada es la guía que me habría gustado encontrar cuando empecé: qué exige Orange España de verdad en el cable, qué tienes que copiar del Livebox antes de desenchufarlo, qué ONT comprar y el ajuste que está mal en la mayoría de las guías de los foros.

Una aclaración de alcance antes de nada: esto es para una línea sobre la red de fibra propia de Orange (lo que en los foros se llama *fibra directa*). Si tu servicio de Orange llega por la planta de Movistar como línea indirecta NEBA o VULA, el registro de la ONT usa la forma hexadecimal de la contraseña y algunos detalles de abajo cambian.

## ¿Merece la pena hacerlo?

Probablemente no, salvo que algo del Livebox te esté molestando de verdad. Quiero ser honesto con el tamaño de la ganancia, porque los foros no lo son.

Lo que ganas:

- Un router en vez de dos, así que las redirecciones de puertos son de un solo salto y el NAT loopback funciona.
- Ningún equipo controlado por el operador dentro de tu perímetro. Sin gestión remota TR-069, sin firmware sorpresa, sin radios que no has encendido tú.
- Una balda del armario con mucho más sitio. ¡Este era mi motivo número uno!

{{< figure src="img/11-huawei-on-shelf.jpg" figureClass="fig-right fig-30" alt="El mismo armario después: una Huawei HG8310M del tamaño de una mano, tumbada en la balda junto a un mini PC" caption="Después. La HG8310M tiene el tamaño de una baraja de cartas y va tumbada en la balda. La parte de arriba del rack queda vacía." >}}

{{< figure src="img/12-closet-before.jpg" figureClass="fig-right fig-30" alt="Armario de red antes: un Livebox 6 de pie sobre un pequeño rack, encima de un NAS Synology, dos mini PC y un gateway UniFi" caption="Antes. El Livebox 6 de pie sobre el rack, el único sitio donde cabía, con su propia fuente y un cable bajando hasta el gateway." >}}

Lo que pierdes:

- **El fijo.** Orange bloquea el registro SIP de terceros desde 2019 más o menos. Si usas el teléfono fijo, para aquí.
- **El soporte de primer nivel.** Cuenta con que Orange te pida volver a enchufar el Livebox antes de mirar nada. Guárdalo; es suyo y lo devuelves cuando te des de baja.
- **Actualizaciones de firmware y diagnóstico remoto** en el lado de la ONT. La HG8310M que compres de segunda mano corre lo que corre.
- **Orange TV, solo *SI* usas sus DESCODIFICADORES.** Si ves Orange TV con la app de Orange en la tele, el Apple TV, etc., no pasa nada.

**Orange TV, si la ves con la app.** Esta es la duda que frena a la mayoría, y es un problema del descodificador, no de la red. El descodificador físico de Orange necesita IPTV multicast en una segunda VLAN (la 838) entregada en un puerto dedicado del router, y eso es engorroso en cualquier router de terceros. La app de Orange TV en un Apple TV o en el móvil es streaming HTTPS normal y le da igual lo que haya aguas arriba. Orange vende la misma app a clientes de Digi y de Movistar como [“Orange TV Libre”](https://bandaancha.eu/articulos/orange-tv-libre-abre-clientes-cualquier-11347) (un usuario de Digi confirmó en bandaancha en 2024 que el descodificador falla fuera de la red con el error S80 mientras [la aplicación funciona sin problemas fuera de la red de Orange](https://bandaancha.eu/foros/configurar-orange-router-neutro-mantener-1752283)): la app no puede depender de un router de Orange. Nosotros no tenemos descodificador, así que no cambió nada.

La velocidad, igual. Mi referencia de 53 días con el Livebox era una mediana de 869 Mbps de bajada y 893 de subida. La primera prueba con la Huawei dio 837 de bajada y 840 de subida. Eso entra en la variación normal según la hora del día, y el objetivo nunca fue la velocidad.

## ¿Qué necesita Orange España de tu equipo exactamente?

Tres cosas, y la respuesta es más sencilla de lo que las guías dan a entender.

1. **Una ONT GPON registrada con la contraseña PLOAM de tu línea.** Es una cadena de 10 caracteres que identifica tu abono ante el terminal de línea óptica (OLT) de la central. El Livebox la tiene en su interfaz de gestión y tú la copias a la ONT nueva.
2. **Tráfico de Internet etiquetado con la VLAN 832, prioridad 802.1p 0, DHCP a secas.** Sin PPPoE, sin usuario, sin opciones DHCP. El puerto WAN de tu router pone la etiqueta.
3. **Nada más.** En mi línea no hizo falta clonar la MAC. La IP pública fue la misma antes y después, lo que te dice que Orange ata la concesión a la credencial PLOAM y no al hardware.

## La (des)conexión francesa de Orange

Ahora la trampa si investigas cómo hacerlo por Internet: **Orange Francia también usa la VLAN 832**, y las guías francesas superan a las españolas diez a una. ¡Pero la configuración de Francia no es la de España!

Francia necesita prioridad 802.1p 6 y las opciones DHCP 60, 77 y 90 con una identidad de cuenta `fti/`. **España no necesita nada de eso.** Si una guía menciona `fti/`, la opción 90 o CoS 6, es francesa, y si la sigues fallará. Lo que pide Orange España es estrictamente lo más fácil de los dos.

## ¿Qué ONT comprar?

La que corresponda al fabricante de la OLT de tu zona, y el problema es que eso no se puede averiguar a distancia. Una ONT que no encaja no falla del todo: se registra y luego va lenta, que es el peor fallo posible de diagnosticar. [Hay informes en bandaancha.eu](https://bandaancha.eu/foros/que-ont-compatibles-olt-nokia-alcatel-1745781) de ONT Nokia sobre planta ZTE cayendo de 600/600 a unos 300/86 Mbps.

| Fabricante de la OLT en tu zona | ONT a comprar |
|---|---|
| Alcatel-Lucent / Nokia (despliegues antiguos de Orange) | Nokia G-010G-P |
| ZTE (planta heredada de Jazztel) o Huawei | Huawei HG8310M o HG8010H |

Las dos cuestan entre 10 y 15 euros de segunda mano en Wallapop. Compré una de cada por si acaso y la Huawei funcionó a la primera, así que la Nokia sigue en su caja. La elección de la HG8310M salió de [un hilo de bandaancha de abril de 2026](https://bandaancha.eu/foros/orange-tv-funcionando-ont-huawei-hg8310m-1759776) en el que un usuario la puso a funcionar con Orange España, televisión incluida. Evita la ZTE F601 ([fallos documentados en fibra directa de Orange](https://bandaancha.eu/foros/ftth-orange-directo-no-consigo-funcionar-1745136) con la configuración correcta) y la Ubiquiti UFiber Loco ([su campo PLOAM no tiene la longitud correcta](https://bandaancha.eu/foros/ubiquiti-ufiber-loco-router-asus-rt-1745605) para algunos Livebox).

Solo necesitas un latiguillo de fibra si el de la pared no llega. Es **SC/APC, el conector verde en ángulo**, monomodo. No el SC/UPC azul.

## ¿Qué hay que copiar del Livebox antes de tocar nada?

Tres valores, todos en la interfaz de administración del Livebox en `192.168.1.1`:

1. **La contraseña PLOAM.** *Información y diagnóstico > Parámetros del sistema > FIBRA*, fila 2.7, “contraseña de ONT”. El Livebox la muestra dos veces: la forma ASCII de 10 caracteres y la forma hexadecimal entre paréntesis empezando por `0x`. Apunta las dos tal cual aparecen. Algunos firmwares de ONT quieren una forma y otros la otra; la Huawei toma la ASCII. El hilo de bandaancha sobre [cómo sacar la clave GPON del Livebox](https://bandaancha.eu/foros/sacar-clave-gpon-idont-ploam-password-1742394) cubre los modelos antiguos y los casos raros.
2. **La dirección MAC WAN del Livebox.** Seguramente no la necesites, pero si en 30 minutos no llega IP, clonarla en la WAN de tu router es lo primero que hay que probar.
3. **La potencia óptica de recepción.** Misma página, fila 2.6, “potencia recepción”. La mía marcaba -19,0 dBm. Tras el cambio, una lectura mucho peor en la ONT nueva apunta al conector, no a la ONT.

{{< figure src="img/00-livebox-fibra-ploam.jpg" alt="Página de administración del Livebox Fibra, Parámetros del sistema, pestaña FIBRA, con la fila 2.7 contraseña de ONT señalada" caption="La página FIBRA del Livebox. La fila 2.7 es la contraseña PLOAM en sus dos formas; la 2.6, la potencia de recepción. Captura de un miembro de bandaancha, del [hilo sobre la PLOAM](https://bandaancha.eu/foros/sacar-clave-gpon-idont-ploam-password-1742394); los valores los difuminó el autor original." >}}

## ¿Cómo me preparé?

- Un portátil con una dirección fija para la red de banco de la ONT.
- Material de limpieza de fibra (alcohol isopropílico y toallitas sin pelusa, o un limpiador de un clic) y tapones antipolvo para la óptica del Livebox y para el latiguillo de la pared.
- La consola de UniFi, una ventana SSH y la interfaz del Livebox, todo abierto antes de desenchufar nada.

## ¿Cómo se configura la Huawei HG8310M?

Primero en el banco, con la fibra todavía en el Livebox.

1. Conecta un portátil al único puerto Ethernet de la Huawei, ponle la IP fija `192.168.100.2/24` y abre `http://192.168.100.1`.

   {{< figure src="img/01-mac-static-ip-bench.jpg" figureClass="fig-right fig-40" alt="Ajustes de red de macOS con una IP manual 192.168.100.2 y máscara 255.255.255.0" caption="El montaje de banco: el portátil con la IP fija 192.168.100.2 para poder entrar en la interfaz web de la ONT en 192.168.100.1." >}}

2. Entra en la página de la Huawei. En el aparato no hay credenciales impresas. Las de fábrica son `telecomadmin` / `admintelecom`, y `root` / `adminHG` como alternativa ([hack-gpon.org](https://hack-gpon.org/ont-huawei-hg8010h/) mantiene la lista de toda la familia HG80xx, y [maquinasvirtuales.eu](https://www.maquinasvirtuales.eu/ont-configurar-huawei-hg8010h/) tiene capturas de la misma interfaz web).

{{< figure src="img/02-huawei-device-info-o1.jpg" alt="Página Device Information de la Huawei HG8310M con ONT Registration Status O1 (Initial state) y ONT ID 255" caption="Recién sacada de la caja, en el banco y sin fibra: estado de registro O1, ONT ID 255. Número de serie e identificador de producto tapados." >}}

{{< figure src="img/03-huawei-optical-no-fiber.jpg" alt="Página de información óptica de la Huawei con las potencias TX y RX en guiones" caption="Optical Information sin fibra: guiones en TX y RX. Échale un vistazo ahora para reconocer la página luego." >}}

3. Configura la **autenticación de la ONT**. Aquí es donde eché a perder mi primer intento de registro, y donde creo que la mayoría de las guías en español confunden.

   Pon **Authentication Mode** en **Password**, introduce la **PLOAM** en ASCII, pulsa **Apply** y **reinicia la ONT** (*System Tools > Reboot*). Cuando lo hice así, pasó a O5 unos diez minutos después de conectar la fibra.

   ¡La primera vez la fastidié! Elegí **LOID** en vez de **Password** como **Authentication Mode**, porque las guías de los foros dicen “introduce la PLOAM” sin decir dónde, y el campo PLOAM de LOID es el sitio obvio para teclear una cadena de 10 caracteres. Eso hice. La ONT estuvo diez minutos rebotando entre O3 y O2: la OLT la descubría y acto seguido la desactivaba. Si acabas en O2, seguramente has cometido mi mismo error. No sigas reintentando. Los registros fallidos repetidos pueden hacer que bloqueen tu PLOAM en la OLT, y eso tumba también el Livebox hasta que Orange lo desbloquee.

{{< figure src="img/04-huawei-auth-loid-wrong.jpg" alt="Página ONT Authentication de la Huawei con Authentication Mode en LOID y la PLOAM metida en el campo LOID" caption="Mal. Authentication Mode = LOID con la PLOAM en el campo LOID. La OLT descubre la ONT y la desactiva al momento (O3 a O2, una y otra vez). Esto me costó el primer intento de registro." >}}

{{< figure src="img/07-huawei-auth-password-right.jpg" alt="Página ONT Authentication de la Huawei con Authentication Mode en Password, Password Mode ASCII string y el número de serie GPON de fábrica" caption="Bien. Authentication Mode = Password, Password Mode = ASCII string, la PLOAM en el campo Password, número de serie sin tocar. Apply, reinicio y O5 al siguiente intento." >}}

Deja en paz el número de serie GPON. Todos los casos de éxito documentados en España conservaron el serial propio de la ONT; falsear el serial es una complicación francesa y polaca que aquí no aplica.

Una cosa más que comprobar en el banco: el firmware de mi unidad (V3R015C10S106) tiene cuatro pestañas, Status, LAN, Security y System Tools. **No hay pestaña WAN**, lo que significa que la ONT no puede etiquetar la VLAN 832 por sí misma. La OLT entrega la VLAN etiquetada y tu router tiene que encargarse. Esto contradice al menos una guía muy compartida que en realidad hablaba de [la HG8120C](https://bandaancha.eu/foros/ont-router-neutro-sin-perder-tv-1759672), un aparato distinto con router dentro.

## ¿Cómo se configura el router?

{{< figure src="img/09-unifi-wan1-vlan-832.jpg" figureClass="fig-right fig-30" alt="Panel de ajustes de Internet 1 en UniFi con VLAN ID activado y puesto a 832, QoS Tag y MAC Address Clone sin marcar, conexión IPv4 DHCP" caption="Toda la configuración del lado del router en un gateway UniFi: VLAN ID 832, nada más marcado, DHCP." >}}

Yo tengo un UniFi Dream Machine Pro Max, así que estos son campos de UniFi, pero cualquier router serio tiene los mismos cuatro ajustes.

*Settings > Internet > tu WAN*:

| Campo | Valor |
|---|---|
| Conexión IPv4 | DHCPv4 |
| VLAN ID | Activado, **832** |
| QoS tag | Ninguno |
| DHCP CoS | Ninguno |
| Clonar dirección MAC | Desactivado, salvo que no llegue IP en 30 minutos |
| IPv6 | Desactivado (la fibra residencial de Orange España no te da IPv6) |

“*Ninguno*” en los dos campos de prioridad es lo correcto. Una etiqueta enviada sin prioridad explícita lleva 802.1p 0, que es exactamente lo que quiere Orange España. No pongas nada en el QoS tag: se aplica a todo el tráfico de salida y hay informes de un UDM cayendo a 30 Mbps con ese campo puesto. Si usas OPNsense en vez de UniFi, [este hilo de bandaancha](https://bandaancha.eu/foros/opnsense-orange-ftth-espana-ont-propia-1758841) y [este del foro de OPNsense](https://forum.opnsense.org/index.php?topic=50206.0) cubren la misma configuración de la VLAN 832, el segundo incluyendo la VLAN de televisión para un descodificador.

Puedes hacer esto con el Livebox todavía en servicio si configuras un puerto libre del router como segunda WAN, lo que convierte la noche del cambio en mover un cable. Yo lo hice de la forma simple, en la WAN principal, durante el corte.

## ¿Cómo es el cambio en sí?

En orden, con mis horas del 2 de noviembre:

1. **12:25** Fibra fuera del Livebox. Tapa la óptica del Livebox. Inspecciona y limpia el conector del latiguillo de la pared. Fibra a la Huawei, una vez. Cada ciclo de conexión en ese conector SC/APC es desgaste en la única pieza que no puedes sustituir tú, así que enchúfalo y déjalo.
2. Ethernet del puerto LAN de la Huawei al puerto WAN del router, en lugar del cable que iba al puerto 1 del Livebox.
3. **12:35** La página Status de la Huawei muestra **O5 (Operation state)**. Si se queda diez minutos entre O1 y O4, algo está mal. Apunta la potencia RX.
4. **12:54** IP pública visible en la WAN de UniFi, una vez activada la VLAN 832. Una dirección `192.168.1.x` aquí significa que la etiqueta no se está aplicando.
5. Comprueba el acceso desde fuera con el móvil en datos. Mis servicios detrás de Cloudflare volvieron solos porque la IP pública no había cambiado.

{{< figure src="img/08-huawei-device-info-o5.jpg" alt="Página Device Information de la Huawei con ONT Registration Status O5 (Operation state) y ONT ID 90" caption="Registrada: O5 (Operation state), y la OLT ha asignado el ONT ID 90 en lugar del 255 provisional." >}}

{{< figure src="img/05-huawei-optical-after.jpg" alt="Página de información óptica de la Huawei con TX 2,37 dBm y RX -22,60 dBm" caption="Optical Information con la fibra conectada: TX +2,37 dBm, RX -22,60 dBm, 44 °C. El Livebox marcaba -19,0 en la misma fibra." >}}

{{< figure src="img/10-unifi-latency-gap.jpg" alt="Gráfica de latencia del panel de UniFi de las últimas 24 horas con un hueco hacia las 12:25 y latencia sin cambios por lo demás" caption="La gráfica de latencia de UniFi durante el cambio. El hueco de las 12:25 es el movimiento de la fibra; a ambos lados todo es igual." >}}

Luego déjalo en paz y que tu monitorización trabaje. La mía siguió registrando latencia y pruebas de velocidad durante todo el proceso, y de ahí salen los números de abajo.

## ¿Qué números de rendimiento salieron?

| | Livebox 6 (referencia de 53 días) | Huawei HG8310M (primer día) |
|---|---|---|
| Bajada, mediana | 869 Mbps | 837 Mbps (una muestra a mediodía) |
| Subida, mediana | 893 Mbps | 840 Mbps |
| Latencia a Microsoft / Google / Cloudflare | 13 / 13 / 5 ms | 13 / 13 / 5 ms |
| RX óptica | -19,0 dBm | **-22,6 dBm** |
| TX óptica | +3,0 dBm | +2,4 dBm |
| IP pública | sin cambios | sin cambios |
| Reinicios superados sin intervención | n/a | 1 (apagado de la ONT, IP de vuelta en 5 minutos) |

La cifra de RX es la que vigilo. Una caída de 3,6 dB en la misma fibra es más de lo que esperaba, y es o bien que la óptica de la Huawei mide distinto que la del Livebox, o bien un conector que no limpié lo bastante. Está holgadamente dentro de la ventana de recepción GPON (el suelo de la especificación ronda los -27 dBm) y los contadores de errores están a cero. Así que se queda como está hasta que derive o aparezcan errores.

Una cosa que no tenía prevista: la Huawei venía con una fuente de alimentación de cable muy corto. Pide 12 V, 0,5 A, positivo al centro, y la fuente del Livebox es de 12 V, 2 A, positivo al centro, así que reutilicé la fuente del Livebox que ya estaba instalada. El corte de cinco minutos de las 13:00 en mi gráfica de latencia es ese cambio.

## ¿Qué haría distinto?

- **Modo Password, no LOID, desde el principio.** Esa es la razón entera de esta entrada.
- **Preconfigurar la VLAN en un puerto WAN libre** antes de la noche. Así el corte dura lo que tardas en mover un cable, no 29 minutos.
- **Limpiar mejor el conector de la pared**, o al menos fotografiar la férula con el macro del móvil antes de enchufar. Me gustaría saber si ese -22,6 es la fibra o la óptica.

## En resumen

Si el Livebox solo te convierte luz en Ethernet, una HG8310M de 15 euros hace el mismo trabajo sin el segundo router, sin las radios de más, sin la gestión remota y sin ocupar sitio. Copia primero la contraseña PLOAM y la MAC WAN del Livebox, compra la ONT que corresponda al fabricante de la OLT de tu zona, pon Authentication Mode en **Password** y etiqueta la **VLAN 832** en la WAN de tu router sin prioridad y con DHCP a secas. El fijo dejará de funcionar, la app de la tele no se entera y la velocidad no cambia.

Guarda el Livebox. Es de Orange, y el día que necesites soporte será lo primero que te pidan.

## Fuentes

La comunidad de fibra de bandaancha.eu hizo la mayor parte del trabajo previo. Los hilos en los que me apoyé:

- [Orange TV funcionando con ONT Huawei HG8310M](https://bandaancha.eu/foros/orange-tv-funcionando-ont-huawei-hg8310m-1759776), abril de 2026. El caso de éxito que me hizo comprar este aparato.
- [ONT + router neutro sin perder la TV](https://bandaancha.eu/foros/ont-router-neutro-sin-perder-tv-1759672), abril de 2026. El hilo de la HG8120C: útil para la VLAN de televisión, engañoso para la HG8310M.
- [Qué ONT son compatibles con OLT Nokia/Alcatel](https://bandaancha.eu/foros/que-ont-compatibles-olt-nokia-alcatel-1745781) y [Qué ONTs recomendáis](https://bandaancha.eu/foros/que-onts-recomendais-conexion-fibra-1742395). La cuestión del fabricante de la OLT.
- [Sacar clave GPON / ID ONT / PLOAM password](https://bandaancha.eu/foros/sacar-clave-gpon-idont-ploam-password-1742394) y [Solución: cambia la ONT, password Livebox 6](https://bandaancha.eu/foros/solucion-cambia-ont-password-livebox-6-1744855).
- [VLAN y prioridad por operadora](https://bandaancha.eu/foros/vlan-prioridad-operadora-internet-voz-tv-1742983). La tabla de VLAN y prioridades por operador en España.
- [OPNsense + Orange FTTH España con ONT propia](https://bandaancha.eu/foros/opnsense-orange-ftth-espana-ont-propia-1758841) y la [continuación sobre DHCP con ZTE](https://bandaancha.eu/foros/opnsense-orange-ftth-ont-propia-zte-dhcp-1760854).
- [Orange TV Libre se abre a clientes de cualquier operador](https://bandaancha.eu/articulos/orange-tv-libre-abre-clientes-cualquier-11347), el artículo de bandaancha sobre la app.
- [hack-gpon.org](https://hack-gpon.org/), la referencia de credenciales por defecto y rarezas de firmware de las ONT: [Huawei HG8010H](https://hack-gpon.org/ont-huawei-hg8010h/), [Nokia G-010G-P](https://hack-gpon.org/ont-nokia-g-010g-p/).
- [Tutorial de ADSLZone para la Nokia G-010G-P](https://foro.adslzone.net/fibra-optica.94/tutorial-configurar-ont-nokia-g-010g-p.441070/), por si acabas por la vía Nokia.
- [Foro de OPNsense, Orange Spain FTTH with own ONT](https://forum.opnsense.org/index.php?topic=50206.0), diciembre de 2025. La mejor explicación del caso con descodificador (VLAN 838), que yo no necesité.
