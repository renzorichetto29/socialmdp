# Panel Social Mar del Plata

Panel de rendimiento por jugador, armado con los informes de partido de
[BasketStatsApp](https://www.basketstatsapp.com/). Es una sola página web, sin
servidor: se publica gratis en GitHub Pages y se abre con una contraseña.

---

## Lo único que hay que entender

| Archivo | ¿Sube a GitHub? | Por qué |
|---|---|---|
| `index.html` | **Sí** — obligatorio | Es la aplicación |
| `datos-cifrados.js` | **Sí** — obligatorio | Las estadísticas, cifradas. Va en la carpeta principal, al lado de `index.html` |
| `.nojekyll` | Sí | Le dice a GitHub que publique la carpeta tal cual |
| `README.md` | Sí, si querés | Estas instrucciones |
| `.gitignore` | Da igual | Sólo sirve si se sube por consola |
| `herramientas/` | **Da igual** | Ver abajo |
| `NO-SUBIR/` | **NO. Nunca.** | Los datos SIN cifrar |

Lo mínimo que tiene que estar arriba para que el panel funcione es **`index.html` y
`datos-cifrados.js`**. Con eso solo ya anda.

### Los dos archivos de datos, que son distintos

Son parecidos de nombre a propósito, pero hacen cosas opuestas:

- **`datos-cifrados.js`** — está en la carpeta principal, al lado de `index.html`.
  Es el que lee la página web. **Este sube a GitHub.**
- **`NO-SUBIR/datos-en-claro.json`** — está adentro de la carpeta NO-SUBIR. Es la
  fuente que se edita para agregar partidos. **Este nunca sube.**

El segundo genera al primero cuando pasás por la herramienta de cifrar.

### La carpeta NO-SUBIR

Adentro está `datos-en-claro.json`: las estadísticas en claro. Si ese archivo termina publicado,
cualquiera las lee sin contraseña y el cifrado no sirve para nada. Se queda siempre en
tu computadora.

### La carpeta herramientas

Es la que usás para generar el `datos-cifrados.js` cifrado. **Subirla o no da lo mismo para la
seguridad:** no contiene ni los datos ni la contraseña, sólo el procedimiento. Que se
conozca cómo está cifrado no debilita el cifrado — lo que lo protege es la clave.

Como la vas a usar siempre desde tu computadora, lo más simple es no subirla. Subila
sólo si querés tenerla a mano desde cualquier lado.

### La contraseña

**No está escrita en ningún archivo.** Ni en `datos-cifrados.js`, ni en `index.html`. No es que
esté escondida: no está. Es lo que genera la llave que abre los datos. Existe sólo
donde vos la anotes. Si la perdés y no tenés `datos-en-claro.json`, los datos no se recuperan.

### Ojo con los nombres en Windows

El Explorador esconde las extensiones: vas a ver `datos` e `index` en vez de `datos-cifrados.js`
e `index.html`. Los nombres reales son con extensión y **no hay que cambiarlos** — si el
archivo no se llama exactamente `datos-cifrados.js`, la página no lo encuentra.

## Publicar por primera vez

1. Entrá a [github.com](https://github.com) y creá una cuenta si no tenés.
2. Arriba a la derecha, **+ → New repository**. Ponele un nombre
   (por ejemplo `panel-social-mdp`), dejalo en **Public** y crealo.
   Público está bien: los datos van cifrados.
3. En la página del repositorio vacío, hacé clic en **uploading an existing file**.
4. **Arrastrá estos archivos:**
   - `index.html` ← imprescindible
   - `datos-cifrados.js` ← imprescindible
   - `.nojekyll`
   - `README.md` (opcional)

   **No arrastres la carpeta `NO-SUBIR`.** La carpeta `herramientas` es opcional.
5. Abajo apretá **Commit changes**.
6. Andá a **Settings → Pages**. En *Source* elegí **Deploy from a branch**, rama
   **main**, carpeta **/ (root)**, y **Save**.
7. Esperá dos o tres minutos y recargá esa misma pantalla: aparece el link,
   con la forma `https://TU-USUARIO.github.io/panel-social-mdp/`.

Ese es el link que se comparte. La contraseña se manda aparte, nunca en el mismo
mensaje que el link.

---

## Cargar un partido nuevo

**Opción A — la fácil.** Pasame el PDF del informe. Te devuelvo el `datos-cifrados.js` ya
cifrado y el `datos-en-claro.json` actualizado. Vos sólo hacés el paso 3 de acá abajo.

**Opción B — hacerlo vos.** No hace falta instalar nada.

1. Abrí `NO-SUBIR/datos-en-claro.json` con el Bloc de notas y agregá el partido nuevo,
   copiando la forma de cualquiera de los que ya están (la estructura está más
   abajo). Guardá.
2. Abrí `herramientas/cifrar.html` con doble clic — se abre en el navegador.
   Elegí el `datos-en-claro.json`, escribí la contraseña y apretá **Generar datos.js**.
   Se descarga el archivo nuevo. Nada sale de tu computadora: la página trabaja sola.
3. En GitHub, entrá a tu repositorio, hacé clic en el `datos-cifrados.js` viejo, después en
   el ícono del **lápiz ✏️ → Replace file**, y subí el nuevo. Abajo, **Commit changes**.
4. En dos minutos el link muestra el partido nuevo. No hay que tocar nada más.

`index.html` no se toca nunca para cargar partidos. Sólo cambia si modificamos el
diseño o agregamos funciones.

---

## Cerrar sesión

Arriba a la derecha hay un botón **Salir**. Olvida la contraseña guardada y vuelve
a la pantalla de acceso. Conviene usarlo si alguien mira el panel desde un celular
o una computadora prestada.

Igual la sesión no queda abierta para siempre: la contraseña se recuerda sólo
mientras esa pestaña esté abierta. Al cerrar el navegador se pide de nuevo.

---

## Cambiar la contraseña

Mismo camino que un partido nuevo, pero sin editar nada: abrí `herramientas/cifrar.html`,
elegí el `datos-en-claro.json` de siempre, escribí **la contraseña nueva**, generá el archivo y
reemplazá el `datos-cifrados.js` en GitHub. Desde ese momento la clave vieja deja de funcionar.

Avisale al equipo antes: la clave es una sola para todos.

## Si perdés el archivo en claro

Mientras tengas la contraseña se recupera. Abrí `herramientas/cifrar.html`, andá a
**Recuperar datos.json**, elegí el `datos-cifrados.js` publicado y escribí la clave.

---

## Cómo funciona la protección

Los datos se cifran con **AES-256-GCM**. La llave se deriva de la contraseña con
**PBKDF2-SHA256**, 250.000 iteraciones y una sal aleatoria distinta cada vez que se
regenera el archivo. El navegador descifra recién cuando alguien escribe la contraseña.

**Qué protege:** ni los nombres de los jugadores ni sus estadísticas aparecen en el
archivo publicado, ni siquiera mirando el código fuente de la página.

**Qué no protege:** cualquiera que tenga la contraseña puede pasársela a otro, y como
es una sola para todos, sacarle el acceso a una persona obliga a cambiársela a todo el
equipo. Si algún día hace falta dar y quitar accesos de a uno, conviene mover la
publicación a Cloudflare Pages con Cloudflare Access, que es gratis hasta 50 personas.

---

## Estructura de un partido en `datos-en-claro.json`

```jsonc
{
  "id": "g8",
  "date": "2026-09-09",        // AAAA-MM-DD
  "opp": "Vikings",            // rival
  "cat": "Primera",            // "Primera" o "+30"
  "venue": "away",             // "home" (local) o "away" (visitante)
  "pf": 54, "pa": 48,          // puntos a favor y en contra
  "q":    [7, 19, 15, 13],     // parciales propios por cuarto
  "qOpp": [13, 6, 17, 12],     // parciales del rival
  "pos": 90, "posOpp": 87,     // posesiones
  "adv": { "ptsOffTO": 6, "paint": 0, "second": 6, "fastBreak": 0,
           "starters": 22, "bench": 32,
           "oPtsOffTO": 4, "oPaint": 0, "oSecond": 9, "oFastBreak": 0,
           "oStarters": 15, "oBench": 33 },
  "box": [ /* una fila por jugador */ ],
  "starters": ["Gordillo, Nahuel", "..."]   // los 5 titulares, por nombre
}
```

Cada fila de `box` es una lista con este orden exacto (también está en la clave `K`):

```
num, nombre, min, pts, fgm, fga, tpm, tpa, dpm, dpa, ftm, fta,
oreb, dreb, ast, tov, stl, blk, sr, pf, pfd, pir, eff, pm
```

`dpm`/`dpa` son los tiros de dos. `min` va entre comillas, como `"26:50"`.

---

## Dos cosas que conviene no olvidar

**El nombre es la clave, no el número de camiseta.** Los informes escriben distinto al
mismo jugador entre partidos, y los números cambian entre categorías. Si un nombre no
coincide exactamente con los ya cargados, ese jugador se parte en dos fichas y los
promedios quedan mal. Equivalencias detectadas hasta ahora:

| En el informe dice | Va cargado como |
|---|---|
| QUIROGA, Emiliano | Quiroga, Emilio |
| CHORAO, Ariel | Charao, Ariel |
| SESTELLO CANGELLI, Lucas | Sestelo Cangelli, L. |
| ISAURRALDE, Nahuel | Insaurralde, Nahuel |
| Pasetti, Valentín | Passetti, Valentín |

**Los totales del informe no cierran con la suma de los jugadores, y está bien.** Los
rebotes y las pérdidas *de equipo* no se le atribuyen a nadie, así que el total del PDF
puede ser mayor. Cualquier otra diferencia sí es un error de transcripción.

---

## Para automatizar (opcional)

Si preferís la línea de comandos, `herramientas/cifrar.py` hace lo mismo que la página:

```bash
pip install cryptography
python3 herramientas/cifrar.py "LA-CONTRASEÑA"
```

Lee `NO-SUBIR/datos.json` y escribe `datos-cifrados.js`.
