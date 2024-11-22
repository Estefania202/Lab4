# Universidad de Costa Rica 
# Facultad de Ciencias Sociales 
# Escuela de Geografía 
# GF-0618 Fotogrametría y Teledetección  

## Laboratorio 4: Imágenes SAR para inundaciones e incendios 

*Estudiante: Wagner Chacon Ulate C02093*
*Asistente: Estefanía Pineda Ortega C06005*
*Docente: Cristian Andres Aguilar-Barboza*
 
### INTRODUCCIÓN

	Las Imágenes se Radar de Apertura Sintética (SAR en ingles=, se definen como una herramienta poderosa en teledetección. Hasselmann et tal (1985(, definen al SAR como, un Sistema que utiliza ondas de radio (radar) para simular una antena mas grande usando la superficie terrestre. Esto es ideal ya que no nececesita de buen clima y de que sea de día en el terreno a eyuddar. 
	Para ello se requiere de la retrodispersión, el concepto mas importante en el estudio y trabajo con SAR. La retrodispersión en imágenes SAR se refiere a la reflexión de las ondas de radar de vuelta hacia la antena emisora. Este fenómeno es crucial para la formación de imágenes SAR y depende de varios factores, incluyendo la rugosidad de la superficie y las propiedades dieléctricas del material. (Hasselmann et tal. 1984) (Chang et tal. 2022). 
	En la retrodispercion, hay dos modelos principales: la aproximación de Kirchho mejorada y la teoría geométrica de la dispersión. La aproximación de Kirchhoff Mejorada, es utilizada para estimar los campos de superficie inducidos por las ondas incidentes, esta técnica mejora la precisión de la simulación de datos SAR al considerar la difracciónde los bordes y las esquinas de los objetos (Chang et tal. 2022). Y la teoría Geométrica de la Difracción ,  se emplea para contabilizar los campos de difracción, esta teoría es esencial para modelar la retrodispersión en escenarios complejo (Chang et tal. 2022) 
	Las imágenes SAR polarimétricas son ampliamente utilizadas para la detección y clasificación de áreas urbanas. La retrodispersión predominante en estos entornos es el doble rebote, aunque factores como la orientación de los edificios y la resolución de los datos SAR pueden afectar esta observación (Blasco et tal. 2020). El análisis de las imágenes SAR de la superficie marina permite una comprensión más completa de los fenómenos de retrodispersión en el océano. Los experimentos y modelos teóricos han mejorado la capacidad de interpretar las estadísticas de retrodispersión de una superficie marina en movimiento (Hasselmann et tal. 1984). La técnica de perfilado tomográfico (TP) permite obtener perfiles de retrodispersión verticales a través de volúmenes biogeofísicos, como nieve, hielo y vegetación. Esta técnica utiliza un esquema de procesamiento similar al SAR para producir imágenes con ángulos de incidencia constantes (Morrison & Bennett. 2014). Son los usos de SAR mas frecuentes que se citan en trabajos revisados. 

### CODIGO
 
#### En siguiente codigo, se pocede a cargar las imagenes de la colencción de Sentinel-1 
Empezamos definiendo el espacio que vamos a enfocar nuestro interes, lo que hacemos es crear un poligo y nombrarlo "ROI" o subir una capa de la zona que queremos estudiar. En este caso es 

<details>
  <summary>Clic</summary>

```js	
// Definir la región de interés (ROI)
var roi = /* Inserta aquí tu región de interés */;
Map.centerObject(roi, 10);
```
</details>
<details>
  <summary>Clic</summary>

```js		
// Cargar la colección Sentinel-1 y filtrar por parámetros específicos
var s1 = ee.ImageCollection('COPERNICUS/S1_GRD')
        .filter(ee.Filter.eq('instrumentMode', 'IW')) // Modo Interferometric Wide
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')) // Órbita descendente
        .filterBounds(roi); // Región de interés
```
</details>

Filtrar imágenes de antes y del después del incendio 

<details>
  <summary>Clic</summary>

```js
// Filtrar imágenes antes y después del incendio
var beforeinc = s1.filterDate('2023-04-01', '2023-04-28');
var afterinc = s1.filterDate('2023-05-10', '2023-06-01');
```
</details>

El mosaico ste es un bloque de codigo para ...

<details>
  <summary>Clic</summary>

```js	
// Crear imágenes únicas usando mosaico
beforeinc = beforeinc.mosaic().clip(roi);
afterinc = afterinc.mosaic().clip(roi);
```
</details>

Este es un bloque de codigo para ...

<details>
  <summary>Clic</summary>

```js
// Aplicar filtro de suavizado para reducir el speckle
var SMOOTHING_RADIUS = 50; // Ajustar radio si es necesario
beforeinc = beforeinc.focal_mean(SMOOTHING_RADIUS, 'circle', 'meters');
afterinc = afterinc.focal_mean(SMOOTHING_RADIUS, 'circle', 'meters');
```
</details>

Este es un bloque de codigo para ...

<details>
  <summary>Clic</summary>

```js	
// Parámetros de visualización
var visualization = {
  bands: ['VH'], // Usar banda 'VV' si es necesario
  min: -20,
  max: -5
};
```
</details>

Este es un bloque de codigo para ...

<details>
  <summary>Clic</summary>

```js	
// Visualizar las imágenes antes y después del incendio
Map.addLayer(beforeinc, visualization, 'Antes del incendio');
Map
```
</details>

### CONCLUSIÓN

### *REFERENCIAS:*

_Blasco, J., Fitrzyk, M., Patruno, J., Ruiz-Armenteros, A., & Marconcini, M. (2020). Effects on the Double Bounce Detection in Urban Areas Based on SAR Polarimetric Characteristics. Remote. Sens., 12, 1187. https://doi.org/10.3390/rs12071187._
_Chiang, C., Chen, K., Yang, Y., & Wang, S. (2022). Computation of Backscattered Fields in Polarimetric SAR Imaging Simulation of Complex Targets. IEEE Transactions on Geoscience and Remote Sensing, 60, 1-13. https://doi.org/10.1109/TGRS.2021.3139669_
_Hasselmann, K., Raney, R., Plant, W., Alpers, W., Shuchman, R., Lyzenga, D., Rufenach, C., & Tucker, M. (1985). Theory of synthetic aperture radar ocean imaging: A MARSEN view. Journal of Geophysical Research, 90, 4659-4686. https://doi.org/10.1029/JC090IC03P04659._
 _Morrison, K., & Bennett, J. (2014). Tomographic Profiling—A Technique for Multi-Incidence-Angle Retrieval of the Vertical SAR Backscattering Profiles of Biogeophysical Targets. IEEE Transactions on Geoscience and Remote Sensing, 52, 1350-1355. https://doi.org/10.1109/TGRS.2013.2250508._
	

