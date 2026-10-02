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

O equivalentemente:

$$
\boxed{
e_i=2d_i\sin(\theta)
}
$$

---

# 3. Modelo generalizado

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
Por ello, **a mayor distancia `d`, mayor incertidumbre de posición para una misma incertidumbre angular**.

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

# 4. Referencias revisadas

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
