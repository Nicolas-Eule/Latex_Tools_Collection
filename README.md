# Latex_Tools_Collection <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNndqaWx6cWF6N2JhcXhwd3N2cWF6NnZlaGJnOHh6cW04bHk3bWZqOCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/26gslU06Q8xWlO8wo/giphy.gif" width="36"/>

Guía práctica para crear **diagramas de bloques profesionales en LaTeX con TikZ**.  
Incluye estilos reutilizables, cómo dibujar bloques, sumarizadores, flechas, colores y un **ejemplo completo** de un servomecanismo con *Feedforward* y *Feedback* listo para compilar.

---

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExNWd2c2w0NGprYzRsdmQ3Njdmcnd5YzB5amR6YmF0bnlma3BrZTZzcyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l0MYRzcWPJ5bZy1JS/giphy.gif" width="22"/> Requisitos

Añade estas librerías en tu preámbulo:

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta,positioning}
```

**Descripción de las librerías:**

- `tikz`: motor de dibujo.  
- `arrows.meta`: puntas de flecha configurables (p. ej. `Stealth`).  
- `positioning`: posicionamiento relativo (`right=of`, `below=of`, etc.).  

> **Sugerencia:** para documentos tamaño carta usa  
> `\documentclass[letterpaper]{article}` y `\usepackage[margin=1in]{geometry}`.

---

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExOXBuamw5dmR4YzFzeHBqMzR1NDdhdTQzbTlxZmJpYjJhMzhkY2tydyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/3o7btPCcdNniyf0ArS/giphy.gif" width="22"/> Estructura base

Todo diagrama vive dentro de:

```latex
\begin{tikzpicture}[node distance=1.5cm and 1.5cm]
  % nodos y conexiones
\end{tikzpicture}
```

- `node distance`: separación por defecto entre nodos (horizontal y vertical).

---

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExaHFkMWNucGthNWlqZWtxOTg3bW42cWFoZGV0b241dnhqZG1tOXp3ciZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l0MYy2n2AloG8LkEE/giphy.gif" width="22"/> Estilos reutilizables

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

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExajd0cHJhN3puNXYyb2V3Z3Vycm5tYmtxYXB3MGU0enhyZ2VxNGs4MiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l1J9u3TZfpmeDLkD2/giphy.gif" width="22"/> Crear bloques (nodos)

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

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExaHYxZmZqY3hpdXM0b3ZyZWd1bGxvbHNmdml5YWFjYjV3dGQ3eDd0NSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/3o6Zt0Kf8Y1J1I4k4c/giphy.gif" width="22"/> Conectar bloques (líneas y direcciones)

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

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExd2Z4NTE5N2M3cDk1cW5pZXplZzBkemRhM2J5eWZxYmF1b3h6b3dmcSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l0IykNQ2qv5jZqf3W/giphy.gif" width="22"/> Signos y etiquetas

```latex
\node[above left=0mm and -1.5mm of sum1] {\small $+$};
\node[below right=0mm and -1.5mm of sum1] {\small $-$};

\draw[ffline] (A) -- node[midway, above=4pt, fill=white, inner sep=2pt]{\small \textbf{Feedforward}} (B);
```

**Tips:**  
- `fill=white` deja fondo blanco detrás del texto para no cruzar líneas.  
- `node[midway, above]` coloca texto encima de la línea.

---

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExc3I0ZGFvaDRta2g2ZHBhNmQxaTd6dmM2ZW1mZ2lnb3N1M2dmd2ZtOCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/3oEduQAsYcJKQH0lIc/giphy.gif" width="22"/> Ejemplo completo: Servomecanismo con Feedforward y Feedback

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

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExM2M4bTR0Yjc0d2J4N2d0ZDQzY2ZpZzZsOGUxNjJpZ3Znc2R6eW50OCZlcD12MV9naWZzX3NlYXJjaCZjdD1n/26gN1F6nMqf3Q6rFK/giphy.gif" width="22"/> Trucos rápidos

- **Ajustar a página:** `\resizebox{\textwidth}{!}{...}` evita recortes.  
- **Conexiones limpias:** usa `.north`, `.south`, `.west`, `.east` para conectar por lados específicos.  
- **Evitar cruces:** combina `|-` y `-|` o crea coordenadas intermedias con `\path ... coordinate (nombre);`.  
- **Colores:** cambia `ffline`/`fbln` a otros colores (`cyan`, `orange`, etc.) sin tocar el resto.  
- **Grosor:** usa `very thick`, `thick` o `line width=1pt`.

---

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExdzk5amNqM21kYmF2b2R4cTRmM2h3aWVxemI5ZWFpdzhmbTFxN3EyOSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/26u4hYx5bK8c7C8Zy/giphy.gif" width="22"/> Recursos

- [PGF/TikZ Manual (CTAN)](https://ctan.org/pkg/pgf?lang=en)  
- [TikZ en Overleaf](https://www.overleaf.com/learn/latex/TikZ_package)  
- [TeXample TikZ Gallery](https://texample.net/tikz/examples/)

---

## <img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExb2M4d3I3M3M3cDBscmJuaHpxN3ZrMGZjajRybjNwN2Z6cm80M3N2MiZlcD12MV9naWZzX3NlYXJjaCZjdD1n/l3q2zSNf3QqRk/giphy.gif" width="22"/> Licencia

Este contenido puede usarse libremente en contextos académicos y profesionales.  
Incluye atribución a este repositorio si reutilizas fragmentos de ejemplo.

