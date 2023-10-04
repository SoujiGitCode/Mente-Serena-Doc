Instalación de entorno de desarrollo en Windows
Prerrequisitos
Antes de comenzar, asegúrese de tener instalado Chocolatey, un administrador de paquetes para Windows. Si aún no lo tiene, siga las instrucciones en su página oficial de instalación.

Instalación de Erlang/OTP y Elixir
Abra un símbolo del sistema (cmd) como administrador y ejecute los siguientes comandos para instalar Erlang/OTP y Elixir.

# Instalar Erlang/OTP 25
choco install erlang --version=25.3

# Instalar Elixir 1.14.4
choco install elixir --version=1.14.4

Una vez que las instalaciones se hayan completado, reinicie su terminal y ejecute erl -version y elixir -v para confirmar que las instalaciones fueron exitosas.


## Añadir Elixir al PATH en Windows

Si después de instalar Elixir, al abrir un nuevo terminal y ejecutar el comando `elixir --version` no se reconoce como un comando, es posible que necesites añadir la ubicación de los binarios de Elixir al PATH de tu sistema. Aquí te indicamos cómo hacerlo:

1. **Encontrar la Ubicación de Elixir**:
    Encuentra la carpeta donde se ha instalado Elixir. Por lo general, esta es `C:\Program Files (x86)\Elixir\bin` o `C:\Program Files\Elixir\bin`, dependiendo de tu sistema.

2. **Añadir Elixir al PATH**:
    - Haz clic derecho en el botón de Inicio y selecciona `Sistema`.
    - Haz clic en `Configuración avanzada del sistema`.
    - Haz clic en `Variables de entorno...`.
    - En las `Variables del sistema`, desplázate hacia abajo y selecciona la variable `Path`, luego haz clic en `Editar...`.
    - Haz clic en `Nuevo` y añade la ruta del directorio `bin` de Elixir.
    - Haz clic en `Aceptar` para cerrar cada una de las ventanas.

3. **Verificar la Configuración**:
    Abre una nueva ventana de la terminal y ejecuta `elixir --version` para asegurarte de que el sistema reconoce el comando.


## Configuración de clave SSH en WSL

### Instalación de WSL y Ubuntu

Si aún no tienes WSL y Ubuntu instalado en tu sistema, puedes seguir estos pasos:

1. **Instale WSL**:
    ```bash
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```
  
2. **Instale Ubuntu** desde la Microsoft Store o usando `choco`:
    ```bash
    choco install wsl-ubuntu-2004
    ```

### Generar una clave SSH

Si aún no tienes una clave SSH generada en WSL, puedes hacerlo con:

```bash
ssh-keygen -t rsa -b 4096 -C "youremail@example.com"
 ```
### Proporcionar la Clave Pública al Líder del Proyecto

Una vez que haya generado su clave SSH, la clave pública resultante debe ser proporcionada al líder del proyecto para que pueda añadirla a Aptible y concederle acceso al repositorio. Por lo general, el archivo que contiene la clave pública tiene una extensión `.pub` (por ejemplo, `id_ed25519.pub`).

Puede mostrar el contenido del archivo `.pub` con el siguiente comando:

```bash
cat ~/.ssh/id_ed25519.pub
 ```
### Generación de Clave SSH y Configuración en Windows

### Generación de Clave SSH

1. **Abrir Git Bash**: Busca "Git Bash" en el menú inicio y ábrelo.

2. **Generar la Clave SSH**: Ejecute el siguiente comando para generar una nueva clave SSH. Sustituya su dirección de correo electrónico.

    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
    ```

3. **Especificar la Ruta del Archivo**: Cuando se le pida que "Ingrese un archivo en el que guardar la clave", presione Enter para aceptar la ubicación predeterminada.

    ```bash
    Enter a file in which to save the key (/c/Users/you/.ssh/id_ed25519): [Press enter]
    ```

4. **Configurar una Contraseña Segura**: Cuando se le pida, escriba una contraseña segura para la clave SSH.

### Agregar la Clave SSH al Agente SSH

1. **Iniciar el Agente SSH en segundo plano**:

    ```bash
    eval $(ssh-agent -s)
    ```

2. **Agregar la Clave SSH al Agente SSH**:

    ```bash
    ssh-add ~/.ssh/tu-clave-ssh-generada
    ```

### Proporcionar la Clave Pública al Líder del Proyecto

Una vez que haya generado su clave SSH, la clave pública resultante debe ser proporcionada al líder del proyecto para que pueda añadirla a Aptible y concederle acceso al repositorio. Por lo general, el archivo que contiene la clave pública tiene una extensión `.pub` (por ejemplo, `id_ed25519.pub`).

Puede mostrar el contenido del archivo `.pub` con el siguiente comando:

```bash
cat ~/ruta-de/clave-ssh-generada.pub
```

> Nota: Este paso es específico del repositorio y debe realizarse en cada repositorio que requiera esta clave SSH.

### Instalación del CLI de Aptible

1. **Instalar Aptible CLI a través de Chocolatey**:
    ```bash
    choco install aptible
    ```
    Sigue las instrucciones en pantalla para completar la instalación.

2. **Login en Aptible**:
    Una vez que hayas instalado el CLI de Aptible, deberás autenticarte:
    ```bash
    aptible login
    ```
    Este comando abrirá una nueva ventana en tu navegador web para que puedas iniciar sesión en Aptible y autorizar el CLI.

    **Nota**: Solicita las credenciales de inicio de sesión a tu líder de proyecto o administrador de sistema de forma segura.

3. **Configuración del entorno de Aptible**:
    Después de iniciar sesión, asegúrate de configurar tu entorno de Aptible según las necesidades de tu proyecto. Puedes usar el comando `aptible apps:list` para listar tus aplicaciones, y otros comandos similares para interactuar con bases de datos, dominios, etc.


 ### Clonar los Repositorios

Ahora, puedes clonar los repositorios de Aptible:

1. **Clonar el Repositorio API**:
    ```sh
    git clone git@beta.aptible.com:ms-dev/api-dev.git
    ```

2. **Clonar el Repositorio FE**:
    ```sh
    git clone git@beta.aptible.com:ms-dev/fe-dev.git
    ```

Después de clonar los repositorios, podrás acceder a los directorios `api-dev` y `fe-dev` y comenzar a trabajar en los proyectos.


## Configuración del .gitignore en api-dev

Para el proyecto `api-dev`, es necesario que ciertos archivos de configuración no sean rastreados por Git. Esto se puede lograr añadiendo las rutas de los archivos al archivo `.gitignore` del proyecto.

1. **Navegar al Directorio del Proyecto**:
    ```sh
    cd api-dev
    ```

2. **Añadir las Rutas de los Archivos al .gitignore**:
    ```sh
    echo "config/dev.exs" >> .gitignore
    echo "config/config.exs" >> .gitignore
    ```

3. **Verificar los Cambios en .gitignore**:
    Puedes verificar que las entradas se han añadido correctamente abriendo el archivo `.gitignore` con un editor de texto, o usando el comando `cat`:
    ```sh
    cat .gitignore
    ```

Con estos pasos, los archivos `config/dev.exs` y `config/config.exs` serán ignorados por Git y no se rastrearán los cambios en estos archivos.


## Configuración del SSH Agent shortcut

Para evitar tener que levantar el agente SSH cada vez, se puede configurar un comando de Git que especifica la clave SSH a utilizar para ese repositorio en particular:

Hacerlo den dev, qa y production en API y FE

```sh

git config core.sshCommand "ssh -i /c/Users/souji/nombre-de-tu-clave-ssh -F /dev/null"
