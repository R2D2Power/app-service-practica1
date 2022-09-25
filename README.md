# app-service-practica1
Práctica de laboratorio para subir un app service a Azure.

## Salida esperada para index.php
![Imagen de de la salida esperada](https://raw.githubusercontent.com/R2D2Power/app-service-practica1/main/assets/tabla-ejemplo.png)

## Pasos Para realizar la práctica
## Creando el repositorio
1. Copiar y pegar el archivo `index.php` y `test.php` dentro de una carpeta para el poyecto. En este caso, por ejemplo, la carpeta se puede llamar app-service-practica1. 
2. Crear el repositorio
```Github
git init
```
3. Probar la aplicación localmente. En mi caso utilicé `Apache` pero tu eres libre de utilizar el servicio que te guste más.

## Creando el App Service
1. Lo primero es crear el grupo de recursos para la aplicación. 

```CLI de Azure
az group create -l <Locacion_de_la_region> -n <nombre_del_grupo>
```

2. Creamos el plan del App Service para este ejercicio

```CLI de Azure
az appservice plan create --name <nombre_del_plan> --resource-group <nombre_del_grupo> -sku F1
```
- El término `-sku` se refiere al tamaño (CPU, RAM) y costo del equipo del plan para este App Service; donde `F1` quiere decir que estamos usando un plan gratis. 

3. Creamos el App Service. 

```CLI de Azure
az webapp create --name <nombre_de_tu_app_service> --runtime "PHP|7.4" --plan <nombre_del_plan> --resource-group <nombre_del_grupo>
```

## Haciendo el Deploy
1. Una vez creado el App Service necesitamos configurar el deployment source para al aplicación

```CLI de Azure
az webapp deployment source config-local-git --name <nombre_del-app_service> --resource-group <nombre_del_grupo>
```

2. Recuperar las crendenciales de publicación. Esto es importante para autenticarnos en Azure cuando hacemos un push. 

```CLI de Azure
az webapp deployment list-publishing-credentials \
        --name <nombre_del-app_service> \
        --resource-group <nombre_del_grupo> \
        --query "{Username:Nombre_de_Usuario, Password:contraseña}"
```

3. Después obtenemos la dirección URL para asignarla como nuestro origen remoto. 

```CLI de Azure
az webapp deployment source config-local-git -n <nombre_del_app_service> -g <nombre_del_grupo>
```
- Copiamos y guardamos la dirección URL que nos arroja este comando. 

4. Después de esto debemos agregar el origen remoto a nuestro repositorio **Git**.

```Git 
git remote add azure <La_URL_va_aquí>
```

5. Por último podemos hacer un **push** al origen que acabamos de asignar. 

```Git
git push azure main:master
```

  > 👀 **Ojo:** Es importante aclarar que Azure asigna una rama master por defecto. 

[Ejemplo de la palicación corriendo en un App Service de Azure](https://app-service-practica1.azurewebsites.net/)
