# API_SIMEM

## Índice
1. [Descripción general](#section1)
2. [Variables disponibles para consumir en la API XM](#section2)
3. [Soluciones diseñadas para consumir la API](#section3)
4. [Cómo realizar solicitudes filtrando por atributos específicos\?](#section4)
5. [Restricciones de la API](#section5)
6. [Comentarios finales](#section6)
7. [Elementos necesarios para utilizar el servicio desde cualquier cliente](#section7)

<a id='section1'></a>
## Descripción general
Con el propósito de brindar información pertinente para la utilización de la API se ha creado este repositorio en el cual se da explicación de como utilizarla con herramientas de consulta para obtener la información requerida. 
El objetivo de esta guía es que el usuario tenga la capacidad de consumir los servicios expuestos haciendo uso de la herramienta que prefiera para dicho fin. 

<a id='section2'></a>
## Variables disponibles para consumir en la API XM

<strong>Python</strong>

<code>import http.client/n 
  conn = http.client.HTTPSConnection("func-simemfile-prb-01.azurewebsites.net")/n
  payload = ''
  headers = {}
  conn.request("GET", "/api/Http_Get_ParquetData?fechaInicio=2023-01-16&fechaFin=2023-01-17&dataId=6E1BCC6B-2F1D-4E6B-976F-54CA056F9729&descargable=false", payload, headers)
  res = conn.getresponse()
  data = res.read()
  print(data.decode("utf-8"))</code>

<strong>Jquery</strong>

<code>
var settings = {
      "url": "https://func-simemfile-prb-01.azurewebsites.net/api/Http_Get_ParquetData?fechaInicio=2023-01-16&fechaFin=2023-01-17&dataId=6E1BCC6B-2F1D-4E6B-976F-54CA056F9729&descargable=false",
      "method": "GET",
      "timeout": 0,
    };
    
    $.ajax(settings).done(function (response) {
      console.log(response);
    });</code>

<strong>Ruby</strong>
<code>
  require "uri"
  require "net/http"
  
  url = URI("https://func-simemfile-prb-01.azurewebsites.net/api/Http_Get_ParquetData?fechaInicio=2023-01-16&fechaFin=2023-01-17&dataId=6E1BCC6B-2F1D-4E6B-976F-54CA056F9729&descargable=false")
  
  https = Net::HTTP.new(url.host, url.port)
  https.use_ssl = true
  
  request = Net::HTTP::Get.new(url)
</code>

<a id='section3'></a>
## Soluciones diseñadas para consumir la API

Tal como se indicó al inicio, el equipo de Analítica ha diseñado dos aproximaciones para consumir el servicio en los siguientes lenguajes:

|Lenguaje|Nombre|Tipo|Instalación|Habilidad requerida|
|--------|------|----|-----------|-------------------|
|Python|pydataxm|Librería| <code> $ pip install pydataxm </code>|Low Code|
|Excel (VBA) | Consulta_API_XM.xlsm|Macro|No Aplica|No Code|

<a id='section4'></a>
## ¿Cómo realizar solicitudes filtrando por atributos específicos? _(Parámetro opcional)_
En caso de no ser especificado dentro de la solicitud, el servicio retornará todos los registros disponibles. 

Con este parámetro se permite extraer datos para una serie de entidades personalizada. Las métricas que pueden ser filtradas son todas aquellas que tienen cruces por:

1. Agente (código SIC del agente _i.e._ CASC, EPMC, ENDG, entre otros)
2. Recurso (código SIC del recurso _i.e._ EPFV, TBST, JEP1, entre otros)
3. Embalse (nombre del embalse _i.e._ EL QUIMBO, GUAVIO, PENOL, entre otros)
4. Río (nombre del río _i.e._ FLORIDA II, BOGOTA N.R., DESV. MANSO, entre otros)

Para conocer el detalle de los códigos SIC de cada recurso o agente le invitamos a consultar las métricas _ListadoRecursos_ y _ListadoAgentes_ disponibles en este mismo servicio.

Para conocer el detalle de los nombres de cada río o embalse le invitamos a consultar las métricas _ListadoRios_ y _ListadoEmbalse_ disponibles en este mismo servicio.

En la carpeta _examples_ encontrará los ejemplos para consumir el servicio usando filtros. [Ir a ejemplos](https://github.com/EquipoAnaliticaXM/API_XM/tree/master/examples)

<a id='section5'></a>
## Restricciones de la API:
Con el fin de no congestionar el servicio, se han establecido restricciones a las consultas así:
* Para datos horarios y diarios, máximo 30 días por llamado
* Para datos mensuales, máximo 731 días por llamado
* Para datos anuales, máximo 366 días por llamado

<a id='section6'></a>
## Comentarios finales
Tener en cuenta que el formato de fecha que recibe la API es YYYY-MM-DD

<a id='section7'></a>
## Elementos necesarios para utilizar el servicio desde cualquier cliente
A continuación, presentamos el listado de métricas disponibles y los parámetros requeridos para realizar peticiones de información:

1. Método: POST
2. Endpoint: 
* www.servapibi.xm.com.co/hourly
* www.servapibi.xm.com.co/daily
* www.servapibi.xm.com.co/monthly
* www.servapibi.xm.com.co/lists
3. Body petición:
```
{"MetricId": "MetricID",
"StartDate": _"YYYY-MM-DD",
"EndDate":_"YYYY-MM-DD",
"Entity": "Cruce",
"Filter":["Listado de codigos"]}
```
**Nota:** El parámetro _Filter_ es opcional y solo aplica para variables diferente al cruce por _Sistema_
## Ejemplo para realizar una petición

```
POST: http://servapibi.xm.com.co/hourly

Body:
{"MetricId": "Gene",
"StartDate":"2022-09-01",
"EndDate":"2022-09-02",
"Entity": "Recurso",
"Filter":["TBST","GVIO"]}
```
Para conocer el inventario total de variables, cruces y filtros opcionales, consultar:

```
http://servapibi.xm.com.co/lists

Body:
{"MetricId": "ListadoMetricas"}
```
