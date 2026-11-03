---
title: "Convertir un iMac de 27 pulgadas de 2015 muerto en un monitor 5K"
slug: "imac-27-2015-to-5k-monitor-r1811"
date: 2025-09-02
lastmod: 2026-11-03
draft: false
# Staging: reachable by URL, hidden from lists, feeds and sitemap. Remove this block to publish.
build:
  list: never
  render: always
sitemap:
  disable: true
description: "Cómo convertí un iMac 27 de 2015 (A1419) con la placa lógica muerta, comprado por 120 euros, en un monitor 5K con una placa controladora R1811 de Stonetaskin, errores incluidos: el parpadeo por la alimentación USB-C y lo que costó todo."
summary: "Compré en Wallapop un iMac 27 de finales de 2015 con la placa lógica muerta por 120 euros y lo convertí en un monitor 5K con una placa controladora R1811, ahorrándome al menos 1.000 euros frente a un monitor 5K nuevo. Escrito sobre la marcha, con los fallos: el tornillo T8 escondido, la orientación del cable de retroiluminación, el parpadeo con alimentación USB-C y los bordes rosas de los paneles de 2015."
tags: ["imac", "5k", "r1811", "monitor", "diy", "3d-printing"]
categories: ["Making"]
showTableOfContents: true
showHero: false
---

Así convertí un iMac de 27 pulgadas de finales de 2015, comprado por 120 euros, en un monitor 5K, ahorrándome al menos 1.000 euros. Esta guía empezó como [un README en GitHub](https://github.com/kurtshuler/imac-5k-monitor-conversion); esto es el mismo material, ordenado.

Se basa en el trabajo de la comunidad del [hilo DIY 5K Monitor Success de MacRumors](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/).

{{< figure src="img/2015-imac-27.jpg" alt="Un iMac de 27 pulgadas de finales de 2015 sobre una sábana blanca, con la pantalla apagada" caption="De esto: el \"Mystery Mac\", un iMac 27 de 2015 con la placa lógica muerta, 120 euros puesto en casa." figureClass="fig-right fig-40" >}}

## Gracias a la comunidad DIY 5K

Este proyecto no existiría sin los manitas del [hilo DIY 5K Monitor Success de MacRumors](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/). Los que más me ayudaron:

- [**@PaulD-UK**](https://forums.macrumors.com/members/pauld-uk.374804/): demasiadas aportaciones buenas para contarlas. Diagramas, consejos de cableado y sentido común práctico.
- [**@Regulus67**](https://forums.macrumors.com/members/regulus67.1356758/): siempre dispuesto a responder, y muy paciente con los novatos.
- [**@Aiwi**](https://forums.macrumors.com/members/aiwi.505077/): análisis a fondo y registros detallados que desmitificaron la R1811. Su propio montaje está en GitHub: [aiwipro/5K-iMac-Studio-Display-Stock-Look](https://github.com/aiwipro/5K-iMac-Studio-Display-Stock-Look).
- [**@ItsAShaunParty**](https://forums.macrumors.com/members/itsashaunparty.823966/): creó el [GPT DIY 5K at MacRumors](https://chatgpt.com/g/g-683af4dc61448191ad6868a38e8cb1b5-diy-5k-monitor-gpt) para ChatGPT.

Si te metes en este proyecto, [léete el hilo entero](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/) antes de preguntar.

## Cómo está escrita esta guía

En vez de enseñar los aciertos y decir "¡tachán!" como un mago sacando un conejo de la chistera, la escribí mientras hacía el proyecto, con los fallos y los caminos equivocados. El ~~texto tachado~~ significa que me equivoqué o que al final tomé otro camino. Espero que sea más útil que una historia de éxito perfectamente lineal.

{{< clear >}}

## Por qué lo hice

Me acababa de comprar una impresora 3D y buscaba un monitor más grande para usar con mi MacBook Pro M1 de 2020 y Fusion 360. Hay montones de monitores 4K asequibles, pero el escalado del texto y los detalles en un Mac siempre me ha parecido raro. Así que empecé a buscar monitores 5K y me sorprendió encontrar... poca cosa. Y lo que hay es carísimo. Buscando análisis de monitores 5K encontré el hilo de MacRumors, y mi vida cambió a mejor.

Busqué iMac de 27 pulgadas de segunda mano en Wallapop. Imagina mi sorpresa al encontrar un iMac 27 de finales de 2015 (A1419) con la placa lógica muerta por 100 euros. El vendedor decía que la pantalla funcionaba bien antes de morir la placa, y que una tienda le había dado un presupuesto de reparación demasiado caro. Me arriesgué, apostando a que tanto el vendedor como el técnico eran honrados.

{{< figure src="img/wallapop-receipt.jpg" alt="Pedido de Wallapop: iMac 27 2015 (Apple) - Piezas, transacción completada el 19 ago 2025, 120,28 euros" caption="Lo que pagué de verdad: 120,28 euros con el envío y las comisiones de Wallapop, 19 de agosto de 2025." >}}

Cuando llegó el paquete me encontré un panel `LM270QQ1 (SD)(B1)`. Por supuesto, sin placa lógica ni placa controladora no tenía ni idea de si funcionaba. Bauticé mi nueva posesión como el "Mystery Mac".

{{< pair src1="img/panel-model-number.jpg" alt1="Etiqueta en la parte trasera del panel LG con el texto LM270QQ1 (SD)(B1)" cap1="El número de modelo del panel del Mystery Mac." src2="img/2015-imac-27.jpg" alt2="El iMac antes de destriparlo" cap2="El Mystery Mac antes de destriparlo. En este momento no sabía si el panel funcionaría." >}}

## ¿Qué quería conseguir?

Con el iMac comprado y el panel identificado, tocaba elegir la placa controladora y el resto de piezas. Antes, los requisitos:

1. **Sin audio.** No quería activar los altavoces del iMac. La gente del foro tenía problemas con ellos, y además estoy diseñando un sistema de sonido 2.1 impreso en 3D.
2. **Sin webcam ni micrófono.** Ya tengo una webcam Obsbot Meet SE con mejor imagen que la cámara interna del iMac, y a los del foro les costaba mucho montar un buen micrófono interno. Poco valor para mucho trabajo.
3. **Sin carga por USB-C.** Tengo un dock CalDigit TS3 Plus y quiero que sea el dock el que cargue el Mac. La pega es que el dock solo pasa 5K por sus puertos USB-C / Thunderbolt, y no sabía si eso haría que la placa intentara cargar el Mac por Power Delivery. Si pasaba, conectaría el Mac al monitor con un cable DisplayPort 1.4 aparte.
   - Lo que aprendí después: en la configuración directa *Mac, cable Thunderbolt, monitor*, sin dock, la placa sí intenta alimentar el Mac, y la fuente que viene es demasiado floja para eso (ver el apartado del parpadeo). **En conexión directa necesito un cable USB-C a DisplayPort del Mac a la placa.** Confirmado solo para la configuración directa.
   - La configuración *Mac, dock CalDigit, cable Thunderbolt, monitor* sigue pendiente de probar.
4. **Silencioso.**
   - **Conservé el ventilador interno del iMac.** La caja está diseñada para aspirar aire por detrás con el ventilador centrífugo y expulsarlo por abajo, lo que ayuda a refrigerar todas las placas. Las pegas: necesita un convertidor reductor de 24 V a 12 V, y limita dónde poner la R1811 si quiero las entradas accesibles. Ya tenía un controlador de ventilador [Noctua NA-FC1](https://noctua.at/en/na-fc1) para bajar las revoluciones hasta el silencio.
   - **El ventilador de la R1811 lleva resistencia.** La placa viene con un ventilador que la gente encuentra ruidoso. Probé unos [adaptadores Noctua NA-RC7 Low-Noise](https://noctua.at/en/na-src7) de otro proyecto y funcionan; tengo pedidas en AliExpress resistencias de 180 ohmios y 2 A para sustituirlos.
   - **Fuente de alimentación externa**, para que no caliente el interior de la caja. La de Stonetaskin es de **24 V y 5 A** con conector de barril de **5,5 mm / 2,5 mm**. Cargar un portátil por USB-C PD necesitaría más amperaje, y de todos modos no lo quiero.
5. **Mando a distancia IR.** Odio tener que meter la mano por detrás de un monitor para cambiar ajustes. Quiero un mando que apunte al frente para llegar al OSD de la placa.

## ¿Qué placa controladora, y a quién comprársela?

Resumen por delante: la [R1811 V4 con DP 1.4 y HDMI 2.1](https://stonetaskin.com/products/diy-5k-universal-r1811-v-4-driver-board-usb-c-freesync-edp-dp1-4-compatible-for-imac-lm270qq1-lm270qq2-screen-external-monitor-1) y su compañera, la [placa de corriente constante DZ-LP0818 de 50 W](https://stonetaskin.com/products/high-quality-diy-universal-driver-board-50w-current-board-dz-lp0818-resolve-the-panel-brightness-problem-lm270qq1-lm270qq2-1?variant=45363844120730), las dos de [Stonetaskin](https://stonetaskin.com).

Por qué la R1811, aunque sea la placa más cara:

1. Recibe actualizaciones de firmware más a menudo que las otras.
2. Los usuarios del hilo informan de menos problemas y rarezas con ella.
3. Viene con mando IR y fuente de alimentación.
4. La placa opcional DZ-LP0818 puede subir el brillo de la retroiluminación si hace falta. La compré porque no sabía cuántas horas tenía el panel de 2015 ni si la retroiluminación había perdido brillo.

Compré en Stonetaskin por su reputación en el foro, y la decisión se confirmó cuando su chat resultó rápido y útil durante el pedido. Las dos placas llegaron a España 8 días después de pedirlas.

## ¿Qué costó todo?

Precios finales. El envío desde China fue gratis.

| Concepto | Precio |
|---|---|
| iMac 27 A1419 de finales de 2015, placa lógica muerta, Wallapop, en casa | 120,28 € |
| [Placa controladora Stonetaskin R1811 V4, pack B](https://stonetaskin.com/products/diy-5k-universal-r1811-v-4-driver-board-usb-c-freesync-edp-dp1-4-compatible-for-imac-lm270qq1-lm270qq2-screen-external-monitor-1) (placa, botonera, mando IR, fuente de 24 V 5 A, cables) | 298,00 $ |
| [Placa de corriente constante DZ-LP0818 de 50 W](https://stonetaskin.com/products/high-quality-diy-universal-driver-board-50w-current-board-dz-lp0818-resolve-the-panel-brightness-problem-lm270qq1-lm270qq2-1?variant=45363844120730) | 36,30 $ |
| Divisor de barril, convertidor reductor de 24 V a 12 V, kit de conectores JST (Amazon.es) | unos 25 € |
| Controlador Noctua NA-FC1 y adaptadores NA-RC7 | ya los tenía |

Pongamos 450 euros por una pantalla 5K. Un monitor 5K nuevo empieza en cuatro veces eso.

## Destripar el iMac

Saqué casi todo, porque no voy a reutilizar los altavoces. La caja venía sin la tapa de la RAM, así que imprimiré una en 3D más adelante. Quité la toma de corriente porque la fuente se queda fuera de la caja.

1. **Conservé el ventilador de la caja** y le soldé cables más largos para el controlador Noctua NA-FC1.
2. **Dejé el botón de encendido** y le alargué los cables, para conectarlo después al botón de encendido de la botonera de la R1811.

El despiece está en iFixit: [sustitución de la placa lógica del iMac Intel 27 pulgadas Retina 5K](https://www.ifixit.com/Guide/iMac+Intel+27-Inch+Retina+5K+Display+Logic+Board+Replacement/30529). El kit de herramientas de iFixit me vino muy bien. Hacen falta puntas **T5, T8, T10 y T25** y una **extensión**, para un tornillo que queda debajo de la placa lógica y se alcanza por un agujero de la misma. En el A1419 ese tornillo es **T8, no T10 como dice la guía de iFixit**. Hay más sobre esto en el hilo de MacRumors.

{{< pair src1="img/board-speakers-ps.jpg" alt1="Interior del iMac sin altavoces ni fuente, con la posición del tornillo escondido de la placa lógica rodeada con un círculo" cap1="Altavoces y fuente fuera. La posición del dichoso tornillo T8, rodeada." src2="img/logic-board-screw.jpg" alt2="Primer plano del tornillo T8 escondido de la placa lógica a través de su agujero de acceso" cap2="El tornillo T8 escondido de la placa lógica." >}}

Al sacar las piezas vi que tanto la placa lógica como la fuente tenían daños que parecían de un arco eléctrico. Ya no estaba tan seguro de que el panel funcionara.

{{< figure src="img/imac-case-empty.jpg" alt="Caja del iMac vacía, solo con el ventilador y el botón de encendido" caption="La caja con solo el ventilador y el botón de encendido." >}}

{{< pair src1="img/power-cable-extended.jpg" alt1="Botón de encendido del iMac con cables alargados soldados" cap1="El botón de encendido con sus cables alargados." src2="img/imac-fan.jpg" alt2="Ventilador de la caja del iMac con cables nuevos, con los colores del controlador Noctua" cap2="El ventilador de la caja con cables nuevos, con los colores que espera el controlador Noctua." >}}

## Preparar la primera prueba de encendido

El plan era probar el panel solo con la R1811 y añadir la placa de retroiluminación DZ-LP0818 una vez supiera que la R1811 y el panel funcionaban.

### Conectar los cables de la R1811 al panel

El cable de datos de la pantalla es puñetero de encajar. Un par de vídeos de YouTube ayudaron.

{{< figure src="img/panel-cables.jpg" alt="Parte trasera del panel LG con el cable de retroiluminación a la izquierda y el de datos a la derecha" caption="Los dos cables conectados. Izquierda: retroiluminación. Derecha: datos." >}}

{{< pair src1="img/display-data-cable.jpg" alt1="Cable de datos de la pantalla enchufado al panel" cap1="Cable de datos de la pantalla." src2="img/display-data-cable-closeup.jpg" alt2="Primer plano del conector de datos asentado en el panel" cap2="Primer plano del conector de datos." >}}

{{< figure src="img/backlight-cable.jpg" alt="Primer plano del cable de retroiluminación de la R1811 unido al del panel" caption="Primer plano de la conexión de retroiluminación." >}}

**La orientación importa.** El lado del círculo negro encaja con el lado blanco que tiene la línea y las muescas a izquierda y derecha. Hay más orientación en el hilo del foro. Si lo conectas al revés la retroiluminación no se enciende, pero por lo visto no se quema nada.

### Conectar la R1811 al panel

{{< figure src="img/panel-r1811-power-on.jpg" alt="Placa R1811 junto al panel con los cables de retroiluminación y datos conectados" caption="Conexiones de retroiluminación y datos de la R1811." >}}

{{< figure src="img/r1811-connections.jpg" alt="Primer plano de la placa R1811 con sus cables" caption="Primer plano de las conexiones de la R1811. El cableado de la retroiluminación cambia después, cuando entra la DZ-LP0818." >}}

## Encendido: el panel funciona

¡OSD! Le di la vuelta al panel y lo apoyé sobre bloques de espuma para poder verlo.

{{< figure src="img/panel-osd.jpg" alt="El panel mostrando el menú en pantalla de la R1811 en el primer encendido" caption="El OSD de arranque de la R1811. El panel vive." >}}

### Cambiar el OSD a inglés

{{< pair src1="img/osd-chinese.jpg" alt1="OSD de la R1811 en chino" cap1="El OSD en chino, como viene." src2="img/osd-english.jpg" alt2="OSD de la R1811 con el idioma puesto en inglés" cap2="Other > Language > English." >}}

### Comprobar el 5K desde el Mac

Conecté mi MacBook Pro M1 de 2020 con un cable Thunderbolt 4 que tenía por casa. Tanto el OSD como macOS muestran 5K.

{{< pair src1="img/mac-retina-resolution.jpg" alt1="El panel convertido mostrando el escritorio de macOS a resolución 5K" cap1="Resolución 5K." src2="img/mac-usb-c.jpg" alt2="Indicador de entrada del OSD de la R1811 mostrando USB Type-C" cap2="Entrada USB Type-C." >}}

{{< figure src="img/mac-brightness.jpg" alt="El panel convertido al 50 por ciento de brillo con una imagen de satélite de fondo" caption="El panel al 50 % de brillo." >}}

### Power Delivery por USB-C y parpadeo de la pantalla

En este punto la pantalla parpadeaba, lo que desanimaba. Al final me di cuenta de que el MacBook estaba tirando de corriente por el cable USB-C desde la fuente de 24 V y 5 A de la placa, aunque el MacBook estaba enchufado a su propia fuente.

**Si conectas el ordenador al puerto USB-C de la R1811 con una fuente escasa, la placa intenta alimentar el ordenador y la pantalla parpadea.** La fuente de 24 V y 5 A de Stonetaskin vale para el panel solo, pero no puede además alimentar un portátil. Dos salidas:

1. Un **cable USB-C a DisplayPort**, que da la resolución completa sin tirar de la fuente del monitor.
2. Una **fuente más grande**, como las Mean Well que se comentan en el foro.

Como de todos modos no quería Power Delivery desde el monitor, un cable USB-C a DisplayPort lo arregló. Se acabó el parpadeo.

### Bordes rosas en los paneles LG LM270QQ1 SD(B1) de 2015

Mi panel tiene la coloración rosa en los bordes típica de los LM270QQ1 SD(B1), explicada [aquí en el hilo](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/post-33552575). No ocurre en los iMac de 2017 y posteriores, con paneles SD(C1/D1/E1/F1).

En el mío el rosa se ve sobre fondos blancos o grises cerca de los bordes, como un degradado translúcido, más oscuro en el borde y que desaparece a unos 2,5 cm hacia dentro. Se nota, pero no es decisivo a este precio. Si hago otra conversión, probablemente iría a por un iMac 27 de 2017, aunque se venden a 350 o 400 euros en Wallapop.

## Añadir la placa de corriente constante DZ-LP0818

Esta placa suministra y mantiene corriente extra para la retroiluminación. Las retroiluminaciones pierden brillo con los años, y esto lo corrige. Como ya la había comprado, la instalé.

{{< pair src1="img/r1811-dz-lp0818-connections.jpg" alt1="Placas R1811 y DZ-LP0818 cableadas entre sí y con el cable de retroiluminación del panel" cap1="Conexiones de la R1811 y la DZ-LP0818." src2="img/r1811-dz-lp0818-connections-closeup.jpg" alt2="Primer plano de las conexiones de la placa DZ-LP0818" cap2="Primer plano." >}}

Volví a probar la alimentación por USB-C con la placa de corriente constante puesta, por ver si el parpadeo desaparecía. No desaparece.

### Un perfil de color con el Datacolor Spyder5

Usar el Spyder5 fue un suplicio porque ya no tiene soporte y el software no funcionaba bien en mi Mac. Conseguí un perfil que se ve mejor que el "iMacPro5K" que macOS asignó al conectar el panel por primera vez. Volveré sobre esto cuando termine el interior.

## Silenciar el ventilador de la R1811

Para bajar el ruido del ventilador añadí resistencia para reducir su tensión. Tenía dos [adaptadores Noctua NA-RC7 Low-Noise](https://noctua.at/en/na-src7) y los conecté en serie con el ventilador usando conectores JST XH-2P de [este kit](https://www.amazon.es/dp/B0CKSJPVZF). Necesito JST para otro proyecto, así que esto fue práctica. Tengo los dedos gordos y la vista mala.

{{< figure src="img/board-fan-resistors.jpg" alt="El ventilador de la placa R1811 con dos adaptadores Noctua de bajo ruido empalmados con conectores JST" caption="El ventilador de la R1811 con las resistencias empalmadas. Amarillo y rojo son positivo, negro es masa." >}}

Ahora el ventilador es casi silencioso.

## Alimentar el ventilador de la caja del iMac

El ventilador de la caja ya tenía sus cables alargados; faltaba alimentarlo. En vez de tirar de los pads de 12 V de la R1811, dividí la salida de la fuente de 24 V, añadí un convertidor reductor de 24 V a 12 V (con un fusible entre él y la fuente, por si acaso) y el controlador [Noctua NA-FC1](https://noctua.at/en/na-fc1).

{{< figure src="img/power-setup.jpg" alt="Fuente, divisor de barril, fusible, convertidor reductor y controlador de ventilador Noctua dispuestos y conectados" caption="El montaje de alimentación. La salida del convertidor va al ventilador de la caja." >}}

### La trampa del tamaño del conector de barril

La toma de alimentación de la R1811 admite un barril de **5,5 mm exterior y 2,5 mm interior**. El tamaño "estándar" es 5,5 / 2,1 mm, y ese es el [divisor](https://www.amazon.es/dp/B0DX6ZDM3J) que compré. Encajó en la fuente de Stonetaskin pero necesitó un adaptador para la toma de la R1811. Tenía uno en el cajón de los cachivaches.

{{< figure src="img/y-splitter-plus-adapter.jpg" alt="Conectores de barril de la placa R1811, la fuente, el divisor y el adaptador, uno junto a otro" caption="Tamaños de barril de la placa, la fuente, el divisor y el adaptador." >}}

### Ajustar el convertidor reductor

El convertidor es [este](https://www.amazon.es/dp/B09DYDV3S5).

{{< pair src1="img/step-down-converter.jpg" alt1="El convertidor reductor con sus entradas, salidas y controles de ajuste señalados" cap1="Entradas, salidas y controles del convertidor reductor." src2="img/set-step-down-converter.jpg" alt2="Multímetro marcando algo más de 12 voltios en la salida del convertidor" cap2="Ajustado a 12 V en la pantalla del convertidor; el multímetro marca un poco más." >}}

## Cómo está ahora, y qué falta

A noviembre de 2026 el monitor está en uso diario como pantalla 5K. La electrónica vive dentro de la caja del iMac, y la botonera del OSD cuelga por detrás, que funciona pero no es bonito. Pendiente:

- **Botones del OSD.** El plan es meter la botonera en los huecos de los puertos traseros con [este soporte de MakerWorld](https://makerworld.com/en/models/3519595-imac-27-conversion-to-monitor-r1820-cy-5key#profileId-4022780), diseñado para la botonera de 5 teclas, si la mía resulta ser la placa compatible. El forero @jag001 hizo algo parecido desde cero, [documentado aquí](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-33?post=32702935#post-32702935).
- **Conducto de entrada para el ventilador de la caja.** Un conducto impreso en 3D entre el ventilador y la rejilla trasera. @Xarli publicó uno en [Thingiverse](https://www.thingiverse.com/thing:6775247), y hay [otra versión](https://www.thingiverse.com/thing:7100773).
- **Soportes para las placas y el convertidor.** Impresos en 3D; no he encontrado ninguno para la R1811 y la DZ-LP0818 que encaje con mi distribución.
- **Mover el receptor IR** de la botonera al hueco de la cámara FaceTime, para que el mando funcione desde delante.
- **Una tapa de RAM**, porque el Mystery Mac vino sin ella.

## En resumen

Un iMac 5K de 27 pulgadas muerto es un panel de 120 euros con caja gratis, y por unos 330 dólares de placas de Stonetaskin se convierte en un monitor que macOS trata como pantalla Retina nativa. Compra la R1811 si quieres las mínimas sorpresas; mira el código del panel antes de comprar el iMac si los bordes rosas te molestarían (los de 2017 en adelante están limpios); usa un cable USB-C a DisplayPort salvo que compres también una fuente capaz de cargar el portátil; y léete el hilo de MacRumors antes de escribir en él.

## Fuentes y enlaces útiles

- [Hilo DIY 5K Monitor Success de MacRumors](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/), y su [índice WikiPost de montajes](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-1#post-30975623).
- [@PaulD-UK sobre cableado y alimentación (#1822)](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-109#post-32717747)
- [@Aiwi sobre la configuración de la R1811 (#822)](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-35#post-31094713)
- [El proyecto R9A18 de @Kaeslin (#1409)](https://forums.macrumors.com/threads/diy-5k-monitor-success.2253100/page-57#post-32247683)
- [Guía de iFixit para sustituir la placa lógica](https://www.ifixit.com/Guide/iMac+Intel+27-Inch+Retina+5K+Display+Logic+Board+Replacement/30529)
- [Stonetaskin](https://stonetaskin.com), el vendedor de las placas
- Controlador [Noctua NA-FC1](https://noctua.at/en/na-fc1) y adaptadores [NA-RC7](https://noctua.at/en/na-src7)
- [Cable Ugreen USB-C a DisplayPort 1.4](https://www.ugreen.com/products/usb-c-to-displayport-8k-cable), probado por muchos en el hilo
- La guía original: [kurtshuler/imac-5k-monitor-conversion](https://github.com/kurtshuler/imac-5k-monitor-conversion) en GitHub
