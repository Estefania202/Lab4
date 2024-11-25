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

#### Analisis de imagenes de inundación con Sentinel-1
El codigo se encuentra en este link: https://code.earthengine.google.com/3aec8f0f18df775cf37bae0046297701
La iamgenes se encuentran en el codigo adujto, se nota que en la zona más plana es donde hay una mayor afectación por inundación.

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
Vamos a cargar la colección de copernicus que corresponden a imágenes radar (SAR) estas cuentan con una buena calidad. La idea de aplicar filtros con el fin de reducir la cantidad de datos que no favorecen al objeto de estudio. Hay filtros por fechas, por zona a estudiar.  
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

El Filtrar imágenes de antes y del después del incendio; propicia algunos puntos a cubrir con la metodología de comparación de fotos del antes y del después fomentaría una, evaluación de daños, un monitoreo con cambios más detallados de la vegetación y erosión. asi como ver contemplar métodos de recuperción. Como un buen diseño de reforestación, indentificar factores que fueron o serían un futuro factor de riesgo.

<details>
  <summary>Clic</summary>

```js
// Filtrar imágenes antes y después del incendio
var beforeinc = s1.filterDate('2023-04-01', '2023-04-28');
var afterinc = s1.filterDate('2023-05-10', '2023-06-01');
```
</details>

El mosaico tiene como objetivo combinar múltiples imágenes superpuestas de una colección en una sola imagen única. Al obtener el mosaico se puede facilitar los análisis y poder hacer una facil comparación.   

<details>
  <summary>Clic</summary>

```js	
// Crear imágenes únicas usando mosaico
beforeinc = beforeinc.mosaic().clip(roi);
afterinc = afterinc.mosaic().clip(roi);
```
</details>

El specle, es un factor que dificultan el observar una imagen como se debe y este puede ser causado por muchos motivos. Es como tomarle una foto a un papel arrugado y que refleja de forma irregular la luz, así pasa con estas imágenes SAR. Aunque en Sar, estos ruidos oscurecen sectores muy finos y específicos de la imagen. P}Para corregir esto, están disponibles filtros como el de Lee, que calcula la varianza local y con ella calcula el valor de cada píxel. Otro es el filtro de Kuan, similar al de Le pero que pondera de otra manera los pixeles. Y el filtro de Frost, este usa la técnica estadística de máxima verosimilitud y así se adapta a la imagen.

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

#### En siguiente codigo, se pocede a cargar las imagenes de la colencción de Sentinel-2
</details>

Empezamos cargando la coleccion de imagenes de Sentinel2

<details>
  <summary>Clic</summary>

```js	
var s2 = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED").filterBounds(roi) //s2 fue el nombre que le coloque a la coleccion que filtre.
  .filterDate('2023-01-01', '2023-12-31') //Defina el rango de fechas.
  .filterBounds(roi) // filtro de area.
  .map(cloudMask) // aca ejecutamos el enmascador de nubes que programamos antes. 
  print(s2) 
```
</details>

</details>

Luego empezamos a realizar filtros, que como mencionamos antes esto facilitara el analisis comparativo de los daños generados por el incendio. 

<details>
  <summary>Clic</summary>

```js	
// Filtros de la coleccion ANTES del incendio
var antes = s2.filter(ee.Filter.or(
 ee.Filter.date('2023-04-01', '2023-04-28')))
print(antes, 'antes del incendio s2');

// Filtros de la coleccion DESPUES del incendio
var despues = s2.filter(ee.Filter.or(
 ee.Filter.date('2023-05-10', '2023-06-01')))
print( despues, 'despues del incendio s2');
```
</details>

</details>

En este proceso se generan imágenes limpias del antes y después del incendio y permite visualizarlas en el mapa. Depende del analisis se puede tomar como variable el mosaic, la mean o median

<details>
  <summary>Clic</summary>

```js	
var antes2 = antes.mosaic().clip(roi) //puedes cambiar mosaic por mean or median
var despues2 =  despues.mosaic().clip(roi)
print(antes, 'imagen antes del incendio')
print(despues, 'imagen despues del incendio')

Map.addLayer( antes2,{bands: ['B4', 'B3', 'B2'], min: 354.3920564417735, max: 1282.2158558183125, gamma: 1.2}, 'antes del incendio s2', 0);

```
</details>

</details>
Se espera que se calcule el indice de quemadura para imagenes del antes y despues del incendio, por medio de este podemos comprender el imacto que tuvo el incendio sobre la vegetación o el suelo
<details>
  <summary>Clic</summary>

```js	
var preNBR = antes2.normalizedDifference(['B8', 'B12']).rename('nbr');
var postNBR = despues2.normalizedDifference(['B8', 'B12']).rename('nbr');
print(preNBR)
```
</details>

</details>

</details>
Aqui hay una vizualización del NBR de las imagenes.
<details>
  <summary>Clic</summary>

```js	
Map.addLayer(preNBR, 
{bands: ['nbr'], min: 0.018902123252200934, max:0.7007203002942072 , gamma: 1.2}, 'nbr antes'); 
//Visualizacion
Map.addLayer(postNBR, 
{bands: ['nbr'], min: 0.018902123252200934, max:0.7007203002942072 , gamma: 1.2}, 'nbr despues'); 
```
</details>

</details>
En este apartado se procura calcular la diferencia para ver variaciones de vegetacion del antes y del despues del incendio
<details>
  <summary>Clic</summary>

```js	
// The result is called delta NBR or dNBR
var dNBR_unscaled = preNBR.subtract(postNBR);

// Scale product to USGS standards
var dNBR = dNBR_unscaled.multiply(1000);

// Add the difference image to the console on the right
print("Difference Normalized Burn Ratio: ", dNBR);
```
</details>

</details>
En este apartado esta enfocado en visualizar los mapas en gris
<details>
  <summary>Clic</summary>

```js	
var grey = ['white', 'black'];

Map.addLayer(dNBR, {min: -1000, max: 1000, palette: grey}, 'dNBR greyscale');
```
</details>
</details>
A continuación se trata de calsificar lo más quemado a lo que no se quemo
<details>
  <summary>Clic</summary>

```js	
var sld_intervals =
  '<RasterSymbolizer>' +
    '<ColorMap type="intervals" extended="false" >' +
      '<ColorMapEntry color="#ffffff" quantity="-500" label="-500"/>' +
      '<ColorMapEntry color="#7a8737" quantity="-250" label="-250" />' +
      '<ColorMapEntry color="#acbe4d" quantity="-100" label="-100" />' +
      '<ColorMapEntry color="#0ae042" quantity="100" label="100" />' +
      '<ColorMapEntry color="#fff70b" quantity="270" label="270" />' +
      '<ColorMapEntry color="#ffaf38" quantity="440" label="440" />' +
      '<ColorMapEntry color="#ff641b" quantity="660" label="660" />' +
      '<ColorMapEntry color="#a41fd6" quantity="2000" label="2000" />' +
    '</ColorMap>' +
  '</RasterSymbolizer>';

// Add the image to the map using both the color ramp and interval schemes.
Map.addLayer(dNBR.sldStyle(sld_intervals), {}, 'dNBR classified');
```
</details>

### CONCLUSIÓN
Mediante el uso de imágenes captadas por satélites, como las disponibles en Sentinel-2, se hace posible llevar a cabo un análisis de los efectos de eventos tales como los incendios forestales, siendo en este sentido de especial utilidad el uso del índice NBR (Normalised Burnt Ratio), así como la comparación de imágenes que se tomaron antes y después de que se produjera un incendio forestal. De esta forma se hace posible evaluar las superficies afectadas y la severidad de las mismas. Este tipo de metodología permite no solo aportar información útil no sólo para la respuesta inmediata y la planificación de la recuperación, sino también garantizar la previsión y la mitigación de riesgos a futuro, contemplando en su implementación la exigencia de poder sustentar la toma de decisiones, la cual debe ser llevada a cabo por la gestión del ambiente y la gestión de desastres.
Por otro lado, la posibilidad de contar con las herramientas que ofrecen la teledetección esas imágenes de satélites, junto con sistemas de información geográfica (GIS), permite llevar a cabo un seguimiento de terrenos extensos de una forma sencilla y mucho más eficiente; se consigue de este modo una optimización de recursos y una gran reducción de tiempos de intervención. Las técnicas utilizadas en este sentido son en especial el mosaico, el NDVI (normalized difference vegetation index), el NBR (normalized burn ratio), así como la analítica de series temporales para generar mapas donde se pueden visualizar los impactos producidos por los incendios.
En conclusión, el análisis de imágenes satelitales y la aplicación de diversas metodologías que se desprenden de la teledetección permiten optimizar las estrategias de gestión de desastres, restauración ecológica y prevención; ello hace que la gestión del medio afectado por un incendio pueda ser llevada a cabo de una forma más eficiente y sostenible del mismo. 

### *REFERENCIAS:*

_Blasco, J., Fitrzyk, M., Patruno, J., Ruiz-Armenteros, A., & Marconcini, M. (2020). Effects on the Double Bounce Detection in Urban Areas Based on SAR Polarimetric Characteristics. Remote. Sens., 12, 1187. https://doi.org/10.3390/rs12071187._
_Chiang, C., Chen, K., Yang, Y., & Wang, S. (2022). Computation of Backscattered Fields in Polarimetric SAR Imaging Simulation of Complex Targets. IEEE Transactions on Geoscience and Remote Sensing, 60, 1-13. https://doi.org/10.1109/TGRS.2021.3139669_
_Hasselmann, K., Raney, R., Plant, W., Alpers, W., Shuchman, R., Lyzenga, D., Rufenach, C., & Tucker, M. (1985). Theory of synthetic aperture radar ocean imaging: A MARSEN view. Journal of Geophysical Research, 90, 4659-4686. https://doi.org/10.1029/JC090IC03P04659._
 _Morrison, K., & Bennett, J. (2014). Tomographic Profiling—A Technique for Multi-Incidence-Angle Retrieval of the Vertical SAR Backscattering Profiles of Biogeophysical Targets. IEEE Transactions on Geoscience and Remote Sensing, 52, 1350-1355. https://doi.org/10.1109/TGRS.2013.2250508._
	

