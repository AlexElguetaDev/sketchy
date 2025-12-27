# Configuración de Sketchybar

[![GitHub](https://img.shields.io/badge/GitHub-AlexElguetaDev%2Fsketchy-blue)](https://github.com/AlexElguetaDev/sketchy)

Este repositorio contiene una configuración personalizada de [Sketchybar](https://github.com/FelixKratz/SketchyBar), una barra de estado minimalista y altamente personalizable para macOS.

**Repositorio**: [https://github.com/AlexElguetaDev/sketchy](https://github.com/AlexElguetaDev/sketchy)

## 📋 Descripción

Sketchybar es una alternativa moderna a la barra de menú nativa de macOS que permite crear barras de estado completamente personalizadas. Esta configuración incluye:

- **Detección automática de entorno**: Cambia automáticamente entre configuración de laptop y desktop según el monitor principal
- **Plugins personalizados**: Múltiples widgets funcionales para monitorear el sistema
- **Integración con Spotify**: Control y visualización del estado de reproducción
- **Información del sistema**: Espacios de trabajo, aplicación frontal, clima, batería, volumen y más

## 🚀 Instalación

### Prerrequisitos

1. **Sketchybar**: Debes tener Sketchybar instalado. Si no lo tienes:
   ```bash
   brew install sketchybar
   ```

2. **Fuente Nerd Font**: Esta configuración utiliza `JetBrainsMono Nerd Font`. Instálala con:
   ```bash
   brew install --cask font-jetbrains-mono-nerd-font
   ```

3. **Dependencias adicionales**:
   - `jq` (para procesar JSON en el plugin de clima):
     ```bash
     brew install jq
     ```
   - `curl` (generalmente ya viene instalado en macOS)

### Configuración

1. Clona este repositorio a tu directorio de configuración:
   ```bash
   git clone https://github.com/AlexElguetaDev/sketchy ~/.config/sketchybar
   ```
   
   O si ya tienes los archivos, asegúrate de que estén en `~/.config/sketchybar`

2. Haz los scripts ejecutables:
   ```bash
   chmod +x ~/.config/sketchybar/sketchybarrc
   chmod +x ~/.config/sketchybar/sketchybarrc-laptop
   chmod +x ~/.config/sketchybar/sketchybarrc-desktop
   chmod +x ~/.config/sketchybar/plugins/*.sh
   chmod +x ~/.config/sketchybar/plugins-laptop/*.sh
   chmod +x ~/.config/sketchybar/plugins-desktop/*.sh
   ```

3. Inicia Sketchybar con la configuración:
   ```bash
   sketchybar --config ~/.config/sketchybar/sketchybarrc
   ```

4. Para que Sketchybar se inicie automáticamente al iniciar sesión, puedes usar `brew services`:
   ```bash
   brew services start sketchybar
   ```

## 📁 Estructura del Proyecto

```
~/.config/sketchybar/
├── sketchybarrc              # Archivo principal que detecta el entorno
├── sketchybarrc-laptop       # Configuración para MacBook (con notch)
├── sketchybarrc-desktop      # Configuración para monitor externo
├── plugins/                  # Plugins compartidos entre ambas configuraciones
│   ├── clock.sh              # Reloj con fecha y hora
│   ├── current_space.sh      # Espacio de trabajo actual (Mission Control)
│   ├── front_app.sh          # Aplicación frontal activa
│   ├── volume.sh             # Control de volumen
│   └── weather.sh            # Clima y fase lunar
├── plugins-laptop/           # Plugins específicos para laptop
│   ├── battery.sh            # Estado de la batería
│   └── spotify.sh            # Integración con Spotify
└── plugins-desktop/          # Plugins específicos para desktop
    └── spotify.sh            # Integración con Spotify
```

## 🎨 Características

### Detección Automática de Entorno

El archivo `sketchybarrc` detecta automáticamente si estás usando el monitor interno (laptop) o un monitor externo (desktop) y carga la configuración correspondiente:

- **Laptop**: Incluye batería y está optimizado para el notch del MacBook
- **Desktop**: Configuración más limpia sin batería

### Plugins Incluidos

#### Plugins Compartidos (`plugins/`)

- **Clock** (`clock.sh`): Muestra la fecha y hora actual
- **Current Space** (`current_space.sh`): Muestra el espacio de Mission Control actual
- **Front App** (`front_app.sh`): Muestra el icono y nombre de la aplicación activa
- **Volume** (`volume.sh`): Control y visualización del volumen del sistema
- **Weather** (`weather.sh`): Muestra el clima actual, temperatura y fase lunar basado en tu ubicación IP

#### Plugins Laptop (`plugins-laptop/`)

- **Battery** (`battery.sh`): Estado de la batería con iconos que cambian según el nivel y estado de carga
- **Spotify** (`spotify.sh`): Control de Spotify con información de reproducción

#### Plugins Desktop (`plugins-desktop/`)

- **Spotify** (`spotify.sh`): Control de Spotify con información de reproducción

## ⚙️ Personalización

### Personalizar Colores y Fuentes

Edita los archivos `sketchybarrc-laptop` o `sketchybarrc-desktop` para cambiar:

- **Fuente**: Modifica la variable `FONT_FACE` (actualmente `JetBrainsMono Nerd Font`)
- **Colores**: Ajusta los valores `background.color`, `icon.color`, `label.color` (formato hexadecimal)
- **Tamaños**: Modifica `height`, `padding_left`, `padding_right`, etc.

### Agregar o Modificar Plugins

1. Crea un nuevo script en el directorio correspondiente (`plugins/`, `plugins-laptop/` o `plugins-desktop/`)
2. Hazlo ejecutable: `chmod +x nombre_plugin.sh`
3. Agrega el item en el archivo de configuración correspondiente usando `sketchybar --add item`
4. Recarga la configuración: `sketchybar --reload`

### Ejemplo de Personalización

Para cambiar el color de fondo de la barra, edita la línea en `sketchybarrc-laptop` o `sketchybarrc-desktop`:

```zsh
sketchybar --bar \
    height=32 \
    color=0x00000000 \  # Cambia este valor (formato: 0xAARRGGBB)
    ...
```

## 🔄 Recargar la Configuración

Después de hacer cambios, recarga Sketchybar:

```bash
sketchybar --reload
```

O reinicia el servicio:

```bash
brew services restart sketchybar
```

## 🐛 Solución de Problemas

### La barra no aparece

1. Verifica que Sketchybar esté corriendo:
   ```bash
   ps aux | grep sketchybar
   ```

2. Revisa los logs:
   ```bash
   tail -f ~/.sketchybar.log
   ```

3. Asegúrate de que todos los scripts sean ejecutables (ver sección de Instalación)

### Los iconos no se muestran correctamente

- Verifica que `JetBrainsMono Nerd Font` esté instalada correctamente
- Reinicia Sketchybar después de instalar la fuente

### El plugin de clima no funciona

- Verifica tu conexión a internet
- Asegúrate de que `jq` esté instalado: `brew install jq`
- El plugin usa `ipinfo.io` y `wttr.in` - verifica que estos servicios estén accesibles

### Spotify no se actualiza

- Asegúrate de que Spotify esté abierto
- Verifica que el evento de Spotify esté configurado correctamente en la configuración

## 📝 Notas

- Esta configuración está optimizada para macOS con Mission Control habilitado
- Algunos plugins requieren permisos de accesibilidad (como el control de volumen)
- El plugin de clima detecta automáticamente tu ubicación basándose en tu IP pública

## 📄 Licencia

Este es un proyecto personal de configuración. Siéntete libre de usarlo y modificarlo según tus necesidades.

## 🙏 Créditos

- [Sketchybar](https://github.com/FelixKratz/SketchyBar) - El proyecto principal
- [Nerd Fonts](https://www.nerdfonts.com/) - Fuentes con iconos
- [wttr.in](https://wttr.in/) - API de clima
- [ipinfo.io](https://ipinfo.io/) - Servicio de geolocalización por IP
