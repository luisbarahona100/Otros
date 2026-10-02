# Otros
# Estimación del error de posicionamiento del Bucket ante falla de inclinómetros

## 1. Objetivo

El sistema de posicionamiento de un equipo de carguío minero utiliza **GPS/GNSS de doble antena** para establecer un origen y orientación de coordenadas, junto con inclinómetros instalados en diferentes partes de la extremidad mecánica:

* **Cabina**
* **Boom**
* **Stick**
* **Bucket**

A partir de la posición/orientación de estos elementos, el sistema puede determinar la **posición tridimensional UTM del target**, es decir, el extremo de la extremidad mecánica que se desea posicionar con precisión.

En condiciones normales, la posición del target se obtiene utilizando la información disponible de los inclinómetros correspondientes.

El presente documento plantea un método geométrico para **estimar el error de posicionamiento del target cuando uno o más inclinómetros dejan de funcionar**.

---

# 2. Concepto general

La idea consiste en estimar cuánto podría desplazarse el target debido a una incertidumbre angular del elemento cuyo inclinómetro ha fallado.

Se considera:

* `d`: distancia entre la ubicación del inclinómetro que ha fallado y el **target**.
* `α`: margen angular de error considerado.
* `e`: error máximo estimado de posición del target debido exclusivamente a dicha incertidumbre angular.

La geometría corresponde a dos posiciones posibles del target separadas angularmente por un determinado ángulo.

Para un ángulo incluido `θ`, la distancia entre ambas posiciones posibles se obtiene mediante la **ley de cosenos**:

$$
e = \sqrt{d^2+d^2-2d^2\cos(\theta)}
$$

Por tanto:

$$
\boxed{e=d\sqrt{2(1-\cos\theta)}}
$$

---

# 4. Aplicación al Bucket

El caso principal considerado corresponde a la **falla del inclinómetro del Bucket**.

Normalmente, el inclinómetro del Bucket proporciona información necesaria para determinar la orientación de la cuchara y, consecuentemente, la posición del target.

Si el inclinómetro del Bucket deja de proporcionar información válida, puede estimarse un **error de posicionamiento** a partir de:

$$
d_B = \text{distancia entre el inclinómetro del Bucket y el target}
$$

Con un margen angular de:

$$
\pm10^\circ
$$

el error estimado sería:

$$
\boxed{
e_B=2d_B\sin(10^\circ)
}
$$

o equivalentemente:

$$
\boxed{
e_B=d_B\sqrt{2(1-\cos20^\circ)}
}
$$

Por tanto:

$$
\boxed{
e_B\approx0.3473d_B
}
$$

Este valor representa una **estimación geométrica del error de posicionamiento del target**, no una medición directa del error real del sistema.

---

# 5. Parámetro `Bucket Stimation`

Se propone utilizar el resultado de esta estimación como base para un parámetro denominado:

```text
Bucket Stimation
```

El objetivo del parámetro es proporcionar una indicación del **grado de incertidumbre de la posición calculada del target cuando la información del inclinómetro del Bucket no está disponible**.

Conceptualmente:

```text
Inclinómetro Bucket OK
        │
        ├── Sí → posición del target calculada normalmente
        │
        └── No
             │
             ▼
       Determinar d
             │
             ▼
       Aplicar margen angular
             │
             ▼
       Calcular error estimado e
             │
             ▼
       Bucket Stimation
```

El valor de `Bucket Stimation` debería interpretarse como una **estimación de incertidumbre de posicionamiento**, y no como una medición adicional proveniente de un sensor.

---

# 6. Extensión a falla del Stick

El mismo principio geométrico puede extenderse al caso en que falle el inclinómetro del **Stick**.

En este caso:

$$
d_S=
\text{distancia entre la ubicación del inclinómetro del Stick y el target}
$$

Con un margen de ±10°:

$$
\boxed{
e_S=2d_S\sin(10^\circ)
}
$$

Por tanto:

$$
\boxed{
e_S\approx0.3473d_S
}
$$

La lógica sería:

```text
Falla del inclinómetro Stick
          │
          ▼
Determinar distancia
Stick → Target
          │
          ▼
Aplicar incertidumbre angular
          │
          ▼
Calcular error estimado
          │
          ▼
Estimar incertidumbre del Target
```

Debido a que el target se encuentra más alejado del punto de referencia del Stick que del Bucket, el valor de `d` puede ser diferente y, consecuentemente, también lo será el error estimado.

---

# 7. Extensión a falla del Boom

El mismo procedimiento puede aplicarse si falla el inclinómetro del **Boom**.

En este caso:

$$
d_{Bo}=
\text{distancia entre la ubicación del inclinómetro del Boom y el target}
$$

Para ±10°:

$$
\boxed{
e_{Bo}=2d_{Bo}\sin(10^\circ)
}
$$

o:

$$
\boxed{
e_{Bo}\approx0.3473d_{Bo}
}
$$

La lógica es:

```text
Falla del inclinómetro Boom
          │
          ▼
Determinar distancia
Boom → Target
          │
          ▼
Aplicar incertidumbre angular
          │
          ▼
Calcular error estimado
          │
          ▼
Estimar incertidumbre del Target
```

---

# 8. Modelo generalizado

El modelo puede generalizarse independientemente del elemento cuyo inclinómetro haya fallado.

Sea:

$$
d_i=\text{distancia entre el inclinómetro disponible y el target}
$$

y sea:

$$
\alpha=\text{margen angular configurado}
$$

Si `α` representa el margen **±α**, entonces:

$$
\boxed{
e_i=d_i\sqrt{2\left(1-\cos(2\alpha)\right)}
}
$$

o:

$$
\boxed{
e_i=2d_i\sin(\alpha)
}
$$

Para el margen actualmente considerado:

$$
\alpha=10^\circ
$$

se obtiene:

$$
\boxed{
e_i\approx0.3473d_i
}
$$

### Tabla conceptual

| Inclinómetro con falla |      Distancia utilizada `d` | Error estimado con ±10° |
| ---------------------- | ---------------------------: | ----------------------: |
| Bucket                 | Inclinómetro Bucket → Target |        `eB = 0.3473 dB` |
| Stick                  |  Inclinómetro Stick → Target |        `eS = 0.3473 dS` |
| Boom                   |   Inclinómetro Boom → Target |      `eBo = 0.3473 dBo` |

La principal diferencia entre los casos es, por tanto, **la distancia desde el punto cuyo ángulo deja de conocerse hasta el target**.

---

# 9. Interpretación geométrica

La estimación puede visualizarse como una incertidumbre angular alrededor del punto donde se encuentra el inclinómetro.

Si el inclinómetro deja de proporcionar la orientación exacta, el target puede encontrarse dentro de un conjunto de posiciones posibles determinado por:

1. La distancia `d` entre el inclinómetro y el target.
2. El margen angular permitido `±α`.

Para un único margen angular, las dos posiciones extremas del target están separadas por una cuerda:

$$
e=2d\sin(\alpha)
$$

Por ello, **a mayor distancia `d`, mayor incertidumbre de posición para una misma incertidumbre angular**.

Ejemplo conceptual:

```text
                 Target (+α)
                     ●
                    /
                   / d
                  /
                 O  ← Inclinómetro
                  \
                   \ d
                    \
                     ●
                 Target (-α)

                  <--- e --->
```

Donde:

* `O` = ubicación del inclinómetro.
* `d` = distancia inclinómetro → target.
* `+α / -α` = límites de incertidumbre angular.
* `e` = separación entre las posiciones extremas estimadas del target.

---

# 10. Consideraciones sobre el alcance del modelo

Esta estimación representa únicamente el **componente de error asociado a la incertidumbre angular considerada**.

No necesariamente incluye otros errores del sistema, por ejemplo:

* Error de posicionamiento GNSS.
* Error de orientación/heading de las dos antenas GNSS.
* Error de calibración de los inclinómetros.
* Error de montaje mecánico.
* Error de medición de las distancias.
* Holguras mecánicas de boom, stick o bucket.
* Deformación estructural.
* Error de transformación entre sistemas de coordenadas.
* Errores derivados de la calibración HPGPS.
* Errores de comunicación o pérdida de datos.
* Otros errores propios del algoritmo de cálculo del target.

Por ello, `Bucket Stimation` debería documentarse como un **indicador de incertidumbre estimada asociado a la pérdida del inclinómetro**, y no necesariamente como el error total del sistema HPGPS.

---

# 11. Posible extensión del concepto

El modelo permite establecer una lógica de degradación del sistema:

```text
                    Inclinómetros
                         │
          ┌──────────────┼──────────────┐
          │              │              │
        Boom           Stick          Bucket
          │              │              │
          └──────────────┼──────────────┘
                         │
                  Estado de sensores
                         │
             ┌───────────┴───────────┐
             │                       │
          Todos OK              Alguno falla
             │                       │
             ▼                       ▼
       Target normal          Calcular d
                                     │
                                     ▼
                              Aplicar ±α
                                     │
                                     ▼
                              Calcular e
                                     │
                                     ▼
                           Indicador de
                           incertidumbre
```

Este mecanismo permitiría que el sistema no trate necesariamente la pérdida de un inclinómetro como una pérdida inmediata de la capacidad de posicionamiento, sino que pueda **cuantificar la incertidumbre adicional introducida por dicha pérdida**.

---

# 12. Referencias revisadas

> **Espacio destinado a incorporar las referencias técnicas, documentación, código fuente, procedimientos de calibración y evidencias utilizadas para validar este modelo.**

### Documentación HPGPS / Inclinómetros

* [Propuesta de Inclinómetros Inalámbricos — Alcance del proyecto](https://ms4m.atlassian.net/wiki/spaces/II/pages/4059529245/Alcance+del+proyecto?utm_source=chatgpt.com)
* [HPGPS — GNSS Receivers + Inclinómetros + NTRIP](https://ms4m.atlassian.net/wiki/spaces/ST/pages/2033188865/HPGPS?utm_source=chatgpt.com)
* [Margen de Error Target Coordenadas Topo. vs Target Coordenadas ControlBox](https://ms4m.atlassian.net/wiki/spaces/ST/pages/2965569557/Validaciones+HPGPS?utm_source=chatgpt.com)

### Calibración HPGPS

* [Calibración HPGPS usando ControlBox System](https://ms4m.atlassian.net/wiki/spaces/ST/pages/2046394374/Calibraci+n+HPGPS+por+ControlBox?redirectedFromRestrict=true&utm_source=chatgpt.com)
* [Calibración HPGPS con ControlBox — Confluence](https://ms4m.atlassian.net/wiki/spaces/ControlBox/pages/3965845569/07.+Calibraci+n?utm_source=chatgpt.com)
* **OneDrive → MS4M ServiceDesk → HW-support → Calibration**

  * `HPGPS Excavator Calibration`
  * `Calibración HPGPS Perforadora`

> En la carpeta **Calibration** se encuentra documentación necesaria para comprender el funcionamiento del proceso de calibración HPGPS. **No se ha identificado en dicha documentación una referencia explícita a la existencia del parámetro `Bucket Stimation`.**

### ControlBox

* [Seguimiento de cambios ControlBox](https://ms4m.atlassian.net/wiki/spaces/ControlBox/pages/3512532996/Seguimiento+de+cambios+en+ControlBox?xpis=eyJicmlkZ2UiOiJxdWlja0ZpbmQiLCJpZCI6IjE3OTA4NjI5NzU4NTMiLCJzb3VyY2UiOiJjb25mbHVlbmNlIn0%3D&utm_source=chatgpt.com)
* [Repositorio Bitbucket de ControlBox](https://bitbucket.org/teamms4m/csm_controlbox/src/master/?utm_source=chatgpt.com)

### IRIS

* [Calibración con IRIS — Funcionamiento de P-GINA](https://ms4m.atlassian.net/wiki/spaces/C4M/pages/3400400912/Funcionamiento+de+P+GINA?utm_source=chatgpt.com)
* [Manual de Usuario IRIS](https://msspe-my.sharepoint.com/:w:/g/personal/edwin_cabanillas_ms4m_com/IQDyF2GEIQMLSqj3vK4vhh7-AfiRYgAyDbZDwOPo4vQ9Q4k?e=jtnIWd&utm_source=chatgpt.com)

---

# 13. Observación pendiente de validación

La ecuación geométrica permite obtener una **estimación teórica del error angular**, pero antes de utilizar `Bucket Stimation` como indicador operativo del sistema debería verificarse:

1. Qué representa exactamente el parámetro `Bucket Stimation` en el código actual de **ControlBox**.
2. Si el valor configurable `10°` representa **±10°** o un **ángulo total de 10°**.
3. Si el error mostrado por el sistema está expresado en **metros, centímetros, porcentaje u otra magnitud**.
4. Si `d` corresponde a una distancia geométrica 3D o a una distancia proyectada.
5. Si el cálculo se realiza únicamente cuando falla el Bucket o también ante fallas de Stick/Boom.
6. Cómo se combina este error con el error propio del GNSS/HPGPS y con los errores de calibración.
7. Qué comportamiento debe tener el indicador cuando fallan simultáneamente varios inclinómetros.

**Punto especialmente importante:** si el requerimiento funcional establece que el usuario configura `±10°`, la ecuación geométricamente consistente es:

$$
\boxed{\text{Error estimado}=2d\sin(10^\circ)=0.3473d}
$$

mientras que:

$$
d\sqrt{2(1-\cos10^\circ)}
$$

corresponde a un **ángulo incluido de 10°**, no a un margen de ±10°. Esta distinción debería quedar explícita en la especificación de `Bucket Stimation`.
