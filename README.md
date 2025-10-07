# Latex_Tools_Collection

Guía práctica para crear **diagramas de bloques profesionales en LaTeX con TikZ**.  
Incluye estilos reutilizables, cómo dibujar bloques, sumarizadores, flechas, colores y un **ejemplo completo** de un servomecanismo con *Feedforward* y *Feedback* listo para compilar.

---

## 📦 Requisitos

Añade estas librerías en tu preámbulo:

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
```

**Descripción de las librerías:**

- `tikz`: motor de dibujo.
- `arrows.meta`: puntas de flecha configurables (p. ej. `Stealth`).
- `positioning`: posicionamiento relativo (`right=of`, `below=of`, etc.).

💡 **Sugerencia:** para documentos tamaño carta usa  
`\documentclass[letterpaper]{article}` y `\usepackage[margin=1in]{geometry}`.

---

## 🧱 Estructura base

Todo diagrama vive dentro de:

```latex
\begin{tikzpicture}[node distance=1.5cm and 1.5cm]
  % nodos y conexiones
\end{tikzpicture}
```

- `node distance`: separación por defecto entre nodos (horizontal y vertical).

---

## 🎨 Estilos reutilizables

Define una vez y reúsalo en todos tus diagramas:

```latex
\tikzset{
  block/.style={draw, rounded corners, minimum width=2.8cm, minimum height=1.15cm, align=center},
  sum/.style={draw, circle, minimum size=7mm, inner sep=0pt},
  >={Stealth[length=2.2mm]},         
  sig/.style={-Stealth, line width=0.8pt},   
  ffline/.style={-Stealth, blue, very thick}, 
  fbln/.style={-Stealth, red, very thick}   
}
```

- `block`: define un bloque rectangular con esquinas redondeadas.  
- `sum`: define un nodo circular con símbolo de suma.  
- `sig`: estilo de línea base.  
- `ffline`: línea azul gruesa para *feedforward*.  
- `fbln`: línea roja gruesa para *feedback*.

---

## 🧩 Crear bloques (nodos)

```latex
\node[block] (ref) {Referencia\\(pos/vel/par deseado)};
\node[sum,   right=1.5cm of ref] (sum1) {$\sum$};
\node[block, right=1.5cm of sum1] (pid) {Controller\\(PID)};
```

- `block` / `sum`: estilos definidos.  
- `(ref)`, `(sum1)`: nombres internos de nodos.  
- `right=1.5cm of ref`: posiciona relativo a otro nodo.  
- `\\`: salto de línea dentro del bloque.  

---

## 🔗 Conectar bloques (líneas y direcciones)

```latex
\draw[sig] (ref)  -- (sum1);
\draw[sig] (sum1) -- (pid);
```

| Ruta | Significado |
|------|--------------|
| `--` | Línea recta |
| `|-` | Sube y luego va a la derecha |
| `-|` | Va a la derecha y luego baja |

Ejemplo de retorno al sumador:

```latex
\draw[fbln] (sensor.west) -| (sum1.south);
```

---

## ➕ Signos y etiquetas

```latex
\node[above left=0mm and -1.5mm of sum1] {\small $+$};
\node[below right=0mm and -1.5mm of sum1] {\small $-$};

\draw[ffline] (A) -- node[midway, above=4pt, fill=white, inner sep=2pt]{\small \textbf{Feedforward}} (B);
```

**Tips:**  
- `fill=white` deja fondo blanco detrás del texto para no cruzar líneas.  
- `node[midway, above]` coloca texto encima de la línea.

---

## 🧪 Ejemplo completo: Servomecanismo con Feedforward y Feedback

> **Compila tal cual** (pdflatex). Ajustado a **hoja carta** y al ancho total de página.

```latex
\documentclass[letterpaper]{article}
\usepackage[margin=1in]{geometry}
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}

\tikzset{
  block/.style={draw, rounded corners, minimum width=2.8cm, minimum height=1.15cm, align=center},
  sum/.style={draw, circle, minimum size=7mm, inner sep=0pt},
  >={Stealth[length=2.2mm]},
  ffline/.style={-Stealth, blue, very thick},
  fbln/.style={-Stealth, red,  very thick},
  sig/.style={-Stealth, line width=0.8pt},
}

\begin{document}
\centering
\resizebox{\textwidth}{!}{%
\begin{tikzpicture}[node distance=1.5cm and 1.5cm, line cap=round, shorten >=1pt, shorten <=1pt]

\node[block] (ref) {Referencia\\(pos/vel/par deseado)};
\node[sum,   right=1.5cm of ref] (sum1) {$\sum$};
\node[block, right=1.5cm of sum1] (pid) {Controller\\(PID)};
\node[sum,   right=1.5cm of pid] (sum2) {$\sum$};
\node[block, right=1.5cm of sum2, minimum width=2.6cm] (drive) {Servo Drive\\(Amplificador de potencia)};
\node[block, right=1.5cm of drive] (motor) {Servomotor};
\node[block, right=1.5cm of motor] (trans) {Transmisión};
\node[block, right=1.5cm of trans] (mech) {Mecanismo / Carga};
\node[block, below=1.5cm of mech] (sensor) {Sensor\\(encoder / tacómetro)};
\node[block, above=1.5cm of sum2, minimum width=2.6cm] (ff) {Feedforward\\(modelo)};

\draw[sig] (ref) -- (sum1) -- (pid) -- (sum2) -- (drive) -- (motor) -- (trans) -- (mech);
\draw[sig] (mech.east) -- ++(0.9,0) node[right]{\small Salida real $y$ (pos/vel/par)};

\draw[fbln] (mech.south) -- (sensor.north);
\path (sum1.south) ++(0,-0.9) coordinate (bendfb);
\draw[fbln] (sensor.west) -| (bendfb) -- (sum1.south);

\node[above left=0mm and -1.5mm of sum1] {\small $+$};
\node[below right=0mm and -1.5mm of sum1] {\small $-$};
\node[above left=0mm and -1.5mm of sum2] {\small $+$};
\node[below right=0mm and -1.5mm of sum2] {\small $+$};

\path (ff.north) ++(-4.0,0.9) coordinate (ffTopLeft);
\draw[ffline] (ref.north) |- (ffTopLeft)
  -- node[midway, above=4pt, fill=white, inner sep=2pt, text=black]{\small \textbf{Feedforward}} (ff.west);
\draw[ffline] (ff.south) -- (sum2.north);

\end{tikzpicture}}
\end{document}
```

![Tarea_Servos_page-0001](https://github.com/user-attachments/assets/afedc7b9-9b67-4964-8169-099ac56d21a6)


---

## 🛠️ Trucos rápidos

- **Ajustar a página:** `\resizebox{\textwidth}{!}{...}` evita recortes.  
- **Conexiones limpias:** usa `.north`, `.south`, `.west`, `.east` para conectar por lados específicos.  
- **Evitar cruces:** combina `|-` y `-|` o crea coordenadas intermedias con `\path ... coordinate (nombre);`.  
- **Colores:** cambia `ffline`/`fbln` a otros colores (`cyan`, `orange`, etc.) sin tocar el resto.  
- **Grosor:** usa `very thick`, `thick` o `line width=1pt`.

---

## 📚 Recursos

- [PGF/TikZ Manual (CTAN)](https://ctan.org/pkg/pgf?lang=en)  
- [TikZ en Overleaf](https://www.overleaf.com/learn/latex/TikZ_package)  
- [TeXample TikZ Gallery](https://texample.net/tikz/examples/)

---

## 📝 Licencia

Este contenido puede usarse libremente en contextos académicos y profesionales.  
Incluye atribución a este repositorio si reutilizas fragmentos de ejemplo.
