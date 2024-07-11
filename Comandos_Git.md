# Comandos GIT
por [Emanuel da Silva](https://www.linkedin.com/in/emanuel-da-silva-5487a72a9/)

## Intro
Guia de comandos de consola en git que pueden ser de ayuda

## Lista de comandos

  1. [Ignorar Codificación](#ignorar-codificación)
  2. [SSL certificate problem](#certificate-problem)
  3. [Comando tre](#comando-tres)
  4. [Comando cuatro](#comando-cuatro)
  5. [Comando cinco](#comando-cinco)
  6. [Comando seis](#comando-seis)

## Ignorar Codificación

  <a name="Ignorar codificación"></a><a name="1.1"></a>
  - [1.1](#campo) Comando para cuando se clona un repositorio puede haber errores de codificación de los archivos. Detecta cambios en los ficheros sin haber y no deja rollback.

    ```javascript
    git config core.fileMode false

    ```

## SSL certificate problem {.tabset} 

### Linux
<a name="SSL certificate problem"></a><a name="1.1"></a>
  - [1.1](#campo) Podemos recibir el mensaje con este error al intentar hacer commit: SSL certificate problem: self-signed certificate in certificate chain error in GIT. Para solucionarlo podemos abrir una consola de git dónde está nuestro proyecto y ejecutar la siguiente línea de comando 

    ```javascript
    git config --global http.sslCAInfo /path/to/ca.pem

    ```

### Windows
<a name="SSL certificate problem"></a><a name="1.1"></a>
  - [1.1](#campo) Podemos recibir el mensaje con este error al intentar hacer commit: SSL certificate problem: self-signed certificate in certificate chain error in GIT. Para solucionarlo podemos abrir una consola de git dónde está nuestro proyecto y ejecutar la siguiente línea de comando 

    ```javascript
    git config --global http.sslBackend schannel

    ```
    
## Quarterly Results {.tabset} 

### By Product

(tab content)

### By Region

(tab content)