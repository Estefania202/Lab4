#Universidad de Costa Rica 
#Facultad de Ciencias Sociales 
#Escuela de Geografía 
#GF-0618 Fotogrametría y Teledetección  

## Laboratorio 4: Imágenes SAR para inundaciones e incendios 

###*Estudiante: Wagner Chacon Ulate C02093*
###*Asistente: Estefanía Pineda Ortega C06005*
###*Docente: Cristian Andres Aguilar-Barboza*


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
