# Pipeline Sample API
 Es un proyecto que tiene como finalidad automatizar el testeo del codigo ingresado al repositorio, utilizando el framework Lippia.

## Consideraciones
El proyecto incluye la imagen de Lippia con todas las herramientas necesarias para los tests:
- Cuando se realiza un commit el pipeline se ejecuta automaticamente.
- Se puede ejecutar el pipeline de forma manual.

## Como se usa

* Un nuevo commit en el repositorio a cualquier branch dispara el pipeline automaticamente, iniciando las pruebas pertinentes. Si se desea cambiar esto se puede modificar en la siguiente secci�n del documento:

```

branches:    #En lugar de 'default' escribimos 'branches'
    staging: #El pipeline solo se ejecuta cuando se hace un commit al branch 'staging'
      - step:
          script:
            - echo "Hello"
```
**para mas informacion ver [documentacion bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/configure-bitbucket-pipelinesyml/ "documentacion bitbucket.")**

* Este pipeline trabaja con la version de lippia 3.1.2.2, en caso de querer modificarla utilizar una imagen desde el siguiente link

>https://hub.docker.com/r/crowdar/lippia/tags


- Para que el pipeline automatico funcione primero deben configurarse las siguientes variables de entorno en Repository settings --> Repository variables: 
![Env-var](docs/images/Env-var.png)
- En el caso del pipeline manual estas se configuran en la siguiente seccion, o al disparar el pipeline:
```
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
![Run-man](docs/images/Run-Man.png)
- Los valores de dichas variables se encuentran en el archivo POM.xml

  * **TAG**: lleva el nombre de la prueba
  * **TESTTYPE**:  determina el tipo de pruebas a realizar
  * **LANG**: determina el idioma
  
**NOTA:  el pipeline permite modificar o agregar mas variables de entorno dentro del apartado "variables", o "Repository Variables" segun corresponda**

* para realizar las pruebas utilizamos el comando: 
```
$ mvn clean test
```
* En caso de agregar o modificar variables de entorno realizar los cambios necesarios en el script del test en los archivos YAML

### Los reportes son almacenados en un zip que se encuentra en la opcion DOWNLOADS en la barra lateral de las opciones del repositorio, se eliminan automaticamente luego de 14 dias, y seran generados una vez que la ejecucion de las pruebas haya finalizado.

**para mas informacion ver [documentacion lippia.](https://github.com/Crowdar/lippia-web-sample-project#getting-started "documentacion lippia.")**
