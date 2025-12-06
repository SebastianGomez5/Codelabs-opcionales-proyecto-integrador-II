## 1. ¿Qué es un ESP32 y qué características lo hacen adecuado para crear un servidor web pequeño?
El **ESP32** es un "System on a Chip" (SoC) de bajo costo y bajo consumo, desarrollado por Espressif Systems. Es el hermano mayor del famoso ESP8266.
**Características clave para servidor web:**
* **Conectividad Dual:** Trae WiFi y Bluetooth integrados de fábrica. ¡Una belleza!
* **Doble Núcleo:** Procesador potente (Xtensa LX6) que permite manejar la conexión WiFi en un núcleo y tu código en el otro.
* **Memoria:** Tiene suficiente RAM y Flash para almacenar páginas HTML sencillas y manejar conexiones sin "colgarse" fácilmente.
* **I/O (Entradas/Salidas):** Muchos pines para controlar LEDs, sensores y relés directamente desde la interfaz web.

## 2. Concepto de servidor web y el ESP32
Un **servidor web** es, en términos sencillos, un dispositivo o programa que espera peticiones (como alguien tocando una puerta) y entrega información (páginas web, datos JSON, imágenes) a quien lo pide (cliente/navegador).
* **El ESP32 como servidor:** Al conectarse al WiFi, el ESP32 obtiene una dirección IP. Usando librerías de código, se queda "escuchando" en esa IP. Cuando tú escribes su IP en el navegador (ej: `192.168.1.50`), el ESP32 recibe la petición, procesa qué quieres (ej: ver la página de inicio) y te envía el código HTML de respuesta.

## 3. Uso de la biblioteca `WiFi.h`
Es la caja de herramientas fundamental para que el ESP32 deje de ser un chip aislado y se hable con el mundo.
**Funcionalidades principales:**
* **Conectar a redes:** `WiFi.begin(ssid, password)`.
* **Modo Estación (STA):** Para conectarse a tu router como un celular más.
* **Modo Punto de Acceso (AP):** Para que el ESP32 cree su propia red WiFi y tú te conectes a él.
* **Diagnóstico:** Revisar intensidad de señal (RSSI), IP asignada, y estado de la conexión (`WiFi.status()`).

## 4. El papel del protocolo HTTP
**HTTP (Hypertext Transfer Protocol)** es el "idioma" que hablan el navegador y el ESP32.
* **Petición (Request):** El navegador dice "Oye, ESP32, dame el archivo `/index.html`" (usualmente mediante un método `GET`).
* **Respuesta (Response):** El ESP32 responde "Listo, aquí está, todo OK (Código 200)" y entrega el texto del HTML.
Sin HTTP, solo se estarían enviando bits sin sentido; HTTP pone las reglas de la conversación.

## 5. El proceso de "escuchar" en un puerto (Port 80)
Imagina que la dirección IP es la dirección de un edificio de apartamentos. El **puerto** es el número del apartamento específico.
* **Puerto 80:** Es el estándar universal para tráfico web no seguro (HTTP).
* **Importancia:** El ESP32 debe estar "con la oreja parada" (listening) específicamente en el puerto 80. Si escuchara en el 8080 pero el navegador toca en el 80, nadie abre la puerta. Es vital para que la comunicación se establezca correctamente.

## 6. Manejo de peticiones entrantes
El ESP32 actúa como un recepcionista.
1.  Detecta que un cliente se conectó (`server.available()`).
2.  Lee lo que el cliente envía (la petición HTTP) carácter por carácter o línea por línea.
3.  **Ejemplo LED:** El código busca palabras clave en la URL.
    * Si la petición dice `GET /H`, el código detecta la "H" y pone el pin del LED en `HIGH`.
    * Si dice `GET /L`, detecta la "L" y lo pone en `LOW`.

## 7. Dirección IP local
Es la identificación única del ESP32 dentro de tu red doméstica (LAN).
* **Asignación:** Generalmente la asigna el router mediante un protocolo llamado **DHCP** cuando el ESP32 se conecta con usuario y contraseña.
* **Importancia:** Sin ella, el navegador no sabría a dónde enviar la petición. Es como saber el nombre de tu amigo pero no su número de celular; necesitas el número (IP) para llamar.

## 8. WiFi Abierta vs. Segura
* **Abierta:** No requiere contraseña. Los datos viajan "al aire" y cualquiera puede conectarse.
* **Segura (WPA2/WPA3):** Requiere contraseña y los datos de conexión viajan encriptados.
* **En proyectos ESP32:** Usar redes seguras es obligatorio si no quieres que el vecino controle tus luces. En el código, la diferencia es pasar o no el parámetro `password` en `WiFi.begin(ssid, password)`.

## 9. Desafíos de seguridad y mitigación
Exponer un microcontrolador a una red tiene riesgos.
* **Desafíos:** El ESP32 tiene poca potencia para encriptación pesada, ataques de denegación de servicio (DoS) pueden "tumbarlo" fácil, y HTTP envía datos en texto plano (sin cifrar).
* **Mitigación:**
    * No exponerlo directamente a Internet (usar VPN si necesitas acceso remoto).
    * Validar siempre los datos de entrada (que no te envíen comandos raros).
    * Usar redes WiFi exclusivas para IoT (Guest Network) si es posible.

## 10. Uso de `WiFiClient`
Es el objeto que representa al "visitante" (el navegador web) dentro del código del ESP32.
**Ciclo de vida:**
1.  **Conexión:** `WiFiClient client = server.available();` (Nace el cliente).
2.  **Verificación:** `if (client)` (¿Hay alguien ahí?).
3.  **Lectura/Escritura:** `client.read()` (Escuchar qué pide) y `client.print()` (Responderle).
4.  **Cierre:** `client.stop()` (Colgar la llamada). Es crucial cerrar la conexión para liberar recursos para el siguiente visitante.

## 11. Inicialización del servidor web
Se realiza típicamente en el `setup()`.
* **Código:** `server.begin();`
* **Puerto:** Por defecto, en la mayoría de ejemplos se declara al inicio como `WiFiServer server(80);`, por lo que escucha en el **puerto 80**.

## 12. Creación de instancia `WiFiServer`
Antes del `setup`, se crea el objeto globalmente:
`WiFiServer server(80);`
Aquí le estás diciendo al ESP32: "Crea un servidor y configura la 'puerta de entrada' en el número 80". Si quisieras otro puerto (como 8080), cambiarías ese número ahí.

## 13. Propósito de `scanNetworks()`
Sirve para mirar qué redes hay alrededor, como cuando abres el WiFi en tu celular.
* **Información impresa:** Generalmente imprime el **SSID** (nombre de la red), el **RSSI** (fuerza de la señal, en dBm) y el tipo de **encriptación** (si tiene candadito o no).

## 14. Utilidad del escaneo de redes
* **Diagnóstico:** Saber si el ESP32 está muy lejos del router (RSSI bajo, tipo -90dBm).
* **Configuración:** Permite crear proyectos donde el usuario selecciona su red de una lista en lugar de escribirla a ciegas (hardcoded).

## 15. Establecimiento de conexión WiFi
Se usa `WiFi.begin(ssid, password)`.
* **Si falla:** La conexión no es instantánea. Por eso el código no pasa inmediatamente; se queda en un bucle esperando. Si nunca conecta (clave errónea o router apagado), el ESP32 se quedará intentando eternamente o hasta que programes un tiempo límite (timeout).

## 16. Bucle de espera de conexión
Es el famoso `while` en el `setup()`:
```cpp
while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
}