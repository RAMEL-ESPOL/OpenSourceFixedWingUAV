# Ensamble reconstruido — campaña CFD de agosto 2026

Esta carpeta contiene el ensamble del prototipo de foam board **reconstruido en
agosto de 2026** y la campaña CFD corrida sobre él.

## Qué NO es

**No es la geometría que produjo la Tabla IV del artículo.** El ensamble
original de esa caracterización ya no está disponible. Este se reconstruyó a
partir de las mismas dimensiones externas medidas:

| Dimensión | Valor |
|---|---|
| Envergadura | 1058.6 mm |
| Cuerda en la raíz | 167.0 mm |
| Cuerda en la punta | 157.8 mm |

La carpeta `../CAD_SolidWorks/Avion_White/` contiene la geometría que se usó
como **referencia** para esta reconstrucción; tampoco es el ensamble original.

## Diferencias medidas frente al estudio original

Corrida de verificación en el mismo punto de operación
($\alpha = 0^\circ$, $\beta = 0^\circ$, $V = 15$ m/s):

| Coeficiente | Estudio original (Tabla IV) | Reconstrucción |
|---|---|---|
| $C_L$ | 0.008 | 0.100 |
| $C_D$ | 0.155 | 0.065 |
| $C_m$ | 0.057 | 0.056 † |

† El momento de la reconstrucción está referido a la **punta de la nariz**, no
al centro de gravedad, por lo que **no es comparable** hasta reubicar el sistema
de coordenadas en el CG.

La diferencia en $C_L$ corresponde a una incidencia alar efectiva de
aproximadamente **2°** que la reconstrucción no reproduce.

## Qué es válido

Los coeficientes transferidos al gemelo digital y **todos** los resultados de
simulación del artículo provienen del estudio original. Esta reconstrucción se
publica como **referencia dimensional**, no como medio para regenerar la
Tabla IV.

Pendiente: recaracterizar esta geometría en los barridos completos de $\alpha$
y $\beta$, con el origen de momentos reubicado en el centro de gravedad.
