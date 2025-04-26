<div align="center"> 
  
  # Optimización Geoespacial para la Expansión de Sucursales OXXO
  
  ![Geopandas](https://img.shields.io/badge/Geopandas-1000D0?style=for-the-badge&logo=geopandas&logoColor=white)
  ![Folium](https://img.shields.io/badge/Folium-13B1A8?style=for-the-badge&logo=folium&logoColor=white)
  ![Shapely](https://img.shields.io/badge/Shapely-00B7A7?style=for-the-badge&logo=shapely&logoColor=white)
  ![Pandas](https://img.shields.io/badge/Pandas-2C2D72?style=for-the-badge&logo=pandas&logoColor=white)
  ![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge)
  ![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)

  </div>

Procesamiento de datos para resolver el problema de MCLP. Datos obtenidos de la INEGI: [Directorio Estadístico Nacional de Unidades Económicas (DENUE)](https://www.inegi.org.mx/app/mapa/denue/default.aspx) y [Mapa Digital de México](https://gaia.inegi.org.mx/mdm6/?v=bGF0OjI0LjY1ODYxLGxvbjotMTA1Ljk4MjY3LHo6MixsOnQxMTFzZXJ2aWNpb3M=)


## Definición del modelo

**Conjuntos**

- $I$: Conjunto de nodos de demanda, representados por las manzanas dentro de los clusters, con una demanda medida por la población total.
- $J$: Conjunto de sitios candidatos para abrir nuevas tiendas OXXO, representados por los nodos con distancia mínima a tiendas existentes mayor a 75 metros.
- $L$: Conjunto de niveles de cobertura, definidos por la distancia entre los nodos. Nivel 1 para distancias menores o iguales a 500 metros y Nivel 2 para distancias menores o iguales a 1000 metros.
- $M_{il}$: Conjunto de configuraciones de cobertura para el nivel $l$ y demanda $i$, que depende de la distancia entre los nodos y si cumplen con la cobertura.

**Parámetros**

- $w_i$: Demanda en el objeto $i \in I$, representada por población total en cada nodo de demanda.
- $p$: Número de instalaciones a ubicar, representado por el número de nuevas tiendas OXXO a instalar.
- $c_{ilm}$: Fracción de cobertura proporcionada a la demanda $i$ por la configuración $m$ en el nivel $l$, basado en la distancia.

**Variables de decisión**

- $x_j \in \{0,1\}$:

$$
x_j = \begin{cases} 
1 & \text{si el sitio } j \in J \text{ es seleccionado como ubicación de tienda OXXO} \\
0 & \text{en otro caso}
\end{cases}
$$

Esta variable indica si un sitio candidato $j$ es seleccionado para abrir una tienda OXXO.

- $z_{ilm} \in \{0,1\}$:

$$
z_{ilm} = 
\begin{cases} 
1 & \text{si el nodo de demanda } i \in I \text{ es cubierto por el sitio candidato } j \text{ en el nivel } l \\
0 & \text{en otro caso}
\end{cases}
$$

Esta variable indica si un nodo de demanda $i$ es cubierto por un sitio candidato $j$ a una cierta distancia o nivel de cobertura $l$.

---

**Función objetivo**

Maximizar la cobertura total:

$$
\max_{x, z} \sum_{i \in I} \sum_{l \in L} \sum_{m \in M_{il}} w_i c_{ilm} z_{ilm}
$$

---

**Restricciones**

1. La cobertura solo es posible si la tienda fue seleccionada

$$
z_{ilm} \leq x_j \quad \forall i \in I, \; \forall l \in L, \; \forall m \in M_{il}, \; \forall j \in J
$$

2. Un nodo de demanda puede ser cubierto a lo mucho por una configuración:

$$
\sum_{l \in L} \sum_{m \in M_{il}} z_{ilm} \leq 1 \quad \forall i \in I
$$

3. Se deben seleccionar exactamente $p$ tiendas

$$
\sum_{j \in J} x_j = p
$$

4. Naturaleza binaria de las variables

$$
x_j \in \{0, 1\}, \quad z_{ilm} \in \{0, 1\} \quad \forall j \in J,\; \forall i \in I,\; \forall l \in L,\; \forall m \in M_{il}
$$
