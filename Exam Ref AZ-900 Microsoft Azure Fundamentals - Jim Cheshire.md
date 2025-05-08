## Capítulo 1: Describir la computación en la nube

Este capítulo introduce los conceptos generales de la computación en la nube, sus beneficios, los diferentes modelos de nube y los tipos de servicios disponibles.

### Habilidad 1.1: Describir la computación en la nube

-   **Computación en la Nube (Definición):** Uso de computadoras e infraestructura que ejecutan aplicaciones accesibles a través de internet, involucrando a un proveedor de nube que asume parte de la responsabilidad y ofreciendo beneficios financieros al pagar solo por lo necesario.
-   **Modelo de Responsabilidad Compartida:**
    -   La responsabilidad de la implementación en la nube siempre se comparte entre el cliente y el proveedor.
    -   La cantidad de responsabilidad transferida al proveedor depende del tipo de servicio en la nube utilizado.
    -   **On-premises:** El cliente es responsable de todo (infraestructura física, red, SO, aplicaciones, datos, etc.).
    -   **Nube:** El proveedor asume responsabilidades como la infraestructura de red, mientras el cliente puede seguir siendo responsable del SO y la aplicación (varía según el servicio).
-   **Modelos de Nube:**
    -   **Nube Pública:**
        -   Modelo más común (ej. Microsoft Azure, AWS, Google Cloud).
        -   Infraestructura compartida accesible en una red pública.
        -   El proveedor gestiona la infraestructura.
        -   Beneficios: fácil y rápido de adoptar, control de costos (pagar por uso), escalabilidad.
        -   Se conoce como *entorno multitenant* (multiusuario).
    -   **Nube Privada:**
        -   Beneficios de la nube en un entorno privado dedicado a una sola empresa.
        -   Puede ser alojada on-premises o por un proveedor externo.
        -   Opera en una red privada, accesible solo por una organización.
        -   Razones principales: privacidad y preocupaciones regulatorias.
        -   Se conoce como *entorno single-tenant* (monousuario).
        -   La propiedad de la infraestructura no define si es privada, sino la privacidad de la infraestructura y los datos.
    -   **Nube Híbrida:**
        -   Mezcla de nubes públicas y privadas.
        -   Aplicaciones en la nube pública que acceden a datos seguros on-premises.
        -   A menudo es la primera incursión de una empresa en la nube, especialmente con sistemas heredados.
        -   No siempre incluye on-premises; puede ser la combinación de un centro de datos de terceros y una nube pública.
        -   *Azure Stack:* Permite ejecutar servicios de Azure on-premises.
-   **Modelo Basado en el Consumo:**
    -   Pagar solo por los recursos informáticos que se requieren en un momento particular.
    -   Escalar aplicaciones para usar solo el número necesario de VMs y su potencia.
    -   Pagar solo por el tiempo que se consumen los recursos (ej. código que se ejecuta solo cuando se usa la aplicación).
-   **Comparación de Modelos de Nube:**
    -   **Pública:** Máxima transferencia de responsabilidad al proveedor, beneficios financieros. Desventajas: menos control de infraestructura, preocupaciones de seguridad (internet público), configuración limitada por el proveedor.
    -   **Privada:** Mayor seguridad y control. Desventajas (si es on-premises): costos de TI similares a no tener nube, necesidad de personal especializado. Si es con un tercero: pérdida de control sobre la seguridad de datos.
    -   **Híbrida:** A menudo elegida por desafíos de las otras dos. Desafíos: compatibilidad de datos, complejidad de redes, posibles ralentizaciones por distancia geográfica.

### Habilidad 1.2: Describir el beneficio de usar servicios en la nube

-   **Alta Disponibilidad y Escalabilidad:**
    -   **Disponibilidad:** Minimizar interrupciones (red, aplicación, sistema, energía).
        -   Los proveedores ofrecen Acuerdos de Nivel de Servicio (SLA) garantizando un cierto nivel de disponibilidad (ej. 99.9%).
        -   **Interrupción de Red:** Los proveedores invierten en infraestructura robusta.
        -   **Fallo de Aplicación:** El proveedor ofrece herramientas de diagnóstico (ej. Application Insights en Azure).
        -   **Interrupción del Sistema (VMs):** Las VMs son computadoras basadas en software. El proveedor monitorea y recupera VMs no saludables.
        -   **Corte de Energía:** Los proveedores invierten en respaldos y redundancia.
        -   **Problemas con Sistemas Dependientes:** La nube ofrece herramientas para solucionar problemas.
    -   **Escalabilidad:** Ajustar recursos según la necesidad.
        -   *Escalado horizontal (scale out):* Añadir más VMs idénticas.
        -   *Escalado vertical (scale up):* Mover a una VM más potente (más CPU, memoria).
        -   También se puede *escalar hacia adentro (scale in)* y *escalar hacia abajo (scale down)* para reducir recursos.
        -   *Elasticidad:* Capacidad de escalar automáticamente según la demanda (ej. Auto-Scale en Azure).
        -   *Agilidad de la Nube:* Rapidez y flexibilidad para escalar.
-   **Fiabilidad y Previsibilidad:**
    -   *Tolerancia a Fallos (Fault Tolerance):* Sistemas que monitorean y actúan ante recursos no saludables, moviendo automáticamente a un sistema saludable. No confundir con escalabilidad.
    -   *Recuperación ante Desastres (Disaster Recovery):* Planes (BCDR) para proteger datos y aplicaciones ante desastres mayores, replicando recursos en regiones no afectadas.
-   **Seguridad y Gobernanza:**
    -   Proveedores invierten significativamente en seguridad, monitoreando y eliminando amenazas.
    -   Ofrecen características de gobernanza para controlar acceso y uso de recursos.
-   **Manejabilidad en la Nube:**
    -   Aunque se comparte responsabilidad, los proveedores ofrecen herramientas para facilitar la gestión de recursos en la nube (monitoreo, alertas, autoescalado, configuración de seguridad, control de costos).

### Habilidad 1.3: Describir los tipos de servicio en la nube

Equilibrar la necesidad de controlar recursos con la conveniencia de que el proveedor se encargue.

-   **Pirámide de la Nube:** Representa la relación entre control y responsabilidad (IaaS en la base: más control, más responsabilidad; SaaS en la cima: menos control, menos responsabilidad).
-   **Infraestructura como Servicio (IaaS):**
    -   El proveedor ofrece infraestructura virtualizada (VMs).
    -   El cliente es responsable del SO, parches, servicios y aplicaciones.
    -   Máximo control sobre los recursos.
    -   Beneficios: tolerancia a fallos y recuperación ante desastres subyacentes del proveedor.
    -   Se paga solo cuando los recursos están asignados (detener VM detiene facturación).
    -   Casos de uso: pruebas de desarrolladores, VMs potentes temporalmente, control total del SO.
-   **Plataforma como Servicio (PaaS):**
    -   El proveedor ofrece infraestructura, SO, middleware y características para construir y gestionar aplicaciones.
    -   El cliente solo controla la aplicación.
    -   Minimiza la inversión en gestión.
    -   Ejemplos: Azure App Service, Azure SQL Database.
    -   Permite el concepto de *lift-and-shift* (mover aplicaciones de on-premises a la nube con mínimos cambios).
    -   Beneficios: tolerancia a fallos, elasticidad, escalado, backup, etc.
    -   Desventaja: el proveedor controla parches y actualizaciones del SO/componentes.
-   **Software como Servicio (SaaS):**
    -   El proveedor controla todo: infraestructura, SO, middleware y software.
    -   El cliente "alquila" el software y lo usa (generalmente vía web).
    -   Modelo de pago por uso.
    -   Beneficios: accesibilidad desde cualquier dispositivo, el proveedor mantiene y actualiza el software.
    -   Ejemplos: Microsoft 365, Gmail.
    -   Desventaja: poca o nula personalización o control sobre la aplicación.

---

## Capítulo 2: Describir la arquitectura y los servicios de Azure

Este capítulo profundiza en los servicios y soluciones que ofrece Azure, su arquitectura, componentes clave, y aspectos de computación, redes, almacenamiento, identidad, acceso y seguridad.

### Habilidad 2.1: Describir los componentes arquitectónicos principales de Azure

-   **Regiones, Pares Regionales y Regiones Soberanas de Azure:**
    -   **Geografías:** Límites, a menudo fronteras de países, para cumplir regulaciones de manejo de datos.
    -   **Regiones:** Cada geografía tiene dos o más regiones, separadas por cientos de millas (ej. Central US, East US).
    -   **Pares Regionales:** Dentro de cada geografía, dos regiones forman un par. Las actualizaciones se realizan en una región del par a la vez, garantizando disponibilidad. Es crucial para la recuperación ante desastres.
    -   **Regiones Soberanas:** Nubes aisladas para cumplir requisitos específicos de cumplimiento y soberanía de datos.
        -   *Azure Government (EE.UU.):* Centros de datos aislados, personal ciudadano estadounidense. Portal: `portal.azure.us`.
        -   *Azure Germany:* Operada por un fideicomisario de datos local (T-Systems) para cumplir requisitos de la UE/EFTA/UK.
        -   *Azure China (operado por 21Vianet):* Nube separada operada por una empresa china.
-   **Zonas de Disponibilidad (Availability Zones):**
    -   Ubicaciones físicas únicas dentro de una región de Azure, cada una con suministro de energía, refrigeración y red independientes.
    -   Al menos tres zonas por región habilitada.
    -   Protegen contra fallos a nivel de edificio/centro de datos dentro de una región.
    -   No disponibles en todas las regiones ni para todos los servicios.
    -   Ofrecen alta disponibilidad y tolerancia a fallos, pero no necesariamente recuperación ante desastres a gran escala (para eso están los pares regionales).
    -   Garantizan 99.99% de tiempo de actividad para VMs si se despliegan en dos o más zonas.
    -   **Servicios Zonales:** VMs, discos administrados, IPs públicas (se deben desplegar explícitamente en ≥2 zonas).
    -   **Servicios Redundantes de Zona:** Almacenamiento redundante de zona (ZRS), SQL Database con redundancia de zona (Azure replica automáticamente).
    -   No confundir con *Conjuntos de Disponibilidad (Availability Sets)*: VMs en diferentes racks físicos dentro de un centro de datos (SLA 99.95%).
-   **Centros de Datos de Azure (Datacenters):**
    -   Edificios físicos en cada región que albergan el hardware de Azure.
    -   Conectividad de red de baja latencia entre servicios dentro de una misma región.
    -   Suministro de energía aislado y generadores.
    -   Tráfico sobre la red de fibra óptica de Microsoft.
-   **Recursos y Grupos de Recursos de Azure:**
    -   **Recurso de Azure:** Cualquier servicio o componente creado en Azure (VM, red virtual, base de datos, etc.).
    -   **Grupo de Recursos:** Un contenedor lógico para recursos de Azure.
        -   Permite desplegar y gestionar recursos como una única entidad.
        -   Facilita el uso de plantillas ARM (Azure Resource Manager).
        -   Ayuda a organizar y visualizar recursos por aplicación o departamento.
        -   Permite ver costos de todos los recursos del grupo.
        -   Un recurso solo puede existir en un grupo de recursos a la vez (pero se puede mover).
        -   Al eliminar un grupo de recursos, se eliminan todos los recursos contenidos.
-   **Suscripciones de Azure:**
    -   Se obtiene automáticamente al registrarse en Azure. Todos los recursos se crean dentro de una suscripción.
    -   Se pueden crear suscripciones adicionales para agrupar recursos lógicamente o para informes de costos.
    -   Cada suscripción tiene límites/cuotas (ej. número de cuentas de almacenamiento por región).
    -   Permite ver gastos y previsiones de costos.
    -   Tipos: Prueba Gratuita, Pago por Uso, Azure para Estudiantes.
    -   Cada suscripción tiene un ID de suscripción único globalmente.
-   **Grupos de Administración (Management Groups):**
    -   Contenedores para organizar suscripciones y aplicar políticas y control de acceso de forma jerárquica.
    -   Pueden contener solo suscripciones de Azure u otros grupos de administración.
    -   Límites: 10,000 grupos de administración, jerarquía de hasta 6 niveles, un solo padre por grupo/suscripción.
-   **Jerarquía de Grupos de Recursos, Suscripciones y Grupos de Administración:**
    1.  **Grupo de Administración (Raíz del Inquilino por defecto):** Contiene suscripciones u otros grupos de administración.
    2.  **Suscripción de Azure:** Dentro de un grupo de administración.
    3.  **Grupo de Recursos:** Dentro de una suscripción (se debe crear explícitamente).
    4.  **Recurso de Azure:** Dentro de un grupo de recursos.

### Habilidad 2.2: Describir los servicios de computación y redes de Azure

-   **Tipos de Computación:**
    -   **Instancias de Contenedor (Azure Container Instances - ACI):**
        -   Ejecuta aplicaciones en contenedores Docker sin gestionar VMs directamente.
        -   Ideal para aplicaciones simples, se paga por memoria y CPU consumida por el contenedor.
        -   No es ideal para cargas pesadas o escalables (para eso está AKS).
    -   **Máquinas Virtuales (VMs):**
        -   Computadoras basadas en software que se ejecutan en un host físico.
        -   Ofrecen más opciones que ACI (CPU, memoria, disco, SO completo).
        -   Se paga mientras estén asignadas, incluso si no se usan.
    -   **Funciones (Azure Functions):**
        -   Servicio para ejecutar microservicios (pequeños componentes con capacidades específicas).
        -   Se ejecutan en Azure App Service, pagando solo por el tiempo de ejecución.
        -   Modelo sin servidor (serverless).
-   **Opciones para Máquinas Virtuales de Azure:**
    -   Al crear una VM (ej. Ubuntu Server), se especifican grupo de recursos, nombre, autenticación (contraseña/clave SSH).
    -   La VM es un "guest" en un host físico en un centro de datos de Azure.
    -   Eventos que pueden causar tiempo de inactividad: mantenimiento planificado, mantenimiento no planificado, tiempo de inactividad inesperado.
    -   **Conjuntos de Disponibilidad (Availability Sets):**
        -   Protegen contra eventos de mantenimiento y fallos de hardware en un rack.
        -   Requieren ≥2 VMs desplegadas en el conjunto.
        -   Utilizan *dominios de actualización* (protegen contra reinicios por mantenimiento planificado, por defecto 5) y *dominios de fallo* (representan racks físicos, por defecto 2).
        -   SLA 99.95% para despliegues multi-VM.
        -   Desventajas: creación explícita de cada VM, necesidad de un balanceador de carga, costos.
    -   **Conjuntos de Escala de Máquinas Virtuales (Virtual Machine Scale Sets - VMSS):**
        -   Permiten crear y gestionar un grupo de VMs idénticas (hasta 1,000).
        -   Se despliegan automáticamente en conjuntos de disponibilidad y son compatibles con zonas de disponibilidad.
        -   Permiten escalado automático (auto-scale) basado en métricas.
        -   SLA 99.95% con múltiples VMs; 99.9% para VM única con almacenamiento premium.
    -   **Azure Virtual Desktop (AVD):**
        -   Servicio PaaS para virtualización de escritorios y aplicaciones.
        -   Permite a los empleados acceder a SOs y aplicaciones desde casi cualquier dispositivo.
        -   Componentes: inquilino AVD, grupos de hosts (con hosts de sesión - VMs de Azure) y grupos de aplicaciones.
        -   Utiliza Windows 10/11 Multi-User.
-   **Recursos Necesarios para Máquinas Virtuales:**
    -   Además de la VM: red virtual, dirección IP pública, grupo de seguridad de red, interfaz de red y un disco (para el SO).
    -   También requieren un grupo de administración, suscripción y grupo de recursos.
-   **Opciones de Alojamiento de Aplicaciones:**
    -   **Web Apps en Azure App Service:**
        -   Oferta PaaS para alojar aplicaciones web.
        -   Se ejecutan en VMs preconfiguradas dentro de un *Plan de App Service*.
        -   Plan de App Service: define región, número de VMs y sus propiedades (niveles de precios: Free, Shared, Basic, Standard, Premium).
        -   Permite elegir runtime (Java, .NET, PHP) o contenedor Docker.
    -   **Azure Kubernetes Service (AKS):**
        -   Servicio PaaS para alojar clústeres de Kubernetes (orquestación de contenedores).
        -   Kubernetes: gestiona *pods* (grupos de contenedores relacionados) en *nodos* (workers), controlados por un *plano de control*.
        -   AKS simplifica la creación y gestión del clúster (Microsoft gestiona el plano de control). Gratuito, solo se paga por los recursos de cómputo.
    -   **Azure Spring Cloud:** Servicio PaaS para alojar aplicaciones Java Spring.
    -   **Máquinas Virtuales (IaaS):** Máximo control, pero mayor responsabilidad de configuración y gestión.
-   **Redes Virtuales (Virtual Networking):**
    -   **Azure Virtual Network (VNet):** Permite a los servicios de Azure comunicarse entre sí y con internet, y con recursos on-premises.
    -   Compuesta por NICs virtuales, IPs. Se pueden crear subredes.
    -   Las IPs dentro de una VNet son privadas. Se necesita una IP pública para acceso desde internet.
    -   **Emparejamiento de Redes Virtuales (VNet Peering):**
        -   Conecta VNets para que los recursos se comuniquen.
        -   Tráfico sobre la red troncal privada de Microsoft, no internet.
        -   Puede ser en la misma región o diferentes (*emparejamiento global*).
        -   Requiere crear un emparejamiento en ambas redes.
        -   Limitación con Azure Load Balancer Básico en emparejamiento global.
    -   **Azure DNS:**
        -   Servicio para gestionar registros DNS (mapeo de nombres de dominio a IPs).
        -   Integrado con Azure para control de acceso, logs, políticas.
        -   Tipos de zonas DNS:
            -   *Zona Pública:* Entradas DNS para internet (endpoints públicos).
            -   *Zona Privada:* Entradas DNS para uso dentro de una VNet (endpoints privados).
        -   Se configuran registros (A, AAAA, CNAME, MX, etc.).
    -   **Azure VPN Gateway:**
        -   Permite conexiones seguras (VPN) entre una VNet de Azure y otras redes.
        -   Usa IPSec e IKE para cifrar el tráfico.
        -   Requiere una *subred de puerta de enlace (gateway subnet)*.
        -   Tipos de conexión: VNet-to-VNet (entre dos VNets de Azure), Site-to-Site (VNet a red on-premises, requiere dispositivo VPN on-premises), Point-to-Site (VNet a un dispositivo individual).
    -   **Azure ExpressRoute:**
        -   Conexión privada dedicada de alta velocidad (hasta 10 Gbps o más con ExpressRoute Direct) entre redes on-premises y Azure.
        -   No usa internet público. Tráfico a través de un proveedor de conectividad o conexión directa a routers Microsoft Enterprise Edge (MSEE).
        -   Se conoce como un *circuito*. Mayor fiabilidad de ancho de banda.

### Habilidad 2.3: Describir los servicios de almacenamiento de Azure

-   **Servicios de Almacenamiento de Azure:**
    -   **Azure Blob Storage:**
        -   Para datos no estructurados (texto, imágenes, videos, documentos).
        -   *Blobs:* Entidades almacenadas.
            -   *Block Blobs:* Archivos usados por aplicaciones.
            -   *Append Blobs:* Optimizados para operaciones de anexo (logs).
            -   *Page Blobs:* Archivos de disco duro virtual (.vhd) para VMs de Azure.
        -   Se almacenan en *contenedores* para organización.
    -   **Azure Disks:**
        -   Discos usados en VMs.
        -   Disco temporal (se pierden datos en reinicio/mantenimiento) y discos de datos (persistentes).
        -   Tipos: HDD, SSD (Standard, Premium).
        -   Se pueden crear a partir de instantáneas (snapshots), blobs o vacíos.
    -   **Azure Files:**
        -   Recurso compartido de archivos totalmente gestionado, montable como un recurso SMB.
        -   Para aplicaciones que usan NAS o recursos compartidos SMB.
        -   Montable en VMs de Azure y on-premises (Windows, Linux, macOS). Requiere puerto TCP 445 abierto.
    -   **Azure Queues (Azure Queue Storage):**
        -   Servicio para encolar muchos mensajes (hasta 64KB por mensaje).
        -   Accesible vía URL, autenticado.
        -   Utilizado para desacoplar componentes de aplicaciones.
        -   TTL por defecto de 7 días para mensajes (configurable).
-   **Niveles de Almacenamiento (Storage Tiers) para Blob Storage:**
    -   **Hot (Frecuente):** Datos accedidos a menudo. Mayor costo de almacenamiento, menor costo de acceso.
    -   **Cool (Esporádico):** Datos almacenados más tiempo, accedidos menos a menudo. Menor costo de almacenamiento, mayor costo de acceso. Mínimo 30 días de almacenamiento.
    -   **Archive (Archivo Muerto):** Almacenamiento a largo plazo. Costo de almacenamiento más bajo, costo de acceso más alto. Mínimo 180 días. Recuperación lenta (hasta 15 horas, a menos que sea alta prioridad).
        -   *Rehidratación:* Mover un blob de Archive a Hot/Cool para accederlo.
-   **Opciones de Redundancia:**
    -   Configuradas a nivel de cuenta de almacenamiento. No se pueden cambiar después de la creación.
    -   **Redundancia en Región Primaria:**
        -   *Almacenamiento con redundancia local (LRS):* 3 copias en un único centro de datos. Menos costoso, menos duradero. Escrituras síncronas.
        -   *Almacenamiento con redundancia de zona (ZRS):* 3 copias en diferentes zonas de disponibilidad (centros de datos separados) dentro de la región primaria. Protege contra fallos de un centro de datos. Escrituras síncronas.
    -   **Redundancia en Múltiples Regiones:**
        -   *Almacenamiento con redundancia geográfica (GRS):* 3 copias en la región primaria (usando LRS) + 3 copias en una región secundaria (usando LRS), separada por cientos de millas. Replicación asíncrona a la secundaria.
        -   *Almacenamiento con redundancia de zona geográfica (GZRS):* 3 copias en zonas de disponibilidad en la región primaria (usando ZRS) + 3 copias en una región secundaria (usando LRS). Replicación asíncrona a la secundaria.
        -   Se puede habilitar acceso de lectura a la región secundaria (RA-GRS, RA-GZRS).
-   **Cuentas de Almacenamiento y Tipos de Almacenamiento:**
    -   Recurso subyacente para todos los servicios de almacenamiento.
    -   **Standard general-purpose v2 (GPv2):** Recomendado para la mayoría de los usos, soporta todos los servicios.
    -   **Premium:** Para alto rendimiento (usa SSDs).
        -   *Block Blobs:* Para Block Blobs y Append Blobs con alta transaccionalidad.
        -   *File shares:* Para Azure Files, necesario para NFS.
        -   *Page Blobs:* Para Page Blobs de alto rendimiento.
    -   No se puede cambiar el tipo de cuenta después de la creación.
-   **Mover Archivos hacia y desde Azure Storage:**
    -   **AzCopy:** Herramienta de línea de comandos para copiar blobs/archivos. Autenticación con token SAS o Azure AD.
    -   **Azure Storage Explorer:** Aplicación gratuita multiplataforma para gestionar recursos de almacenamiento (arrastrar y soltar, generar SAS, etc.).
    -   **Azure File Sync:** Sincroniza archivos de Azure Files con servidores on-premises para acceso local rápido.
-   **Migrar a Azure:**
    -   **Azure Migrate:** Servicio para descubrir, evaluar y migrar servidores, bases de datos, web apps, VDI a Azure (desde on-premises u otras nubes).
        -   Proceso: Descubrir -> Evaluar -> Migrar.
        -   Usa un proyecto de Azure Migrate y, opcionalmente, un appliance de Azure Migrate.
    -   **Azure Data Box:** Servicio para transferir grandes cantidades de datos (offline) a Azure Storage.
        -   *Data Box Disk:* Hasta 5 SSDs (aprox. 35 TB total). Para una única cuenta de almacenamiento.
        -   *Data Box:* Dispositivo robusto (aprox. 80 TB). Para hasta 10 cuentas de almacenamiento. Cifrado AES 256-bit.
        -   *Data Box Heavy:* Appliance grande (aprox. 770 TB). Para hasta 10 cuentas de almacenamiento. Interfaces 40 GbE.
        -   Tras la copia, los discos/dispositivos se devuelven a Microsoft para la carga de datos y borrado seguro.

### Habilidad 2.4: Describir la identidad, el acceso y la seguridad de Azure

-   **Servicios de Directorio en Azure:**
    -   **Autenticación:** Proceso de verificar la identidad de un usuario (ej. con usuario y contraseña).
    -   **Autorización:** Proceso de determinar qué acciones puede realizar un usuario autenticado.
    -   **Azure Active Directory (Azure AD):**
        -   Servicio de identidad basado en la nube para controlar el acceso a recursos.
        -   Directorio de usuarios (identidad: ID de usuario, contraseña, roles de directorio).
        -   Se crea automáticamente con una suscripción de Azure.
        -   También gestiona *entidades de servicio (service principals)* para aplicaciones y *managed identities* para recursos de Azure.
    -   **Azure Active Directory Domain Services (Azure AD DS):**
        -   Implementación en la nube de AD DS (servicios de dominio de Active Directory).
        -   Proporciona dominios gestionados, unión a dominio, políticas de grupo, autenticación Kerberos/NTLM.
        -   Microsoft gestiona los controladores de dominio (DCs) en un *conjunto de réplicas*.
        -   **Azure AD Connect:** Sincroniza dominios AD on-premises con Azure AD/Azure AD DS.
        -   SKUs: Standard, Enterprise, Premium (varían en objetos, autenticaciones/hora, confianzas de bosque).
-   **Métodos de Autenticación en Azure:**
    -   **Single Sign-On (SSO):**
        -   Permite a los usuarios acceder a recursos corporativos con una única autenticación (la del inicio de sesión en su dispositivo).
        -   Requiere que el dispositivo esté unido a Azure AD.
        -   Métodos: Sincronización de hash de contraseña (password hash synchronization) o Autenticación de paso a través (pass-through authentication) con Azure AD Connect.
    -   **Autenticación Multifactor (MFA):**
        -   Requiere dos o más factores de verificación: algo que sabes (contraseña), algo que tienes (teléfono), algo que eres (biometría).
        -   Azure MFA es de dos factores (verificación en dos pasos).
        -   Se habilita por usuario o mediante políticas.
        -   Métodos: Microsoft Authenticator app, Windows Hello for Business, clave de seguridad FIDO2, token OAUTH, SMS, llamada telefónica.
    -   **Autenticación sin Contraseña (Passwordless):**
        -   Usa algo que tienes (dispositivo, clave) y algo que eres (biometría), o un PIN, en lugar de solo contraseña.
        -   Aún puede involucrar MFA.
        -   Métodos: Microsoft Authenticator, FIDO2, Windows Hello for Business.
-   **Identidades Externas y Acceso de Invitados:**
    -   **Miembros:** Usuarios de Azure AD que pertenecen a la organización.
    -   **Invitados:** Usuarios de fuera de la organización añadidos a Azure AD para colaborar.
        -   **Azure AD B2B (business-to-business) collaboration:** Permite invitar usuarios externos.
    -   Se pueden añadir aplicaciones empresariales a Azure AD (Facebook, Twitter, etc.) para gestionarlas y permitir SSO.
    -   Una *entidad de servicio (service principal)* representa una aplicación en Azure AD para concederle acceso a recursos.
-   **Acceso Condicional de Azure AD (Conditional Access):**
    -   Políticas que se aplican a usuarios para controlar el acceso a recursos.
    -   **Asignaciones:** Definen a quién se aplica la política (usuarios, grupos, roles, invitados) y condiciones (plataforma, ubicación IP).
    -   **Controles de Acceso:** Determinan cómo se aplica la política (bloquear acceso, requerir dispositivo compatible, MFA, etc.).
    -   Disponible en niveles Premium de Azure AD.
-   **Control de Acceso Basado en Roles (RBAC):**
    -   Autoriza a usuarios según roles definidos.
    -   Elementos:
        -   *Entidad de Seguridad (Security Principal):* Usuario, grupo, entidad de servicio, identidad administrada.
        -   *Rol (Definición de Rol):* Define qué puede hacer la entidad de seguridad con un recurso (ej. leer, escribir, eliminar).
        -   *Ámbito (Scope):* Nivel al que se aplica el rol (grupo de administración, suscripción, grupo de recursos, recurso individual).
        -   *Asignaciones de Roles:* Asignar un rol a una entidad de seguridad en un ámbito específico.
    -   Roles integrados comunes: Propietario (Owner), Colaborador (Contributor), Lector (Reader). Muchos otros roles específicos de recursos.
    -   Las asignaciones de roles son aditivas (los permisos se acumulan).
    -   RBAC es aplicado por Azure Resource Manager (ARM).
-   **Defensa en Profundidad y Confianza Cero (Zero-Trust):**
    -   **Defensa en Profundidad (Castle Approach):** Múltiples capas de seguridad (ej. Azure Firewall, NSGs, Azure DDoS Protection).
    -   **Confianza Cero (Zero-Trust):**
        -   Marco de seguridad moderno: "nunca confiar, siempre verificar".
        -   Asume que cualquier acceso puede ser una brecha hasta que se verifique lo contrario.
        -   Se basa en identificación explícita, uso de políticas de Acceso Condicional, MFA, y segmentación de red.
        -   Aplica a identidades, endpoints, datos, aplicaciones, infraestructura y red.
-   **Microsoft Defender for Cloud:**
    -   Solución unificada para proteger recursos en la nube (Azure, AWS, GCP) y on-premises, y ayudar con el cumplimiento normativo.
    -   Áreas:
        -   *Postura de Seguridad (Security Posture):* Secure Score, recomendaciones.
        -   *Cumplimiento Normativo (Regulatory Compliance):* Visión general del cumplimiento.
        -   *Protección de Cargas de Trabajo (Workload Protections):* Porcentaje de protección por tipo de servicio.
        -   *Firewall Manager:* Seguridad de redes en Azure.
    -   Permite añadir proveedores de nube y servidores no Azure (on-premises).

---

## Capítulo 3: Describir la gestión y la gobernanza de Azure

Este capítulo cubre la gestión de costos en Azure, las herramientas para la gobernanza y el cumplimiento, las características para gestionar y desplegar recursos, y las herramientas de monitorización.

### Habilidad 3.1: Describir la gestión de costos en Azure

-   **Factores que pueden afectar los costos:**
    -   **Tipo de Recurso:** Cada servicio tiene *medidores* que rastrean el uso (ej. tráfico de red, almacenamiento).
    -   **Compra del Recurso:** Acuerdos empresariales (Enterprise Agreements) o Proveedores de Soluciones en la Nube (CSP) pueden ofrecer descuentos.
    -   **Regiones de Azure:** Los costos varían por región debido a diferencias en costos operativos de Microsoft.
    -   **Zona de Facturación (Billing Zone):** Regiones agrupadas en zonas con diferentes costos de tráfico de red saliente.
    -   Tráfico entrante a un centro de datos de Azure es gratuito; el saliente se cobra después de los primeros 100 GB/mes.
-   **Reducción de costos en Azure:**
    -   **Reservas de Azure (Azure Reservations):** Descuentos por comprometerse al uso de recursos (VMs) por adelantado (1 o 3 años).
    -   **Capacidad Reservada (Reserved Capacity):** Similar a las reservas, pero para Azure SQL Database, Cosmos DB, Synapse Analytics.
    -   **Beneficio Híbrido de Azure (Azure Hybrid Benefit):** Usar licencias propias de Windows Server o SQL Server en Azure para ahorrar costos en VMs y SQL Database.
    -   **VMs de Azure Spot (Azure Spot VMs):** Usar capacidad de servidor no utilizada de Microsoft con grandes descuentos para cargas de trabajo interrumpibles.
-   **Calculadora de Precios y Calculadora de Costo Total de Propiedad (TCO):**
    -   **Calculadora de Precios (Pricing Calculator):**
        -   Estima gastos de Azure basados en productos, regiones, configuraciones.
        -   Permite seleccionar productos, configurar detalles (ej. nivel de servicio, tamaño de instancia), y ver un estimado mensual.
        -   Se pueden guardar, exportar (Excel) y compartir estimaciones.
    -   **Calculadora de Costo Total de Propiedad (TCO Calculator):**
        -   Estima ahorros al migrar de on-premises a Azure.
        -   Se ingresan detalles de cargas de trabajo on-premises (servidores, bases de datos, almacenamiento, red).
        -   Se ajustan suposiciones (costos de TI, software assurance, etc.).
        -   Genera un informe de ahorros a 5 años, comparando costos on-premises vs. Azure.
-   **Azure Cost Management and Billing:**
    -   Herramienta para analizar costos a nivel granular.
    -   Permite crear presupuestos, establecer alertas al acercarse a límites, y analizar costos detalladamente.
    -   Se accede desde el portal de Azure.
-   **Etiquetas (Tags):**
    -   Pares nombre/valor que se aplican a recursos de Azure (incluyendo grupos de recursos).
    -   Facilitan el seguimiento de gastos y la categorización (ej. por departamento, proyecto, evento).
    -   Aparecen en la factura de Azure, permitiendo filtrar y generar informes.
    -   Las etiquetas en un grupo de recursos no se heredan automáticamente a los recursos dentro del grupo.

### Habilidad 3.2: Describir las características y herramientas en Azure para la gobernanza y el cumplimiento

-   **Azure Blueprints:**
    -   Servicio para definir y desplegar entornos en la nube que cumplan con estándares, políticas y mejores prácticas.
    -   Permite configurar un entorno (grupos de recursos, plantillas ARM, asignaciones de políticas, asignaciones de roles) y guardarlo para duplicarlo.
    -   *Artefactos:* Elementos añadidos a un blueprint.
    -   Se guardan en una suscripción o grupo de administración (para uso en suscripciones de la jerarquía).
    -   Son recursos de Azure, versionados, y mantienen una conexión con los recursos desplegados desde ellos. No reemplazan las plantillas ARM, sino que las utilizan.
    -   Proceso: Crear borrador -> Añadir artefactos -> Guardar borrador -> Publicar (con versión) -> Asignar a una suscripción.
-   **Azure Policy:**
    -   Define reglas que se aplican al crear y gestionar recursos de Azure para asegurar el cumplimiento de políticas corporativas (ej. restringir tamaños de VM, regiones, etiquetado).
    -   *Iniciativa:* Agrupación de múltiples políticas.
    -   Se pueden usar políticas integradas o crear definiciones de políticas personalizadas (usando plantillas ARM).
    -   *Efectos de la política:*
        -   *Append:* Añade propiedades (ej. etiquetas).
        -   *Audit:* Registra una advertencia si no se cumple.
        -   *AuditIfNotExists:* Advierte si un recurso dependiente no existe.
        -   *Deny:* Niega la operación de creación/actualización.
        -   *DeployIfNotExists:* Despliega automáticamente un recurso dependiente si no está incluido.
        -   *Disabled:* La política no está en efecto.
-   **Bloqueos de Recursos (Resource Locks):**
    -   Previenen cambios o eliminaciones accidentales de recursos.
    -   Se aplican a todos, independientemente del rol RBAC.
    -   Requiere rol Propietario o Administrador de Acceso de Usuario para crear/eliminar bloqueos.
    -   Niveles de aplicación: recurso, grupo de recursos, suscripción.
    -   Tipos:
        -   *Read-only (Solo lectura):* Más restrictivo. Previene cambios y eliminaciones.
        -   *Delete (Eliminar):* Previene eliminaciones, pero permite cambios.
    -   Solo aplican a operaciones gestionadas por Azure Resource Manager (ARM). Algunas operaciones internas del recurso pueden no verse afectadas.
    -   Los bloqueos se heredan. Si hay bloqueos anidados, el más restrictivo prevalece.
-   **Portal de Confianza del Servicio (Service Trust Portal):**
    -   Portal web (`aka.ms/STP`) con información sobre confianza, seguridad y cumplimiento de Microsoft.
    -   Incluye documentación, informes de auditoría, guías de cumplimiento, FAQs, herramientas.
    -   Enlaces a Compliance Manager, Security & Compliance Center.

### Habilidad 3.3: Describir las características y herramientas para gestionar y desplegar recursos de Azure

-   **Portal de Azure:**
    -   Interfaz web personalizable para crear y gestionar recursos.
    -   Componentes: Home, barra de búsqueda, Cloud Shell, Filtro, Notificaciones, Configuración, menú de servicios, blades (ventanas de contenido), Dashboard (personalizable con tiles).
-   **Azure PowerShell:**
    -   Módulo `Az` para PowerShell para gestionar recursos de Azure desde la línea de comandos.
    -   Multiplataforma (Windows, Linux, macOS con PowerShell Core).
    -   Instalación: `Install-Module -Name Az -AllowClobber [-Scope CurrentUser]`.
    -   Conexión: `Connect-AzAccount` (autenticación vía navegador/código).
    -   Establecer suscripción activa: `Set-AzContext -Subscription "subscription_id"`.
    -   Sintaxis común: Verbo-AzObjeto (ej. `New-AzResourceGroup`, `Remove-AzResourceGroup -Force`).
-   **Interfaz de Línea de Comandos de Azure (Azure CLI):**
    -   Herramienta de línea de comandos para crear y gestionar recursos, ideal para scripting (Python, Ruby, etc.).
    -   Multiplataforma.
    -   Login: `az login` (abre navegador).
    -   Establecer suscripción: `az account set --subscription "subscription_id"`.
    -   Ayuda: `az command --help` (ej. `az resource create --help`).
    -   Modo interactivo: `az interactive` (autocompletado, scoping).
    -   Extensible mediante `az extension add --name extension_name`.
-   **Azure Cloud Shell:**
    -   Entorno de línea de comandos (Bash para CLI, PowerShell para Az module) accesible desde el navegador (portal de Azure) o la app móvil.
    -   Requiere una cuenta de almacenamiento para persistir archivos y configuraciones.
    -   Incluye un editor de código (Monaco Editor) y previsualización web.
    -   Integrado con la documentación de Microsoft (botones "Try It").
-   **Azure Arc:**
    -   Extiende la gestión y gobernanza de Azure a recursos fuera de Azure (on-premises, otras nubes).
    -   *Azure Arc-enabled servers:* Gestionar servidores físicos/VMs externos instalando el agente Azure Connected Machine. Permite RBAC, Policy, tags, Defender for Cloud.
    -   *Azure Arc-enabled Kubernetes:* Gestionar clústeres Kubernetes externos instalando un agente. Usa GitOps. Permite ejecutar servicios de datos de Azure (SQL Managed Instance, PostgreSQL Hyperscale) y servicios de aplicación (App Service, Functions) en estos clústeres.
-   **Azure Resource Manager (ARM) y Plantillas ARM:**
    -   **ARM:** Servicio de gestión y despliegue en Azure. Todas las interacciones con servicios de Azure pasan por ARM.
        -   Autentica y autoriza solicitudes.
        -   Interactúa con *proveedores de recursos* (ej. `Microsoft.Web` para App Service).
        -   Todas las herramientas de gestión (portal, PowerShell, CLI) usan la API de ARM.
        -   Flujo: Herramienta -> API ARM -> ARM (autenticación/autorización) -> Proveedor de Recurso -> Recurso.
    -   **Plantillas ARM (ARM Templates):**
        -   Archivos JSON que usan *sintaxis declarativa* para definir recursos a desplegar o modificar.
        -   Definen el estado deseado, no los pasos para alcanzarlo.
        -   Permiten despliegues predecibles y repetibles.
        -   Soportan dependencias entre recursos (ej. un certificado debe desplegarse antes que la web app que lo usa).
        -   Se pueden exportar plantillas desde recursos existentes en el portal.

### Habilidad 3.4: Describir las herramientas de monitorización en Azure

-   **Azure Advisor:**
    -   Proporciona recomendaciones personalizadas para optimizar recursos de Azure.
    -   Categorías: Alta disponibilidad, Seguridad, Rendimiento, Costo, Excelencia Operativa.
    -   Se pueden ver detalles y, a veces, aplicar correcciones directamente.
    -   Las recomendaciones se pueden descargar (CSV, PDF).
-   **Azure Service Health:**
    -   Proporciona una vista personalizada del estado de los servicios de Azure que afectan tus recursos.
    -   Muestra:
        -   *Problemas de Servicio (Service Issues):* Impactos actuales.
        -   *Mantenimiento Planeado (Planned Maintenance):* Próximos mantenimientos.
        -   *Avisos de Salud (Health Advisories):* Problemas relacionados con la configuración del usuario.
    -   Se puede ver un mapa de estado por región y descargar informes de incidentes.
-   **Azure Monitor:**
    -   Plataforma centralizada para recopilar, analizar y actuar sobre telemetría de entornos Azure y on-premises.
    -   **Métricas (Metrics):** Recopila métricas de servicios de Azure. Se pueden crear gráficos personalizables y anclarlos al dashboard.
    -   **Alertas (Alerts):**
        -   Notifican o ejecutan acciones cuando se cumplen condiciones definidas en reglas.
        -   Condiciones basadas en señales (ej. % CPU de una VM).
        -   *Grupos de Acción (Action Groups):* Definen notificaciones (email, SMS, app Azure) y acciones (ejecutar Logic App, Function App, webhook).
    -   **Application Insights:**
        -   Característica de Azure Monitor para monitorizar aplicaciones web y VMs.
        -   Recopila métricas de rendimiento (tasas de solicitud, tiempos de respuesta, errores, vistas de página, contadores de rendimiento, etc.).
        -   Se puede habilitar al crear web apps o en VMs desde el blade Insights.
    -   **Log Analytics:**
        -   Servicio de Azure Monitor para agregar y consultar datos de logs.
        -   Usa Kusto Query Language (KQL).
        -   Permite ejecutar consultas predefinidas o personalizadas y visualizar resultados (tablas, gráficos).
        -   Los datos de Application Insights se almacenan en un espacio de trabajo de Log Analytics.