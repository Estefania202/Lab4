# Universidad de Costa Rica 
# Facultad de Ciencias Sociales 
# Escuela de Geografía 
# GF-0618 Fotogrametría y Teledetección  

## Laboratorio 4: Imágenes SAR para inundaciones e incendios 

*Estudiante: Wagner Chacon Ulate C02093*
*Asistente: Estefanía Pineda Ortega C06005*
*Docente: Cristian Andres Aguilar-Barboza*
 
<span style="font-family: 'Times New Roman', serif; color: #01A6AB;">Introducción</span>

 
Empezamos definiendo el espacio que vamos a enfocar nuestro interes, lo que hacemos es crear un poligo y nombrarlo "ROI" o subir una capa de la zona que queremos estudiar. En este caso es 
<details>
  <summary>Clic</summary>
	
// Definir la región de interés (ROI)
var roi = /* Inserta aquí tu región de interés */;
Map.centerObject(roi, 10);
</details>

En siguiente codigo, se pocede a cargar las imagenes de la colencción de Sentinel-1 

<details>
  <summary>Clic</summary>
	
// Cargar la colección Sentinel-1 y filtrar por parámetros específicos
var s1 = ee.ImageCollection('COPERNICUS/S1_GRD')
        .filter(ee.Filter.eq('instrumentMode', 'IW')) // Modo Interferometric Wide
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')) // Órbita descendente
        .filterBounds(roi); // Región de interés
</details>

Filtrar imágenes de antes y del después del incendio 

<details>
  <summary>Clic</summary>
	
// Filtrar imágenes antes y después del incendio
var beforeinc = s1.filterDate('2023-04-01', '2023-04-28');
var afterinc = s1.filterDate('2023-05-10', '2023-06-01');
</details>

El mosaico ste es un bloque de codigo para ...

<details>
  <summary>Clic</summary>
	
// Crear imágenes únicas usando mosaico
beforeinc = beforeinc.mosaic().clip(roi);
afterinc = afterinc.mosaic().clip(roi);
</details>

Este es un bloque de codigo para ...

<details>
  <summary>Clic</summary>
	
// Cargar la colección Sentinel-1 y filtrar por parámetros específicos
var s1 = ee.ImageCollection('COPERNICUS/S1_GRD')
        .filter(ee.Filter.eq('instrumentMode', 'IW')) // Modo Interferometric Wide
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')) // Órbita descendente
        .filterBounds(roi); // Región de interés

</details>

Este es un bloque de codigo para ...

<details>
  <summary>Clic</summary>
	
// Cargar la colección Sentinel-1 y filtrar por parámetros específicos
var s1 = ee.ImageCollection('COPERNICUS/S1_GRD')
        .filter(ee.Filter.eq('instrumentMode', 'IW')) // Modo Interferometric Wide
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')) // Órbita descendente
        .filterBounds(roi); // Región de interés

</details>

* REFERENCIAS:*

_Blasco, J., Fitrzyk, M., Patruno, J., Ruiz-Armenteros, A., & Marconcini, M. (2020). Effects on the Double Bounce Detection in Urban Areas Based on SAR Polarimetric Characteristics. Remote. Sens., 12, 1187. https://doi.org/10.3390/rs12071187._
_Chiang, C., Chen, K., Yang, Y., & Wang, S. (2022). Computation of Backscattered Fields in Polarimetric SAR Imaging Simulation of Complex Targets. IEEE Transactions on Geoscience and Remote Sensing, 60, 1-13. https://doi.org/10.1109/TGRS.2021.3139669_
_Hasselmann, K., Raney, R., Plant, W., Alpers, W., Shuchman, R., Lyzenga, D., Rufenach, C., & Tucker, M. (1985). Theory of synthetic aperture radar ocean imaging: A MARSEN view. Journal of Geophysical Research, 90, 4659-4686. https://doi.org/10.1029/JC090IC03P04659._
 _Morrison, K., & Bennett, J. (2014). Tomographic Profiling—A Technique for Multi-Incidence-Angle Retrieval of the Vertical SAR Backscattering Profiles of Biogeophysical Targets. IEEE Transactions on Geoscience and Remote Sensing, 52, 1350-1355. https://doi.org/10.1109/TGRS.2013.2250508._
	

