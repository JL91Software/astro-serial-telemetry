# 🚀 Astro Serial Telemetry Base
🌐 **[VER DEMO EN VIVO](https://astrost.pages.dev/)** (Pruébalo con el Simulador integrado)

Un boilerplate (plantilla base) moderno y ultra rápido para visualizar telemetría desde microcontroladores (Arduino, ESP32, STM32) directamente en el navegador.

Este proyecto utiliza la ***Web Serial API*** para leer datos por USB sin necesidad de instalar servidores locales, *Node.js* en el backend, ni bases de datos. Simplemente conecta, abre la web y visualiza.

![Aplicación ScreenShot](/src/assets/AstroSerial_SS.png "Captura de pantalla")

## ✨ Características
* 🔌 __Cero Backend:__ Comunicación directa entre el navegador y el puerto serial del microcontrolador.
* ⚡ __Astro:__ Renderizado estático súper rápido y estructura de componentes modular.
* 📊 __Gráficas en Tiempo Real:__ Integración lista para usar con _Chart.js_ para visualizar métricas.
* 🧠 __Parseo JSON Inteligente:__ El terminal muestra todos los logs, pero extrae y grafica automáticamente los datos formateados en _JSON_.
* 🎨 __Premium Dark Mode:__ Interfaz inspirada en software de laboratorio/ingeniería con _Bootstrap 5_.
* 🧪 __Modo Simulación:__ Botón integrado para probar la UI generando datos falsos, ideal para el desarrollo del frontend sin tener hardware a la mano.

## 🛠️ Stack Tecnológico
* __Framework:__ [Astro](https://astro.build/)
* __Estilos:__ _[Bootstrap 5](https://getbootstrap.com/)_ + CSS Personalizado _(Vanilla)_
* __Lógica Serial:__ Vanilla JavaScript _(Web Serial API)_
* __Gráficos:__ _[Chart.js](https://www.chartjs.org/)_
* __Fuentes:__ Inter (UI) y Fira Code (Terminal)

## 🚀 Instalación y Uso (Desarrollo Frontend)
1. __Clona el repositorio:__ 

    ```
    git clone https://github.com/JL91Software/astro-serial-telemetry.git
    cd astro-serial-telemetry
    ```

2. __Instala las dependencias:__

    ```
    npm install
    ```
3. __Inicia el servidor de desarrollo:__

    ```
    npm run dev
    ```
4. __Abre tu navegador__ _(basado en Chromium)_ en 
`http://localhost:4321`.

## ⚙️¿Cómo enviar datos desde Arduino/ESP32?
El dashboard está programado para ignorar el texto plano en las gráficas (mostrándolo solo en el terminal) y buscar __cadenas en formato JSON__.

Sube este código de ejemplo a tu placa:

```
    void setup() {
    // Asegúrate de que el Baud Rate coincida con el selector en la Web
    Serial.begin(115200);
    delay(1000);
    Serial.println("Iniciando sensores..."); // Esto se verá solo en el terminal
    }
    
    void loop() {
    // Simulamos la lectura de sensores
    float temperatura = 24.5 + random(-10, 10) / 10.0;
    int humedad = random(40, 80);
    int motorRPM = random(1000, 1500);
    
    // Formato estricto JSON: {"temp": 24.5, "hum": 60, "rpm": 1200}
    String payload = "{";
    payload += "\"temp\":" + String(temperatura) + ",";
    payload += "\"hum\":" + String(humedad) + ",";
    payload += "\"rpm\":" + String(motorRPM);
    payload += "}";
    
    // Enviar por serial con salto de línea (\n)
    Serial.println(payload);
    
    delay(500); // Tasa de refresco
    }
```

## 🧩 ¿Cómo adaptar las gráficas a tus propios sensores?
Si quieres medir Voltaje en lugar de Temperatura:
1. __En tu Arduino:__ Cambia la clave JSON a 
`"voltaje": 5.0`.
2. __En tu código Astro (__`index.astro` __o el archivo de scripts):__ Busca la función `updateCharts(dataObj)` y modifica la validación:

    ```
    // Cambiar esto:
    if (dataObj.temp !== undefined) { ... }

    // Por esto:
    if (dataObj.voltaje !== undefined) {
    lineChart.data.labels.push('');
    lineChart.data.datasets[0].data.push(dataObj.voltaje);
    // ...
    }
    ```

3. Recuerda actualizar las etiquetas (`label`) en la configuración de `Chart.js`.

## ⚠️ Compatibilidad de Navegadores
La __Web Serial API__ es una tecnología moderna y por razones de seguridad no está disponible en todos los navegadores.
* ✅ __Soportado:__ Google Chrome, Microsoft Edge, Opera (Desktop).
* ❌ __No Soportado:__ Safari, Firefox (por defecto), navegadores móviles (iOS/Android).

## 📄 Licencia
Este proyecto está bajo la Licencia MIT - mira el archivo [LICENSE](https://github.com/JL91Software/astro-serial-telemetry/blob/main/LICENSE) para más detalles.

Creado con ☕ y 💻 por [JL91 Software](https://github.com/JL91Software)