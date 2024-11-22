#Universidad de Costa Rica 
#Facultad de Ciencias Sociales 
#Escuela de Geografía 
#GF-0618 Fotogrametría y Teledetección  

## Laboratorio 4: Imágenes SAR para inundaciones e incendios 

*Estudiante: Wagner Chacon Ulate C02093*
*Asistente: Estefanía Pineda Ortega C06005*
*Docente: Cristian Andres Aguilar-Barboza*


Este es un bloque de codigo para ...

<details>
  <summary>Clic</summary>

  
//Coleccion de imagenes de Sentinel-1
var s1 = ee.ImageCollection('COPERNICUS/S1_GRD')
        //.filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV','VH'))
        .filter(ee.Filter.eq('instrumentMode', 'IW'))
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')) // puede ajustar a ASCENDING
        .filterBounds(roi)
  

</details>
##REFERENCIAS:

_Blasco, J., Fitrzyk, M., Patruno, J., Ruiz-Armenteros, A., & Marconcini, M. (2020). Effects on the Double Bounce Detection in Urban Areas Based on SAR Polarimetric Characteristics. Remote. Sens., 12, 1187. https://doi.org/10.3390/rs12071187._
_Chiang, C., Chen, K., Yang, Y., & Wang, S. (2022). Computation of Backscattered Fields in Polarimetric SAR Imaging Simulation of Complex Targets. IEEE Transactions on Geoscience and Remote Sensing, 60, 1-13. https://doi.org/10.1109/TGRS.2021.3139669_
_Hasselmann, K., Raney, R., Plant, W., Alpers, W., Shuchman, R., Lyzenga, D., Rufenach, C., & Tucker, M. (1985). Theory of synthetic aperture radar ocean imaging: A MARSEN view. Journal of Geophysical Research, 90, 4659-4686. https://doi.org/10.1029/JC090IC03P04659._
 _Morrison, K., & Bennett, J. (2014). Tomographic Profiling—A Technique for Multi-Incidence-Angle Retrieval of the Vertical SAR Backscattering Profiles of Biogeophysical Targets. IEEE Transactions on Geoscience and Remote Sensing, 52, 1350-1355. https://doi.org/10.1109/TGRS.2013.2250508._
	

