# 🪐 Nexora Notify

## 🚀 Características Principales

* 🖱️ **Drag & Drop:** Sistema interactivo para arrastrar la interfaz con el ratón en tiempo real.
* 💾 **Lugar Notificaciones:** La posición elegida por el usuario se guarda para siempre en su PC.
* 🧼 **Cola Inteligente:** Las notificaciones se apilan de forma fluida sin pisarse unas a otras.
* 🎨 **Glow Gradients:** 4 tipos de alertas visuales con animaciones y barras de progreso dinámicas (`success`, `error`, `warning`, `system`).
* 🔊 **Audio Nativo:** Sonido sutil sintetizado por el navegador.

---

## 🎥 Video Nexora Notify

[YouTube Link](https://www.youtube.com/watch?v=xawtX6WA_CU)

---

## 📦 Instalación y Requisitos

1. Descarga e introduce la carpeta `nexora_notify` en el directorio `resources/` de tu servidor.
2. Asegúrate de que el nombre de la carpeta sea exactamente `nexora_notify`.
3. Añade la siguiente línea en tu archivo `server.cfg`:
   ```cfg
   ensure nexora_notify
   ```

---

## 🔌 Compatibilidad Global Automatizada

Si quieres que **nexora_notify** reemplace AUTOMÁTICAMENTE todas las notificaciones por defecto de tu servidor (tiendas, garajes, trabajos, etc.) sin tener que modificar tus scripts uno a uno, sigue estos pasos:

1. Abre el archivo `es_extended/client/functions.lua` de tu base de datos.
2. Busca la función original llamada `ESX.ShowNotification`.
3. Reemplázala por completo por el siguiente código optimizado:

```lua
ESX.ShowNotification = function(msg, notifyType)
    local type = notifyType == 'error' and 'error' or (string.find(msg, "~r~") and 'error' or (string.find(msg, "~y~") and 'warning' or 'success'))
    if notifyType == 'info' then type = 'system' end
    local cleanMsg = msg:gsub("~%l~", ""):gsub("~[^~]+~", "")
    TriggerEvent('nexora_notify:send', type, cleanMsg, 'Notificación')
end
```

4. Guarda el archivo y reinicia tu servidor.

> [!NOTE]
> A partir de ahora, cualquier script que use el comando clásico de ESX se verá con **Nexora Notify** de forma automática.

---

## 🛠️ Comandos de Prueba (Solo Administradores)

El script incluye tres comandos nativos para probar el diseño y la integración con ESX. Puedes configurar el grupo requerido en el archivo `config.lua`:

* `/testsuccess` - Muestra una notificación verde de éxito.
* `/testerror` - Muestra una notificación morada de error.
* `/testwarning` - Muestra una notificación amarilla de advertencia.

---

## 📐 Cómo Mover las Notificaciones

Cualquier jugador dentro del servidor puede personalizar la ubicación de sus alertas utilizando el comando configurado:

1. Abre el chat y escribe: `/movernotify` (Puedes cambiar el comando en el `config.lua`).
2. Aparecerá el **Modo Edición** con un recuadro guía y el cursor del ratón.
3. Mantén pulsado el **clic izquierdo** sobre la notificación de prueba y arrástrala a cualquier parte de tu pantalla.
4. Haz clic en **Guardar** para registrar las coordenadas para siempre, o en **Cancelar** para descartar los cambios.

---

## 💻 Documentación para Desarrolladores

Si prefieres llamar al script de forma directa o añadir títulos personalizados avanzados en tus sistemas propios, utiliza las siguientes vías de integración:

### 🔹 Desde el Cliente (Client-Side)
```lua
-- Estructura: exports['nexora_notify']:SendNotification(type, message, title)

exports['nexora_notify']:SendNotification('success', 'Has recibido tu salario de \$1,500.', 'Nómina')
exports['nexora_notify']:SendNotification('error', 'No tienes suficientes llaves de ganzúa.', 'Error')
exports['nexora_notify']:SendNotification('warning', 'Tu vehículo se está quedando sin combustible.', 'Alerta')
```

### 🔹 Desde el Servidor (Server-Side)
Para enviar la notificación desde un script del servidor a un jugador específico (`source`), utiliza el evento de red indicando la id de origen (`src`):

```lua
-- Estructura: TriggerClientEvent('nexora_notify:send', source, type, message, title)

TriggerClientEvent('nexora_notify:send', source, 'success', 'Compra realizada correctamente.', 'Tienda')
TriggerClientEvent('nexora_notify:send', source, 'error', 'No tienes suficiente dinero en el banco.', 'Banco')
```

---

## ⚙️ Configuración (`config.lua`)

```lua
Config = {}

Config.MoveCommand = "movernotify" -- Comando para activar el ratón y arrastrar
Config.DefaultTop = "150px"          -- Coordenada vertical inicial por defecto
Config.DefaultLeft = "75vw"          -- Coordenada horizontal inicial por defecto
Config.Duration = 5000               -- Duración de la alerta en pantalla (ms)
Config.EnableSound = true            -- Activar/Desactivar el sonido
Config.GroupRequired = 'admin'       -- Rango de ESX para usar los comandos /test
```

Desarrollado con 💜 por **Nexora**.
