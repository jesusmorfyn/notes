## Capítulo 1: Introducción a Kubernetes

### Antecedentes de Kubernetes
*   **¿Qué es Kubernetes?** Es un orquestador de aplicaciones, principalmente para aplicaciones de microservicios nativas de la nube y contenerizadas.
*   **Orquestador:** Sistema que despliega y gestiona aplicaciones. Puede:
    *   Desplegar aplicaciones.
    *   Escalar (hacia arriba/abajo) dinámicamente según la demanda.
    *   Auto-reparar cuando algo falla.
    *   Realizar actualizaciones continuas (rolling updates) y reversiones (rollbacks) sin tiempo de inactividad.
    *   Funciona de forma autónoma una vez configurado.
*   **Aplicación Contenerizada:** App que se ejecuta en un contenedor (evolución de servidores físicos -> máquinas virtuales -> contenedores). Son más rápidas, ligeras y adecuadas para requisitos modernos.
*   **Aplicación Nativa de la Nube (Cloud-Native):** Diseñada para demandas tipo nube: autoescalado, autoreparación, actualizaciones continuas, etc. Puede ejecutarse en cualquier lugar donde haya Kubernetes (nube pública, on-premise). Se trata del *comportamiento* de la app.
*   **Aplicación de Microservicios:** Construida a partir de muchas partes pequeñas, especializadas e independientes que colaboran. Cada parte (microservicio) puede ser desarrollada, desplegada, escalada y actualizada independientemente. Usualmente, cada microservicio corre como uno o más contenedores.
*   **Definición Re-formulada:** Kubernetes despliega y gestiona (orquesta) aplicaciones empaquetadas como contenedores (contenerizadas) y construidas (microservicios nativos de la nube) para escalar, autorepararse y actualizarse según los requisitos modernos tipo nube.
*   **Origen:** Creado por Google basándose en la experiencia con sistemas internos (Borg, Omega) para gestionar contenedores a escala. Donado a la CNCF en 2014 como proyecto open-source. Objetivo: Abstraer la infraestructura subyacente (como AWS) y facilitar la portabilidad de aplicaciones entre nubes.
*   **Kubernetes y Docker:** Docker construye imágenes y ejecuta contenedores. Kubernetes orquesta estos contenedores. K8s usa una Interfaz de Runtime de Contenedor (CRI) para interactuar con runtimes como `containerd` (una versión simplificada de Docker, ahora el default en muchos clusters). Las imágenes Docker (formato OCI) funcionan perfectamente en Kubernetes.
*   **Kubernetes vs Docker Swarm:** Kubernetes ganó la "guerra de orquestadores", aunque Swarm sigue activo y es una alternativa simple.
*   **Kubernetes y Borg/Omega:** Kubernetes comparte "ADN" y lecciones aprendidas con Borg/Omega de Google, pero es un proyecto distinto construido desde cero.
*   **Nombre y Logo:** "Kubernetes" viene del griego "Timonel". El logo es el timón de un barco con 7 radios (referencia a "Seven of Nine" de Star Trek, nombre que no se pudo usar por copyright). Abreviatura común: "K8s".

### Kubernetes como Sistema Operativo de la Nube
*   **Abstracción:** Así como un SO (Linux/Windows) abstrae el hardware del servidor, Kubernetes abstrae los recursos de la nube (computación, red, almacenamiento). No importa si la infraestructura es on-premise o nube pública/privada.
*   **Habilitador de Nube Híbrida/Multi-Nube:** Facilita mover cargas de trabajo entre diferentes infraestructuras de nube sin problemas.
*   **Escala de Nube:** Gestiona la complejidad de miles de contenedores que surgen al migrar de cientos de VMs. Proporciona un framework estándar.
*   **Programación de Aplicaciones:** Kubernetes gestiona dónde se ejecutan los microservicios (Pods), similar a cómo un SO gestiona qué CPU/memoria usa un proceso. Los desarrolladores no necesitan preocuparse por la infraestructura subyacente ("tratar la infraestructura como ganado, no como mascotas").
*   **Analogía del Servicio de Mensajería:** Empaquetas tu app (en un contenedor), le pones una etiqueta (manifiesto K8s), y Kubernetes se encarga de toda la logística (despliegue, salud, etc.).

## Capítulo 2: Principios de Operación de Kubernetes

### Kubernetes desde 40.000 pies
*   **Dos cosas:** Un clúster para ejecutar aplicaciones y un orquestador de microservicios nativos de la nube.
*   **Clúster K8s:** Conjunto de máquinas ("nodos") que alojan aplicaciones. Compuesto por:
    *   **Plano de Control (Control Plane):** Cerebro del clúster. Expone la API, planifica el trabajo (scheduler), mantiene la salud de las apps (autoreparación, escalado, etc.).
    *   **Nodos Trabajadores (Worker Nodes):** Ejecutan las aplicaciones del usuario.
*   **Proceso de Despliegue:**
    1.  Diseñar la app como microservicios.
    2.  Empaquetar cada microservicio en un contenedor.
    3.  Envolver cada contenedor en un Pod de Kubernetes.
    4.  Desplegar Pods usando controladores de nivel superior (Deployments, StatefulSets, etc.).
*   **Modelo Declarativo:** Describes el estado deseado en ficheros YAML (manifiestos), Kubernetes se encarga de alcanzar y mantener ese estado.

### Plano de Control y Nodos Trabajadores
*   **Nodos:** Hosts Linux (VMs, bare metal, instancias cloud, etc.).
*   **Plano de Control:**
    *   Ejecuta los servicios del sistema que gestionan el clúster.
    *   Componentes principales (ejecutados en cada nodo del plano de control):
        *   **API Server:** Punto central de comunicación. Expone una API RESTful (usualmente sobre HTTPS). Valida y procesa peticiones.
        *   **Cluster Store (etcd):** Base de datos distribuida (clave-valor) que almacena la configuración y estado completo del clúster. Esencial y con estado. Requiere alta disponibilidad (HA). Usa Raft para consenso.
        *   **Controller Manager:** Ejecuta los controladores principales (Deployment controller, StatefulSet controller, etc.). Cada controlador es un bucle que observa la API y reconcilia el estado actual con el deseado para su recurso específico.
        *   **Scheduler:** Asigna Pods a nodos trabajadores adecuados basándose en requisitos y disponibilidad. Filtra nodos no aptos (predicados) y puntúa los aptos (prioridades).
        *   **Cloud Controller Manager (opcional):** Interactúa con la API de la nube subyacente (AWS, GCP, Azure) para gestionar recursos como balanceadores de carga o volúmenes.
    *   Se recomienda no ejecutar cargas de usuario en nodos del plano de control. HA con 3 o 5 nodos es vital para producción.
*   **Nodos Trabajadores:**
    *   Ejecutan las cargas de trabajo del usuario (Pods).
    *   Tareas principales: Observar la API por nuevo trabajo, ejecutarlo, reportar estado al plano de control.
    *   Componentes principales (ejecutados en cada nodo trabajador):
        *   **Kubelet:** Agente principal de K8s en el nodo. Registra el nodo en el clúster. Recibe instrucciones del API Server y gestiona los contenedores a través del runtime. Monitoriza Pods.
        *   **Container Runtime:** Software que ejecuta contenedores (e.g., `containerd`, `CRI-O`). Kubelet interactúa con él a través de la CRI (Container Runtime Interface).
        *   **Kube-proxy:** Gestiona la red del nodo. Mantiene reglas de red (iptables/IPVS) para permitir la comunicación hacia los Pods/Services. Asegura que cada nodo tenga una IP única.

### DNS de Kubernetes
*   Cada clúster tiene un servicio DNS interno (basado en CoreDNS).
*   Esencial para el descubrimiento de servicios (Service Discovery).
*   Tiene una IP estática conocida por todos los Pods.
*   El registro de Servicios es automático.

### Empaquetando Apps para Kubernetes
*   Requisitos:
    *   App contenerizada.
    *   Envuelto en un Pod.
    *   Desplegado (usualmente) vía un controlador (e.g., Deployment) definido en un manifiesto YAML.
*   El Deployment añade funcionalidades como escalado, autoreparación y actualizaciones.

### El Modelo Declarativo y el Estado Deseado
*   **Declarativo:** Defines el *qué* (estado deseado) en un manifiesto YAML.
*   **Estado Deseado:** Cómo quieres que sea tu aplicación (imagen, réplicas, puertos, etc.).
*   **Estado Observado:** El estado actual real en el clúster.
*   **Reconciliación:** Los controladores trabajan constantemente para que el estado observado coincida con el estado deseado.
*   **Beneficios:** Simplicidad, auto-reparación, escalado, idempotencia, auto-documentación, control de versiones (GitOps).
*   **Ejemplo:** Si el estado deseado son 10 réplicas y un nodo falla, el controlador detecta que solo hay 8 (estado observado) y crea 2 nuevas para alcanzar las 10 deseadas.

### Pods
*   **Unidad atómica de despliegue y planificación en K8s.** Todo corre dentro de un Pod.
*   Un Pod es un entorno de ejecución para uno o más contenedores *estrechamente acoplados*.
*   Contenedores en un mismo Pod comparten:
    *   Namespace de red (misma IP, rango de puertos).
    *   Volúmenes de almacenamiento.
    *   IPC namespace, memoria compartida, etc.
*   Comunicación inter-contenedor dentro de un Pod: vía `localhost`.
*   **Unidad de escalado:** Se escala añadiendo/quitando Pods, no contenedores a un Pod existente.
*   **Despliegue atómico:** El Pod completo (con todos sus contenedores) se inicia o no se inicia. Un Pod siempre se ejecuta en un *único* nodo.
*   **Ciclo de vida:** Los Pods son mortales/efímeros. Cuando mueren, son reemplazados por nuevos (con nueva IP/ID), no resucitados. Las apps deben ser diseñadas para esto (sin estado en el Pod).
*   **Inmutabilidad:** Una vez creado, un Pod no se modifica. Para cambios/actualizaciones, se reemplaza por uno nuevo.

### Deployments
*   Controlador de K8s de alto nivel, ideal para aplicaciones *sin estado*.
*   Gestiona Pods (indirectamente, a través de ReplicaSets).
*   Añade funcionalidades clave:
    *   Auto-reparación (reemplaza Pods fallidos).
    *   Escalado (cambia el número de réplicas).
    *   Actualizaciones continuas (rolling updates) sin tiempo de inactividad.
    *   Reversiones (rollbacks) a versiones anteriores.

### Objetos Service y Red Estable
*   **Problema:** Los Pods tienen IPs efímeras que cambian con fallos, escalados y actualizaciones.
*   **Solución:** El objeto `Service` proporciona una abstracción de red estable.
*   Características del Service:
    *   IP virtual estable (ClusterIP).
    *   Nombre DNS estable (registrado en el DNS del clúster).
    *   Puerto estable.
    *   Descubre Pods dinámicamente usando selectores de etiquetas (`labels`).
    *   Balancea la carga entre los Pods que coinciden con el selector.
*   Permite que los clientes (otros Pods o externos) se conecten a un punto final fiable sin preocuparse por los Pods individuales.
*   No tienen inteligencia de aplicación (Capa 4 - TCP/UDP). Para enrutamiento HTTP (Capa 7) se usa Ingress.

## Capítulo 3: Obtener Kubernetes

### Crear un Clúster K8s en tu Laptop
*   **Docker Desktop:** Fácil de instalar en Mac/Windows. Proporciona un clúster K8s de un solo nodo y `kubectl`. Bueno para empezar.
*   **K3d:** Herramienta para crear clústeres K8s multi-nodo locales usando K3s (distribución ligera) dentro de contenedores Docker. Requiere Docker instalado. Permite simular entornos más realistas.
*   **KinD (Kubernetes in Docker):** Similar a K3d, crea clústeres K8s multi-nodo usando contenedores Docker. Requiere Docker y `kubectl`. Se configura mediante un fichero YAML.

### Crear un Clúster K8s Alojado en la Nube
*   **Servicios Gestionados:** Proveedores cloud (AWS, Azure, GCP, Linode, etc.) ofrecen K8s como servicio. Gestionan el plano de control, HA, actualizaciones. Simplifican la puesta en marcha de clústeres de producción.
*   **GKE (Google Kubernetes Engine):** Servicio K8s de GCP. Rápido, fácil, "grado producción". Integrado con servicios GCP. Se gestiona vía consola web o CLI `gcloud`. Cuesta dinero.
*   **LKE (Linode Kubernetes Engine):** Servicio K8s de Linode. Plano de control gestionado, integración con volúmenes/balanceadores de Linode. Requiere instalar y configurar `kubectl` manualmente descargando el `kubeconfig`. Cuesta dinero.
*   **¡Advertencia!** Los clústeres en la nube cuestan dinero. ¡Bórralos cuando no los uses!

### Instalar y Trabajar con `kubectl`
*   **`kubectl`:** Herramienta CLI principal para interactuar con K8s.
*   **Compatibilidad de Versión:** La versión de `kubectl` debe estar dentro de +/- 1 versión menor respecto a la del clúster.
*   **Funcionamiento:** Convierte comandos en peticiones REST HTTP (con payload JSON) al API Server. Usa el fichero `kubeconfig` para saber a qué clúster conectar y con qué credenciales autenticarse.
*   **Fichero `kubeconfig`:** (`$HOME/.kube/config`) Contiene definiciones de:
    *   **Clusters:** Lista de clústeres K8s (nombre, endpoint API server, certificado CA).
    *   **Users:** Credenciales de usuario (nombre, token/certificado).
    *   **Contexts:** Agrupa un cluster y un user bajo un nombre amigable. Define el namespace por defecto para ese contexto.
*   **Comandos útiles:**
    *   `kubectl config view`: Muestra la configuración actual (datos sensibles ocultos).
    *   `kubectl config current-context`: Muestra el contexto activo.
    *   `kubectl config use-context <nombre-contexto>`: Cambia el contexto activo.

## Capítulo 4: Trabajando con Pods

### Teoría de Pods
*   Revisión: Unidad atómica de planificación. Contenedores siempre dentro de Pods.
*   **¿Por qué Pods?**
    *   **Aumentan Contenedores:** Añaden metadatos K8s (labels, anotaciones), políticas (restart, seguridad), sondas (probes), afinidad, límites de recursos, etc. Usa `kubectl explain pods` para ver atributos.
    *   **Ayudan en Planificación:** Garantizan co-planificación (contenedores del mismo Pod en el mismo nodo). Reglas de afinidad/anti-afinidad controlan ubicación.
    *   **Permiten Compartir Recursos:** Entorno de ejecución compartido (red, volúmenes, memoria, etc.) para contenedores dentro del Pod.
*   **Pods Estáticos vs Controladores:**
    *   **Estáticos:** Definidos directamente en un manifiesto de Pod. Gestionados solo por el `kubelet` del nodo. Sin HA/escalado/rollouts a nivel de clúster. Usar solo para casos específicos (e.g., componentes del propio K8s).
    *   **Gestionados por Controlador:** Desplegados vía Deployments, StatefulSets, etc. El controlador (en el plano de control) proporciona HA, escalado, rollouts. Es la forma recomendada.
*   **Pods Mono-Contenedor y Multi-Contenedor:**
    *   **Mono-contenedor:** El caso más simple y común.
    *   **Multi-contenedor:** Para contenedores estrechamente acoplados que necesitan compartir recursos (e.g., red, volúmenes). Patrones comunes:
        *   **Sidecar:** Contenedor auxiliar que añade/mejora funcionalidad al contenedor principal (e.g., proxy de service mesh, recolector de logs).
        *   **Adapter:** Contenedor auxiliar que adapta/transforma la salida del contenedor principal a un formato estándar (e.g., logs Nginx -> formato Prometheus).
        *   **Ambassador:** Contenedor auxiliar que actúa como proxy para conectar el contenedor principal con sistemas externos.
        *   **Init Container:** Contenedor especial que se ejecuta *antes* que los contenedores principales. Se usa para tareas de inicialización (e.g., esperar dependencias, preparar datos). Debe completarse con éxito para que los contenedores principales arranquen.
*   **Anatomía del Pod (Revisión):** Namespaces compartidos (net, pid, mnt, etc.). Red compartida (IP única por Pod, comunicación inter-contenedor vía `localhost`). Red de Pods (overlay plana entre nodos).
*   **Despliegue Atómico y Ciclo de Vida (Revisión):** Todo o nada. Estados: Pending, Running, Succeeded, Failed. Políticas de reinicio. Pods de larga vs corta duración.
*   **Inmutabilidad y Escalado (Revisión):** Los Pods no se modifican, se reemplazan. El escalado se hace añadiendo/quitando Pods.

### Práctica con Pods
*   **Manifiesto de Pod (YAML):** Campos clave: `kind: Pod`, `apiVersion: v1`, `metadata` (name, labels), `spec` (`containers` - name, image, ports; `volumes`).
*   **Manifiestos como Documentación ("Empathy as Code"):** Ayudan a entender requisitos de la app, facilitan onboarding, mejoran comunicación dev/ops.
*   **Despliegue:** `kubectl apply -f pod.yml`.
*   **Introspección:**
    *   `kubectl get pods`: Estado básico. Opciones: `-o wide` (más info), `-o yaml` (config completa, incluye `.spec` - deseado y `.status` - observado).
    *   `kubectl describe pods <nombre-pod>`: Resumen detallado, incluye eventos del ciclo de vida.
    *   `kubectl logs <nombre-pod> [-c <nombre-contenedor>]`: Ver logs de stdout/stderr del contenedor.
    *   `kubectl exec <nombre-pod> [-c <nombre-contenedor>] -- <comando>`: Ejecutar un comando dentro del contenedor.
    *   `kubectl exec -it <nombre-pod> [-c <nombre-contenedor>] -- sh` (o `bash`): Obtener una shell interactiva dentro del contenedor.
*   **Hostnames de Pod:** Heredan el nombre del Pod (`metadata.name`).
*   **Inmutabilidad (Prueba):** `kubectl edit pod <nombre-pod>` mostrará que muchos campos no son modificables.
*   **Ejemplo Init Container:** Usar `initContainers` en el spec del Pod. Espera a que una dependencia (e.g., un Service) esté disponible antes de iniciar el contenedor principal.
*   **Ejemplo Sidecar:** Dos contenedores en el mismo Pod compartiendo un volumen (`emptyDir`). Uno (principal, e.g., Nginx) sirve contenido del volumen, el otro (sidecar, e.g., git-sync) actualiza el contenido del volumen desde una fuente externa (e.g., repo Git).

## Capítulo 5: Clústeres Virtuales con Namespaces

### Casos de Uso para Namespaces
*   **Partición Lógica:** Dividen un clúster K8s físico en múltiples clústeres virtuales.
*   **Ámbito de Recursos:** La mayoría de los objetos K8s (Pods, Services, Deployments, PVCs, ConfigMaps, Secrets, Roles, RoleBindings) son *namespaced*. Algunos (Nodes, PVs, StorageClasses, ClusterRoles, ClusterRoleBindings) son *cluster-scoped* (no pertenecen a un namespace). `kubectl api-resources` muestra el ámbito.
*   **Organización:** Útiles para separar entornos (dev, test, prod), equipos o proyectos dentro de una *misma organización de confianza*.
*   **Gestión de Recursos:** Permiten aplicar cuotas (`ResourceQuota`) y rangos de límites (`LimitRange`) por namespace.
*   **Control de Acceso:** Permiten definir permisos RBAC específicos por namespace (Roles, RoleBindings).
*   **¡No son un Límite de Seguridad Fuerte!** Un Pod comprometido en un namespace *puede* afectar a otros. Para aislamiento fuerte (multi-tenant hostil), usar clústeres separados.

### Inspeccionando Namespaces
*   **Namespaces por Defecto:** `default` (para objetos sin namespace especificado), `kube-system` (componentes del sistema K8s), `kube-public` (datos legibles públicamente), `kube-node-lease` (heartbeats de nodos).
*   **Comandos:**
    *   `kubectl get namespaces` (o `ns`): Lista namespaces.
    *   `kubectl describe ns <nombre-ns>`: Detalles de un namespace.
    *   `-n <nombre-ns>` o `--namespace <nombre-ns>`: Filtra comandos `kubectl` por namespace (e.g., `kubectl get pods -n kube-system`).
    *   `--all-namespaces`: Muestra objetos de todos los namespaces.

### Creando y Gestionando Namespaces
*   **Creación Imperativa:** `kubectl create ns <nombre-ns>`.
*   **Creación Declarativa:** Usando un manifiesto YAML (`kind: Namespace`, `apiVersion: v1`, `metadata: { name: <nombre-ns> }`).
*   **Eliminación:** `kubectl delete ns <nombre-ns>`. ¡Cuidado! Elimina el namespace y *todos* los objetos dentro de él.
*   **Configurar `kubectl` para un Namespace Específico:** `kubectl config set-context --current --namespace <nombre-ns>`. Evita tener que usar `-n` constantemente. (Recordar volver a `default` después: `kubectl config set-context --current --namespace default`).

### Desplegando a Namespaces
*   **Método Imperativo:** Usar `-n <nombre-ns>` en los comandos `kubectl create/run`.
*   **Método Declarativo (Preferido):** Especificar `metadata.namespace: <nombre-ns>` en el manifiesto YAML del objeto.
*   **Acceso entre Namespaces:**
    *   Dentro del mismo namespace, los servicios se pueden alcanzar usando solo su nombre corto (e.g., `mi-servicio`).
    *   Para alcanzar un servicio en otro namespace, se debe usar el nombre de dominio completamente calificado (FQDN): `<nombre-servicio>.<nombre-namespace>.svc.cluster.local`.

## Capítulo 6: Kubernetes Deployments

### Teoría de Deployments
*   **Propósito:** Controlador K8s diseñado para gestionar aplicaciones *sin estado*.
*   **Funcionalidades Clave:** Proporciona auto-reparación, escalado, actualizaciones continuas (rollouts) y reversiones (rollbacks) para Pods.
*   **Componentes:**
    *   **Spec (Especificación):** Definida en YAML (`kind: Deployment`, `apiVersion: apps/v1`). Describe el estado deseado (imagen, réplicas, estrategia de actualización, plantilla de Pod).
    *   **Controller (Controlador):** Proceso en el plano de control que ejecuta un bucle de reconciliación para asegurar que el estado observado coincida con el deseado.
*   **Estructura Jerárquica:** `Deployment` -> gestiona -> `ReplicaSet` -> gestiona -> `Pods`.
    *   **ReplicaSet (RS):** Un controlador más simple cuyo único objetivo es asegurar que un número específico de réplicas de un Pod estén ejecutándose. Gestiona la auto-reparación y el escalado básico. Los Deployments crean y gestionan ReplicaSets automáticamente. No se suelen gestionar RS directamente.
*   **Estado Deseado vs Observado (Revisión):** Fundamental para Deployments. El controlador actúa cuando hay discrepancias.
*   **Actualizaciones Continuas (Rollouts / Rolling Updates):**
    *   **Proceso:** Se actualiza la plantilla de Pod en el spec del Deployment (e.g., nueva versión de imagen). K8s crea un *nuevo* ReplicaSet para la nueva versión. Gradualmente, escala el nuevo RS hacia arriba mientras escala el viejo RS hacia abajo, manteniendo la disponibilidad.
    *   **Estrategia (`spec.strategy`):**
        *   `type: RollingUpdate` (default).
        *   `rollingUpdate.maxUnavailable`: Máximo/número de Pods que pueden no estar disponibles durante la actualización.
        *   `rollingUpdate.maxSurge`: Máximo/número de Pods adicionales que pueden crearse por encima del deseado durante la actualización.
    *   Requiere que las aplicaciones sean sin estado y preferiblemente compatibles hacia atrás/adelante.
*   **Reversiones (Rollbacks):**
    *   Los Deployments guardan el historial de ReplicaSets anteriores (`spec.revisionHistoryLimit`).
    *   Se puede revertir a una revisión anterior (`kubectl rollout undo deployment <nombre> --to-revision=<num>`).
    *   Esto activa el ReplicaSet de la revisión elegida, escalándolo hacia arriba mientras el actual se escala hacia abajo (esencialmente, un rollout inverso).
*   **Selectores de Etiquetas (`spec.selector`):** El Deployment usa `matchLabels` para identificar qué Pods (creados a partir de `spec.template.metadata.labels`) debe gestionar. Es inmutable una vez creado.

### Práctica con Deployments
*   **Manifiesto de Deployment (YAML):** Revisar campos clave: `replicas`, `selector`, `template` (contiene la definición del Pod), `strategy`.
*   **Despliegue:** `kubectl apply -f deploy.yml`.
*   **Inspección:** `kubectl get deployments` (o `deploy`), `kubectl describe deploy <nombre>`, `kubectl get rs`, `kubectl describe rs <nombre>`.
*   **Acceso:** Usualmente se accede a los Pods de un Deployment a través de un `Service`.
*   **Escalado:**
    *   Imperativo: `kubectl scale deployment <nombre> --replicas=<num>`. (¡Desincroniza el manifiesto!).
    *   Declarativo (Preferido): Modificar `spec.replicas` en el YAML y re-aplicar con `kubectl apply -f deploy.yml`.
*   **Rollout (Actualización):**
    1.  Modificar la plantilla del Pod en el YAML (e.g., cambiar `spec.template.spec.containers[0].image`).
    2.  `kubectl apply -f deploy.yml`.
    3.  Monitorizar: `kubectl rollout status deployment <nombre>`.
    4.  Pausar/Reanudar: `kubectl rollout pause/resume deployment <nombre>`.
*   **Rollback (Reversión):**
    1.  Ver historial: `kubectl rollout history deployment <nombre>`.
    2.  Revertir: `kubectl rollout undo deployment <nombre> [--to-revision=<num>]`. (¡Comando imperativo! Actualizar el YAML después si se quiere mantener la versión revertida).

## Capítulo 7: Kubernetes Services

### Teoría de Services
*   **Propósito:** Abstraer un conjunto de Pods (que tienen IPs efímeras) detrás de un punto final de red *estable* y *fiable*. Esencial para la comunicación entre microservicios y para exponer aplicaciones.
*   **Características:**
    *   **IP Estable (ClusterIP):** Una IP virtual interna asignada al Service que no cambia.
    *   **Nombre DNS Estable:** El nombre del Service (`metadata.name`) se registra automáticamente en el DNS interno del clúster.
    *   **Puerto Estable:** El Service escucha en un puerto definido.
    *   **Descubrimiento de Pods:** Usa selectores (`spec.selector`) para encontrar dinámicamente los Pods cuyas etiquetas (`metadata.labels`) coinciden.
    *   **Balanceo de Carga:** Distribuye el tráfico entre los Pods encontrados (balanceo L4 - TCP/UDP básico).
*   **EndpointSlices:** Objeto asociado automáticamente a cada Service (con selector). Mantiene la lista actualizada de IPs y puertos de los Pods *sanos* que coinciden con el selector. `kube-proxy` usa esta información para configurar las reglas de red en los nodos. Más escalable que el antiguo objeto `Endpoints`.
*   **Networking Dual-Stack (IPv4/IPv6):** Si el clúster lo soporta, los Services pueden tener IPs IPv4 e IPv6. Configurable vía `spec.ipFamilyPolicy` y `spec.ipFamilies`.
*   **Tipos de Service:**
    *   **`ClusterIP` (Default):** Expone el Service en una IP interna del clúster. Solo accesible desde dentro del clúster. Ideal para comunicación interna entre microservicios.
    *   **`NodePort`:** Expone el Service en un puerto estático (`spec.ports.nodePort`, rango 30000-32767) en la IP de *cada* nodo del clúster. Permite acceso externo vía `<IP-Nodo>:<NodePort>`. Construye sobre `ClusterIP` (sigue teniendo ClusterIP y nombre DNS).
    *   **`LoadBalancer`:** Expone el Service externamente usando el balanceador de carga del proveedor cloud. Automáticamente crea y configura un balanceador de carga en la nube (AWS ELB, GCP LB, Azure LB) que apunta a los `NodePort`s del Service. Es la forma más común de exponer servicios en la nube. Proporciona una IP/DNS pública. Construye sobre `NodePort` y `ClusterIP`.
    *   **`ExternalName`:** Mapea el Service a un nombre DNS externo (usando un registro CNAME). No actúa como proxy ni balanceador.

### Práctica con Services
*   **Networking Dual-Stack (Práctica):** Requiere clúster configurado. `ipFamilyPolicy: PreferDualStack` o `RequireDualStack`. `ipFamilies: [IPv4, IPv6]` (o al revés para cambiar IP primaria). `kubectl get svc` muestra IP primaria, `describe` muestra ambas.
*   **Creación Imperativa (No Recomendada):** `kubectl expose deployment <nombre-deployment> --type=<tipo> --port=<puerto-svc> --target-port=<puerto-pod>`.
*   **Creación Declarativa (Preferida):** Usando manifiesto YAML (`kind: Service`, `apiVersion: v1`). Campos clave: `metadata.name`, `spec.type`, `spec.ports` (port, targetPort, nodePort, protocol), `spec.selector`.
*   **Inspección:** `kubectl get svc`, `kubectl describe svc <nombre-svc>`. Ver `Endpoints` o `EndpointSlices` (`kubectl get endpointslices -l kubernetes.io/service-name=<nombre-svc>`).
*   **Acceso:**
    *   `ClusterIP`: Desde otro Pod en el clúster, usando el nombre DNS del Service (e.g., `curl http://mi-servicio:8080`).
    *   `NodePort`: Desde fuera del clúster, usando `<IP-Nodo>:<NodePort>`.
    *   `LoadBalancer`: Desde fuera del clúster, usando la IP o DNS pública asignada por el proveedor cloud (ver `EXTERNAL-IP` en `kubectl get svc`).

## Capítulo 8: Ingress

### Preparando el Escenario para Ingress
*   **Problema:** Exponer múltiples servicios web (HTTP/S) externamente.
    *   `NodePort` es incómodo (puertos altos, requiere IP del nodo).
    *   `LoadBalancer` funciona bien, pero crea un balanceador de carga cloud (costoso) por *cada* Service. Límites de LBs por cuenta/región.
*   **Solución Ingress:** Permite exponer *múltiples* Services a través de un *único* punto de entrada (usualmente un único Service `LoadBalancer` gestionado por el Ingress Controller). Actúa como un proxy inverso inteligente (Capa 7).

### Arquitectura de Ingress
*   **Componentes Principales:**
    *   **Ingress Resource:** Un objeto K8s (`kind: Ingress`, `apiVersion: networking.k8s.io/v1`) que define *reglas* de enrutamiento basadas en host (nombre de dominio) y/o path (ruta URL).
    *   **Ingress Controller:** Un Pod/Deployment (que *no* viene instalado por defecto en la mayoría de los clústeres K8s) que lee los Ingress Resources y configura un proxy inverso (e.g., Nginx, HAProxy, Traefik) para implementar esas reglas. Usualmente se expone a través de un Service `LoadBalancer` o `NodePort`.
*   **Funcionamiento:** Tráfico externo llega al IP del Ingress Controller (vía LB) -> El Controller inspecciona la petición HTTP (host/path) -> Aplica las reglas definidas en los Ingress Resources -> Reenvía el tráfico al `Service` (ClusterIP) apropiado -> El Service lo envía a un Pod backend.
*   **Ingress Class:** Objeto K8s (`kind: IngressClass`) que permite tener múltiples Ingress Controllers en un clúster. Se asocia un controller a una clase, y los Ingress Resources especifican qué `ingressClassName` deben usar.
*   **Enrutamiento L7:** Basado en:
    *   **Host:** `Host: api.example.com` -> `svc-api`, `Host: web.example.com` -> `svc-web`.
    *   **Path:** `Host: example.com`, `Path: /api/*` -> `svc-api`, `Path: /*` -> `svc-web`.

### Práctica con Ingress
*   **Requisitos:** Clúster K8s (preferiblemente en cloud con soporte LB), `kubectl`, ficheros de ejemplo.
*   **Instalar un Ingress Controller:** Ejemplo con Nginx Ingress Controller (aplicando un YAML desde el repo oficial de K8s). Crea un Namespace (`ingress-nginx`), Deployment, Service (`LoadBalancer` o `NodePort`), RBAC, etc. Verificar que el Pod del controller esté `Running`.
*   **Ingress Classes:** Verificar la clase creada (`kubectl get ingressclass`). Si hay múltiples controllers, se usa `spec.ingressClassName` en el Ingress resource.
*   **Desplegar Aplicaciones Backend:** Crear Deployments y Services de tipo `ClusterIP` para las apps que se quieren exponer.
*   **Crear el Ingress Resource (YAML):**
    *   `metadata.name`, `metadata.annotations` (para config específica del controller, e.g., `nginx.ingress.kubernetes.io/rewrite-target`).
    *   `spec.ingressClassName` (si aplica).
    *   `spec.rules`: Lista de reglas. Cada regla puede tener `host` y/o `http.paths`.
    *   `http.paths`: Lista de paths. Cada path tiene `path`, `pathType` (`Prefix`, `Exact`, `ImplementationSpecific`), y `backend.service` (con `name` y `port.number` del Service ClusterIP destino).
*   **Desplegar Ingress:** `kubectl apply -f ingress.yml`.
*   **Inspeccionar:** `kubectl get ingress` (o `ing`), `kubectl describe ing <nombre-ing>`. Anotar la `ADDRESS` (IP/DNS del LB externo).
*   **Configurar DNS:** Apuntar los nombres de host usados en las reglas Ingress a la `ADDRESS` del Ingress Controller/LB. Para pruebas locales, se puede modificar el fichero `/etc/hosts` (Linux/Mac) o `C:\Windows\System32\drivers\etc\hosts` (Windows).
*   **Probar:** Acceder desde un navegador usando los hosts/paths definidos en las reglas. Verificar que el tráfico llega a las aplicaciones correctas.

## Capítulo 9: Profundización en Service Discovery

### Antecedentes Rápidos
*   **Problema:** En un entorno dinámico como K8s (Pods que aparecen/desaparecen), ¿cómo encuentran las aplicaciones los servicios con los que necesitan comunicarse?
*   **Solución:** Service Discovery (Descubrimiento de Servicios).

### Registro de Servicios (Service Registration)
*   **Proceso:** Hacer que un servicio sea localizable por otros.
*   **En Kubernetes:**
    *   El DNS interno del clúster (`kube-dns`, basado en CoreDNS) actúa como el **registro de servicios**.
    *   El registro es **automático**. Cuando se crea un objeto `Service` en K8s:
        1.  Se le asigna una `ClusterIP` estable.
        2.  Se crean `EndpointSlices` que rastrean las IPs de los Pods sanos que coinciden con el `selector` del Service.
        3.  El nombre del Service (`metadata.name`) y su `ClusterIP` se registran **automáticamente** en el DNS del clúster.
*   Las aplicaciones *no* necesitan registrarse ellas mismas; el sistema lo hace por ellas a través del objeto `Service`.

### Descubrimiento de Servicios (Service Discovery)
*   **Proceso:** Encontrar la ubicación (IP/puerto) de un servicio registrado.
*   **En Kubernetes:**
    1.  **Configuración Automática:** Cada contenedor obtiene un fichero `/etc/resolv.conf` configurado para usar el `kube-dns` del clúster como su servidor DNS. Este fichero también incluye dominios de búsqueda (`search domains`) para resolver nombres cortos.
    2.  **Aplicación Conoce el Nombre:** El desarrollador codifica la aplicación para que conozca el *nombre* del `Service` al que quiere conectarse (e.g., `mi-backend-svc`).
    3.  **Resolución DNS:** La aplicación intenta conectar usando el nombre del Service. La consulta DNS va al `kube-dns`.
    4.  **DNS Responde:** `kube-dns` devuelve la `ClusterIP` del Service solicitado.
    5.  **Enrutamiento:** La aplicación envía tráfico a la `ClusterIP`. Como esta IP es virtual y no está directamente enrutada, el tráfico pasa por el `kube-proxy` del nodo, que usa reglas (iptables/IPVS) basadas en `EndpointSlices` para redirigir el tráfico a una de las IPs *reales* de los Pods backend sanos.

### Service Discovery y Namespaces
*   **Dominio del Clúster:** Usualmente `cluster.local`.
*   **FQDN (Fully Qualified Domain Name) de un Service:** `<nombre-servicio>.<namespace>.svc.cluster.local`.
*   **Resolución dentro del Namespace:** Gracias a los `search domains` en `/etc/resolv.conf` (que incluye `<namespace>.svc.cluster.local`), se pueden usar nombres cortos (e.g., `mi-servicio`) para conectar a Services *dentro del mismo Namespace*.
*   **Resolución entre Namespaces:** Para conectar a un Service en un Namespace *diferente*, se debe usar su FQDN (e.g., `otro-servicio.otro-ns.svc.cluster.local`).

### Troubleshooting Service Discovery
*   **Verificar Componentes del DNS:**
    *   Pods de CoreDNS: `kubectl get pods -n kube-system -l k8s-app=kube-dns` (deben estar `Running`).
    *   Deployment de CoreDNS: `kubectl get deploy -n kube-system -l k8s-app=kube-dns` (debe estar `Available`).
    *   Service `kube-dns`: `kubectl get svc -n kube-system -l k8s-app=kube-dns` (debe tener `ClusterIP` y puerto 53).
    *   EndpointSlices de `kube-dns`: `kubectl get endpointslices -n kube-system -l k8s-app=kube-dns` (deben apuntar a las IPs de los Pods CoreDNS).
    *   Logs de Pods CoreDNS: `kubectl logs <pod-coredns> -n kube-system`.
*   **Verificar Configuración del Cliente (Pod):**
    *   `kubectl exec <pod> -- cat /etc/resolv.conf` (verificar `nameserver` y `search domains`).
*   **Probar Resolución desde un Pod:**
    *   Usar un Pod de debug (e.g., `dnsutils`): `kubectl run -it dnsutils --image=k8s.gcr.io/e2e-test-images/dnsutils:1.3 -- sh`.
    *   Dentro del Pod, usar `nslookup <nombre-servicio>` o `nslookup <servicio>.<namespace>`.
    *   Probar con `nslookup kubernetes.default` (debería resolver siempre).
    *   Probar `curl` a un servicio por nombre.
*   **Si falla la resolución:** Considerar reiniciar los Pods de CoreDNS (`kubectl delete pod -n kube-system -l k8s-app=kube-dns`). Verificar Network Policies que puedan bloquear el tráfico DNS (puerto 53 UDP/TCP).

## Capítulo 10: Almacenamiento en Kubernetes

### Panorama General
*   **Necesidad:** Aplicaciones stateful necesitan persistir datos más allá del ciclo de vida de un Pod.
*   **Volúmenes K8s:** Abstracción para exponer almacenamiento (block, file, object) de diversas fuentes (cloud, on-prem) a los Pods.
*   **Componentes:**
    *   **Proveedores de Almacenamiento:** Sistemas externos (AWS EBS, GCE PD, Azure Disk, NFS, Ceph, NetApp, EMC, etc.).
    *   **Plugins (CSI):** Interfaz estándar (Container Storage Interface) para que K8s interactúe con los proveedores. Permite desarrollo "out-of-tree".
    *   **Subsistema de Volúmenes Persistentes K8s:** Objetos API para gestionar el almacenamiento: `PersistentVolume` (PV), `PersistentVolumeClaim` (PVC), `StorageClass` (SC).

### Proveedores de Almacenamiento y CSI
*   Cada proveedor necesita un driver CSI para integrarse con K8s.
*   El driver CSI se encarga de operaciones como crear, adjuntar, montar y eliminar volúmenes.
*   El usuario interactúa principalmente con los objetos K8s (PV, PVC, SC), no directamente con el CSI.

### Subsistema de Volúmenes Persistentes K8s
*   **PersistentVolume (PV):** Representa una pieza de almacenamiento *físico* (o virtual) en el clúster. Tiene atributos como capacidad, modos de acceso, StorageClass, política de recuperación. Es un recurso *cluster-scoped*. Puede ser provisionado estática o dinámicamente.
*   **PersistentVolumeClaim (PVC):** Es una *solicitud* de almacenamiento hecha por un usuario/Pod. Especifica requisitos (tamaño, modos de acceso, opcionalmente StorageClass). K8s busca un PV que satisfaga la solicitud (o lo crea dinámicamente si se usa una SC). Es un recurso *namespaced*. Actúa como un "puntero" al PV.
*   **StorageClass (SC):** Describe una "clase" de almacenamiento ofrecida (e.g., "ssd-rápido", "hdd-lento"). Define qué *provisioner* (driver CSI) usar y qué `parameters` pasarle. Habilita el **provisionamiento dinámico**. Es un recurso *cluster-scoped*.

### Provisionamiento Dinámico con Storage Classes
*   **Flujo:**
    1.  Admin crea una o más `StorageClass`es.
    2.  Usuario crea una `PersistentVolumeClaim` especificando una `storageClassName`.
    3.  El controlador de K8s ve la PVC y la SC asociada.
    4.  Invoca al `provisioner` definido en la SC para crear el volumen físico en el backend.
    5.  El provisioner crea el volumen y también crea un objeto `PersistentVolume` (PV) en K8s que lo representa.
    6.  K8s automáticamente **vincula (bind)** la PVC al nuevo PV.
    7.  El Pod puede ahora usar la PVC para montar el volumen.
*   **Beneficios:** Abstrae los detalles del backend, automatiza la creación de almacenamiento, permite autoservicio a los usuarios.
*   **Configuración de StorageClass (YAML):**
    *   `provisioner`: Nombre del driver CSI.
    *   `parameters`: Opciones específicas del provisioner (e.g., tipo de disco, IOPS, zona).
    *   `reclaimPolicy`: Qué hacer con el PV (y el volumen físico) cuando se borra la PVC.
        *   `Delete` (default para dinámico): Borra PV y volumen físico. ¡Pérdida de datos!
        *   `Retain`: Mantiene PV y volumen físico. Requiere limpieza manual.
    *   `volumeBindingMode`: Cuándo vincular y provisionar.
        *   `Immediate`: Tan pronto como se crea la PVC.
        *   `WaitForFirstConsumer`: Espera a que un Pod use la PVC (asegura que el volumen se cree en la misma zona/nodo que el Pod). Recomendado.
    *   `allowVolumeExpansion`: Permite redimensionar el volumen después de creado.
*   **Modos de Acceso (Access Modes):** Definen cómo se puede montar el volumen.
    *   `ReadWriteOnce` (RWO): Montable como lectura/escritura por un *único* nodo. Típico para block storage (EBS, GCE PD).
    *   `ReadOnlyMany` (ROX): Montable como solo lectura por *múltiples* nodos.
    *   `ReadWriteMany` (RWX): Montable como lectura/escritura por *múltiples* nodos. Típico para file storage (NFS, EFS, Azure File).
    *   Un PV solo puede estar vinculado en un modo a la vez. La PVC y el Pod deben solicitar un modo compatible con el PV.

### Práctica con Almacenamiento (Ejemplo GKE)
*   **Usar una StorageClass Existente:**
    1.  Listar SCs: `kubectl get sc`. Identificar una (e.g., `standard-rwo`, `premium-rwo`).
    2.  Crear una PVC (YAML) referenciando la SC: `kind: PersistentVolumeClaim`, `spec.storageClassName`, `spec.resources.requests.storage`, `spec.accessModes`.
    3.  `kubectl apply -f pvc.yml`.
    4.  Verificar estado PVC (`Pending` si `WaitForFirstConsumer`). `kubectl get pvc`. Verificar PV (`kubectl get pv`).
    5.  Crear un Pod (YAML) que use la PVC: `spec.volumes` (referencia a PVC por `persistentVolumeClaim.claimName`), `spec.containers.volumeMounts` (monta el volumen en un path).
    6.  `kubectl apply -f pod.yml`.
    7.  Verificar estado PVC (`Bound`), verificar PV (`Bound`). Verificar Pod (`Running`).
    8.  Limpieza: `kubectl delete pod <nombre>`, `kubectl delete pvc <nombre>`. Observar si el PV se borra (según `reclaimPolicy`).
*   **Crear y Usar una Nueva StorageClass:**
    1.  Crear SC (YAML): `kind: StorageClass`, `provisioner`, `parameters`, `reclaimPolicy`, etc.
    2.  `kubectl apply -f sc.yml`. Verificar con `kubectl get sc`.
    3.  Crear PVC referenciando la nueva SC.
    4.  Crear Pod usando la nueva PVC.
    5.  Verificar todo.
    6.  Limpieza: Borrar Pod, PVC. Borrar PV manualmente si `reclaimPolicy: Retain`. Borrar SC (`kubectl delete sc <nombre>`). ¡Borrar almacenamiento físico en el backend si es necesario!

## Capítulo 11: ConfigMaps y Secrets

### Panorama General
*   **Problema:** Acoplamiento de configuración y código de aplicación en imágenes de contenedor es un anti-patrón. Dificulta reutilización, actualizaciones, gestión de entornos (dev/test/prod).
*   **Solución K8s:** Decoupling. Empaquetar solo la aplicación en la imagen. Almacenar la configuración externamente e inyectarla en tiempo de ejecución.
*   **Objetos K8s:**
    *   **ConfigMap (CM):** Para datos de configuración *no sensibles* (variables de entorno, ficheros de config, argumentos).
    *   **Secret:** Para datos de configuración *sensibles* (passwords, tokens API, certificados TLS).

### Teoría de ConfigMap
*   **Qué es:** Objeto K8s (API core v1) que almacena datos de configuración como pares clave-valor.
*   **Estructura:** Esencialmente un mapa (`data` field en YAML). Cada entrada tiene una `clave` (string) y un `valor` (string, puede ser multilínea).
*   **Creación:**
    *   Imperativa: `kubectl create configmap <nombre> --from-literal=<clave>=<valor> --from-file=<path-fichero>`. Si es desde fichero, la clave por defecto es el nombre del fichero.
    *   Declarativa (Preferida): YAML (`kind: ConfigMap`, `apiVersion: v1`, `metadata: { name: <nombre> }`, `data: { clave1: valor1, clave2: valor2 }`). Usar `|` para valores multilínea literales.
*   **Inyección en Pods/Contenedores:**
    *   **Variables de Entorno:** Mapear claves de CM a variables de entorno en el `spec.containers.env` del Pod. (`valueFrom.configMapKeyRef`).
    *   **Argumentos de Comando:** Usar variables de entorno (inyectadas desde CM) en `spec.containers.command` o `args`.
    *   **Ficheros en un Volumen:** Montar el CM como un volumen (`spec.volumes` de tipo `configMap`). Cada clave del CM se convierte en un fichero dentro del directorio de montaje (`spec.containers.volumeMounts`).

### Práctica con ConfigMaps
*   Crear CMs imperativa y declarativamente.
*   Inspeccionar CMs: `kubectl get configmaps` (o `cm`), `kubectl describe cm <nombre>`, `kubectl get cm <nombre> -o yaml`.
*   Inyectar como variables de entorno: Definir `env` en el spec del contenedor usando `valueFrom.configMapKeyRef`. Verificar con `kubectl exec <pod> -- env`. **Importante:** Las variables de entorno inyectadas de esta forma son **estáticas**; si el CM cambia, la variable en el Pod en ejecución *no* se actualiza.
*   Inyectar como argumentos: Similar a env vars, usar `$(VAR_NAME)` en `command`/`args`. También estático.
*   Inyectar como volumen:
    1.  Definir un `volume` en `spec.volumes` de tipo `configMap` referenciando el CM por nombre.
    2.  Montar ese volumen en un `mountPath` dentro de `spec.containers.volumeMounts`.
    3.  Verificar los ficheros creados: `kubectl exec <pod> -- ls <mountPath>`.
    4.  **Importante:** Los ficheros montados desde un CM **se actualizan dinámicamente** (con un pequeño retraso) si el ConfigMap original cambia. Esta es la forma más flexible y recomendada. Probar editando el CM (`kubectl edit cm <nombre>`) y verificando el cambio dentro del Pod.

### Práctica con Secrets
*   **Similitud con ConfigMaps:** Almacenan datos clave-valor, se inyectan de forma similar.
*   **Diferencia Clave:** Diseñados para datos sensibles.
*   **Seguridad (o falta de):**
    *   Por defecto, los datos en etcd solo están **codificados en base64**, no cifrados. ¡Base64 es trivial de decodificar!
    *   Se pueden habilitar mecanismos de cifrado en reposo (EncryptionConfiguration).
    *   Se transfieren a los nodos (potencialmente sin cifrar en tránsito, a menos que se use mTLS/service mesh).
    *   Se montan en los Pods como **texto plano** (en `tmpfs`, memoria RAM, no disco) para que las apps los usen fácilmente.
    *   Considerar herramientas externas (e.g., HashiCorp Vault) para una gestión de secretos más robusta.
*   **Creación:**
    *   Imperativa: `kubectl create secret generic <nombre> --from-literal=<clave>=<valor>`. Los valores se codifican automáticamente a base64.
    *   Declarativa (YAML): `kind: Secret`, `apiVersion: v1`. Usar `data` para valores ya codificados en base64. Usar `stringData` para proporcionar valores en texto plano en el YAML (K8s los codificará a base64 al guardar). `type: Opaque` es el tipo genérico.
*   **Inspección:** `kubectl get secrets`, `kubectl describe secret <nombre>`. `kubectl get secret <nombre> -o yaml` (muestra datos en base64). Para decodificar: `echo "<valor-base64>" | base64 -d`.
*   **Uso en Pods (Volumen - Preferido):**
    1.  Definir `volume` en `spec.volumes` de tipo `secret` referenciando el Secret por nombre (`secretName`).
    2.  Montar ese volumen en `spec.containers.volumeMounts`. Cada clave del Secret se convierte en un fichero (con el valor decodificado) en el `mountPath`.
    3.  Por defecto, los volúmenes Secret se montan como **solo lectura**.
    4.  Los ficheros se actualizan dinámicamente si el Secret cambia.
*   También se pueden inyectar como variables de entorno (estático, menos seguro al exponer en el entorno).

## Capítulo 12: StatefulSets

### Teoría de StatefulSets
*   **Propósito:** Controlador K8s diseñado para gestionar aplicaciones *con estado* (stateful), como bases de datos, colas de mensajes, o cualquier app que necesite una identidad de red estable y/o almacenamiento persistente único por instancia.
*   **Comparación con Deployments:** Ambos gestionan Pods, ofrecen escalado, auto-reparación y actualizaciones.
*   **Garantías Únicas de StatefulSet (Identidad "Pegajosa" - Sticky ID):**
    *   **Nombres de Pod Predecibles y Persistentes:** Formato `<nombre-statefulset>-<ordinal>`, donde `<ordinal>` es un índice entero basado en 0 (0, 1, 2...). El nombre se mantiene incluso si el Pod es recreado.
    *   **Nombres de Host DNS Estables y Predecibles:** A través de un **Headless Service** asociado, cada Pod obtiene una entrada DNS única: `<nombre-pod>.<nombre-headless-service>.<namespace>.svc.cluster.local`. Permite descubrimiento entre peers.
    *   **Vinculación de Volumen Persistente y Estable:** Cada Pod ordinal obtiene su propio `PersistentVolumeClaim` (PVC) único, creado a partir de `volumeClaimTemplates`. Este PVC se re-adjunta al Pod con el mismo ordinal si es recreado, incluso en otro nodo.
*   **Operaciones Ordenadas y Controladas:**
    *   **Creación/Escalado Ascendente:** Los Pods se crean **uno a uno**, en orden de su ordinal (0, 1, 2...). El siguiente Pod no se crea hasta que el anterior esté `Running` y `Ready`.
    *   **Eliminación/Escalado Descendente:** Los Pods se eliminan **uno a uno**, en orden **inverso** a su ordinal (N, N-1, ... 0). El siguiente Pod no se elimina hasta que el anterior haya terminado completamente. Esto es crucial para aplicaciones clusterizadas que necesitan quorum.
    *   Se puede cambiar a modo `Parallel` con `spec.podManagementPolicy` (pero mantiene nombres/volúmenes estables).
*   **Headless Service:** Un `Service` normal con `spec.clusterIP: None`. Se especifica en `spec.serviceName` del StatefulSet. No tiene IP propia, pero su propósito es crear los registros DNS (A y SRV) para cada Pod del StatefulSet, permitiendo el descubrimiento directo de peers.
*   **Volume Claim Templates (`spec.volumeClaimTemplates`):** Sección en el YAML del StatefulSet que define una plantilla para las PVCs. K8s crea automáticamente una PVC única por cada Pod, nombrandola `<nombre-volumeClaimTemplate>-<nombre-pod>`. Esta PVC persiste si el Pod muere y se re-adjunta al reemplazo.
*   **Manejo de Fallos:** El controlador reemplaza Pods fallidos con uno nuevo que tiene la misma identidad (nombre, hostname, volumen). ¡Precaución con fallos de nodo! K8s es conservador para evitar "cerebro dividido" (split-brain), a menudo requiere intervención manual para forzar la eliminación de un Pod en un nodo no contactable.

### Práctica con StatefulSets (Ejemplo GKE)
*   **Requisitos:** Clúster K8s (ejemplo usa GKE para SC), `kubectl`, ficheros.
*   **Pasos:**
    1.  **Crear StorageClass (si es necesario):** Definir una SC que soporte provisionamiento dinámico (YAML, `kind: StorageClass`). `kubectl apply -f sc.yml`.
    2.  **Crear Headless Service:** Definir un Service (YAML, `kind: Service`) con `spec.clusterIP: None` y un `selector` que coincida con las etiquetas de los Pods del StatefulSet. `kubectl apply -f headless-svc.yml`.
    3.  **Crear StatefulSet (YAML):**
        *   `kind: StatefulSet`, `apiVersion: apps/v1`.
        *   `metadata.name`.
        *   `spec.replicas`.
        *   `spec.selector` (para encontrar sus Pods).
        *   `spec.serviceName` (nombre del Headless Service).
        *   `spec.template` (definición del Pod, incluyendo `metadata.labels` que coincidan con el selector).
        *   `spec.volumeClaimTemplates`: Lista de plantillas de PVC. Cada una tiene `metadata.name` (para el volumen en el Pod) y `spec` (accessModes, storageClassName, resources.requests.storage).
    4.  **Desplegar:** `kubectl apply -f sts.yml`.
*   **Verificar Creación Ordenada:** `kubectl get pods --watch`. Observar cómo se crean `sts-0`, `sts-1`, `sts-2` secuencialmente.
*   **Verificar PVCs:** `kubectl get pvc`. Observar los nombres (`<vct-name>-<pod-name>`) y que están `Bound`.
*   **Probar Descubrimiento DNS:** Usar un Pod de debug (`kubectl exec -it <pod-debug> -- bash`). Ejecutar `dig SRV <headless-svc-name>.<namespace>.svc.cluster.local`. Debería devolver los FQDNs de los Pods del StatefulSet.
*   **Probar Escalado:**
    *   **Descendente:** Editar YAML (`replicas: 2`), `kubectl apply`. Verificar que se borra el Pod de mayor ordinal (`sts-2`). Verificar que *todas* las PVCs *permanecen*.
    *   **Ascendente:** Editar YAML (`replicas: 3`), `kubectl apply`. Verificar que se crea `sts-2`. `kubectl describe pod sts-2` para ver que usa la PVC `webroot-sts-2` existente.
*   **Probar Fallo de Pod:** `kubectl delete pod sts-0`. Observar (`kubectl get pods --watch`) cómo se recrea `sts-0`. Verificar (`describe pod`) que se re-adjunta a la PVC `webroot-sts-0`.
*   **Eliminación:** Si el orden importa, escalar a 0 réplicas primero (`kubectl scale sts <nombre> --replicas=0`). Luego borrar el StatefulSet (`kubectl delete sts <nombre>` o `kubectl delete -f sts.yml`). ¡Las PVCs (y el almacenamiento físico) **no** se borran por defecto al borrar el StatefulSet! Hay que borrarlas manualmente (`kubectl delete pvc -l <etiqueta-del-sts>`). Borrar también el Headless Service y la StorageClass si no se necesitan más.

## Capítulo 13: Seguridad de la API y RBAC

### Panorama General de la Seguridad API
*   **Todo pasa por la API Server:** `kubectl`, componentes del clúster (kubelet, controllers), Pods (vía ServiceAccounts).
*   **Flujo de Petición:** Cliente -> TLS -> **API Server** -> [ 1. Autenticación ] -> [ 2. Autorización ] -> [ 3. Control de Admisión ] -> Persistencia (etcd) / Ejecución.
*   **Analogía del Aeropuerto:** Check-in (AuthN), Tarjeta de embarque (AuthZ), Controles de seguridad (Admission Control).

### Autenticación (AuthN)
*   **Propósito:** Verificar *quién* está haciendo la petición. ¿Es quien dice ser?
*   **Funcionamiento en K8s:**
    *   Es **pluggable**. K8s **no tiene una base de datos de usuarios interna**.
    *   Métodos comunes:
        *   **Certificados de Cliente (X.509):** Usado a menudo para usuarios (`kubectl`) y componentes del clúster. El certificado debe estar firmado por una CA de confianza para el API Server. El `Subject` del cert contiene el `CN` (username) y `O` (groups).
        *   **Tokens (Bearer Tokens):** Tokens estáticos, Service Account Tokens (JWTs montados en Pods), OpenID Connect (OIDC) Tokens (para integrar con IdPs externos como Google, Auth0).
        *   **Webhook:** Delega la verificación a un servicio externo.
        *   **Proxy de Autenticación.**
    *   **Fichero `kubeconfig`:** Almacena credenciales del cliente (certs/tokens) para `kubectl`.
    *   **ServiceAccounts:** Identidad para procesos *dentro* de los Pods que necesitan hablar con la API. K8s crea un token SA y lo monta automáticamente como un Secret en el Pod (se puede deshabilitar con `automountServiceAccountToken: false`).

### Autorización (AuthZ) - RBAC
*   **Propósito:** Verificar *si* la identidad autenticada tiene permiso para realizar la acción solicitada sobre el recurso solicitado. ¿Puede `usuarioX` *crear* `Pods` en el `namespaceY`?
*   **Funcionamiento en K8s:**
    *   También es **pluggable**. Múltiples módulos pueden activarse (Node, RBAC, Webhook, ABAC - obsoleto). Si *cualquiera* autoriza, la petición pasa.
    *   **RBAC (Role-Based Access Control):** El módulo más común y recomendado. Estable/GA desde K8s 1.8.
    *   **Principios RBAC:**
        *   **Deny-by-default:** Todo está prohibido a menos que se permita explícitamente.
        *   **Allow rules only:** Solo se definen reglas que *permiten* acciones. No hay reglas de denegación explícita. Simplifica la gestión.
*   **Objetos RBAC K8s:**
    *   **`Role`:** Conjunto de permisos (reglas) sobre recursos *dentro de un Namespace específico*. **Namespaced**.
    *   **`ClusterRole`:** Conjunto de permisos (reglas) sobre:
        *   Recursos *cluster-scoped* (e.g., Nodes, PVs).
        *   Recursos *namespaced* pero aplicados a *todos* los namespaces.
        *   URLs no relacionadas con recursos (e.g., `/healthz`).
        *   **Cluster-scoped**.
    *   **`RoleBinding`:** Vincula (asigna) un `Role` o `ClusterRole` a un `Subject` (User, Group, ServiceAccount) *dentro de un Namespace específico*. **Namespaced**.
    *   **`ClusterRoleBinding`:** Vincula (asigna) un `ClusterRole` a un `Subject` a nivel de *todo el clúster*. **Cluster-scoped**.
*   **Estructura de Reglas (en Role/ClusterRole):**
    *   `apiGroups`: A qué grupo API pertenece el recurso (e.g., `""` para core, `"apps"`, `"networking.k8s.io"`). `"*"` para todos.
    *   `resources`: Qué tipo de recurso (e.g., `"pods"`, `"deployments"`, `"services"`). `"*"` para todos.
    *   `verbs`: Qué acciones se permiten (e.g., `"get"`, `"list"`, `"watch"`, `"create"`, `"update"`, `"patch"`, `"delete"`). `"*"` para todos.
*   **Roles y Bindings Predefinidos:** K8s viene con algunos ClusterRoles útiles (`cluster-admin`, `admin`, `edit`, `view`). A menudo, la configuración inicial de `kubectl` usa un usuario vinculado a `cluster-admin`.

### Control de Admisión (Admission Control)
*   **Propósito:** Último paso antes de persistir un objeto (para peticiones de escritura: create, update, delete). Aplica políticas de gobernanza, seguridad o configuración por defecto.
*   **Funcionamiento:** Cadena de "Admission Controllers" que interceptan la petición *después* de AuthN/AuthZ.
*   **Tipos de Controllers:**
    *   **Mutating:** Pueden *modificar* el objeto de la petición (e.g., añadir labels por defecto, inyectar sidecars, establecer `imagePullPolicy`). Se ejecutan primero.
    *   **Validating:** Solo pueden *validar* el objeto. Si la validación falla, rechazan la petición. No pueden modificarla. Se ejecutan después de los mutating.
*   **Si *cualquier* controller rechaza la petición, esta falla.**
*   **Ejemplos:** `NamespaceLifecycle`, `LimitRanger`, `ServiceAccount`, `ResourceQuota`, `PodSecurity` (el nuevo PSA), `DefaultStorageClass`, `AlwaysPullImages`, `NodeRestriction`.
*   **Habilitación:** Se configuran como flags (`--enable-admission-plugins`, `--disable-admission-plugins`) en el API Server. Muchos vienen habilitados por defecto.

## Capítulo 14: La API de Kubernetes

### Panorama General de la API K8s
*   **API Céntrica:** K8s se controla completamente a través de su API. Todos los componentes interactúan vía la API.
*   **Modelo de Recursos:** Todo en K8s es un *recurso* API (Pods, Services, Nodes, etc.). La mayoría son *objetos* con un estado deseado (`spec`) y un estado observado (`status`).
*   **API RESTful:** La API sigue los principios REST (Representational State Transfer). Es accesible vía HTTP/S.
*   **Operaciones CRUD:** Se interactúa con los recursos usando verbos estándar HTTP (GET, POST, PUT, PATCH, DELETE) que mapean a acciones CRUD (Create, Read, Update, Delete).
*   **Serialización:** Los objetos se envían/reciben como datos serializados, usualmente **JSON**, aunque internamente K8s prefiere **Protobuf** por eficiencia. El cliente/servidor negocian el formato vía cabeceras HTTP (`Content-Type`, `Accept`).

### El API Server
*   **Componente Central:** Expone la API K8s. Es el "frontend" del plano de control.
*   **Comunicación:** Todo (kubectl, kubelets, controllers) se comunica a través del API Server. No hablan directamente entre ellos.
*   **Funciones:**
    *   Servir la API RESTful.
    *   Validar peticiones.
    *   Gestionar Autenticación (AuthN), Autorización (AuthZ) y Control de Admisión.
    *   Persistir el estado de los objetos en `etcd` (el Cluster Store).
    *   Gestionar el mecanismo de `watch` para notificar cambios a los clientes.
*   **Alta Disponibilidad (HA):** Esencial para producción ejecutar múltiples instancias del API Server detrás de un balanceador de carga.
*   **Acceso:** Usualmente escucha en puerto seguro (HTTPS) 443 o 6443.

### La API
*   **Estructura Grupada:** Para escalabilidad y extensibilidad, la API está organizada en grupos.
    *   **Grupo Core (Legado):** Recursos fundamentales originales. Path: `/api/v1`. Contiene Pods, Services, Namespaces, Nodes, Secrets, ConfigMaps, PVs, PVCs, ServiceAccounts, etc. Se referencia en YAML con `apiVersion: v1`.
    *   **Grupos Nombrados:** Recursos más nuevos o específicos. Path: `/apis/<nombre-grupo>/<version>` (e.g., `/apis/apps/v1`, `/apis/networking.k8s.io/v1`). Se referencia en YAML con `apiVersion: <nombre-grupo>/<version>`. Ejemplos de grupos: `apps`, `batch`, `networking.k8s.io`, `storage.k8s.io`, `rbac.authorization.k8s.io`, `apiextensions.k8s.io`.
*   **Versiones API (Alpha, Beta, Stable/GA):**
    *   **Alpha (`v1alpha1`, `v2alpha1`...):** Experimental, inestable, puede cambiar/desaparecer. Usar con precaución, a menudo deshabilitado por defecto.
    *   **Beta (`v1beta1`, `v1beta2`...):** Casi estable, bien probado. Habilitado por defecto en muchos clústeres. Usado en producción con cautela. K8s garantiza soporte por ~9 meses o hasta la siguiente Beta/GA.
    *   **Stable/GA (`v1`, `v2`...):** Listo para producción. Fuerte compromiso de compatibilidad hacia atrás. Política de deprecación clara (soporte por al menos 12 meses o 3 releases tras anuncio).
*   **Descubrimiento de la API:**
    *   `kubectl api-resources`: Lista todos los tipos de recursos, sus shortnames, grupo API, si son namespaced, y su `Kind`.
    *   `kubectl api-versions`: Lista todas las versiones de grupos API habilitadas en el clúster.
    *   `kubectl explain <tipo-recurso>`: Muestra la documentación y estructura (campos) de un recurso API.
    *   **Acceso Directo (para explorar):** Usar `kubectl proxy [--port=<puerto>] &` para exponer la API en `localhost` sin manejar AuthN. Luego usar `curl`, un navegador, o herramientas como Postman para hacer peticiones GET a los paths API (e.g., `curl http://localhost:9000/api`, `curl http://localhost:9000/apis`, `curl http://localhost:9000/api/v1/pods`).
*   **Extender la API (CRDs):**
    *   **CustomResourceDefinition (CRD):** Objeto K8s (`kind: CustomResourceDefinition`, `apiVersion: apiextensions.k8s.io/v1`) que permite definir *nuevos* tipos de recursos personalizados en la API.
    *   Se especifica el grupo, versión, ámbito (namespaced/cluster), nombres (plural, singular, kind, shortnames), y opcionalmente un schema OpenAPI para validación.
    *   Una vez aplicado el CRD, se pueden crear instancias (objetos) de ese nuevo `Kind` usando `kubectl apply` con YAMLs que usen el `apiVersion: <grupo>/<version>` y `kind: <Kind>` definidos.
    *   Estos objetos personalizados se almacenan en etcd y se pueden gestionar con `kubectl` como los nativos.
    *   Para que hagan algo útil, se necesita un **Custom Controller** (a menudo llamado **Operator**) que observe estos objetos personalizados y actúe sobre ellos (implemente la lógica de reconciliación).

## Capítulo 15: Modelado de Amenazas en Kubernetes

### Modelado de Amenazas
*   **Propósito:** Proceso sistemático para identificar vulnerabilidades y contramedidas.
*   **Modelo STRIDE:** Un framework común para categorizar amenazas:
    *   **S**poofing (Suplantación)
    *   **T**ampering (Manipulación)
    *   **R**epudiation (Repudio)
    *   **I**nformation Disclosure (Divulgación de Información)
    *   **D**enial of Service (Denegación de Servicio)
    *   **E**levation of Privilege (Elevación de Privilegios)

### Spoofing (Suplantación)
*   **Amenaza:** Fingir ser otra entidad (usuario, servicio, Pod) para ganar acceso.
*   **Mitigación en K8s:**
    *   **Comunicación API Server:** Usar mTLS (certificados X.509 firmados por CA de confianza) para autenticar todos los componentes (API server, kubelet, controllers, etcd). Rotación automática de certificados. Asegurar la CA raíz. Separar CAs para componentes internos vs externos si es posible.
    *   **Identidad de Pods:** Usar ServiceAccounts. Limitar permisos RBAC de SAs. Deshabilitar montaje automático de token SA (`automountServiceAccountToken: false`) si el Pod no necesita acceso API. Usar características avanzadas de token SA (audiencia, expiración) para limitar su alcance. Service Mesh (como Istio) puede gestionar mTLS entre Pods.

### Tampering (Manipulación)
*   **Amenaza:** Modificación no autorizada de datos o código (en tránsito o en reposo) para causar daño o ganar privilegios.
*   **Mitigación en K8s:**
    *   **En Tránsito:** Usar TLS/mTLS para todas las comunicaciones (API server, etcd, entre Pods).
    *   **En Reposo (Componentes K8s):**
        *   Restringir acceso físico/SSH a nodos (especialmente plano de control y etcd).
        *   Proteger repositorios de código/configuración (Git).
        *   Usar SSH seguro para bootstrapping remoto.
        *   Verificar checksums de binarios/imágenes.
        *   Auditar accesos/cambios a ficheros críticos (e.g., con `auditd` en Linux).
    *   **En Reposo (Aplicaciones en Pods):**
        *   Montar filesystems como solo lectura (`securityContext.readOnlyRootFilesystem: true`).
        *   Usar `securityContext.allowedHostPaths` para restringir montajes de host y hacerlos R/O.
        *   Firmar imágenes de contenedor y verificar firmas al desplegar.

### Repudiation (Repudio)
*   **Amenaza:** Negar haber realizado una acción o poner en duda la validez de un registro.
*   **Mitigación (No Repudio):** Pruebas irrefutables de quién hizo qué, cuándo y cómo.
    *   **Auditoría K8s:** Habilitar Audit Logging en el API Server. Captura detalles de cada petición (quién, qué, cuándo, desde dónde).
    *   **Logs Agregados:** Recolectar logs de todos los componentes (API server, Kubelet, runtime, etcd, nodos OS, firewalls, aplicaciones) en un sistema centralizado y **seguro** (para evitar tampering de los propios logs). Usar agentes (e.g., Fluentd, Filebeat) desplegados como DaemonSet.
    *   **Correlación:** Usar herramientas (SIEM) para analizar y correlacionar eventos de múltiples fuentes.
    *   **Auditoría a Nivel de Nodo:** Usar `auditd` para rastrear cambios en ficheros o ejecución de comandos específicos.

### Information Disclosure (Divulgación de Información)
*   **Amenaza:** Exposición no autorizada de datos sensibles.
*   **Mitigación en K8s:**
    *   **Proteger `etcd`:** Es el "cofre del tesoro". Restringir acceso de red (solo plano de control), restringir acceso físico/SSH, usar TLS para comunicación cliente-servidor y peer-to-peer. Considerar etcd dedicado y encriptación a nivel de disco.
    *   **Cifrar Secrets en Reposo:** Habilitar la funcionalidad de K8s (`EncryptionConfiguration`) para cifrar los objetos `Secret` en `etcd`. Usar un proveedor KMS externo (cloud o HSM) para gestionar la Key Encryption Key (KEK) de forma segura, fuera del clúster.
    *   **Usar `Secrets`:** Almacenar credenciales, tokens, claves en objetos `Secret` en lugar de ConfigMaps, variables de entorno o código.
    *   **RBAC:** Limitar quién puede leer (`get`, `list`, `watch`) Secrets.
    *   **Network Policies:** Limitar qué Pods pueden comunicarse entre sí para reducir la superficie de ataque si un Pod es comprometido.

### Denial of Service (DoS)
*   **Amenaza:** Hacer que un recurso (API server, etcd, nodo, Pod) no esté disponible.
*   **Mitigación en K8s:**
    *   **Alta Disponibilidad (HA):** Replicar componentes críticos (plano de control, etcd, nodos trabajadores) en múltiples nodos y zonas de disponibilidad.
    *   **Límites y Cuotas:**
        *   `LimitRange`: Establecer límites de CPU/Memoria por defecto para Pods/contenedores en un Namespace.
        *   `ResourceQuota`: Limitar el consumo total de recursos (CPU, Mem, Storage) y el número de objetos (Pods, Services, Secrets, PVCs) por Namespace.
        *   `podPidsLimit`: Limitar el número de procesos que un Pod puede crear (previene fork bombs).
    *   **Proteger API Server:** Usar HA, firewalls para limitar exposición, monitorizar tasas de petición, API Priority and Fairness (APF) para evitar sobrecarga por clientes específicos. RBAC estricto para limitar quién puede hacer qué.
    *   **Proteger `etcd`:** Usar HA, aislamiento de red, monitorización, infraestructura de disco rápida.
    *   **Proteger Aplicaciones:** Usar `NetworkPolicy` para limitar tráfico entrante, requerir autenticación (mTLS, tokens API) a nivel de aplicación, aplicar límites de recursos a Pods.

### Elevation of Privilege (Elevación de Privilegios)
*   **Amenaza:** Obtener permisos mayores a los asignados originalmente.
*   **Mitigación en K8s:**
    *   **API Server (RBAC):** Aplicar principio de mínimo privilegio. Evitar roles demasiado amplios (como `cluster-admin`). Usar `Roles` namespaced siempre que sea posible. Revisar bindings regularmente. Habilitar authorizers `Node` y `RBAC`.
    *   **Seguridad de Pods/Contenedores:**
        *   **No ejecutar como Root:** Usar `securityContext.runAsUser` (con UID > 0) y `runAsNonRoot: true`.
        *   **Minimizar Capacidades Linux:** Usar `securityContext.capabilities.drop: ["ALL"]` y luego `add: [...]` solo las estrictamente necesarias.
        *   **Filtrar Syscalls (seccomp):** Usar perfiles `seccomp` (`RuntimeDefault` o, idealmente, uno personalizado de mínimo privilegio) vía `securityContext.seccompProfile`.
        *   **Prevenir Escalada de Privilegios:** Establecer `securityContext.allowPrivilegeEscalation: false`.
    *   **Pod Security Admission (PSA):** Habilitar y **enforzar** los Pod Security Standards (PSS) a nivel de Namespace (usando labels `pod-security.kubernetes.io/...`). Empezar con `baseline`, aspirar a `restricted`. Usar modos `warn` y `audit` para evaluar impacto antes de `enforce`. Considerar herramientas externas (Kyverno, OPA Gatekeeper) para políticas más complejas/personalizadas.

## Capítulo 16: Seguridad de Kubernetes en el Mundo Real

### Pipeline CI/CD
*   **Imágenes de Contenedor:** Empaquetan app + dependencias. Fáciles de construir/compartir, pero también fáciles de distribuir código malicioso.
*   **Registros de Imágenes:**
    *   Públicos (Docker Hub, etc.) vs Privados (Harbor, Quay, registros cloud). Los privados ofrecen más control.
    *   Usar repositorios privados dentro del registro para control de acceso.
    *   Confiar en imágenes "oficiales" o verificadas sobre las "comunitarias".
*   **Imágenes Base:**
    *   Estandarizar en un conjunto pequeño de imágenes base aprobadas, mínimas y seguras (e.g., basadas en Alpine, distroless).
    *   Mantenerlas actualizadas (parches de seguridad).
    *   Proceso para aprobar excepciones/nuevas bases.
*   **Control de Acceso:**
    *   RBAC en el registro para controlar quién puede hacer push/pull a qué repositorios.
    *   Limitar acceso de red desde nodos K8s a solo los registros aprobados.
    *   Restringir permisos de push (e.g., solo sistemas CI/CD a repos de producción).
*   **Promoción de Imágenes (Dev -> Test -> Prod):**
    *   **Escaneo de Vulnerabilidades:** Integrar escaneo (e.g., Trivy, Clair) en el pipeline CI/CD. Comprobar contra bases de datos CVE. Fallar builds/poner en cuarentena imágenes con vulnerabilidades críticas/altas. Revisar falsos positivos.
    *   **Revisión de Configuración ("Config as Code"):** Analizar Dockerfiles y YAMLs K8s en busca de malas prácticas de seguridad (secretos hardcoded, permisos excesivos). Usar herramientas de policy-as-code (e.g., Checkov, Kyverno CLI).
    *   **Firma de Imágenes:** Firmar imágenes criptográficamente (e.g., con Cosign/Sigstore) en etapas clave del pipeline (build, test, QA). Configurar K8s/runtime para *verificar* firmas antes de ejecutar contenedores (e.g., vía políticas de admisión).

### Infraestructura y Redes
*   **Aislamiento de Cargas (Multi-Tenancy):**
    *   **Hard Multi-Tenancy (No Confiable):** K8s Namespaces **NO** son suficientes. Usar **clústeres K8s separados** para aislar cargas de trabajo no confiables (e.g., clientes diferentes, prod vs non-prod).
    *   **Soft Multi-Tenancy (Confiable):** Usar Namespaces para organizar equipos/entornos dentro de una misma organización de confianza. Combinar con RBAC, Quotas, Network Policies.
*   **Aislamiento de Nodos:** Usar taints/tolerations y afinidad/anti-afinidad para dedicar nodos a cargas de trabajo específicas (e.g., sensibles, que requieren privilegios). Aplicar controles de seguridad más estrictos en esos nodos.
*   **Aislamiento en Tiempo de Ejecución (Runtime):**
    *   Contenedores estándar (Docker/containerd) comparten kernel (aislamiento más débil).
    *   VMs ofrecen aislamiento fuerte (kernel separado, hardware).
    *   **Mejorar Contenedores:** Usar perfiles de seguridad (seccomp, AppArmor/SELinux), `securityContext` (no root, drop caps, no priv escalation).
    *   **Runtimes Alternativos:** Considerar runtimes más seguros como gVisor o Kata Containers (usando RuntimeClass en K8s) para cargas sensibles.
*   **Aislamiento de Red:**
    *   **Network Policies:** **Esencial**. Objeto K8s que define reglas de firewall (L3/L4) para controlar qué Pods pueden comunicarse entre sí y con endpoints externos. Por defecto, todo el tráfico está permitido dentro del clúster. Implementar políticas de "deny-by-default" o "least privilege".
    *   **Impacto CNI:** Entender si el plugin CNI usa Overlay (encapsula tráfico, puede dificultar firewalls externos) o BGP (no encapsula).
    *   **Firewalls Externos:** Considerar firewalls físicos/virtuales en la infraestructura subyacente (host-based o de red). Integrar con Network Policies para defensa en profundidad. Cuidado con procesos de cambio lentos en firewalls centralizados.

### Gestión de Identidad y Acceso (IAM)
*   **Integración:** Usar RBAC de K8s integrado con el proveedor IAM corporativo existente (AD, LDAP, OIDC) para gestionar usuarios y grupos centralizadamente.
*   **Acceso a Nodos (SSH):** **Minimizar**. Reservar para tareas de gestión específicas no posibles vía API, troubleshooting extremo, o escenarios "break-glass". Controlar estrictamente quién tiene acceso, especialmente al plano de control.
*   **Autenticación Multi-Factor (MFA):** **Requerir MFA** para acceso privilegiado a K8s (e.g., cluster-admin) y para acceso SSH a nodos.
*   **Estaciones de Trabajo Seguras:** Proteger las máquinas donde se usa `kubectl` con credenciales privilegiadas.

### Auditoría y Monitorización de Seguridad
*   **Importancia:** Detectar brechas, investigar incidentes (timeline forense), verificar cumplimiento de políticas. **No lanzar a producción sin esto.**
*   **Auditoría de Configuración:** Usar benchmarks (e.g., CIS Kubernetes Benchmark) y herramientas (`kube-bench`) para validar la configuración segura del clúster de forma regular y tras incidentes.
*   **Logs Centralizados y Seguros:** Agregar logs de:
    *   API Server (Audit Logs).
    *   Nodos (OS logs, `auditd`).
    *   Kubelet.
    *   Container Runtime.
    *   Aplicaciones (stdout/stderr o ficheros).
    *   Network Policies (si el CNI lo soporta).
    *   Firewalls externos.
    *   Enviar a un SIEM o sistema de logging seguro (WORM si es posible).
*   **Eventos de Ciclo de Vida:** Monitorizar creación/destrucción de Pods/contenedores. Guardar logs incluso después de que terminen.
*   **Monitorización de Comportamiento:** Usar herramientas de runtime security (e.g., Falco, Sysdig Secure) para detectar actividades sospechosas dentro de los contenedores o en los nodos en tiempo real.
*   **Gestión de Logs:** El volumen será alto. Usar herramientas especializadas para análisis, alertas y retención.
*   **Estrategia de Migración:** Para apps existentes: Crawl (evaluar seguridad actual), Walk (migrar sin empeorar), Run (mejorar seguridad incrementalmente post-migración).
