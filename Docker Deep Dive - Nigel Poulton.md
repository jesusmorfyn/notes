## Parte 1: La Visión General

### Capítulo 1: Contenedores desde 30,000 pies

*   **Problema Original:** Antes, se necesitaba un servidor físico por aplicación, lo que llevaba a servidores sobredimensionados y subutilizados (5-10% de capacidad), desperdiciando recursos y capital.
*   **Solución VMware (VMs):** Las máquinas virtuales permitieron ejecutar múltiples aplicaciones en un solo servidor, mejorando la utilización.
    *   **Desventajas de las VMs:** Cada VM necesita su propio SO (consume recursos, requiere parches y monitorización), son lentas para arrancar y poco portables.
*   **Solución Contenedores:**
    *   Comparten el SO del host, lo que permite ejecutar muchos más contenedores que VMs en el mismo hardware (ej. 10 VMs vs 50 contenedores).
    *   Son más rápidos y portables que las VMs.
*   **Orígenes (Linux):** Basados en tecnologías del kernel de Linux como *namespaces*, *control groups (cgroups)* y *capabilities*. Docker simplificó su uso.
*   **Docker:** Herramienta que facilitó enormemente el uso de contenedores Linux, haciéndolos accesibles masivamente.
*   **Contenedores Windows vs. Linux:**
    *   *Windows Containers:* Ejecutan apps Windows, requieren host Windows. Soportados en Win10/11 Pro/Ent y Server.
    *   *Linux Containers:* Ejecutan apps Linux, requieren host Linux. Pueden ejecutarse en Windows mediante *WSL 2*. Son más pequeños, rápidos y comunes. **El libro se enfoca en contenedores Linux.**
*   **Contenedores Mac:** No existen "Mac Containers". Docker Desktop en Mac ejecuta contenedores Linux dentro de una VM ligera.
*   **Wasm (WebAssembly):** Tecnología emergente. Binarios más pequeños, rápidos, seguros y portables que contenedores. Aún con limitaciones y ecosistema en desarrollo. Docker se está adaptando para coexistir con Wasm.
*   **Docker y IA:** Docker se está convirtiendo en la plataforma preferida para desarrollar y desplegar apps de IA/ML (LLMs, GenAI). Soporta GPUs NVIDIA, con planes de expandir a TPUs, NPUs, etc.
*   **Kubernetes:** Estándar de la industria para *desplegar y gestionar* (orquestar) aplicaciones en contenedores. Usa `containerd` (una versión ligera de Docker) en versiones recientes. Los contenedores Docker son compatibles con Kubernetes.
*   **Resumen del Capítulo:** La evolución fue: 1 app/servidor -> VMs (mejor utilización pero con sobrecarga) -> Contenedores (más eficientes, rápidos, portables gracias a compartir OS). Docker popularizó los contenedores. Wasm y IA son tendencias futuras con las que Docker se integra. Kubernetes orquesta contenedores.

---

### Capítulo 2: Estándares y proyectos relacionados con Docker y contenedores

*   **Docker (Término Dual):**
    *   *Plataforma Docker:* Conjunto de tecnologías para crear, gestionar y orquestar contenedores (CLI + Engine).
    *   *Docker, Inc.:* La empresa que creó la plataforma Docker. Origen en "dotCloud" (PaaS).
*   **Tecnología Docker:**
    *   *CLI:* Interfaz de línea de comandos (`docker`) que traduce comandos a peticiones API.
    *   *Engine (Motor):* Componentes del lado del servidor que ejecutan y gestionan contenedores (anteriormente monolítico, ahora modular con `containerd`, `runc`, etc.).
    *   Simplifica la complejidad subyacente.
*   **Estándares y Proyectos:**
    *   **OCI (Open Container Initiative):** Bajo la Linux Foundation. Gobierna estándares de bajo nivel para asegurar interoperabilidad.
        *   *Specs:* `image-spec`, `runtime-spec`, `distribution-spec`.
        *   Docker implementa estas especificaciones (imágenes OCI, runtime OCI como `runc`, Docker Hub es registro OCI).
    *   **CNCF (Cloud Native Computing Foundation):** Bajo la Linux Foundation. *Hospeda* proyectos importantes como Kubernetes, containerd, Prometheus. Ayuda a madurar proyectos (Sandbox -> Incubating -> Graduated). Docker usa tecnologías CNCF (containerd, Notary).
    *   **Moby Project:** Proyecto comunitario (iniciado por Docker) que provee componentes modulares para construir plataformas de contenedores. Docker (la plataforma) usa componentes de Moby, OCI y CNCF.
*   **Resumen del Capítulo:** Docker (empresa y plataforma) es central en el ecosistema. La OCI estandariza aspectos clave. La CNCF madura proyectos relacionados. Moby provee bloques de construcción. Docker integra componentes de todos ellos.

---

### Capítulo 3: Obteniendo Docker

*   **Métodos de Instalación:** Docker Desktop (recomendado), Multipass, Instalación en servidor Linux.
*   **Docker Desktop:**
    *   Aplicación de escritorio (Win, Mac, Linux). La mejor experiencia. Incluye Engine, UI, plugins, Compose, K8s opcional.
    *   *Licencia:* Gratis para uso personal/educativo/pequeñas empresas. De pago para empresas grandes.
    *   *Windows:* Requiere Win10/11 64-bit Pro/Ent/Home, virtualización HW, WSL 2. Soporta contenedores Linux (default) y Windows (en Pro/Ent).
    *   *Mac:* Instala el daemon en una VM Linux ligera. Solo soporta contenedores Linux.
    *   Instalación simple mediante instalador. Verificar con `docker version`.
*   **Multipass:**
    *   Alternativa si no se puede usar Docker Desktop. Crea VMs Linux fácilmente.
    *   Menos funcionalidades integradas (Scout, Debug, Init).
    *   Comandos básicos: `multipass launch docker --name node1`, `multipass ls`, `multipass shell node1`.
*   **Instalación en Linux:**
    *   Alternativa si no se puede usar Docker Desktop. Menos funcionalidades.
    *   Ejemplo Ubuntu: `sudo snap install docker`.
    *   Requiere `sudo` o añadir usuario al grupo `docker` (`sudo groupadd docker`, `sudo usermod -aG docker $(whoami)`).
*   **Resumen del Capítulo:** Docker Desktop es la opción preferida y más completa. Multipass y la instalación directa en Linux son alternativas viables pero con menos características.

---

### Capítulo 4: La visión general (Práctica)

*   **Perspectiva de Operaciones (Ops):** Enfocada en gestionar contenedores.
    *   `docker version`: Verificar instalación.
    *   `docker pull nginx:latest`: Descargar imagen NGINX.
    *   `docker images`: Listar imágenes locales.
    *   `docker run --name test -d -p 8080:80 nginx:latest`: Crear y ejecutar un contenedor (`-d` detached, `-p` mapeo de puertos).
    *   `docker ps`: Listar contenedores en ejecución.
    *   `docker exec -it test bash`: Ejecutar un comando (shell interactivo) dentro del contenedor.
    *   `exit`: Salir del contenedor.
    *   `docker stop test`: Detener el contenedor.
    *   `docker rm test`: Eliminar el contenedor. (`docker ps -a` para ver detenidos).
*   **Perspectiva de Desarrollo (Dev):** Enfocada en empaquetar aplicaciones.
    *   `git clone <repo>`: Obtener código fuente de la app.
    *   **Dockerfile:** Archivo de texto con instrucciones para construir la imagen (`FROM`, `RUN`, `COPY`, `WORKDIR`, `EXPOSE`, `ENTRYPOINT`/`CMD`).
    *   `docker build -t test:latest .`: Construir la imagen desde el Dockerfile en el directorio actual (`.`). Proceso llamado *containerizar*.
    *   `docker run -d --name web1 --publish 8080:8080 test:latest`: Ejecutar la app containerizada.
    *   Probar accediendo a `localhost:8080` (o IP del host).
    *   Limpieza: `docker rm web1 -f`, `docker rmi test:latest`.
*   **Resumen del Capítulo:** Se realizaron tareas básicas de Ops (pull, run, exec, stop, rm) y Dev (clonar, Dockerfile, build, run, push opcional) para familiarizarse con imágenes y contenedores.

---

## Parte 2: Los Aspectos Técnicos

### Capítulo 5: El Motor Docker (Docker Engine)

*   **Docker Engine:** Componentes del lado servidor que ejecutan y gestionan contenedores. Similar a ESXi de VMware. Es modular.
*   **Evolución:**
    *   Inicialmente: Monolítico (daemon + LXC).
    *   Luego: Reemplazó LXC con `libcontainer` (agnóstico de plataforma).
    *   Actual: Descompuesto en herramientas especializadas (daemon API, containerd, runc, BuildKit) para mejorar rendimiento, innovación y alineación con el ecosistema.
*   **Componentes Clave (Runtime):**
    *   **Docker Daemon (`dockerd`):** Expone la API de Docker. La mayoría de la funcionalidad original ha sido extraída.
    *   **containerd:** *Runtime de alto nivel*. Gestiona el ciclo de vida de los contenedores (start, stop, delete), gestión de imágenes, redes, volúmenes. Proyecto graduado de CNCF.
    *   **Shim (`containerd-shim-runc-v2`):** Proceso ligero que actúa como padre del proceso del contenedor una vez que `runc` termina. Permite "daemonless containers" (el daemon puede reiniciarse sin afectar contenedores), eficiencia y runtimes OCI conectables.
    *   **runc:** *Runtime de bajo nivel*. Implementación de referencia de la OCI runtime-spec. Interactúa directamente con el kernel (namespaces, cgroups) para crear/eliminar contenedores.
*   **Flujo de Creación de Contenedor:**
    1.  `docker run` (CLI) -> Petición API
    2.  Docker Daemon recibe petición -> Llama a `containerd` (vía gRPC)
    3.  `containerd` prepara el OCI bundle -> Llama a `runc`
    4.  `runc` interactúa con el kernel para crear el contenedor -> El contenedor inicia como hijo de `runc` -> `runc` sale.
    5.  El proceso `shim` se convierte en el padre del contenedor.
*   **Daemonless Containers:** La separación permite actualizar/reiniciar el daemon sin detener los contenedores en ejecución.
*   **Implementación Linux:** Binarios separados: `dockerd`, `containerd`, `containerd-shim-runc-v2`, `runc`.
*   **Resumen del Capítulo:** El Docker Engine es modular, construido con componentes como containerd (gestión de ciclo de vida) y runc (ejecución de bajo nivel OCI), permitiendo flexibilidad y resiliencia (daemonless containers).

---

### Capítulo 6: Trabajando con Imágenes

*   **Definición:** Paquete de solo lectura que contiene todo lo necesario para ejecutar una aplicación (código, dependencias, OS mínimo, metadatos). Plantilla para contenedores. Constructo de *tiempo de construcción*.
*   **Características:**
    *   Generalmente pequeñas (excepto Windows).
    *   No incluyen kernel (usan el del host).
    *   "Slim images" son comunes (mínimo indispensable).
*   **Obtención (`docker pull`):** Descarga imágenes desde un *registro* al *repositorio local* (caché). Si no se especifica tag, asume `latest`. Si no se especifica registro, asume Docker Hub.
*   **Registros:** Almacenes centralizados de imágenes (Docker Hub, GHCR, privados). Implementan OCI distribution-spec y Docker Registry API v2.
*   **Repositorios:** Contenidos dentro de registros. Pueden ser *oficiales* (curados, ej: `nginx`) o *no oficiales* (de usuarios/organizaciones, ej: `miusuario/miapp`). Usar no oficiales con precaución.
*   **Nomenclatura y Tags:** Formato: `[registro/][usuario/]repositorio:tag`.
    *   `tag` es una etiqueta (mutable). `latest` no garantiza ser la más reciente.
    *   Varias tags pueden apuntar a la misma imagen (mismo IMAGE ID).
*   **Capas (Layers):** Las imágenes se construyen apilando capas de solo lectura.
    *   Cada instrucción en Dockerfile que añade contenido (`RUN`, `COPY`, `ADD`) crea una capa.
    *   Las capas son independientes y pueden ser compartidas entre imágenes (eficiencia de espacio y descarga).
    *   `docker history`: Muestra historial de construcción.
    *   `docker inspect`: Muestra capas (hashes) y metadatos.
*   **Digests (Resúmenes):** Identificador único e inmutable de una imagen (hash SHA256 del manifiesto). Permite referenciar una versión exacta: `docker pull repo@sha256:...`. Modelo de almacenamiento direccionable por contenido.
*   **Imágenes Multi-Arquitectura:** Una sola tag (ej: `alpine:latest`) puede referenciar imágenes para diferentes arquitecturas (amd64, arm64). Docker elige la correcta automáticamente.
    *   *Manifest List:* Apunta a los *Manifests* específicos de cada arquitectura.
    *   `docker buildx imagetools inspect` o `docker manifest inspect` para ver arquitecturas soportadas.
    *   `docker buildx build --platform ...`: Permite construir imágenes multi-arquitectura (localmente con emulación QEMU o con Docker Build Cloud).
*   **Escaneo de Vulnerabilidades (Docker Scout):** Herramienta (requiere suscripción) para escanear imágenes, detectar CVEs y sugerir remediaciones. Integrado en CLI, Desktop, Hub. Comandos: `docker scout quickview`, `docker scout cves`.
*   **Eliminación (`docker rmi`):** Borra imágenes del repositorio local. No se puede borrar si está en uso por un contenedor (a menos que se fuerce con `-f`). `docker rmi $(docker images -q)` borra todas.
*   **Resumen del Capítulo:** Las imágenes son plantillas en capas, inmutables, identificadas por tag (mutable) o digest (inmutable). Se almacenan en registros, se descargan con `pull`. Pueden ser multi-arquitectura. Se deben escanear por vulnerabilidades.

---

### Capítulo 7: Trabajando con contenedores

*   **Definición:** Instancia en ejecución de una imagen. Constructo de *tiempo de ejecución*.
*   **vs VMs:** Contenedores virtualizan OS (comparten kernel host), VMs virtualizan HW (kernel propio). Contenedores son más ligeros, rápidos, portables pero con aislamiento menos fuerte.
*   **Almacenamiento R/W:** Cada contenedor tiene una capa fina de escritura sobre las capas de la imagen. Los cambios persisten entre `stop`/`start` pero se pierden con `docker rm`.
*   **Ciclo de Vida y Comandos:**
    *   `docker run`: Crea y arranca. Opciones: `-d` (detached), `--name`, `-p` (puertos), `--rm` (borrar al salir), `<imagen>`, `[comando]` (opcional).
    *   `docker ps`: Lista contenedores corriendo. `docker ps -a` lista todos.
    *   `docker stop`: Envía SIGTERM, espera 10s, envía SIGKILL.
    *   `docker start`: Arranca un contenedor detenido.
    *   `docker restart`: Reinicia un contenedor.
    *   `docker rm`: Elimina un contenedor detenido. `docker rm -f` fuerza eliminación de uno corriendo.
*   **Proceso Principal (PID 1):** Contenedores típicamente ejecutan un solo proceso. Si PID 1 termina, el contenedor se detiene.
*   **Interacción:**
    *   `docker exec -it <contenedor> <shell>`: Shell interactivo.
    *   `docker exec <contenedor> <comando>`: Ejecución remota.
    *   `docker attach <contenedor>`: Conectar al proceso principal (PID 1).
    *   `Ctrl-PQ`: Desconectar de `attach` o `exec -it` sin detener el contenedor.
*   **Inspección:** `docker inspect <contenedor>`: Muestra detalles (estado, IP, puertos, volúmenes, configuración).
*   **Docker Debug (Docker Desktop Pro/Team/Business):** Permite obtener un shell y usar herramientas de diagnóstico en contenedores/imágenes "slim" que no las incluyen. Monta herramientas en `/nix` (temporalmente). Cambios en contenedores *en ejecución* persisten; cambios en imágenes o contenedores *detenidos* se pierden al salir. Comandos: `docker debug <contenedor|imagen>`, `install <paquete>`.
*   **Políticas de Reinicio (`--restart`):** Para auto-recuperación.
    *   `no`: (Default) No reiniciar.
    *   `on-failure[:max-retries]`: Reiniciar solo si sale con error (no-cero).
    *   `always`: Reiniciar siempre (incluso si se detiene manualmente y el daemon reinicia).
    *   `unless-stopped`: Como `always`, pero no reinicia si fue detenido manualmente.
*   **Resumen del Capítulo:** Los contenedores son instancias efímeras y (idealmente) inmutables de imágenes. Se gestionan con `docker run/stop/start/rm/exec/inspect`. Docker Debug ayuda a diagnosticar contenedores slim. Las políticas de reinicio ofrecen auto-recuperación básica.

---

### Capítulo 8: Containerizando una aplicación

*   **Proceso de Containerización:** Código Fuente -> Dockerfile -> `docker build` -> Imagen -> `docker run` -> Contenedor.
*   **Dockerfile:** Archivo de texto con instrucciones para construir la imagen.
    *   `docker init`: Puede auto-generar un Dockerfile básico siguiendo buenas prácticas.
    *   **Instrucciones Clave:**
        *   `FROM`: Imagen base.
        *   `WORKDIR`: Establece el directorio de trabajo para instrucciones posteriores (`RUN`, `COPY`, `CMD`, `ENTRYPOINT`).
        *   `COPY`: Copia archivos/directorios desde el contexto de build a la imagen.
        *   `RUN`: Ejecuta comandos durante la construcción (instalar dependencias, compilar). Cada `RUN` crea una capa.
        *   `EXPOSE`: Documenta el puerto que la aplicación usará (no lo publica).
        *   `CMD`: Comando por defecto para ejecutar al iniciar el contenedor (puede ser sobreescrito).
        *   `ENTRYPOINT`: Comando principal para ejecutar (no se sobreescribe fácilmente; `CMD` o args de `run` se añaden como parámetros).
*   **Contexto de Build:** Directorio local (o repo Git) que contiene el código y el Dockerfile. Se envía al daemon Docker. `.dockerignore` para excluir archivos.
*   **Construcción (`docker build`):**
    *   `docker build -t nombre:tag .`: Construye la imagen usando el Dockerfile en el contexto (`.`).
    *   `-f`: Especificar un Dockerfile diferente.
    *   Docker procesa instrucciones secuencialmente, usando caché.
*   **Caché de Build:** Docker reutiliza capas de builds anteriores si la instrucción y su base no han cambiado. Acelera builds. `COPY` y `ADD` invalidan caché si los archivos copiados cambian. Ordenar instrucciones de menos a más cambiantes optimiza el uso de caché. `--no-cache` para forzar rebuild completo.
*   **Builds Multi-Etapa (Multi-Stage Builds):**
    *   Usar múltiples `FROM` en un Dockerfile. Cada `FROM` inicia una nueva etapa (se pueden nombrar con `AS`).
    *   Permite usar una imagen grande con herramientas de compilación en una etapa y copiar *solo* el artefacto compilado a una imagen final pequeña (ej: `FROM scratch` o `alpine`) en otra etapa (`COPY --from=nombre_etapa ...`).
    *   Reduce drásticamente el tamaño de la imagen final de producción.
    *   Se puede construir una etapa específica con `docker build --target nombre_etapa ...`.
*   **Buildx y BuildKit:**
    *   `BuildKit`: Motor de build moderno y eficiente.
    *   `Buildx`: Plugin CLI para interactuar con BuildKit. Es el builder por defecto.
    *   Soporta builds multi-etapa, multi-plataforma, caché avanzada.
    *   *Drivers:* `docker-container` (local, usa QEMU para emulación multi-plataforma), `cloud` (Docker Build Cloud, nativo, rápido, caché compartida, de pago).
    *   Comandos: `docker buildx ls`, `docker buildx create`, `docker buildx use`, `docker buildx inspect`.
*   **Builds Multi-Plataforma:**
    *   `docker buildx build --platform=linux/amd64,linux/arm64 ...`: Construye para varias arquitecturas.
    *   `--push`: Empuja el manifiesto multi-arquitectura y las imágenes al registro.
    *   `--load`: Carga la imagen para la arquitectura del host localmente.
*   **Buenas Prácticas:** Usar caché, minimizar capas (combinar `RUN`), usar imágenes base oficiales/verificadas, instalar solo dependencias necesarias (`--no-install-recommends`), usar builds multi-etapa para producción.
*   **Resumen del Capítulo:** Containerizar implica crear un Dockerfile para instruir a `docker build`. Optimizar con caché y builds multi-etapa es crucial para imágenes de producción eficientes y seguras. Buildx/BuildKit potencian el proceso, incluyendo builds multi-plataforma.

---

### Capítulo 9: Aplicaciones Multi-Contenedor con Compose

*   **Docker Compose:** Herramienta para definir y ejecutar aplicaciones Docker multi-contenedor. Simplifica la gestión de microservicios.
*   **Archivo Compose (`compose.yaml` o `compose.yml`):** Archivo YAML que describe la aplicación:
    *   `services`: Define cada componente/microservicio de la app.
        *   `image` o `build`: Especifica la imagen a usar o cómo construirla.
        *   `command`: Comando a ejecutar en el contenedor.
        *   `ports`: Mapeo de puertos (`"HOST:CONTAINER"`).
        *   `volumes`: Montaje de volúmenes (`"VOLUMEN:RUTA_CONTENEDOR"` o formato largo).
        *   `networks`: Redes a las que conectar el servicio.
        *   `environment`: Variables de entorno.
        *   `depends_on`: Define dependencias de inicio entre servicios.
        *   `healthcheck`: Define cómo verificar la salud del servicio.
        *   `deploy`: (Más relevante para Swarm) Define réplicas, políticas de actualización/reinicio.
    *   `networks`: Define redes personalizadas.
    *   `volumes`: Define volúmenes nombrados.
*   **Flujo de Trabajo:**
    1.  Definir la aplicación en `compose.yaml`.
    2.  Ejecutar `docker compose up [-d]` en el directorio del archivo.
    3.  Compose crea/inicia redes, volúmenes y contenedores definidos.
*   **Comandos Principales:**
    *   `docker compose up`: Crea e inicia la aplicación. `-d` para modo detached.
    *   `docker compose down`: Detiene y elimina contenedores, redes. *No elimina volúmenes por defecto*. `--volumes` para eliminar volúmenes. `--rmi all` para eliminar imágenes construidas.
    *   `docker compose ps`: Lista los contenedores de la aplicación Compose.
    *   `docker compose logs [-f] [servicio]`: Muestra logs. `-f` para seguir.
    *   `docker compose stop/start/restart [servicio]`: Gestiona el estado de los contenedores.
    *   `docker compose build [servicio]`: Construye imágenes definidas con `build`.
    *   `docker compose pull [servicio]`: Descarga imágenes.
    *   `docker compose config`: Valida y muestra la configuración Compose procesada.
    *   `docker compose ls`: Lista proyectos Compose en ejecución.
*   **Ejemplo AI Chatbot:** Aplicación de 3 servicios (frontend, backend, model/Ollama), 1 red, 1 volumen. Se usó `depends_on` para orden de inicio y `healthcheck` para asegurar que el modelo estuviera listo.
*   **Resumen del Capítulo:** Docker Compose simplifica enormemente la definición, ejecución y gestión de aplicaciones multi-contenedor mediante un archivo YAML declarativo y comandos simples. Es ideal para entornos de desarrollo y pruebas, y la base para Docker Stacks en Swarm.

---

### Capítulo 10: Docker y Wasm

*   **Wasm (WebAssembly):** Formato de instrucción binario portable. Se compila desde varios lenguajes (Rust, Go, C++, etc.).
    *   **Ventajas:** Más pequeño, rápido, seguro (sandbox) y portable que contenedores tradicionales. Arranque casi instantáneo.
    *   **Desventajas:** Ecosistema menos maduro, limitaciones actuales en capacidades (red, I/O pesado).
    *   **Casos de Uso:** Funciones serverless, plugins, edge computing, IA.
*   **Integración con Docker:**
    *   Docker Desktop (Beta) incluye *runtimes Wasm* (ej: spin, wasmtime) integrados con containerd. Requiere activar la característica.
    *   Permite empaquetar apps Wasm como imágenes OCI y ejecutarlas usando herramientas Docker.
*   **Contenedores Wasm:**
    *   Dockerfile típicamente usa `FROM scratch` (no necesita OS Linux).
    *   Copia el binario `.wasm` y archivos de configuración necesarios (ej: `spin.toml`).
    *   Se construye con `docker build --platform wasi/wasm ...`.
    *   Se ejecuta con `docker run --runtime=<nombre_runtime> --platform=wasi/wasm ...`.
    *   Se pueden subir/bajar de registros OCI como Docker Hub.
*   **Ejemplo (Rust + Spin):**
    1.  Instalar Rust (con target `wasm32-wasip1`) y Spin.
    2.  Crear app: `spin new ...`.
    3.  Modificar código (`src/lib.rs`).
    4.  Compilar: `spin build` (genera `.wasm`).
    5.  Probar localmente: `spin up`.
    6.  Crear Dockerfile (`FROM scratch`, `COPY *.wasm`, `COPY spin.toml`). Ajustar `spin.toml` si es necesario.
    7.  Construir imagen Wasm: `docker build --platform wasi/wasm -t miimagen:wasm .`.
    8.  Ejecutar como contenedor: `docker run -d --runtime=io.containerd.spin.v2 --platform=wasi/wasm -p ... miimagen:wasm`.
*   **Resumen del Capítulo:** Wasm es una tecnología prometedora que ofrece beneficios sobre contenedores en ciertos casos de uso. Docker está integrando Wasm, permitiendo usar flujos de trabajo familiares (build, push, run) para aplicaciones Wasm empaquetadas como imágenes OCI especiales.

---

### Capítulo 11: Docker Swarm

*   **Docker Swarm:** Funcionalidad nativa de Docker para orquestación de contenedores. Permite crear un clúster de nodos Docker. Más simple que Kubernetes.
*   **Conceptos Clave:**
    *   **Swarm:** Un clúster de nodos Docker operando en "modo swarm".
    *   **Nodos:** Máquinas (físicas/VMs/cloud) corriendo Docker en modo swarm.
    *   **Roles de Nodo:**
        *   *Manager:* Ejecuta el plano de control, gestiona el estado del clúster (usando Raft para consenso), despacha tareas.
        *   *Worker:* Ejecuta los contenedores de las aplicaciones (tareas).
    *   **Alta Disponibilidad (HA) de Managers:** Se recomienda un número impar (3 o 5) de managers para tolerancia a fallos (quorum de Raft). Un manager es el *Leader* (activo), los otros son *Followers* (pasivos, reenvían peticiones al líder). Si el líder falla, se elige uno nuevo.
    *   **Cluster Store:** Base de datos distribuida (basada en etcd) replicada entre managers, almacena el estado y configuración del swarm. Es encriptada.
*   **Seguridad Integrada (Automática):**
    *   IDs de nodo criptográficos.
    *   TLS mutuo para autenticación entre nodos.
    *   Tokens de unión seguros (`manager` y `worker`).
    *   CA (Autoridad Certificadora) integrada con rotación automática de certificados (cada 90 días por defecto).
    *   Redes overlay encriptables.
*   **Comandos de Gestión del Clúster:**
    *   `docker swarm init [--advertise-addr IP[:PUERTO]]`: Inicializa un swarm en el nodo actual (se convierte en el primer manager).
    *   `docker swarm join-token manager|worker`: Muestra el comando y token para unir nodos.
    *   `docker swarm join --token <TOKEN> <IP_MANAGER>:<PUERTO>`: Une el nodo actual al swarm.
    *   `docker node ls`: Lista los nodos del swarm, sus roles y estado.
    *   `docker swarm update --autolock=true`: Activa el bloqueo del swarm (requiere clave para reiniciar managers).
    *   `docker swarm unlock`: Desbloquea un manager reiniciado en un swarm bloqueado.
    *   `docker node update --availability drain <NODO>`: Marca un nodo (usualmente manager) para no aceptar nuevas tareas (contenedores de aplicación).
*   **Resumen del Capítulo:** Docker Swarm transforma nodos Docker individuales en un clúster coordinado y seguro. Proporciona HA para el plano de control y una base segura para desplegar aplicaciones orquestadas (visto en el siguiente capítulo). Su configuración de seguridad es mayormente automática.

---

### Capítulo 12: Desplegando aplicaciones en Swarm

*   **Docker Stacks:** Método para desplegar aplicaciones multi-servicio (definidas en archivos Compose) en un clúster Docker Swarm. Combina la simplicidad de Compose con la orquestación de Swarm.
*   **Archivo Stack:** Es un archivo Compose (`compose.yaml`), pero usado con `docker stack`.
    *   **Importante:** La directiva `build` *no* es soportada por `docker stack deploy`. Las imágenes deben existir previamente en un registro o localmente en los nodos.
    *   La clave `deploy` dentro de `services` es fundamental en Swarm:
        *   `replicas`: Número deseado de instancias (contenedores) del servicio.
        *   `update_config`: Define cómo realizar actualizaciones (rolling updates): `parallelism` (cuántas actualizar a la vez), `delay` (tiempo entre lotes), `failure_action` (`pause`, `continue`, `rollback`).
        *   `restart_policy`: Define cuándo y cómo reiniciar tareas fallidas (`condition`, `delay`, `max_attempts`, `window`).
        *   `placement`: Restricciones (`constraints`, ej: `node.role == worker`) y preferencias para dónde ejecutar las tareas.
*   **Despliegue (`docker stack deploy`):**
    *   `docker stack deploy -c <archivo.yaml> <nombre_stack>`: Despliega o actualiza la aplicación.
    *   Swarm lee el archivo, compara con el estado actual y realiza los cambios necesarios (crea redes/volúmenes si no existen, crea/actualiza/elimina servicios y tareas) para alcanzar el *estado deseado*.
    *   Este proceso se basa en el bucle de *reconciliación* (comparar deseado vs observado).
*   **Gestión Declarativa (Recomendada):** Realizar todos los cambios (escalado, actualización de imagen, etc.) modificando el archivo stack y re-ejecutando `docker stack deploy`. Evita inconsistencias entre la definición y el estado real.
*   **Comandos de Stack:**
    *   `docker stack ls`: Lista los stacks desplegados.
    *   `docker stack ps <nombre_stack>`: Muestra el estado de las tareas (contenedores) de un stack (en qué nodo corren, estado deseado/actual). Muy útil para troubleshooting.
    *   `docker stack services <nombre_stack>`: Lista los servicios del stack, sus réplicas y puertos publicados.
    *   `docker stack rm <nombre_stack>`: Elimina el stack (servicios, redes asociadas). *No elimina volúmenes por defecto*.
*   **Servicios Swarm:** Son la abstracción sobre los contenedores. Un servicio define cómo ejecutar una imagen, cuántas réplicas, cómo actualizar, etc. Swarm se encarga de mantener el número deseado de réplicas (tareas) corriendo.
*   **Resumen del Capítulo:** Docker Stacks usa archivos Compose para desplegar aplicaciones en Swarm de forma declarativa. La clave `deploy` controla cómo Swarm gestiona los servicios (réplicas, actualizaciones, reinicios). Se recomienda la gestión declarativa modificando el archivo y re-desplegando.

---

### Capítulo 13: Redes en Docker

*   **Modelo de Red de Contenedores (CNM):** Especificación de diseño para redes Docker. Define 3 bloques:
    *   *Sandbox:* Stack de red aislado de un contenedor (interfaces, puertos, tablas de ruta, DNS).
    *   *Endpoint:* Interfaz de red virtual que conecta un Sandbox a una Red.
    *   *Network:* Switch virtual (usualmente bridge L2) que agrupa Endpoints.
*   **Libnetwork:** Implementación de referencia del CNM (parte de Moby). Proporciona el plano de control (APIs, service discovery, load balancing básico).
*   **Drivers:** Implementan el plano de datos para tipos específicos de redes.
    *   *Nativos:* `bridge`, `overlay`, `macvlan`, `host`, `none`.
    *   *De Terceros:* Plugins para funcionalidades avanzadas o integración con hardware/software específico.
*   **Redes Bridge (Single-Host):**
    *   Creadas por el driver `bridge` (o `nat` en Windows). Limitadas a un solo host.
    *   Implementadas usando Linux bridges (ej: red `bridge` default usa `docker0`).
    *   Contenedores en la *misma* red bridge pueden comunicarse. Aislados de otras redes bridge y del host (a menos que se expongan puertos).
    *   `docker network create -d bridge <nombre>`: Crea una red bridge.
    *   **Service Discovery:** Contenedores en redes bridge *definidas por el usuario* pueden resolverse por nombre. La red `bridge` *default* NO tiene esta capacidad.
*   **Mapeo de Puertos (`-p` o `--publish`):**
    *   Expone un puerto del contenedor en un puerto del host (`hostPort:containerPort`).
    *   Permite acceso externo a contenedores en redes bridge.
    *   Puede ser limitante (un puerto del host solo puede ser usado por un mapeo).
*   **Redes MACVLAN:**
    *   Creadas por el driver `macvlan` (o `transparent` en Windows).
    *   Conectan contenedores directamente a la red física/VLAN externa.
    *   Cada contenedor obtiene una MAC y una IP únicas en la red externa (parece una VM/máquina física).
    *   Buen rendimiento (sin bridges/NAT).
    *   **Requisito:** La interfaz física del host debe estar en modo promiscuo (puede no estar permitido en clouds/corporativo).
    *   `docker network create -d macvlan --subnet=... --gateway=... -o parent=eth0.VLAN_ID <nombre>`
*   **Descubrimiento de Servicios (Service Discovery):**
    *   Docker incluye un servidor DNS embebido.
    *   Contenedores en la *misma* red Docker pueden encontrarse usando sus nombres de contenedor (o alias de red, o nombres de servicio en Swarm).
    *   Configurable con `--dns` (servidores externos) y `--dns-search` (dominios de búsqueda).
*   **Balanceo de Carga Ingress (Solo Swarm):**
    *   Modo por defecto al publicar puertos (`-p` o `--publish`) en servicios Swarm.
    *   Usa una red overlay especial (`ingress`) que abarca todos los nodos.
    *   Peticiones al puerto publicado en *cualquier* nodo del swarm son redirigidas y balanceadas entre las réplicas del servicio, sin importar en qué nodo corran.
    *   *Modo Host (`--publish mode=host`):* Alternativa que salta la red ingress. Solo se puede acceder al servicio contactando un nodo que *tenga* una réplica corriendo en ese puerto.
*   **Resumen del Capítulo:** Docker Networking se basa en CNM/Libnetwork/Drivers. `bridge` es para single-host, `macvlan` para integración con red física. Service discovery integrado facilita comunicación inter-contenedor. Swarm añade balanceo de carga con la red ingress.

---

### Capítulo 14: Redes Overlay en Docker

*   **Propósito:** Crear redes virtuales L2 que conectan contenedores a través de múltiples hosts Docker (que deben estar en modo Swarm). Permite comunicación directa entre contenedores en diferentes hosts como si estuvieran en la misma red local.
*   **Requisito:** Requiere Docker Swarm activo (usa el cluster store para coordinación y seguridad).
*   **Creación:**
    *   `docker network create -d overlay <nombre_red>`: Crea una red overlay.
    *   `-o encrypted`: Opción para encriptar el tráfico de datos (VXLAN) usando AES-GCM (con posible impacto en rendimiento). El plano de control siempre está encriptado (TLS).
*   **Tecnología Subyacente (VXLAN):**
    *   Virtual Extensible LAN. Encapsula tráfico L2 dentro de paquetes UDP L3.
    *   Permite extender redes L2 sobre infraestructura L3 existente (la *red underlay*).
    *   Docker crea *VXLAN Tunnel Endpoints* (VTEPs) en cada nodo participante.
    *   Los VTEPs encapsulan/desencapsulan tráfico, usando VNIDs (VXLAN Network Identifiers) para mantener el aislamiento entre diferentes redes overlay.
    *   La red underlay solo ve tráfico UDP en el puerto 4789 (IANA standard para VXLAN).
*   **Funcionamiento en Docker Swarm:**
    *   Libnetwork gestiona la creación de la red y la distribución de la información (usando gossip sobre el cluster store).
    *   Se crea un sandbox de red en cada host con un bridge virtual (Br0) y un VTEP.
    *   Los contenedores se conectan al bridge local (Br0) mediante interfaces veth.
    *   La comunicación entre contenedores en diferentes hosts pasa por: Contenedor -> veth -> Br0 -> VTEP (encapsula) -> Red Underlay -> VTEP remoto (desencapsula) -> Br0 remoto -> veth -> Contenedor remoto.
    *   El service discovery funciona a través de la red overlay.
*   **Despliegue:** Las redes overlay solo se extienden a los nodos workers cuando un servicio que la usa agenda una tarea (contenedor) en ese nodo (propagación "lazy").
*   **Adjuntar Contenedores:**
    *   Normalmente se usan con *servicios* Swarm (`docker service create --network <overlay_net> ...`).
    *   Para contenedores *standalone* (`docker run`), la red overlay debe crearse con la opción `--attachable`.
*   **Resumen del Capítulo:** Las redes overlay de Docker proporcionan conectividad L2 multi-host segura y transparente para contenedores en un Swarm, utilizando túneles VXLAN sobre la red física existente. Simplifican enormemente la comunicación entre servicios distribuidos.

---

### Capítulo 15: Volúmenes y datos persistentes

*   **Necesidad:** Las aplicaciones *stateful* (ej: bases de datos) necesitan almacenar datos que persistan más allá del ciclo de vida de un contenedor.
*   **Almacenamiento Efímero del Contenedor:** La capa de escritura superior de un contenedor es adecuada para datos temporales, pero se elimina cuando se elimina el contenedor. No usar para datos persistentes.
*   **Volúmenes Docker:** La solución recomendada para datos persistentes.
    *   Son objetos gestionados por Docker, con su propio ciclo de vida, independientes de los contenedores.
    *   Los datos en volúmenes persisten incluso si los contenedores que los usan son eliminados.
    *   Se pueden montar en directorios específicos dentro de los contenedores.
    *   Facilitan backups, migraciones y compartir datos entre contenedores (con precauciones).
*   **Gestión de Volúmenes:**
    *   `docker volume create <nombre>`: Crea un volumen (usa driver `local` por defecto).
    *   `docker volume ls`: Lista volúmenes.
    *   `docker volume inspect <nombre>`: Muestra detalles (driver, punto de montaje en el host, etc.).
    *   `docker volume rm <nombre>`: Elimina un volumen específico (no se puede si está en uso).
    *   `docker volume prune`: Elimina *todos* los volúmenes no utilizados (¡cuidado!).
*   **Drivers de Volumen:**
    *   `local`: (Default) Almacena datos en un directorio dentro del área de Docker en el host (`/var/lib/docker/volumes/...`). Solo accesible desde ese host.
    *   *De Terceros:* Plugins para integrar con sistemas de almacenamiento externos (NFS, SANs, Cloud storage). Permiten compartir volúmenes entre múltiples hosts.
*   **Uso con Contenedores/Servicios:**
    *   `docker run --mount source=<vol_name>,target=<ruta_container> ...`: Monta un volumen. Si `vol_name` no existe, Docker lo crea (con driver `local`).
    *   Alternativa (legacy): `docker run -v <vol_name>:<ruta_container> ...`.
    *   En archivos Compose/Stack: Se definen en la sección `volumes:` y se montan en `services: ... volumes:`.
*   **Compartir Datos y Corrupción:** Al usar drivers que permiten montar el mismo volumen en contenedores en diferentes hosts, la aplicación debe ser consciente y gestionar el acceso concurrente para evitar corrupción de datos (ej: escrituras perdidas, lecturas inconsistentes).
*   **Resumen del Capítulo:** Los volúmenes Docker son el mecanismo preferido para manejar datos persistentes en contenedores. Son objetos independientes, gestionados por Docker, que desacoplan los datos del ciclo de vida del contenedor. Los drivers permiten usar almacenamiento local o sistemas externos compartidos.

---

### Capítulo 16: Seguridad en Docker

*   **Enfoque General:** Seguridad en capas (defensa en profundidad). Docker combina primitivas de seguridad de Linux con sus propias herramientas. Los defaults son moderadamente seguros.
*   **Tecnologías de Seguridad Linux (Utilizadas por Docker):**
    *   **Namespaces:** Proveen *aislamiento* de recursos (PID, Net, Mnt, IPC, User, UTS). Base de los contenedores.
    *   **Control Groups (cgroups):** Imponen *límites* al uso de recursos compartidos del host (CPU, RAM, I/O). Previenen DoS.
    *   **Capabilities:** Dividen los privilegios de `root` en unidades más finas. Docker ejecuta contenedores como `root` pero *elimina* la mayoría de las capabilities por defecto, siguiendo el principio de mínimo privilegio. Se pueden añadir/quitar capabilities específicas (`--cap-add`, `--cap-drop`).
    *   **MAC (Mandatory Access Control):** Docker aplica perfiles por defecto de AppArmor o SELinux (según la distro) para restringir acciones adicionales. Se pueden personalizar (complejo).
    *   **seccomp (Secure Computing Mode):** Filtra las llamadas al sistema (syscalls) que un contenedor puede hacer al kernel del host. Docker aplica un perfil por defecto que bloquea syscalls potencialmente peligrosos. Se pueden personalizar (complejo).
*   **Tecnologías de Seguridad Docker:**
    *   **Seguridad Swarm:** (Ver Cap 11) TLS mutuo, CA automática, rotación de certificados, tokens seguros, cluster store encriptado, redes encriptables. Seguridad robusta por defecto para el clúster.
    *   **Docker Scout:** Escaneo de vulnerabilidades en imágenes (basado en SBOMs). Identifica CVEs y sugiere remediaciones. Servicio de suscripción.
    *   **Docker Content Trust (DCT):** Permite firmar digitalmente imágenes al subirlas a un registro y verificar la firma al descargarlas. Asegura la integridad y el publicador de la imagen. Usa claves criptográficas y Notary (integrado en registros como Docker Hub). Comandos: `docker trust key generate`, `docker trust signer add`, `docker trust sign`. Se puede forzar con `export DOCKER_CONTENT_TRUST=1`.
    *   **Docker Secrets:** Mecanismo seguro para gestionar datos sensibles (contraseñas, claves API, certificados) para servicios *Swarm*.
        *   Se almacenan encriptados en el cluster store.
        *   Se transmiten encriptados a los nodos donde corren las tareas del servicio.
        *   Se montan en los contenedores como archivos en un sistema de archivos en memoria (tmpfs), *sin encriptar* para que la app los use fácilmente.
        *   Solo accesibles para los servicios explícitamente autorizados.
        *   Comandos: `docker secret create`, `docker secret ls/inspect/rm`, `docker service create --secret ...`.
*   **Resumen del Capítulo:** La seguridad en Docker se basa en múltiples capas, aprovechando mecanismos robustos de Linux (namespaces, cgroups, capabilities, MAC, seccomp) con defaults razonables, y añadiendo herramientas propias como Scout (escaneo), DCT (firmado de imágenes) y Secrets (manejo de datos sensibles en Swarm).
