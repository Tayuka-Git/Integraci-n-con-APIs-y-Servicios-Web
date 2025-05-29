# Integración-con-APIs-y-Servicios-Web

el comando a continuación consulta a la API de OpenWeatherMap para obtener el clima actual en La Serena, Chile.
Puedes remplazar en TU_API_KEY con tu propia clave API de la pagina openweathermap.org para obtener la información del clima.

curl "http://api.openweathermap.org/data/2.5/weather?q=La%20Serena,CL&appid=30ffbbcab161d62c59ee99d603f37d80&units=metric&lang=es"

curl "http://api.openweathermap.org/data/2.5/weather?q=La%20Serena,CL&appid=TU_API_KEY&units=metric&lang=es"

# La estructura de la respuesta en JSON

El comando con JSON nos da respuesta con los datos del clima, temperatura, humedad, presión, descripción del cielo y velocidad del viento, que luego se puede analizar o guardar.
Aqui tambien puedes usar tu porpia API KEY para obtener la información

curl "http://api.openweathermap.org/data/2.5/weather?q=La%20Serena,CL&appid=30ffbbcab161d62c59ee99d603f37d80&units=metric&lang=es" | jq

curl "http://api.openweathermap.org/data/2.5/weather?q=La%20Serena,CL&appid=TU_API_KEY&units=metric&lang=es" | jq

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

curl "https://newsapi.org/v2/top-headlines?country=cl&apiKey=TU_API_KEY" | jq

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

# Comando para obtener la consulta meteorológica para el pronóstico de 5 días

Con este comando puedes obtener el pronóstico de 5 días del clima

curl "http://api.openweathermap.org/data/2.5/forecast?q=Santiago,CL&units=metric&appid=30ffbbcab161d62c59ee99d603f37d80"

curl "http://api.openweathermap.org/data/2.5/forecast?q=Santiago,CL&units=metric&appid=TU_API_KEY"

# Filtrado de Noticias

Con este comando puedes hacer un filtrado de las noticias con la categoria que gustes

curl "https://newsapi.org/v2/top-headlines?country=cl&category=technology&apiKey=b516d2284f1243c8a894c3c0d7c583e4"

curl "https://newsapi.org/v2/top-headlines?country=cl&category=technology&apiKey=TU_API_KEY"

# Manejo de errores HTTP

Usa -i o -w para ver los códigos de respuesta HTTP con curl. Ejemplo de mal uso (sin API key), hay diferentes tipos de errores HTTP como por ejemplo: 401 API key inválida, 404 Ciudad no encontrada, 429 Límite de solicitudes excedido. A continuación se muestra uno de los errores y su solución:

curl -i "https://newsapi.org/v2/top-headlines?country=cl"

HTTP/1.1 401 Unauthorized
Date: Wed, 28 May 2025 20:21:58 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 158
Connection: keep-alive
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
cf-cache-status: DYNAMIC
Report-To: {"endpoints":[{"url":"https:\/\/a.nel.cloudflare.com\/report\/v4?s=BUGRZjg6WEZhT6KkJaGNGLPQNyUPQvtKiD5JZ9jCT%2B8UU1CZjyaW%2BvRNTW6%2B%2BFcszCJ5PYIyLfi5fjSWhZE95yAiRQpvuff8FxGoOejfVWJgb3dFZBmbhoZLL%2Fy%2B"}],"group":"cf-nel","max_age":604800}
NEL: {"success_fraction":0,"report_to":"cf-nel","max_age":604800}
Server: cloudflare
CF-RAY: 9470761e2c25498a-MIA
server-timing: cfL4;desc="?proto=TCP&rtt=112811&min_rtt=110950&rtt_var=34663&sent=5&recv=6&lost=0&retrans=0&sent_bytes=3129&recv_bytes=666&delivery_rate=34390&cwnd=252&unsent_bytes=0&cid=75dbc8a53f01a324&ts=186&x=0"

{"status":"error","code":"apiKeyMissing","message":"Your API key is missing. Append this to the URL with the apiKey param, or use the x-api-key HTTP header."}

Como se puede ver nos muestra el error HTTP 401 Unauthorized, lo que significa que falta la API_KEY o que la API_KEY es incorrecta, Esto significa que la API requiere autenticación mediante una clave, y no se proporciono en el parámetro apiKey.
Esto se soluciona colocando el parametro apiKey en el comando como se muestra a continuación:

curl -i "https://newsapi.org/v2/top-headlines?country=cl&apiKey=b516d2284f1243c8a894c3c0d7c583e4"

curl -i "https://newsapi.org/v2/top-headlines?country=cl&apiKey=TU_API_KEY"

HTTP/1.1 200 OK
Date: Wed, 28 May 2025 20:32:21 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 46
Connection: keep-alive
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
X-Cached-Result: false
cf-cache-status: DYNAMIC
Report-To: {"endpoints":[{"url":"https:\/\/a.nel.cloudflare.com\/report\/v4?s=KeyjQ1fhsiWpqQ0hNbFpavyJtsLI6a7swjl2wQ4t5%2BYVV6UGTAoPALPBty%2BoBO%2FuxZQXqjykpDFiLqULfYxhy2xdl%2BEHHlP7ST7UfhCFuaajhydZzJ%2FkOsm0xI%2Fz"}],"group":"cf-nel","max_age":604800}
NEL: {"success_fraction":0,"report_to":"cf-nel","max_age":604800}
Server: cloudflare
CF-RAY: 947085555b4f8df0-MIA
server-timing: cfL4;desc="?proto=TCP&rtt=139113&min_rtt=122575&rtt_var=46775&sent=5&recv=6&lost=0&retrans=0&sent_bytes=3130&recv_bytes=706&delivery_rate=32403&cwnd=252&unsent_bytes=0&cid=9d5d2b6f4e6c8d45&ts=325&x=0"

{"status":"ok","totalResults":0,"articles":[]}

Y como se puede ver ahora nos muestra HTTP 200 OK, lo que significa que la solicitud fue exitosa.

# Documentacion sobre las APIS
OpenWeatherMap API
¿Que informacion da?
Da la informacion sobre el clima actual de La Serena: temperatura, humedad y descripción.
Ejemplo:
"Clima: 18°C, Soleado."

NewsAPI
¿Que nos proporciona?
Nos da las 3 noticias más importantes de Chile.
Ejemplo:
"Noticia: Nuevas leyes laborales (CNN Chile)."

REST Countries API
¿Que es lo que hace?
Muestra datos básicos de Chile: capital, población y moneda.
Ejemplo:
"Capital: Santiago | Población: 19 millones."

Gmail API
¿Para qué se usa ?
Envia el resumen diario por correo automáticamente.
¿Cómo?
Usando credentials.json para permisos seguros.

¿Cómo trabajan juntas?
dashboard.py: Junta todos los datos (clima + noticias + país).
gmail.py: Envía el resumen final a tu correo todos los días a las 8 AM.


# Requisitos para hacer funcionar el codigo

Requisitos para Ejecutar el Código
Python 3.8+ 
Se Descarga e instala desde python.org.

Archivos esenciales

credentials.json (obtenido desde Google Cloud Console para la API de Gmail).

token.json (se genera automáticamente la primera vez que se ejecuta).

Librerías necesarias:

requests

python-dotenv

google-api-python-client

google-auth-oauthlib

schedule

Se puedenen instalar con el siguiente comando:
pip install requests python-dotenv google-api-python-client google-auth-oauthlib schedule

Las APis y Claves que son necesarias son las de:

La clave de OpenWeatherMap (gratis en openweathermap.org).

Clave de NewsAPI (gratis en newsapi.org).

Ejecución del programa

Corre el archivo Main.py 

¡Listo! El sistema enviará un reporte diario a las 8 AM o cuando lo ejecutes manualmente.

Nota: La primera vez que ejecutes el programa, se abrirá el navegador para autenticar tu cuenta de Gmail (solo ocurre una vez).
