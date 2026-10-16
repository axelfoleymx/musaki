# Prompt para Lovable · Landing Musaki

**Antes de pegar el prompt:** sube a Lovable las imágenes de `Musaki_Landing_Assets_Lovable.zip` con las rutas `/public/img/...` y `/public/media/...`. Si conectas el proyecto a GitHub, copia la carpeta `public` tal cual. El ZIP también trae `referencia_landing.html`, el código de la landing actual. Si Lovable acepta archivos de referencia, adjúntalo: es la fuente exacta de animaciones y medidas.

---

## PROMPT PRINCIPAL (pegar completo)

Crea una landing page de una sola página para **Musaki · Comunidad Artística**: clases de música para peques de 1 a 3 años que toman la clase con mamá, papá o un adulto acompañante. Está en Plaza Murano, Av. Huayacán, Cancún. El objetivo es que la familia agende una **clase muestra gratis** por WhatsApp (55 5461 4560, enlace `https://wa.me/525554614560`).

**Reglas de texto:**

- Todo en español de México.
- Nunca uses la palabra "sesión" ni "sesiones": siempre "clase" o "clases de música". No es terapia.

**Stack:** React + TypeScript + Tailwind + Vite. Usa keyframes de CSS para las animaciones en bucle y `requestAnimationFrame` para el vuelo del colibrí. El sonido del xilófono se genera con la Web Audio API, sin archivos de audio. Respeta `prefers-reduced-motion`: si está activo, apaga todas las animaciones y deja el colibrí quieto en su lugar de descanso.

### 1. Identidad visual (manual de marca, estilo claro y kawaii)

Define estos colores como tokens en `tailwind.config` y en variables CSS:

| Token | Hex | Uso |
|---|---|---|
| blue | #14A3D9 | corporativo |
| sun | #FFB132 | corporativo, botones principales |
| leaf | #92D050 | complementario |
| berry | #E4007D | complementario |
| ink | #0B4A69 | todo el texto (azul corporativo oscurecido; nunca negro ni azul marino) |
| ink-2 | #47738A | texto secundario |
| sky | #E7F6FC | fondo de secciones |
| sky-2 | #C9ECF9 | — |
| cream | #FFF8EC | fondo de secciones |
| line | #D5ECF6 | bordes y sombras planas |
| wa | #1FAF57 | WhatsApp |

**Tipografías (Google Fonts):**

- **Lilita One** para títulos y botones.
- **Quicksand** (500/600/700) para el texto.
- **Poppins** (600/700) para etiquetas en mayúsculas con `letter-spacing:.1em`.

**Recursos de marca que se repiten en toda la página:**

- **Franjas de 4 colores** en este orden: berry, leaf, blue, sun. Aparecen en la barra de progreso, el borde superior del formulario y un separador corto.
- **Cápsula resaltada** (`.pill-hl`) para la palabra clave de cada título:
  - Píldora berry con texto blanco, rotada −3°.
  - Contorno blanco y sombra plana ink debajo: `box-shadow: 0 0 0 .1em #fff, 0 .16em 0 .1em #0B4A69`.
  - Variantes sun, leaf y blue.
- **Sombras planas tipo sticker** (`0 6px 0 #D5ECF6`) y bordes muy redondeados (30px / 20px / 14px).
- **Botón principal sun** con sombra sólida `0 6px 0 #D98E12`:
  - hover: sube 2px;
  - active: baja 4px y la sombra queda en 2px, como un botón que se aprieta.
- **Nubes "inflables"** hechas con círculos blancos (`box-shadow: inset -10px -12px 0 #E3F3FA`). Se mueven lento de lado a lado con la animación `drift`: 38 s lineal infinita, −30px → 40px → −30px.
- **Polaroids de fotos reales:**
  - marco blanco, padding 10/10/38px y pie en Lilita One;
  - cinta adhesiva de color arriba (76×22px, rotada −4°);
  - cada una con una rotación distinta;
  - al hover se enderezan y crecen a 1.03.
- **Divisores de nubes:** un SVG con forma de nubes onduladas en #E7F6FC entre secciones.

**Imágenes disponibles en `/public/img`:**

- **Mascotas:** `bird.webp` (colibrí), `note.webp` (nota musical con carita), `kid.webp` (peque).
- **Notas pequeñas:** `note_orange.png` y `note_white.png`.
- **Logo:** `logo_color.png` y las piezas sueltas en `img/logo/` (`note`, `bird`, `tulip`, `m`, `u`, `s`, `a`, `k`, `i`, `tag`).
- **Fotos reales:** `papa-bebe`, `maracas`, `mama-tul`, `canasta`, `tul-nino`, `aros`, `aros-2`, `mama-baila`, `circulo`, `paracaidas`, `familias` (todas .webp).

**Video:** `/public/media/reel-clase.mp4` con póster `reel-poster.jpg`.

Usa siempre `height:auto` u `object-fit:cover` en las imágenes para que nunca se deformen.

### 2. Estructura y contenido

1. **Barra de progreso fija** arriba, de 6px, con las 4 franjas de color. Crece de izquierda a derecha con el scroll (`transform: scaleX(progreso)`).

2. **Header sticky** blanco al 90% con blur:
   - logo a la izquierda;
   - navegación: "Cómo es una clase", "Para ti", "Horarios", "Preguntas";
   - botón sun "Clase gratis" a la derecha.

3. **Hero.** Fondo degradado #BFE7F8 → sky → blanco, con nubes inflables. Va en dos columnas (en móvil, una).
   - **Columna izquierda:**
     - Etiqueta: "CLASES DE MÚSICA · PLAZA MURANO, CANCÚN".
     - H1 enorme: "La música se aprende **jugando** y en familia." La palabra "jugando" va en cápsula berry.
     - Párrafo: "Clases de música para peques de 1 a 3 años donde mamá, papá o un adulto acompañante participan toda la clase. Juego, movimiento y exploración sonora con nuestras misses."
     - Tags en píldora blanca con punto de color. El primero es amarillo sun y destacado: "Martes y viernes 4:00 p. m.". Le siguen "Clase muestra gratis", "Grupos de 6 peques" y "1 hora en familia".
     - Collage de 3 polaroids encimadas con pies "¡a bailar!", "exploramos sonidos" y "en familia".
     - La mascota nota baila en la esquina inferior con la animación `dance`: 0.7 s alternada, de rotar −8° a 8° y subir 14px, con origen en la base.
   - **Columna derecha:** tarjeta de formulario (ver sección 4).
     - Arriba lleva una barra de franjas de 14px.
     - Un sello circular berry "Gratis / 1ª CLASE" de 94px, rotado 12°, con la animación `wobble` (3 s: rota de 12° a 4° y escala a 1.06).
     - Una flecha dibujada a mano (trazo ink grueso con trazo blanco encima) apunta desde el texto hacia el formulario. Solo en escritorio.

4. **"Lo esencial".** Tarjeta blanca de 5 columnas que se monta sobre el hero (margin-top −56px). Cada columna lleva un ícono en círculo de color:
   - **Dónde:** Plaza Murano, Av. Huayacán.
   - **Cuándo:** **Martes y viernes 4:00 p. m.**, también sábados 11:00 a. m.
   - **Duración:** 1 hora, con mamá, papá o acompañante.
   - **Qué llevar:** ropa cómoda, calcetitas antiderrapantes y termo con agua.
   - **Inversión:** clase muestra gratis, con un botón "Ver precios" que despliega una caja con borde punteado sun y 4 precios:
     - Clase muestra: Gratis (en berry).
     - **Dos clases a la semana: $2,500 al mes.** Martes y viernes, 4:00 a 5:00 p. m. Más música, más práctica. Esta tarjeta va resaltada con fondo #FFF4DC, contorno sun y la píldora "★ Recomendado".
     - Una clase a la semana: $1,400 al mes. Sábados, 11:00 a. m. a 12:00 p. m.
     - Inscripción: $800. Incluye kit de bienvenida.

5. **"Una hora de música, juego y abrazos".** "abrazos" va en cápsula sun.
   - **Izquierda:** un teléfono ink redondeado, rotado −3°, con el video del reel en autoplay, mudo y en loop.
     - Botón blanco "Activar sonido" / "Silenciar" sobre el video.
     - Etiqueta sun "Clase real en Musaki" rotada −8°.
     - La mascota peque flota a un lado con la animación `bob`: 1.2 s alternada, sube 12px.
   - **Derecha:** 4 tarjetas "pilar". Cada una lleva una foto real en círculo de 80px con doble anillo (color + blanco):
     - Sonido (blue)
     - Movimiento (sun)
     - Juego (leaf)
     - Conexión (berry)
   - Al hover cada pilar sube 4px y rota −0.6°.
   - Debajo, 4 estadísticas grandes en blue: "1 a 3 años de edad", "6 peques por grupo", "2 adultos por peque, máximo", "60 minutos de clase".

6. **XILÓFONO INTERACTIVO.** Es la sección estrella; cuídala al detalle. Ver sección 3.

7. **"Lo que crece en tu peque".**
   - **Izquierda:** lista en píldoras con punto de color de doble anillo:
     - Seguimiento de instrucciones
     - Atención selectiva
     - Coordinación
     - Control de impulsos
     - Lenguaje
   - **Derecha:** camino de 3 pasos con círculos numerados (sun, leaf, blue) unidos por una línea punteada vertical. Las tarjetas son color crema #FBF7EE y van rotadas ±0.8°:
     1. **Aculturación** (primer cuatrimestre)
     2. **Imitación** (segundo cuatrimestre)
     3. **Asimilación** (tercer cuatrimestre)
   - Crédito al final: "Basado en la teoría del aprendizaje musical de Edwin Gordon."

8. **"Cuéntanos quién eres"** (fondo cream). Tabs en píldora: "Soy mamá", "Soy papá" y "Soy abuela, abuelo, tía o tío". La tab activa se pinta blue con texto blanco y rota −2°. Cada panel lleva una polaroid y su contenido:
   - **Mamá:** checklist con palomitas en círculos de color.
   - **Papá:** datos esenciales en mini tarjetas, más un botón de precios.
   - **Familia:** un botón verde "Compartir con mamá o papá", que abre `wa.me/?text=` con un mensaje prellenado.

9. **"Así se ven nuestras clases".**
   - Galería de 4 polaroids reales, alternando rotación −2.5° y 2° con desfase vertical.
   - 3 tarjetas: Reuniones de familias, Cápsulas de acompañamiento y Seguimiento individual.
   - Frase grande al cierre: "En Musaki, educar es un acto colectivo y la música **un camino** para crecer juntos."

10. **"Te esperamos en Plaza Murano"** (fondo sky).
    - **Caja de horarios:**
      - El bloque "Mar y vie · 4:00 a 5:00 p. m." va resaltado con fondo #FFF4DC, contorno sun y "★ Recomendado". Texto: "Grupo de dos clases por semana: más música y más práctica en casa."
      - Debajo, "Sábados · 11:00 a. m. a 12:00 p. m." en berry.
    - **Caja "Cómo llegar":**
      - Mapa ilustrado en SVG: calles blancas, Av. Huayacán con línea punteada sun, áreas verdes #C7E8A6 y agua #9ADCF5.
      - Pin berry con una nota sun que flota con la animación `pin` (1.8 s, sube 10px).
      - Dirección completa: Plaza Murano, Av. Huayacán, SMZA 313, MZA 217, Lt 6, Álamos I y II, 77533 Cancún, Q. R.
      - Botón "Abrir en Google Maps" (`https://maps.app.goo.gl/yN5vTqaL3KQhHwgw9`).

11. **Preguntas frecuentes.** Acordeón de tarjetas blancas.
    - El ícono "+" va en un círculo sun. Al abrir, rota 45° y cambia a berry con la cruz blanca.
    - Preguntas: costo, edad, quién entra (hasta 2 adultos por peque), qué llevar, precios, si necesitan saber música, diferencia con guardería o natación, y qué hacer si no pueden en esos horarios.
    - La mascota peque, en sticky, flota a la derecha.

12. **CTA final** (ver sección 3, "Logo final").
    - Fondo degradado #5DBDEA → #BDE5F7 → sky → blanco, con nubes.
    - Título: "Ven a conocernos con tu peque."
    - Texto: "La primera clase es gratis. Te esperamos este **martes o viernes a las 4:00 p. m.** en Plaza Murano…"
    - Píldora gigante de WhatsApp: verde, ícono en círculo blanco y "55 5461 4560" en Lilita One.
    - Mascotas a los lados: el peque con `bob` y la nota con `dance`. En móvil van al 35% de opacidad.
    - Enlace "Ver de nuevo la animación del logo".

13. **Footer** con logo, datos y contacto.

14. **Burbuja flotante de WhatsApp** abajo a la derecha:
    - Círculo verde de 64px con borde blanco y un anillo que se expande sin parar (`ring`: 2.4 s, escala 1 → 1.55, opacidad 0.8 → 0).
    - A los 4 segundos aparece un globo con la animación `popTip`: "**Miss Deya** ¡Hola! ¿Agendamos tu clase muestra gratis?".
    - El globo se puede cerrar y se recuerda en sessionStorage.

### 3. Animaciones clave (respétalas con estos valores)

#### A) XILÓFONO (la más importante)

**Estructura:**

- Tarjeta blanca grande (radio 30px, sombra suave, alto mínimo de 300px).
- Dos rieles de madera horizontales detrás de las barras: 12px de alto, color #D9A867, a 34% del borde superior y 34% del inferior.
- 8 barras verticales que se acortan de izquierda a derecha: alto = 230px − (índice × 16px). Ancho `clamp(26px, 5.4vw, 54px)`, radio 14px y sombra interior `inset 0 -7px 0 rgba(0,0,0,.14)` que da volumen.
- El nombre de la nota va abajo de cada barra en Lilita One 14px, en ink al 80%.

**Notas y colores** (en orden): todas suenan desde la octava 5.

| Nota | Frecuencia (Hz) | Color |
|---|---|---|
| Do | 523.25 | #E4007D |
| Re | 587.33 | #FF6FAE |
| Mi | 659.25 | #FFB132 |
| Fa | 698.46 | #FFD27A |
| Sol | 783.99 | #92D050 |
| La | 880 | #B9E28A |
| Si | 987.77 | #14A3D9 |
| Do (aguda) | 1046.5 | #7FD0F0 |

**Sonido (Web Audio, tipo marimba de juguete):**

- Dos osciladores senoidales:
  - el fundamental a la frecuencia de la nota;
  - un armónico a la frecuencia × 3.93, con ganancia 0.18, que se apaga a los 0.4 s.
- Envolvente: arranca en 0.0001, sube a 0.55 en 8 ms y cae exponencialmente a 0.0001 en 1.4 s.
- El AudioContext se crea en el primer toque y se reanuda si está suspendido (para iOS).

**Interacción:**

- Suena en `pointerdown` (no en click) para que responda al instante en móvil.
- También funciona con Enter o Espacio sobre cada barra y con las teclas 1–8 del teclado cuando la sección está visible (ignóralas si el foco está en un input).
- Al tocar, la barra baja 5px y escala a 0.97 durante 120 ms (transición de 80 ms), como si se golpeara.
- Hover: brillo al 1.06.

**Notas voladoras:**

- Con cada golpe sale una notita (`note_orange.png` o `note_white.png`, alternando; la blanca con un contorno `drop-shadow` blue).
- Nace del centro de la barra, sube 130px, rota 18° y se desvanece en 1.1 s con ease-out.
- Se elimina del DOM al terminar.

**Texto a la izquierda:**

- Etiqueta: "PRUÉBALO CON TU PEQUE".
- Título: "Toca el **xilófono**." con "xilófono" en cápsula leaf.
- Párrafo: "Así empieza la educación musical a esta edad: escuchar, imitar y descubrir que cada barra suena distinto. Toca las barras o usa las teclas del 1 al 8."
- Pista pequeña: "Sube el volumen de tu teléfono."
- La mascota nota bailando debajo.

**Accesibilidad:**

- Cada barra es un `<button>` con aria-label, por ejemplo "Nota Do, tecla 1".
- El grupo lleva `role="group"` con la etiqueta "Xilófono de ocho notas".

#### B) Colibrí guía del hero (al cliente le encantó; debe sentirse vivo)

**Imagen y aleteo:**

- La mascota `bird.webp` es un botón con ancho `clamp(92px, 13vw, 170px)` y sombra `drop-shadow(0 12px 16px rgba(11,74,105,.25))`.
- Aletea siempre con la animación `flap`: 0.16 s alternada infinita, `scaleY(1) → scaleY(.94)` y rotación de 0° a −2°. El origen es el 50% horizontal y el 55% vertical.

**Vuelo de entrada** (500 ms después de cargar):

- Entra desde fuera del lado izquierdo del hero, a la altura del 18%.
- Sigue una trayectoria SVG de curvas Bézier:
  - hace una curva hacia arriba y llega a un punto medio (entre el 20% y el 42% del ancho, alto máximo 30% / 320px);
  - da un **loop** (una vuelta completa en el aire);
  - baja hasta su lugar de descanso: junto a la esquina superior izquierda del formulario (en escritorio) o en la esquina superior derecha del hero (en móvil, para no tapar el texto).
- Dura 3.6 s con easeInOutQuad.
- El ave se orienta según la tangente del camino: se voltea con `scaleX(-1)` cuando va a la izquierda y se inclina entre −18° y 18° según la pendiente.

**Rastro dorado:**

- La línea punteada sun (stroke 5px, dasharray 2 12, caps redondos) se va dibujando detrás del ave conforme avanza. Usa una máscara SVG cuyo `stroke-dashoffset` avanza con el ave.
- Al aterrizar, el rastro se desvanece en 1.2 s.

**Chispas:**

- Cada 140 ms suelta una notita (naranja o blanca) que sube 70px, rota 25°, crece de 0.6 a 1 de escala y se desvanece en 1.4 s.
- Cada notita lleva una deriva horizontal aleatoria de ±20px.

**Reposo:**

- Flota en su lugar con senos: x ± 10px (frecuencia 1.3), y ± 9px (frecuencia 2.1) y rotación ± 5° (frecuencia 1.7).
- De vez en cuando suelta una chispa (aprox. 1.2% de probabilidad por frame).

**Interacción:**

- Al hacer clic en el colibrí, repite el vuelo con loop y vuelve a su lugar.
- Cuando alguien envía el formulario, lanza 10 chispas sobre la tarjeta y el colibrí da otro vuelo de celebración.

**Ajustes:** recalcula el punto de descanso al cambiar el tamaño de la ventana.

#### C) Logo final estilo "kumori"

Se dispara una vez al entrar al 45% en pantalla, con IntersectionObserver. El logo se arma con las piezas PNG posicionadas en porcentajes, sobre un lienzo de 1401×1060. Coordenadas (x, y, ancho, alto):

| Pieza | x | y | ancho | alto |
|---|---|---|---|---|
| note | 364 | 0 | 568 | 643 |
| bird | 1211 | 461 | 190 | 143 |
| tulip | 1183 | 615 | 62 | 74 |
| m | 0 | 694 | 289 | 221 |
| u | 330 | 695 | 192 | 221 |
| s | 554 | 694 | 155 | 224 |
| a | 730 | 695 | 223 | 223 |
| k | 999 | 616 | 162 | 299 |
| i | 1199 | 695 | 30 | 220 |
| tag | 0 | 945 | 1401 | 115 |

**Secuencia:**

1. **Burbujas.** Aparecen 5 burbujas sun (con brillo interior claro y sombra interior naranja #E8901E). Están alrededor del centro de la nota, escalonadas cada 0.15 s, y crecen de 0 a 1 con rebote (`cubic-bezier(.3,1.6,.5,1)`).
2. **Fusión.** A los ~0.75 s las burbujas se juntan hacia el centro y se desvanecen (0.35 s).
3. **Nota.** A 1 s aparece la nota del logo con efecto gelatina: escala 0 → (1.15, 0.85) → (0.95, 1.05) → 1, durante 1 s, con origen abajo.
4. **Colibrí.** A 1.3 s el colibrí entra volando en diagonal desde arriba a la izquierda: `translate(-340%, -260%)`, rotado −25°, durante 1 s.
5. **Letras.** A partir de 2 s, las letras m-u-s-a-k-i saltan una por una (cada 0.15 s): escala 0 → 1, desde 30% más abajo, con rebote.
6. **Tulipán.** A 2.1 s aparece el tulipán con el mismo rebote.
7. **Etiqueta.** A 3.1 s se revela "COMUNIDAD ARTÍSTICA" desde el centro hacia los lados con `clip-path`.

**Durante y después:**

- Todo el logo flota con la animación `float`: 3.4 s, sube 10px.
- Debajo lleva una sombra elíptica difuminada.
- Al mismo tiempo empiezan a subir 12 corazones SVG. Detalle:
  - Colores berry, rosa #FF6FAE, sun, leaf y blue, con borde blanco y brillo.
  - Tamaño aleatorio de 22 a 48px y posición horizontal aleatoria.
  - Animación `rise`: suben 640px rotando de −10° a 14° y se desvanecen. Duración de 5 a 9 s, en bucle, con retrasos aleatorios.
- El enlace "Ver de nuevo la animación del logo" reinicia la secuencia.

### 4. Formulario de agenda (lógica)

**Campos:**

- Tu nombre.
- "Tú eres": Mamá, Papá, Abuela, Abuelo, Tía, Tío u Otro familiar.
- Nombre de tu peque.
- Edad: 1 año, 1 año y medio, 2 años, 2 años y medio o 3 años.
- "¿Quién acompañará a tu peque?". Solo aparece si quien llena no es mamá ni papá. Opciones: "Yo lo acompaño", "Su mamá o su papá" o "Vamos juntos (pueden entrar hasta 2 adultos)".

**Fechas automáticas** (se calculan con la fecha del visitante, así que siempre están al día):

- Horarios: martes 4:00 p. m., viernes 4:00 p. m. y sábado 11:00 a. m.
- Busca hasta 28 días adelante y descarta las clases que empiecen en menos de 3 horas.
- **Impulsa martes y viernes:**
  - Muestra primero las 4 próximas fechas de martes o viernes.
  - Cada una lleva borde sun y una estrellita sun en la esquina, con la nota "★ Recomendado · Martes y viernes a las 4:00 p. m.: el grupo de dos clases por semana".
  - La primera viene preseleccionada.
- Debajo, con el texto "También hay lugar en sábado:", van las 2 próximas fechas de sábado.
- Al final, un chip "Otro día · Lo platicamos".
- Los chips muestran el día y la fecha en formato es-MX (por ejemplo "Viernes 20 de noviembre") y la hora abajo. El chip seleccionado se pinta blue con texto blanco y rota −2°.

**Validación:** un mensaje en berry con lo que falta, por ejemplo "Nos falta tu nombre y su edad.".

**Al enviar:**

- Sin backend. La tarjeta cambia a "¡Casi listo, {nombre}!" y muestra el mensaje armado en una caja sky, con un botón verde "Enviar por WhatsApp" que abre `https://wa.me/525554614560?text=...` en una pestaña nueva.
- Mensaje: "¡Hola, miss Deya! Soy {nombre}, {rol} de {peque} ({edad}). Queremos agendar la clase muestra gratis para el {día fecha} a las {hora}. ¿Nos confirmas el lugar?"
- Si eligió "Otro día", el mensaje pide otras opciones.
- Si quien llena no es mamá ni papá, agrega quién acompaña.
- Debajo: "¿No se abrió? Escríbenos al 55 5461 4560", con un botón "Copiar número" y "Cambiar mis datos" para volver al formulario.
- Al enviar se dispara la celebración del colibrí.

### 5. Calidad

- Diseña primero para móvil, sin scroll horizontal y con márgenes laterales de 16px.
- Probar en 390px y en 1360px.
- Foco visible: contorno berry de 3px.
- Contraste AA.
- Imágenes con `loading="lazy"` debajo del primer pantallazo.
- Los textos alternativos describen las fotos reales.
- La página debe sentirse alegre, cálida y familiar, igual que los reels de Musaki: cielo, nubes, colores de marca y mascotas en movimiento. Nada oscuro ni corporativo.

---

## PROMPTS DE AJUSTE (úsalos después, uno por uno, si hace falta)

**1 · Si el xilófono no suena en iPhone:**

> El xilófono no suena en iPhone. Crea el AudioContext dentro del primer evento pointerdown, llama a ctx.resume() si está en "suspended" y asegúrate de que los osciladores arranquen en ctx.currentTime. No uses archivos de audio.

**2 · Si el colibrí se ve plano:**

> El vuelo del colibrí debe seguir una trayectoria SVG con loop. Muévelo con requestAnimationFrame usando getPointAtLength, voltéalo con scaleX(-1) cuando vaya a la izquierda, inclínalo según la pendiente (máximo ±18°), dibuja detrás un rastro punteado dorado #FFB132 que se revela con una máscara y que suelte notitas cada 140 ms. En reposo debe flotar con senos (±10px, ±9px, ±5°). En móvil su lugar de descanso es la esquina superior derecha del hero, sin tapar el título.

**3 · Si el logo final aparece de golpe:**

> El logo del cierre debe armarse por piezas al entrar en pantalla: 5 burbujas naranjas que crecen con rebote y se funden al centro, después la nota con efecto gelatina, el colibrí entra volando en diagonal, las letras m-u-s-a-k-i saltan una por una cada 0.15 s, el tulipán aparece y al final "COMUNIDAD ARTÍSTICA" se revela desde el centro con clip-path. Usa las piezas de /img/logo y los tiempos 0 / 0.75 / 1 / 1.3 / 2 / 2.1 / 3.1 s.

**4 · Revisión de palabras:**

> Revisa todos los textos del sitio y reemplaza cualquier "sesión" o "sesiones" por "clase" o "clases de música".
