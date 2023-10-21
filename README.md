# Pipeline Sample API
 Es un proyecto que tiene como finalidad automatizar el testeo del codigo ingresado al repositorio, utilizando el framework Lippia.

## Consideraciones
El proyecto incluye la imagen de Lippia con todas las herramientas necesarias para los tests:
- Cuando se realiza un commit el pipeline se ejecuta automaticamente.
- Se puede ejecutar el pipeline de forma manual.


## Como se usa

### CI dentro del repositorio de una app

En caso de incluir el testing en el proceso de integración/delivery de una aplicación, se debe integrar contenido del archivo de configuración del pipeline ejemplo con el bitbucket-pipelines.yml de su proyecto.

Es importante editar las siguientes lineas:

```yaml
  ...
                script: 
                  - git clone https://<BITBUCKET_USER>:<BITBUCKET_PASS>@bitbucket.org/path/to/your/<AUTOMATION_PROJECT>.git
                  - cd <AUTOMATION_PROJECT>
  ...

```

Para que el pipeline clone el repositorio donde se encuentre su proyecto de testing automatizado y ejecute dentro de la carpeta el test.

> Se recomienda configurar las credenciales en variables de entorno de la herramienta de CI para evitar filtrar contraseñas en el código del repositorio.

### CI dentro del repositorio de automation

Antes de comenzar es importante eliminar o comentar los comandos git clone y cd, ya que al encontrarnos en el repositorio donde se encuentra el proyecto de automation no necesitamos realizar el clone.

```yaml
  ...
                script: 
                  #- git clone https://<BITBUCKET_USER>:<BITBUCKET_PASS>@bitbucket.org/path/to/your/<AUTOMATION_PROJECT>.git
                  #- cd <AUTOMATION_PROJECT>
  ...

```

* Un nuevo commit en el repositorio a cualquier branch dispara el pipeline automaticamente, iniciando las pruebas pertinentes. Si se desea cambiar esto se puede modificar en la siguiente sección del documento:

```yaml

branches:    #En lugar de 'default' escribimos 'branches'
    staging: #El pipeline solo se ejecuta cuando se hace un commit al branch 'staging'
      - step:
          script:
            - echo "Hello"
```

**Para más información ver [documentación bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/configure-bitbucket-pipelinesyml/ "documentación bitbucket.")**

* Este pipeline trabaja con la versión de lippia 3.1.2.2, en caso de querer modificarla utilizar una imagen desde el siguiente link

> https://hub.docker.com/r/crowdar/lippia/tags


- En el caso del pipeline manual estas se configuran en la siguiente sección, o al disparar el pipeline:

``` yaml
  custom:
    pipeline-manual: 
      - variables:          
          - name: TAG
            default: "@Smoke"
          - name: TESTTYPE
            default: "parallel"          
          - name: LANG
            default: "@EN"
```

- Los valores de dichas variables se encuentran en el archivo POM.xml

  * **TAG**: lleva el nombre de la prueba
  * **TESTTYPE**:  determina el tipo de pruebas a realizar
  * **LANG**: determina el idioma
  
**NOTA:  el pipeline permite modificar o agregar mas variables de entorno dentro del apartado "variables", o "Repository Variables" segun corresponda**

* para realizar las pruebas utilizamos el comando: 

```bash
$ mvn clean test #Add your -P or -D configuration here
```

* En caso de agregar o modificar variables de entorno realizar los cambios necesarios en el script del test en los archivos YAML

> Los reportes son almacenados en un zip que se encuentra en la opcion DOWNLOADS en la barra lateral de las opciones del repositorio, se eliminan automaticamente luego de 14 dias, y seran generados una vez que la ejecucion de las pruebas haya finalizado.

**Para más informacion ver la [documentación lippia.](https://github.com/Crowdar/lippia-web-sample-project#getting-started "documentación lippia.")**
