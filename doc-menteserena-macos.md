# Instalación del Entorno de Desarrollo en macOS

Este documento proporciona una guía paso a paso para configurar un entorno de desarrollo en macOS, incluyendo la instalación de Erlang/OTP, Elixir, configuración de claves SSH y la instalación de Aptible.

## Instalación de Erlang/OTP desde el Código Fuente

1. **Descargar el Código Fuente**:
    ```sh
    git clone https://github.com/erlang/otp.git
    cd otp
    ```

2. **Listar Versiones Disponibles**:
    ```sh
    git tag
    ```
    Selecciona la versión deseada (en este caso, 25.3) y realiza un checkout a esa versión:
    ```sh
    git checkout OTP-25.3
    ```

3. **Configurar y Compilar**:
    ```sh
    ./configure
    make
    sudo make install
    ```

4. **Verificar la Instalación**:
    ```sh
    erl -version
    ```

## Instalación de Elixir desde el Código Fuente

1. **Clonar el Repositorio de Elixir**:
    ```sh
    git clone https://github.com/elixir-lang/elixir.git
    cd elixir
    ```

2. **Listar Versiones Disponibles**:
    ```sh
    git tag
    ```
    Selecciona la versión deseada (en este caso, v1.14.4) y realiza un checkout a esa versión:
    ```sh
    git checkout v1.14.4
    ```

3. **Compilar e Instalar**:
    ```sh
    make clean test
    sudo make install
    ```

4. **Verificar la Instalación**:
    ```sh
    elixir --version
    ```

## Añadir Elixir al PATH en macOS

Si después de instalar Elixir, al abrir una nueva terminal y ejecutar el comando `elixir --version` no se reconoce como un comando, es posible que necesites añadir la ubicación de los binarios de Elixir al PATH de tu sistema. Aquí te indicamos cómo hacerlo:

1. **Encontrar la Ubicación de Elixir**:
    Abre una terminal y ejecuta el siguiente comando para encontrar la ubicación de los binarios de Elixir:
    ```sh
    which elixir
    ```

2. **Añadir Elixir al PATH**:
    - Abre tu archivo de perfil de shell con un editor de texto. Si estás usando Bash, este archivo será `~/.bash_profile` o `~/.bashrc`. Si estás usando Zsh, el archivo será `~/.zshrc`.
    ```sh
    nano ~/.zshrc # o el archivo correspondiente a tu shell
    ```
    - Añade la siguiente línea al final del archivo, reemplazando `/ruta/a/elixir/bin` con la ruta que obtuviste en el paso anterior.
    ```sh
    export PATH="$PATH:/ruta/a/elixir/bin"
    ```
    - Guarda y cierra el archivo (con `Ctrl + X`, luego `Y` para confirmar los cambios, y finalmente `Enter` para salir).

3. **Verificar la Configuración**:
    Abre una nueva ventana de la terminal y ejecuta `elixir --version` para asegurarte de que el sistema reconoce el comando.


## Configuración de Clave SSH en macOS

### Generar una Clave SSH

Si aún no tienes una clave SSH generada en tu sistema, puedes hacerlo con:

    ```sh
    ssh-keygen -t rsa -b 4096 -C "youremail@example.com"
    ```

### Proporcionar la Clave Pública al Líder del Proyecto

Una vez que hayas generado tu clave SSH, la clave pública resultante debe ser proporcionada al líder del proyecto para que pueda añadirla a Aptible y concederte acceso al repositorio. Por lo general, el archivo que contiene la clave pública tiene una extensión `.pub` (por ejemplo, `id_rsa.pub`).

Puedes mostrar el contenido del archivo `.pub` con el siguiente comando:

    ```sh
    cat ~/.ssh/id_rsa.pub
    ```

## Instalación del CLI de Aptible

1. **Descargar e Instalar el CLI de Aptible**:
    Sigue las instrucciones en la [página oficial de Aptible CLI](https://www.aptible.com/docs/cli) para descargar e instalar el CLI de Aptible en macOS.

2. **Iniciar Sesión en Aptible**:
    Una vez que hayas instalado el CLI de Aptible, deberás autenticarte:
    ```sh
    aptible login
    ```
    Este comando abrirá una nueva ventana en tu navegador web para que puedas iniciar sesión en Aptible y autorizar el CLI.

    **Nota**: Solicita las credenciales de inicio de sesión a tu líder de proyecto o administrador de sistema de forma segura.

3. **Configuración del entorno de Aptible**:
    Después de iniciar sesión, asegúrate de configurar tu entorno de Aptible según las necesidades de tu proyecto. Puedes usar el comando `aptible apps:list` para listar tus aplicaciones, y otros comandos similares para interactuar con bases de datos, dominios, etc.

## Evitar el Ingreso de la Frase de Contraseña

Para evitar tener que ingresar la frase de contraseña cada vez, puedes agregar las siguientes líneas a tu archivo `~/.ssh/config`:

    ```sh
    Host *
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_rsa
    ```

Con esto, has configurado tu entorno de desarrollo en macOS.

## Clonar los Repositorios de Aptible

Una vez que hayas configurado tu entorno de desarrollo y Aptible CLI, puedes proceder a clonar los repositorios necesarios. 

### Levantar el Agente SSH y Añadir la Clave

Antes de clonar los repositorios, asegúrate de que el agente SSH esté en ejecución y que tu clave SSH esté añadida.

1. **Levantar el Agente SSH**:
    ```sh
    eval "$(ssh-agent -s)"
    ```

2. **Añadir la Clave SSH al Agente**:
    ```sh
    ssh-add ~/.ssh/id_rsa
    ```

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
