# Plataformas UAV de Ala Fija de Código Abierto — Reno L250 y prototipo de foam board

Repositorio de respaldo del artículo:

> **Integration and Validation of Open-Source Fixed-Wing UAV Platforms for Low-Cost Autonomous Flight Research**
> S. Martinez, J. Veloz, J. Hurel, C. Tutiven, F. Yumbla
> *IEEE Ecuador Technical Chapters Meeting (ETCM) 2026*

y del proyecto de titulación **"Integración mecatrónica y validación de una plataforma UAV de ala fija de bajo costo basada en arquitectura abierta"** (ESPOL).

Contiene el diseño mecánico CAD, los planos de fabricación, los parámetros del autopiloto, el código de análisis de telemetría y los datos que respaldan las tablas y figuras del artículo.

---

## Dónde está cada cosa del artículo

| Sección / elemento del artículo | Carpeta |
|---|---|
| **Sec. II** — Selección de plataformas, componentes y costos (Tablas I–III) | [`Calculos/`](Calculos/) |
| **Sec. II** — CAD de ambas plataformas (Fig. 1) | [`Modelo3D/`](Modelo3D/) |
| **Sec. II** — Integración de aviónica (Fig. 3) | [`Guia_Mission_Planner.pdf`](Guia_Mission_Planner.pdf) |
| **Sec. III** — Banco de pruebas estático (Fig. 4) | [`Modelo3D/`](Modelo3D/) |
| **Sec. III** — Latencia RCIN/RCOUT y vibraciones FFT (Figs. 6 y 7) | [`12_Codigos_Graficas/`](12_Codigos_Graficas/) |
| **Sec. IV** — Geometría del prototipo de foam board (ver aviso abajo) | [`Modelo3D/Esamble_nuevo_cfd_agosto/`](Modelo3D/Esamble_nuevo_cfd_agosto/) |
| **Sec. V** — Parámetros del autopiloto | [`Parametros/`](Parametros/) |
| **Sec. V** — Órbita de *Loiter* sostenida (Tabla V, Fig. 8) | [`Datos_Articulo/`](Datos_Articulo/) |
| **Sec. V** — Gemelo digital en Isaac Sim (Fig. 9b) | repositorio aparte, ver abajo |

---

## Estructura

```
.
├── Calculos/                  Hoja de cálculo de MTOW, carga alar, T/W,
│                              autonomía y desglose de costos de importación
├── 12_Codigos_Graficas/       Análisis post-vuelo de telemetría .BIN
│   ├── processVibe.py         Extrae el mensaje VIBE y grafica vibraciones
│   └── missionPlanning.ipynb  Compara RCIN vs RCOUT (latencia de actuadores)
├── Modelo3D/
│   ├── CAD_SolidWorks/
│   │   └── Avion_White/       Geometría de referencia del foam board
│   ├── Esamble_nuevo_cfd_agosto/  Ensamble reconstruido + CFD de agosto 2026
│   │                              (NO es la geometría de la Tabla IV)
│   ├── RenoL250/              Fuselaje comercial modelado en Inventor
│   ├── STL_Impresion/         Mallas listas para impresión 3D
│   └── Paracaidas/            Sistema de recuperación (trabajo de tesis;
│                              no forma parte del artículo)
├── Parametros/                Respaldos .param de ArduPilot
└── Datos_Articulo/
    ├── logs_isaac_sim/        CSV de las dos corridas de la Tabla V
    └── scripts_mision/        Scripts de misión y de análisis
```

### Aviso sobre la geometría del prototipo de foam board

**El ensamble CAD que produjo la Tabla IV del artículo ya no está disponible.**

Lo que se publica aquí es una reconstrucción hecha a partir de las mismas
dimensiones externas medidas (envergadura 1058.6 mm, cuerda 167.0/157.8 mm), en
[`Modelo3D/Esamble_nuevo_cfd_agosto/`](Modelo3D/Esamble_nuevo_cfd_agosto/),
junto con la geometría de referencia usada para reconstruirla, en
[`Modelo3D/CAD_SolidWorks/Avion_White/`](Modelo3D/CAD_SolidWorks/Avion_White/).

Una corrida de verificación sobre la reconstrucción devuelve $C_L = 0.100$ y
$C_D = 0.065$ frente a los 0.008 y 0.155 del estudio original, diferencia
compatible con unos 2° de incidencia alar que la reconstrucción no reproduce.
**Los coeficientes del gemelo digital y todos los resultados de simulación del
artículo provienen del estudio original**, no de esta geometría. El detalle está
en el README de esa carpeta.

### Formatos CAD

Las piezas se publican en formato **nativo** (`.SLDPRT`/`.SLDASM` de SolidWorks,
`.ipt`/`.iam` de Inventor) y, donde estaba disponible, en **STEP** (`.stp`).

Si vas a reproducir la geometría, usa el **STEP**: conserva las dimensiones
exactas y lo abre cualquier CAD. Los **STL** de `STL_Impresion/` son mallas
trianguladas — sirven para imprimir y para el visor 3D de GitHub, pero pierden
la parametría y no son la fuente para tomar medidas.

---

## Gemelo digital (Isaac Sim + Pegasus Simulator)

El modelo de planta aerodinámica y el entorno de simulación viven en un
repositorio aparte, porque son un *fork* de un proyecto de terceros con su
propia licencia y linaje:

**https://github.com/stevenijm777/isaac-uav**

| Rama | Commit | Qué contiene |
|---|---|---|
| `legacy/paper-replication` | `edb7485` | La configuración con la que se produjeron los resultados del artículo. **Correr sin banderas reproduce lo publicado.** |
| `experimental/modelo-nuevo` | `ad6fcf3` | Cada corrección del modelo expuesta como una bandera de línea de comandos independiente (`--cg`, `--inertia`, `--gear-shift`, `--thrust`, `--damping`), para poder aislar el efecto de cada una |

El registro completo del diagnóstico —qué se midió, qué se corrigió y qué
quedó sin explicar— está en `HALLAZGOS_SITL.md` dentro de ese repositorio.

Para reproducir la órbita de la Tabla V:

```bash
# Terminal 1 — Isaac Sim + ArduPilot SITL
isaac_run examples/yoy_trainer/13_yoy_trainer_fixedwing.py --mode autonomous

# Terminal 2 — cuando SITL reporte heartbeat
python3 examples/yoy_trainer/scripts/autonomous_flight.py --loiter-now --duration 300
```

---

## Datos que no están en este repositorio

Los registros `.BIN` completos de Mission Planner **no se versionan aquí**: son
demasiado pesados para GitHub, que avisa a partir de 50 MB por archivo y
rechaza los que superan 100 MB.

Lo que sí está publicado son los productos derivados que respaldan las figuras
del artículo: las gráficas de vibraciones y de latencia en
`12_Codigos_Graficas/`, y los CSV de telemetría en `Datos_Articulo/`. Los
scripts de esa misma carpeta regeneran las figuras a partir de esos datos.

Si necesitas los `.BIN` originales, escríbenos al correo de contacto del
artículo.

---

## Requisitos

Para los scripts de análisis de telemetría:

```bash
pip install pymavlink matplotlib numpy pandas
```

Para abrir el CAD: SolidWorks 2021+ o Autodesk Inventor 2021+ para los archivos
nativos; cualquier visor compatible con STEP para el resto.

---

## Cómo citar

Si este material te resulta útil, cita el artículo:

```bibtex
@inproceedings{martinez2026openuav,
  author    = {Martinez, Steven and Veloz, Joel and Hurel, Jorge and
               Tutiven, Christian and Yumbla, Francisco},
  title     = {Integration and Validation of Open-Source Fixed-Wing {UAV}
               Platforms for Low-Cost Autonomous Flight Research},
  booktitle = {IEEE Ecuador Technical Chapters Meeting (ETCM)},
  year      = {2026}
}
```

---

## Contacto

Facultad de Ingeniería en Mecánica y Ciencias de la Producción
Escuela Superior Politécnica del Litoral (ESPOL)
Campus Gustavo Galindo, Guayaquil, Ecuador — `fryumbla@espol.edu.ec`
