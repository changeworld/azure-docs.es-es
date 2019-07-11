---
title: 'Tutorial: Implementación de LEMP en una máquina virtual Linux en Azure | Microsoft Docs'
description: En este tutorial, aprenderá a instalar la pila LEMP en una máquina virtual Linux en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: cynthn
ms.openlocfilehash: 15d88e082f9ab0838f4a560d89801edd9d46d682
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703538"
---
# <a name="tutorial-install-a-lemp-web-server-on-a-linux-virtual-machine-in-azure"></a>Tutorial: Instalación de un servidor web LEMP en una máquina virtual Linux en Azure

En este artículo se ofrecen instrucciones paso a paso para implementar un servidor web NGINX, MySQL y PHP (la pila LEMP) en una máquina virtual de Ubuntu en Azure. La pila LEMP es una alternativa a la popular [pila LAMP](tutorial-lamp-stack.md), que también se puede instalar en Azure. Para ver el servidor LEMP en acción, si lo desea, puede instalar y configurar un sitio de WordPress. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una máquina virtual de Ubuntu (la "L" de la pila LEMP)
> * Apertura del puerto 80 para el tráfico web
> * Instalación de NGINX, MySQL y PHP
> * Comprobación de la instalación y configuración
> * Instalación de WordPress en el servidor LEMP

Esta configuración es para pruebas rápidas o prueba de concepto.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, en este tutorial es preciso que ejecute la CLI de Azure de la versión 2.0.30, u otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Instalación de NGINX, MySQL y PHP

Ejecute el siguiente comando para actualizar los orígenes de paquetes de Ubuntu e instalar NGINX, MySQL y PHP. 

```bash
sudo apt update && sudo apt install nginx && sudo apt install mysql-server php-mysql php-fpm
```

Se le pide que instale los paquetes y otras dependencias. Este proceso instala las extensiones PHP mínimas necesarias para utilizar PHP con MySQL.  

## <a name="verify-installation-and-configuration"></a>Comprobación de la instalación y configuración


### <a name="verify-nginx"></a>Comprobación de NGINX

Compruebe la versión de NGINX con el comando siguiente:
```bash
nginx -v
```

Con NGINX instalado y el puerto 80 abierto para la máquina virtual, se puede acceder ahora al servidor web desde Internet. Para ver la página de bienvenida de NGINX, abra un explorador web y escriba la dirección IP pública de la máquina virtual. Use la dirección IP pública utilizada para SSH en la máquina virtual:

![Página predeterminada de NGINX][3]


### <a name="verify-and-secure-mysql"></a>Comprobación y protección de MySQL

Compruebe la versión de MySQL con el siguiente comando (tenga en cuenta el parámetro `V` en mayúsculas):

```bash
mysql -V
```

Para ayudar a proteger la instalación de MySQL, incluido el establecimiento de una contraseña raíz, ejecute el script `mysql_secure_installation`. 

```bash
sudo mysql_secure_installation
```

Si lo desea, puede configurar el complemento para validar contraseña (se recomienda). A continuación, establezca una contraseña para el usuario raíz de MySQL y configure las opciones de seguridad restantes para su entorno. Se recomienda que responda "Y" (Yes o Sí) para todas las preguntas.

Si desea crear probar características de MySQL (crear una base de datos MySQL, agregar usuarios o cambiar la configuración), inicie sesión en MySQL. Este paso no es necesario para completar este tutorial. 


```bash
sudo mysql -u root -p
```

Cuando haya terminado, escriba `\q` para salir del símbolo del sistema de mysql.

### <a name="verify-php"></a>Comprobación de PHP

Compruebe la versión de PHP con el comando siguiente:

```bash
php -v
```

Configure NGINX para usar el Administrador de procesos FastCGI para PHP (PHP-FPM). Ejecute los siguientes comandos para realizar la copia de seguridad del archivo de configuración del bloque de servidor NGINX original y, a continuación, edite el archivo original en el editor que prefiera:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

En el editor, reemplace el contenido de `/etc/nginx/sites-available/default` por lo siguiente. Vea los comentarios para obtener una explicación de la configuración. Sustituya la dirección IP pública de la máquina virtual para *yourPublicIPAddress*, confirme la versión de PHP en `fastcgi_pass` y deje el resto de opciones de configuración. A continuación, guarde el archivo.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.2-fpm.sock;
    }
}
```

Compruebe la configuración de NGINX para ver si hay errores de sintaxis:

```bash
sudo nginx -t
```

Si la sintaxis es correcta, reinicie NGINX con el siguiente comando:

```bash
sudo service nginx restart
```

Si desea realizar más pruebas, cree una página PHP de información rápida para verla en un explorador. El siguiente comando crea la página de información de PHP:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Ahora puede comprobar la página de información de PHP que creó. Abra un explorador y vaya a `http://yourPublicIPAddress/info.php`. Sustituya la dirección por la dirección IP pública de la máquina virtual. Debe tener un aspecto similar a esta imagen.

![Página de información de PHP][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, implementó un servidor LEMP en Azure. Ha aprendido a:

> [!div class="checklist"]
> * Creación de una máquina virtual de Ubuntu
> * Apertura del puerto 80 para el tráfico web
> * Instalación de NGINX, MySQL y PHP
> * Comprobación de la instalación y configuración
> * Instalación de WordPress en la pila LEMP

Pase al siguiente tutorial para aprender a proteger servidores web con certificados SSL.

> [!div class="nextstepaction"]
> [Protección de un servidor web con SSL](tutorial-secure-web-server.md)

[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
