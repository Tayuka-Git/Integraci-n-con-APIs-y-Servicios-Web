# Integración-con-APIs-y-Servicios-Web

el comando a continuación consulta a la API de OpenWeatherMap para obtener el clima actual en La Serena, Chile.

curl "http://api.openweathermap.org/data/2.5/weather?q=La%20Serena,CL&appid=30ffbbcab161d62c59ee99d603f37d80&units=metric&lang=es"

# La estructura de la respuesta en JSON

El comando con JSON nos da respuesta con los datos del clima, temperatura, humedad, presión, descripción del cielo y velocidad del viento, que luego se puede analizar o guardar.

curl "http://api.openweathermap.org/data/2.5/weather?q=La%20Serena,CL&appid=30ffbbcab161d62c59ee99d603f37d80&units=metric&lang=es" | jq

{
  "coord": {
    "lon": -71.2542,
    "lat": -29.9078
  },
  "weather": [
    {
     "id": 800,
      "main": "Clear",
      "description": "cielo claro",
      "icon": "01d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 17.73,
    "feels_like": 17.2,
    "temp_min": 17.73,
    "temp_max": 17.73,
    "pressure": 1015,
    "humidity": 63,
    "sea_level": 1015,
    "grnd_level": 990
  },
  "visibility": 10000,
  "wind": {
    "speed": 3.6,
    "deg": 270
  },
  "clouds": {
    "all": 0
  },
  "dt": 1748458049,
  "sys": {
    "type": 1,
    "id": 8514,
    "country": "CL",
    "sunrise": 1748431843,
    "sunset": 1748469257
  },
  "timezone": -14400,
  "id": 3884373,
  "name": "La Serena",
  "cod": 200
}

# Comando Curl para obtener las noticias del dia

Con este comando se pueden obtener las noticias del dia en chile 

curl "https://newsapi.org/v2/top-headlines?country=cl&apiKey=b516d2284f1243c8a894c3c0d7c583e4" | jq

# Comando Curl para obtener información de Chile (Moneda, Población, capital)

Con este comando de curl mas JSON podemos obtener la informacion de la Moneda, Capital y Población

curl https://restcountries.com/v3.1/name/chile | jq ".[] | {Capital: .capital[0], Poblacion: .population, Moneda: .currencies}"

{
  "Capital": "Santiago",
  "Poblacion": 19116209,
  "Moneda": {
    "CLP": {
      "symbol": "$",
      "name": "Chilean peso"
    }
  }
}

La respuesta de la API https://restcountries.com/v3.1/name/chile es una lista que contiene un solo objeto con los datos de Chile.

Dentro de este objeto:

"capital" es una lista que contiene la capital del país. En el caso de Chile, es ["Santiago"], por lo que se accede como capital[0].

"population" indica la población total, como 19116209. Es un número entero.

"currencies" es un objeto donde la clave es el código de la moneda (por ejemplo, "CLP"). Dentro de ese objeto, encontramos "name" (por ejemplo, "Chilean peso") y "symbol" (por ejemplo, "$").

