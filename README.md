# Configuración Completa del Stack Tecnológico Archon

Este documento te guiará paso a paso para configurar un entorno de desarrollo completo con Archon, incluyendo Docker, Visual Studio Code, Supabase y la integración con Cline MCP.

## 📋 Requisitos Previos

- Sistema operativo: Windows, macOS o Linux
- Conexión a internet estable
- Cuenta de GitHub (para clonar el repositorio)

## 🚀 Pasos de Instalación

### 1. Instalación y Configuración de Docker

#### Windows:
1. Descarga Docker Desktop desde [docker.com](https://www.docker.com/products/docker-desktop)
2. Ejecuta el instalador y sigue las instrucciones
3. Reinicia tu computadora si es necesario
4. Abre Docker Desktop y espera a que se inicie completamente
5. Verifica la instalación ejecutando en terminal:
   ```bash
   docker --version
   docker-compose --version
   ```

#### macOS:
1. Descarga Docker Desktop para Mac desde [docker.com](https://www.docker.com/products/docker-desktop)
2. Arrastra Docker a la carpeta Applications
3. Abre Docker desde Applications
4. Verifica la instalación:
   ```bash
   docker --version
   docker-compose --version
   ```

#### Linux (Ubuntu/Debian):
```bash
# Actualizar paquetes
sudo apt update

# Instalar Docker
sudo apt install docker.io docker-compose

# Agregar usuario al grupo docker
sudo usermod -aG docker $USER

# Reiniciar sesión o ejecutar
newgrp docker

# Iniciar Docker
sudo systemctl start docker
sudo systemctl enable docker

# Verificar instalación
docker --version
docker-compose --version
```

### 2. Instalación de Visual Studio Code

#### Opción 1: Descarga Directa
1. Ve a [code.visualstudio.com](https://code.visualstudio.com/)
2. Descarga la versión para tu sistema operativo
3. Ejecuta el instalador y sigue las instrucciones

#### Opción 2: Línea de Comandos

**Windows (usando Chocolatey):**
```powershell
choco install vscode
```

**macOS (usando Homebrew):**
```bash
brew install --cask visual-studio-code
```

**Linux (Ubuntu/Debian):**
```bash
sudo snap install code --classic
```

### 3. Preparación del Proyecto

#### Crear directorio y clonar repositorio:
```bash
# Crear carpeta para el proyecto
mkdir ~/archon-project
cd ~/archon-project

# Clonar el repositorio de Archon
git clone https://github.com/coleam00/archon.git

# Navegar al directorio del proyecto
cd archon
```

### 4. Configuración de Supabase

#### Crear proyecto en Supabase:
1. Ve a [supabase.com](https://supabase.com/)
2. Haz clic en "Start your project"
3. Inicia sesión o crea una cuenta
4. Crea un nuevo proyecto:
   - Selecciona tu organización
   - Ingresa el nombre del proyecto
   - Genera una contraseña segura para la base de datos
   - Selecciona la región más cercana
   - Selecciona el plan **Free**
5. Espera a que se complete la configuración del proyecto

### 5. Configuración de la Base de Datos

#### Ejecutar script de configuración:
1. En tu proyecto de Supabase, ve a la sección **SQL Editor**
2. Abre el archivo `Archon/migration/complete_setup.sql` desde tu proyecto local
3. Copia todo el contenido del archivo
4. Pega el contenido en el SQL Editor de Supabase
5. Haz clic en **Run** para ejecutar la migración
6. Verifica que todas las consultas se ejecuten sin errores

### 6. Obtener Credenciales de Supabase

#### Localizar credenciales:
1. En tu proyecto de Supabase, ve a **Settings** → **API**
2. Copia los siguientes valores:
   - **Project URL** (URL del proyecto)
   - **Service Key** (service_role key - ⚠️ mantén esta clave secreta)

### 7. Configuración del Archivo .env

#### Configurar variables de entorno:
1. En el directorio del proyecto Archon, localiza el archivo `.env` o `.env.example`
2. Si solo existe `.env.example`, cópialo como `.env`:
   ```bash
   cp .env.example .env
   ```
3. Abre el archivo `.env` y actualiza las siguientes variables:
   ```env
   SUPABASE_URL=tu_project_url_aqui
   SUPABASE_SERVICE_KEY=tu_service_key_aqui
   ```
4. Guarda el archivo

### 8. Ejecutar Docker Compose

#### Iniciar los servicios:
```bash
# Asegúrate de estar en el directorio del proyecto
cd ~/archon-project/archon

# Construir y ejecutar los contenedores
docker-compose up -d

# Verificar que los contenedores estén ejecutándose
docker-compose ps

# Ver logs (opcional)
docker-compose logs -f
```

#### Comandos útiles de Docker Compose:
```bash
# Detener servicios
docker-compose down

# Reiniciar servicios
docker-compose restart

# Ver logs de un servicio específico
docker-compose logs [nombre-servicio]

# Reconstruir contenedores
docker-compose up -d --build
```

### 9. Instalación de la Extensión Cline en VS Code

#### Instalar Cline:
1. Abre Visual Studio Code
2. Ve a la sección de **Extensions** (Ctrl/Cmd + Shift + X)
3. Busca "Cline"
4. Haz clic en **Install** en la extensión oficial de Cline
5. Reinicia VS Code si es necesario

#### Verificar instalación:
- La extensión Cline debería aparecer en tu barra lateral
- Busca el ícono de Cline en la barra de actividades

### 10. Configuración de MCP en Cline

#### Configurar la conexión MCP:
1. Abre Cline en VS Code
2. Ve a la configuración de Cline
3. Configura los siguientes parámetros de MCP:
   ```json
   {
     "mcp": {
       "servers": {
         "archon": {
           "command": "node",
           "args": ["path/to/archon/mcp-server.js"],
           "env": {
             "SUPABASE_URL": "tu_project_url_aqui",
             "SUPABASE_SERVICE_KEY": "tu_service_key_aqui"
           }
         }
       }
     }
   }
   ```
4. Guarda la configuración
5. Reinicia Cline para aplicar los cambios

## ✅ Verificación de la Instalación

### Comprobar que todo funciona:

1. **Docker:**
   ```bash
   docker ps
   # Deberías ver los contenedores de Archon ejecutándose
   ```

2. **Supabase:**
   - Accede a tu proyecto en Supabase
   - Verifica que las tablas se crearon correctamente

3. **Archon:**
   ```bash
   curl http://localhost:puerto_configurado/health
   # Deberías recibir una respuesta exitosa
   ```

4. **Cline + MCP:**
   - Abre Cline en VS Code
   - Verifica que pueda conectarse a Archon
   - Prueba ejecutar un comando básico

## 🔧 Solución de Problemas Comunes

### Docker no inicia:
- Verifica que Docker Desktop esté ejecutándose
- Reinicia Docker Desktop
- Verifica los puertos disponibles

### Error de conexión a Supabase:
- Verifica las credenciales en el archivo `.env`
- Asegúrate de que el proyecto de Supabase esté activo
- Revisa las reglas de RLS (Row Level Security)

### Cline no se conecta a MCP:
- Verifica la configuración de MCP en Cline
- Revisa los logs de Archon
- Asegúrate de que el puerto esté disponible

### Problemas de permisos:
```bash
# Linux/macOS
sudo chown -R $USER:$USER ~/archon-project
chmod -R 755 ~/archon-project
```

## 📚 Recursos Adicionales

- [Documentación de Docker](https://docs.docker.com/)
- [Documentación de Supabase](https://supabase.com/docs)
- [Documentación de Cline](https://github.com/cline/cline)
- [Repositorio de Archon](https://github.com/coleam00/archon)

## 🤝 Contribución

Si encuentras algún problema con esta configuración o tienes sugerencias de mejora, por favor:
1. Abre un issue en el repositorio
2. Propón una mejora via Pull Request
3. Actualiza esta documentación según sea necesario

## 📄 Licencia

Este proyecto sigue la licencia del repositorio original de Archon.

---

**¡Felicitaciones!** 🎉 Has configurado exitosamente tu stack tecnológico completo con Archon, Docker, Supabase y Cline MCP.