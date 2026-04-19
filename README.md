# Lab 04 - Implementación de Redes Virtuales en Azure

![Azure](https://img.shields.io/badge/Azure-Cloud-blue?logo=microsoftazure&logoColor=white)
![Nivel](https://img.shields.io/badge/Nivel-Intermedio-yellow)
![Duración](https://img.shields.io/badge/Duración-50%20min-green)
![Laboratorio](https://img.shields.io/badge/Lab-04-orange)
![Networking](https://img.shields.io/badge/Focus-Networking-lightgrey)
![ARM](https://img.shields.io/badge/Deployment-ARM%20Template-blueviolet)
![CLI](https://img.shields.io/badge/Tools-Azure%20CLI-red)
![PowerShell](https://img.shields.io/badge/Tools-PowerShell-0078D7)
![DNS](https://img.shields.io/badge/Feature-DNS%20Zones-brightgreen)
![NSG](https://img.shields.io/badge/Security-NSG%20%26%20ASG-critical)

![Bannerlab](./images/FondoREADME.png)

## 📑 Índice

1. [Descripción](#-descripción)  
2. [Escenario del laboratorio](#-escenario-del-laboratorio)  
3. [Esquema Visual del Laboratorio](#esquema-visual-del-laboratorio)  
4. [Habilidades adquiridas](#-habilidades-adquiridas)  
5. [Resultados esperados](#-resultados-esperados)  
6. [Costo Total del Laboratorio](#-costo-total-del-laboratorio)  
7. [Desarrollo del laboratorio](#-desarrollo-del-laboratorio)  
   - [Tarea 1: Crear una red virtual con subredes usando el portal](#tarea1-crear-una-red-virtual-con-subredes-usando-el-portal)  
   - [Tarea 2: Crear una red virtual y subredes usando una plantilla](#-tarea2-crear-una-red-virtual-y-subredes-usando-una-plantilla)  
   - [Tarea 3: Configurar comunicación entre un ASG y un NSG](#-tarea3-configurar-comunicación-entre-un-asg-y-un-nsg)  
     - [Crear el Application Security Group (ASG)](#crear-el-application-security-group-asg)  
     - [Crear el Network Security Group y asociarlo a CoreServicesVnet](#crear-el-network-security-group-y-asociarlo-a-coreservicesvnet)  
     - [Configurar una regla inbound para permitir tráfico desde el ASG](#configurar-una-regla-inbound-para-permitir-tráfico-desde-el-asg)  
     - [Configurar una regla outbound que deniegue acceso a Internet](#configurar-una-regla-outbound-que-deniegue-acceso-a-internet)  
   - [Tarea 4: Configurar zonas DNS públicas y privadas](#-tarea4-configurar-zonas-dns-públicas-y-privadas)
     - [Configurar una zona DNS pública](#configurar-una-zona-dns-pública)
     - [Configurar una zona DNS privada](#configurar-una-zona-dns-privada)
8. [Resultado final](#-resultado-final)
9. [Eliminación de Recursos](#️-eliminación-de-recursos)
10. [Contribuciones](#-contribuciones)
11. [Licencia](#licencia)

---

## 📘 Descripción

Este laboratorio corresponde al módulo **AZ-104 Microsoft Azure Administrator**, específicamente el **Lab 04: Implement Virtual Networking**.  
Los laboratorios oficiales se encuentran disponibles en [Microsoft Learning Labs AZ-104](https://microsoftlearning.github.io/AZ-104-MicrosoftAzureAdministrator/).  

El objetivo es practicar la **implementación de redes virtuales en Azure**, explorando conceptos fundamentales de **subnetting**, **seguridad de red** y **resolución de nombres**. A lo largo del laboratorio se trabajan diferentes métodos de despliegue y configuración: portal, plantillas ARM, y la integración con **Network Security Groups (NSG)** y **Application Security Groups (ASG)**.  

En este laboratorio se refuerzan los principios de diseño de redes en la nube, destacando la importancia de **evitar solapamiento de direcciones IP** para garantizar escalabilidad y simplificar la resolución de problemas. Se introduce la creación de **subredes organizadas por función** (servicios compartidos, bases de datos, sensores), lo que permite segmentar el tráfico y aplicar reglas de seguridad más específicas.  

Además, se explora la interacción entre **ASG y NSG**, mostrando cómo proteger grupos de servidores con funciones comunes y aplicar reglas de acceso granular. Este enfoque es esencial en escenarios empresariales donde la seguridad y el aislamiento de cargas de trabajo son prioritarios.  

El laboratorio también aborda la configuración de **zonas DNS públicas y privadas** en Azure. Con las zonas públicas se simula la publicación de dominios accesibles desde Internet, mientras que las privadas permiten la resolución de nombres internos dentro de las redes virtuales. Esta dualidad refleja cómo las organizaciones gestionan tanto la exposición externa de servicios como la comunicación interna segura.  

>💡 **Nota profesional:**  
>
>Este laboratorio no solo enseña a crear redes virtuales, sino que también introduce buenas prácticas de diseño y seguridad en entornos de nube. Al combinar portal, plantillas ARM, NSG, ASG y DNS, se establece un marco sólido de administración que complementa otras prácticas de gobernanza como **RBAC y Azure Policy**, preparando el terreno para escenarios avanzados de administración y cumplimiento normativo en Azure.

⏱ **Tiempo estimado:** 50 minutos.

---

## 🌍 Escenario del laboratorio

La organización global planea implementar redes virtuales para acomodar recursos existentes y proyectar crecimiento futuro. Se crean dos redes principales:  

- **CoreServicesVnet**: con un espacio de direcciones amplio para soportar crecimiento.
- **ManufacturingVnet**: diseñada para sistemas de manufactura con múltiples dispositivos conectados.

Se busca evitar solapamiento de direcciones IP y establecer una arquitectura escalable.

---

## Esquema Visual del Laboratorio

![Esquema Laboratorio](./images/Diagrama%20Lab4.png)

---

## 🛠 Habilidades adquiridas

- Creación de redes virtuales y subredes en Azure.  
- Implementación de NSG y ASG para control de tráfico.  
- Configuración de zonas DNS públicas y privadas.  
- Uso del portal y plantillas ARM para despliegues.  

---

### 🎯 Resultados esperados

1. Creamos y configuramos correctamente las redes virtuales en Azure.  
2. Definimos subredes con espacios de direcciones que permiten crecimiento futuro.  
3. Configuramos reglas de seguridad en NSG y ASG para controlar el tráfico de manera granular.  
4. Establecemos zonas DNS públicas y privadas que permiten la resolución de nombres tanto externos como internos.  
5. Validamos que los recursos desplegados cumplen con los objetivos del laboratorio y que la arquitectura es escalable y segura.

---

## 💰 Costo Total del Laboratorio

Usando la calculadora de precios de Azure, este laboratorio genera costos mínimos ya que la mayoría de los recursos de red (VNets, subredes, NSG, ASG) **no tienen cargo directo**.  
El único recurso con costo asociado es la creación de **zonas DNS** (una pública y una privada).

> **NOTA**  
> El costo aproximado de una **zona DNS pública** es de **USD 0,50 al mes**, y el de una **zona DNS privada** es de **USD 0,10 al mes**.  
> En total, el laboratorio tendría un costo mensual aproximado de **USD 0,60**.

> **NOTA**  
> Si consideramos un mes de 30 días y 24 horas, el cálculo por hora sería:  
> **USD 0,60 ÷ (30 × 24) ≈ USD 0,00083 por hora**.  
> Este valor es meramente referencial, ya que depende de la cantidad de registros y consultas DNS realizadas.  

⚠️ **Importante:**  
El costo real puede variar si se asocian **máquinas virtuales** u otros recursos adicionales a las redes creadas en este laboratorio.  

---

## 🧩 Desarrollo del laboratorio

Cada tarea se desarrolla paso a paso en el portal de Azure, editando plantillas ARM y configurando reglas de seguridad. Se refuerza el uso de **Cloud Shell**, **PowerShell** y **CLI** como herramientas complementarias.

### Tarea 1: Crear una red virtual con subredes usando el portal

En esta tarea se construye la infraestructura básica de red dentro de Azure mediante el portal gráfico. Se define una red virtual (VNet) y se segmenta en subredes para organizar los recursos de manera lógica y segura. Este paso establece el espacio de direcciones IP y la estructura inicial sobre la cual se desplegarán servicios y aplicaciones.

1. Iniciamos sesión en el [Azure Portal](https://portal.azure.com).  
2. Buscamos y seleccionamos **Redes Virtuales**.
![Redes virtuales](./images/1.png)

3. En la página de redes virtuales, seleccionamos **Crear**.
![Crear red](./images/2.png)

4. Completamos la pestaña **Datos básicos** para la red **CoreServicesVnet**:  
   - **Resource Group:** `az104-rg4` (lo creamos si no existe)  
   - **Name:** `CoreServicesVnet`  
   - **Region:** `(US) East US`
![Datos básicos](./images/3.png)

5. Nos movemos a la pestaña **Address Space**.  
   - **IPv4 address space:** reemplazamos el valor predefinido por `10.20.0.0/16`.
![Direcciones IP](./images/4.png)

>⚠️ Eliminamos la subred predeterminada antes o después de crear las nuevas.

1. Seleccionamos **+ Agregar una subred** y completamos la información para cada subred:  
   - **SharedServicesSubnet:**  
     - Dirección Inicial: `10.20.10.0`  
     - Tamaño: `/24`  
   - **DatabaseSubnet:**  
     - Dirección Inicial: `10.20.20.0`  
     - Tamaño: `/24`  
![Creando Subnets](./images/5.png)
![Creando Subnets](./images/6.png)

2. Seleccionamos **Revisar y Crear** para finalizar la creación de la red y sus subredes.
![Creando Subnets](./images/7.png)
3. Verificamos que la configuración haya pasado la validación y seleccionamos **Crear**.
![Creando Subnets](./images/8.png)
4. Esperamos a que se complete el despliegue y luego seleccionamos **Ir al recurso**.
![Creando Subnets](./images/9.png)
![Creando Subnets](./images/10.png)
5. En la sección **Automation**, seleccionamos **Exportar Plantilla** y esperamos a que se genere.
![Creando Subnets](./images/11.png)
6. Abrimos la pestaña **Template** y seleccionamos **Download template**.
7. Cambiamos a la pestaña **Parameters** y seleccionamos **Download parameters**.
![Creando Subnets](./images/12.png)  
8. Navegamos en nuestro equipo local a la carpeta **Downloads**.  
9. Verificamos que tengamos el archivo `template.json`.  
    > Este archivo lo utilizaremos para crear la **ManufacturingVnet** en la siguiente tarea.

---

### 🧩 Tarea 2: Crear una red virtual y subredes usando una plantilla

En esta tarea se utiliza un archivo de plantilla (ARM/JSON) para automatizar la creación de la red virtual y sus subredes. El objetivo es demostrar cómo la infraestructura puede ser reproducida y parametrizada de forma consistente, evitando configuraciones manuales. Este enfoque permite escalar y reutilizar configuraciones en diferentes entornos de manera eficiente.

1. Localizamos el archivo `template.json` exportado en la tarea anterior dentro de la carpeta **Descargas**.  
2. Editamos el archivo con el editor de nuestra preferencia (por ejemplo, Visual Studio Code en ventana confiable).  
3. Reemplazamos todas las ocurrencias de **CoreServicesVnet** por **ManufacturingVnet**.
![Creando Subnets](./images/13.png)

4. Reemplazamos todas las ocurrencias de `10.20.0.0` por `10.30.0.0`.
![Creando Subnets](./images/14.png)
5. Modificamos las subredes:  
   - Cambiamos **SharedServicesSubnet** por **SensorSubnet1** con dirección `10.30.20.0/24`.
   ![Creando Subnets](./images/15.png)
   ![Creando Subnets](./images/16.png)
   - Cambiamos **DatabaseSubnet** por **SensorSubnet2** con dirección `10.30.21.0/24`.
   ![Creando Subnets](./images/17.png)
   ![Creando Subnets](./images/18.png)
6. Revisamos el archivo completo para confirmar que los nombres y direcciones coinciden con el diagrama de arquitectura.  
7. Guardamos los cambios realizados en el archivo `template.json`.
8. Localizamos el archivo `parameters.json` exportado en la tarea anterior dentro de la carpeta **Descargas**.  
9. Editamos el archivo y reemplazamos la única ocurrencia de **CoreServicesVnet** por **ManufacturingVnet**.
![Creando Subnets](./images/19.png)
10. Guardamos los cambios en el archivo `parameters.json`.  
11. En el portal de Azure, buscamos y seleccionamos **Implementar una plantilla personalizada**.
![Creando Subnets](./images/20.png)
12. Seleccionamos **Cree su propia plantilla en el editor** y luego **Cargar archivo**.
![Creando Subnets](./images/21.png)
13. Cargamos el archivo `template.json` modificado y seleccionamos **Save**.
![Creando Subnets](./images/22.png)
14. Seleccionamos **Edit parameters** y luego **Load file**.
![Creando Subnets](./images/23.png)
15. Cargamos el archivo `parameters.json` modificado y seleccionamos **Save**.
![Creando Subnets](./images/24.png)
16. Confirmamos que el grupo de recursos seleccionado sea `az104-rg4`.  
17. Seleccionamos **Review + create** y luego **Create** para desplegar la plantilla.
![Creando Subnets](./images/25.png)
![Creando Subnets](./images/26.png)
18. Esperamos a que se complete el despliegue y verificamos en el portal que la red **ManufacturingVnet** y sus subredes se hayan creado correctamente.
![Creando Subnets](./images/27.png)
![Creando Subnets](./images/28.png)
19. Si el despliegue falla parcialmente, eliminamos manualmente los recursos creados y repetimos el proceso.

---

### 🧩 Tarea 3: Configurar comunicación entre un ASG y un NSG

En esta tarea se implementan controles de seguridad de red. Se crea un Application Security Group (ASG) para agrupar máquinas virtuales con funciones similares y un Network Security Group (NSG) para definir reglas de tráfico. Se establece una regla inbound que permite acceso controlado desde el ASG y una regla outbound que bloquea el acceso a Internet. Con ello se refuerza la seguridad y se asegura un flujo de tráfico restringido y administrado.

---

#### Crear el Application Security Group (ASG)

1. En el portal de Azure, buscamos y seleccionamos **Grupos de seguridad de la aplicación**.
![ASG](./images/29.png)
![ASG](./images/30.png)

2. Seleccionamos **Crear** y proporcionamos la información básica:  
   - **Subscription:** nuestra suscripción  
   - **Resource group:** `az104-rg4`  
   - **Name:** `asg-web`  
   - **Region:** East US
   ![ASG](./images/31.png)
   ![ASG](./images/32.png)
3. Seleccionamos **Review + create** y, tras la validación, seleccionamos **Create**.  

>⚠️ Nota:
>
>En este punto asociaríamos el ASG a máquinas virtuales. Estas VMs se verán afectadas por la regla inbound del NSG que configuraremos a continuación.  

---

#### Crear el Network Security Group y asociarlo a CoreServicesVnet

1. En el portal de Azure, buscamos y seleccionamos **Network security groups**.
![ASG](./images/33.png)
   > 💡 Nota:
   >
   >También podemos localizar este recurso desde el menú del portal (icono superior izquierdo) → **Crear recurso** → **Redes** → **Grupo de Seguridad de red**.
   ![ASG](./images/34.png)

2. Seleccionamos **+ Crear** y completamos la pestaña **Datos Básicos**:  
   - **Suscripción:** nuestra suscripción  
   - **Grupo de Recursos:** `az104-rg4`  
   - **Nombre:** `myNSGSecure`  
   - **Región:** East US
   ![ASG](./images/35.png)

3. Seleccionamos **Revisar y Crear** y, tras la validación, seleccionamos **Crear**.
![ASG](./images/36.png)

4. Una vez desplegado el NSG, seleccionamos **Ir al receurso**.
![ASG](./images/37.png)
5. En **Configuración → Subredes**, seleccionamos **Asociar** y configuramos:  
![ASG](./images/38.png)
   - **Virtual network:** `CoreServicesVnet (az104-rg4)`  
   - **Subnet:** `SharedServicesSubnet`  
6. Seleccionamos **OK** para guardar la asociación.
![ASG](./images/39.png)

---

#### Configurar una regla inbound para permitir tráfico desde el ASG

1. Continuamos trabajando con el NSG. En el área **Configuración**, seleccionamos **Reglas de seguridad de entrada**.
2. Revisamos las reglas inbound predeterminadas. Observamos que solo otras VNets y load balancers tienen acceso permitido.  
3. Seleccionamos **+ Agregar**.  
4. En el panel de creación de regla inbound, configuramos:  

- **Origen:** Application security group  
- **Grupos de seguridad de la aplicación de origen:** `asg-web`  
- **Intervalos de puertos de origen:** `*`  
- **Destino:** Any  
- **Servicio:** Custom  
- **Intervalos de puertos de destino:** `80,443`  
- **Protocolo:** TCP  
- **Acción:** Allow  
- **Prioridad:** 100  
- **Nombre:** `AllowASG`  
![ASG](./images/40.png)

1. Seleccionamos **Agregar** para guardar la regla.
![ASG](./images/41.png)

---

#### Configurar una regla outbound que deniegue acceso a Internet

1. En el NSG, seleccionamos **Reglas de seguridad de salida**.  
2. Observamos la regla predeterminada **AllowInternetOutBound**, que no puede eliminarse y tiene prioridad 65001.  
3. Seleccionamos **+ agregar** y configuramos la nueva regla outbound:  

- **Fuente:** Any  
- **Intervalos de puertos de origen:** `*`  
- **Destino:** Service tag  
- **Etiqueta de servicio de destino:** Internet  
- **Servicio:** Custom  
- **Intervalo de puertos de destino:** `*`  
- **Protocolo:** Any  
- **Acción:** Deny  
- **Prioridad:** 4096  
- **Nombre:** `DenyInternetOutbound`  
![ASG](./images/42.png)

1. Seleccionamos **Agregar** para guardar la regla.

---

### 🧩 Tarea 4: Configurar zonas DNS públicas y privadas

Este paso introduce la resolución de nombres en Azure. Se crea una zona DNS pública para gestionar nombres accesibles desde Internet y una zona DNS privada para resolver nombres dentro de las redes internas. La configuración garantiza que los recursos puedan ser identificados mediante nombres amigables en lugar de direcciones IP, tanto en escenarios externos como internos, mejorando la administración y la usabilidad de la infraestructura.

---

#### Configurar una zona DNS pública

1. En el portal de Azure, buscamos y seleccionamos **Zona DNS**.
![DNS](./images/43.png)
![DNS](./images/44.png)

2. Seleccionamos **+ Crear**.
3. En la pestaña **Datos básicos**, configuramos:  
   - **Suscripción:** nuestra suscripción  
   - **Grupo de Recursos:** `az104-rg4`  
   - **Nombre:** `contoso.com` (si está reservado, ajustamos el nombre)  
   - **Region:** East US
   ![DNS](./images/45.png)
4. Seleccionamos **Revisar y crear** y luego **Crear**.
![DNS](./images/46.png)
5. Esperamos a que la zona DNS se despliegue y seleccionamos **Ir al Recurso**.
![DNS](./images/47.png)
6. En el panel **Información general**, observamos los cuatro servidores de nombres asignados a la zona. Copiamos uno de ellos, lo necesitaremos en un paso posterior.
![DNS](./images/48.png)  
7. Expandimos el panel **Administración de DNS** y seleccionamos **Registros**.
![DNS](./images/49.png)
8. Seleccionamos **+ Agregar** y configuramos:  
   - **Nombre:** `www`  
   - **Tipo:** A  
   - **TTL:** 1  
   - **Dirección IP:** `10.1.1.4`
   ![DNS](./images/50.png)
   ⚠️ Nota: En un escenario real, ingresaríamos la IP pública de nuestro servidor web.  
9. Seleccionamos **Agregar** y verificamos que el dominio tenga un registro A llamado `www`.  
10. Abrimos una consola y ejecutamos:  

    ```shell
    nslookup www.contoso.com <name server copiado en el paso 6>
    ```  

![DNS](./images/51.png)

1. Verificamos que el nombre `www.contoso.com` se resuelva a la dirección IP configurada.

---

#### Configurar una zona DNS privada

1. En el portal de Azure, buscamos y seleccionamos **Zonas de DNS privado**.
![DNS](./images/52.png)
![DNS](./images/53.png)
2. Seleccionamos **+ Crear**.  
3. En la pestaña **Datos Básicos**, configuramos:  

- **Subscription:** nuestra suscripción  
- **Resource group:** `az104-rg4`  
- **Name:** `private.contoso.com` (ajustamos si fue necesario renombrar)  
- **Region:** East US  
![DNS](./images/54.png)
![DNS](./images/55.png)

1. Seleccionamos **Revisa y crear** y luego **Crear**.
2. Esperamos a que la zona DNS se despliegue y seleccionamos **Ir al recurso**.
![DNS](./images/56.png)

3. En el panel **Información general**, notamos que no hay registros de servidores de nombres.  
4. Expandimos el panel **Administración de DNS** y seleccionamos **Vínculos de virtual network**.  
5. Configuramos el vínculo:  

- **Nombre del Vínculo:** `manufacturing-link`  
- **Virtual Network:** `ManufacturingVnet`  
![DNS](./images/58.png)

1. Seleccionamos **Crear** y esperamos a que el vínculo se cree.  
2. Desde el panel **Administración DNS**, seleccionamos **Registros**.  
3. Agregamos un registro para una máquina virtual interna:  

- **Nombre:** `sensorvm`  
- **Tipo:** A  
- **TTL:** 1  
- **Dirección IP:** `10.1.1.4`
![DNS](./images/59.png)
   ⚠️ Nota: En un escenario real, ingresaríamos la IP de una VM específica de manufactura.  

---

### ✅ Resultado final

Al finalizar este laboratorio:

- Contamos con dos redes virtuales: **CoreServicesVnet** y **ManufacturingVnet**, listas para soportar crecimiento y segmentación de cargas de trabajo.  
- Implementamos seguridad mediante **Network Security Groups (NSG)** y **Application Security Groups (ASG)**, asegurando control de tráfico entrante y saliente.  
- Configuramos resolución de nombres con **zonas DNS públicas y privadas**, garantizando conectividad tanto hacia Internet como dentro de nuestras redes internas.  
- Consolidamos buenas prácticas de diseño de redes en la nube, evitando solapamiento de direcciones IP y aplicando segmentación lógica.  

Este resultado nos proporciona una base sólida para continuar con escenarios más avanzados de administración, seguridad y gobernanza en Azure.
![DNS](./images/60.png)

---

Perfecto, Diego. Para que tu README quede completo y fiel al laboratorio, aquí tienes el apartado de **eliminación de recursos** en formato Markdown, con el mismo estilo inclusivo que venimos usando:

---

## 🗑️ Eliminación de recursos

Al finalizar el laboratorio, eliminamos los recursos creados para liberar la suscripción y minimizar costos.  
La forma más sencilla es eliminar directamente el **grupo de recursos** del laboratorio.

1. En el **Portal de Azure**, seleccionamos el grupo de recursos.  
2. Seleccionamos **Eliminar el grupo de recursos**.  
3. Ingresamos el nombre del grupo de recursos.  
4. Hacemos clic en **Elimninar**.  
![DNS](./images/61.png)

También podemos hacerlo mediante línea de comandos:

- **Azure PowerShell**:  

  ```powershell
  Remove-AzResourceGroup -Name resourceGroupName
  ```

- **Azure CLI**:  

  ```bash
  az group delete --name resourceGroupName
  ```

>⚠️ **Nota:**
>
>Este paso es importante para evitar costos innecesarios y mantener la suscripción limpia.

---

## 🤝 Contribuciones

Este README fue adaptado y traducido al español para fines de estudio del examen **AZ-104**. Puedes contribuir mejorando ejemplos, diagramas o agregando pasos con CLI/PowerShell.  

---

## Licencia

Este laboratorio se basa en materiales oficiales de Microsoft Learning Labs para AZ-104. Uso educativo y de práctica.
