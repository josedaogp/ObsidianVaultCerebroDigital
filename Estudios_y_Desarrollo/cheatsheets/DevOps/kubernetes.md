#  Kubernetes (K8s) Cheatsheet: Orquestación de Contenedores 🚀

## 🌟 **Conceptos Fundamentales**

*   **Kubernetes (K8s):** Una plataforma de *código abierto* para la *orquestación de contenedores*.  Automatiza el despliegue, el escalado, la gestión y la operación de aplicaciones en contenedores.
*   **Clúster (Cluster):**  Un conjunto de máquinas (nodos) que ejecutan aplicaciones en contenedores gestionadas por Kubernetes.
*   **Nodo (Node):**  Una máquina (física o virtual) en un clúster de Kubernetes.  Los nodos ejecutan los *pods*.
*   **Pod:**  La unidad *más pequeña y básica* de despliegue en Kubernetes.  Un pod representa un *conjunto de contenedores* (uno o más) que se ejecutan en el mismo nodo y comparten recursos (almacenamiento, red).  Normalmente, un pod ejecuta un solo contenedor principal, pero puede tener contenedores *sidecar* (ej: para logging, monitoring).
*   **Despliegue (Deployment):**  Define el *estado deseado* de una aplicación (qué imagen de contenedor usar, cuántas réplicas, etc.).  Kubernetes se encarga de mantener ese estado deseado.  Los deployments gestionan *ReplicaSets*.
*   **ReplicaSet:**  Asegura que un número específico de *réplicas* de un pod se estén ejecutando en todo momento.  Normalmente no se usan directamente; se usan a través de Deployments.
*   **Servicio (Service):**  Una forma abstracta de *exponer una aplicación* que se ejecuta en un conjunto de pods como un *servicio de red*.  Proporciona una IP y un nombre DNS estables, y *balancea la carga* entre los pods.
*   **Namespace:**  Un mecanismo para *aislar* grupos de recursos dentro de un mismo clúster.  Útil para organizar aplicaciones, equipos, entornos (ej: `dev`, `staging`, `prod`), etc.  Por defecto, Kubernetes tiene los namespaces `default`, `kube-system`, `kube-public`.
*   **Etiqueta (Label):**  Pares clave/valor que se adjuntan a los objetos de Kubernetes (pods, deployments, services, etc.).  Se usan para *seleccionar* y *filtrar* objetos.
*   **Anotación (Annotation):**  Pares clave/valor que se adjuntan a los objetos, pero *no* se usan para seleccionar objetos.  Se usan para añadir metadatos no identificativos.
*   **ConfigMap:**  Almacena datos de configuración (no confidenciales) como pares clave/valor o archivos.  Los pods pueden consumir los ConfigMaps como variables de entorno, argumentos de línea de comandos o archivos de configuración.
*   **Secret:**  Almacena información *confidencial* (contraseñas, tokens, claves SSH).  Similar a ConfigMap, pero los datos se almacenan de forma más segura.
*   **Volumen (Volume):**  Un directorio (con datos) que es accesible para los contenedores de un pod.  Los volúmenes persisten más allá del ciclo de vida de un contenedor individual.  Kubernetes soporta muchos tipos de volúmenes (ej: `emptyDir`, `hostPath`, volúmenes de proveedores de nube, etc.).
*   **PersistentVolume (PV):**  Un *recurso* de almacenamiento en el clúster (provisionado por un administrador).
*   **PersistentVolumeClaim (PVC):**  Una *solicitud* de almacenamiento por parte de un usuario.  Los PVCs se vinculan a PVs.
*  **Ingress**:  Expone servicios a tráfico externo (HTTP/HTTPS).  Actúa como un punto de entrada al clúster.
*   **Controlador (Controller):**  Un bucle de control que observa el estado del clúster y realiza cambios para acercarlo al estado deseado.  Ejemplos: Deployment controller, ReplicaSet controller, StatefulSet controller, etc.
*  **StatefulSet**:  Gestiona el despliegue y escalado de un conjunto de pods, garantizando el orden y la unicidad.  Útil para aplicaciones con estado (ej: bases de datos).
*  **DaemonSet**:  Asegura que *todos* (o algunos) nodos ejecuten una copia de un pod.  Útil para agentes de logging, monitoring, etc.
* **Job**:  Crea uno o más pods y se asegura de que se completen con éxito.  Útil para tareas *batch*.
*  **CronJob**:  Ejecuta *jobs* en un horario (similar a `cron` en Linux).
* **Horizontal Pod Autoscaler (HPA)**:  Escala automáticamente el número de pods en un deployment, replicaset o statefulset según la utilización de CPU o métricas personalizadas.
* **Kubelet**: Agente que se ejecuta en *cada nodo* del clúster.  Se asegura de que los contenedores se estén ejecutando en los pods.
*  **kube-proxy**:  Proxy de red que se ejecuta en cada nodo.  Mantiene las reglas de red y permite la comunicación con los pods.
*  **kube-apiserver**:  El componente del plano de control que expone la API de Kubernetes.
* **kube-scheduler**:  Componente del plano de control que decide en qué nodo se ejecuta un pod.
*  **kube-controller-manager**:  Componente del plano de control que ejecuta los controladores (Deployment, ReplicaSet, etc.).
* **etcd**:  Almacén de datos clave-valor *consistente* y *altamente disponible* que Kubernetes usa para almacenar todos los datos del clúster.
* **kubectl**:  Herramienta de línea de comandos para interactuar con un clúster de Kubernetes.

## 💻 **Comandos de `kubectl` (Comandos Comunes)**

*   **`kubectl get`:**  Lista recursos.

    ```bash
    kubectl get pods                # Lista todos los pods en el namespace actual
    kubectl get pods -n kube-system  # Lista los pods en el namespace kube-system
    kubectl get deployments
    kubectl get services
    kubectl get nodes
    kubectl get configmaps
    kubectl get secrets
    kubectl get pv                 # PersistentVolumes
    kubectl get pvc                # PersistentVolumeClaims
    kubectl get namespaces
    kubectl get all                # Lista (casi) todos los recursos
    kubectl get <tipo> <nombre> -o yaml # Obtiene la definición YAML de un recurso
    ```

*   **`kubectl describe`:**  Muestra información detallada sobre un recurso.

    ```bash
    kubectl describe pod mi-pod
    kubectl describe deployment mi-deployment
    kubectl describe node mi-nodo
    ```

*   **`kubectl create`:**  Crea un recurso a partir de un archivo o de la entrada estándar.

    ```bash
    kubectl create -f mi-deployment.yaml
    kubectl create namespace mi-namespace
    # Crear un recurso a partir de un string YAML:
    kubectl create -f - <<EOF
    apiVersion: v1
    kind: Pod
    # ...
    EOF
    ```

*   **`kubectl apply`:**  Aplica una configuración a un recurso (crea el recurso si no existe, o lo actualiza si existe).  *Recomendado* para gestionar recursos de forma declarativa.

    ```bash
    kubectl apply -f mi-deployment.yaml
    ```

*   **`kubectl delete`:**  Elimina un recurso.

    ```bash
    kubectl delete pod mi-pod
    kubectl delete deployment mi-deployment
    kubectl delete service mi-servicio
    kubectl delete -f mi-recurso.yaml
    kubectl delete namespace mi-namespace # ¡Cuidado! Elimina todos los recursos dentro del namespace
    ```

*   **`kubectl exec`:**  Ejecuta un comando dentro de un contenedor.

    ```bash
    kubectl exec -it mi-pod -- bash  # Abre una shell interactiva dentro del pod
    ```

*   **`kubectl logs`:**  Muestra los logs de un contenedor.

    ```bash
    kubectl logs mi-pod
    kubectl logs -f mi-pod     # Sigue los logs en tiempo real
    kubectl logs mi-pod -c mi-contenedor # Muestra los logs de un contenedor específico dentro del pod
    ```

*   **`kubectl edit`:**  Edita un recurso en el servidor (abre un editor de texto).  *No recomendado* para uso general (mejor usar `kubectl apply`).

*   **`kubectl scale`:**  Escala el número de réplicas de un deployment, replicaset o statefulset.

    ```bash
    kubectl scale deployment mi-deployment --replicas=5
    ```

*   **`kubectl rollout`:**  Gestiona el despliegue de una nueva versión de una aplicación.
    *   `kubectl rollout status deployment/mi-deployment`:  Muestra el estado del despliegue.
    *   `kubectl rollout history deployment/mi-deployment`:  Muestra el historial de despliegues.
    *   `kubectl rollout undo deployment/mi-deployment`:  Revierte a una versión anterior.
    *   `kubectl rollout pause deployment/mi-deployment`: Pausa un rollout
    *   `kubectl rollout resume deployment/mi-deployment`: Resume un rollout
    *   `kubectl rollout restart deployment/mi-deployment`: Reinicia un rollout

*   **`kubectl config`:**  Gestiona la configuración de `kubectl`.
    *   `kubectl config view`:  Muestra la configuración actual.
    *   `kubectl config current-context`:  Muestra el contexto actual.
    *   `kubectl config use-context <contexto>`:  Cambia al contexto especificado.
    *   `kubectl config get-contexts`: Lista los contextos disponibles.

*   **`kubectl cluster-info`:**  Muestra información sobre el clúster.
*   **`kubectl explain`:** Muestra la documentación de un recurso (ej: `kubectl explain pod`, `kubectl explain deployment.spec`).
*   **`kubectl port-forward`:**  Redirige un puerto local a un puerto de un pod.
*   **`kubectl proxy`:**  Inicia un proxy a la API de Kubernetes.
*   **`kubectl label`**: Añade o modifica etiquetas de un recurso.
* **`kubectl annotate`**: Añade o modifica anotaciones de un recurso.
*  **`kubectl autoscale`**:  Configura el escalado automático (Horizontal Pod Autoscaler).
* **`kubectl top`**: Muestra el uso de recursos (CPU, memoria) de nodos y pods.
* **`kubectl cordon`, `kubectl uncordon`, `kubectl drain`**:  Gestiona la programación de pods en un nodo (cordon: marca el nodo como no programable, uncordon: lo marca como programable, drain: desaloja los pods del nodo).

## 📝 **Archivos de Manifiesto (YAML)**

*   Los recursos de Kubernetes se definen generalmente en archivos YAML.
*   **Campos comunes:**
    *   `apiVersion`:  Versión de la API de Kubernetes.
    *   `kind`:  Tipo de recurso (ej: `Pod`, `Deployment`, `Service`, `ConfigMap`, etc.).
    *   `metadata`:  Metadatos del recurso (nombre, etiquetas, anotaciones, etc.).
        *   `name`:  Nombre del recurso.
        *   `namespace`:  Namespace al que pertenece el recurso.
        *   `labels`:  Etiquetas.
        *   `annotations`:  Anotaciones.
    *   `spec`:  Especificación del estado deseado del recurso.  El contenido de `spec` depende del tipo de recurso.

```yaml
# Ejemplo:  Despliegue (Deployment)
apiVersion: apps/v1  # API version para Deployments
kind: Deployment
metadata:
  name: mi-deployment
  labels:
    app: mi-app
spec:
  replicas: 3  # Número de réplicas (pods)
  selector:  # Selector de pods (coincide con las etiquetas de los pods)
    matchLabels:
      app: mi-app
  template:  # Plantilla para los pods
    metadata:
      labels:
        app: mi-app  # Etiquetas de los pods
    spec:
      containers:
      - name: mi-contenedor
        image: mi-imagen:latest
        ports:
        - containerPort: 8080
        env: #Variables de entorno
          - name: VARIABLE
            value: "valor"
        resources: #Recursos que va a usar el contenedor.
            limits:  #Límites máximos
              memory: "128Mi"
              cpu: "500m"
            requests: #Recursos que se reservan
              memory: "64Mi"
              cpu: "250m"
      # volumes, imagePullSecrets, etc.
```

```yaml
# Ejemplo: Servicio (Service)
apiVersion: v1
kind: Service
metadata:
  name: mi-servicio
spec:
  selector:
    app: mi-app  # Selecciona los pods con la etiqueta app=mi-app
  ports:
    - protocol: TCP
      port: 80  # Puerto del servicio
      targetPort: 8080  # Puerto del contenedor
  type: ClusterIP # Tipo de servicio (ClusterIP, NodePort, LoadBalancer, ExternalName)
```

*   **Tipos de servicio (`spec.type` en un Service):**
    *   **`ClusterIP` (por defecto):**  Expone el servicio en una IP interna del clúster.  Solo accesible desde dentro del clúster.
    *   **`NodePort`:**  Expone el servicio en un puerto *estático* en cada nodo del clúster.  Accesible desde fuera del clúster usando `<IP_del_nodo>:<NodePort>`.
    *   **`LoadBalancer`:**  Expone el servicio usando un balanceador de carga del proveedor de nube (ej: ELB en AWS, Cloud Load Balancing en GCP, Azure Load Balancer).
    *   **`ExternalName`:**  Mapea el servicio a un nombre DNS externo.

## ➕ **Temas Avanzados**

*   **Helm:**  Gestor de paquetes para Kubernetes.  Simplifica el despliegue y la gestión de aplicaciones complejas.
*   **Operadores (Operators):**  Patrón para empaquetar, desplegar y gestionar *aplicaciones Kubernetes nativas*.  Extienden la API de Kubernetes para automatizar tareas específicas de la aplicación.
*   **CRDs (Custom Resource Definitions):**  Permiten definir *tus propios* tipos de recursos en Kubernetes.
*   **Service Mesh (Istio, Linkerd, etc.):**  Infraestructura para gestionar la comunicación entre microservicios (control de tráfico, observabilidad, seguridad).
*   **Knative:**  Plataforma para construir, desplegar y gestionar aplicaciones *serverless* y basadas en eventos en Kubernetes.
*   **Seguridad:**
    *   **RBAC (Role-Based Access Control):**  Control de acceso basado en roles.
    *   **Network Policies:**  Control del tráfico de red entre pods.
    *   **Pod Security Policies (PSPs):**  (Obsoleto en Kubernetes 1.25) Control de la seguridad de los pods.
    *   **Secrets:**  Gestión de información confidencial.
    *  **Audit Logging**: Registro de auditoría.
*   **Monitoreo y Logging:**  Prometheus, Grafana, Elasticsearch, Fluentd, Kibana (EFK stack), etc.
*  **Ingress Controllers**:  Gestionan el tráfico entrante (HTTP/HTTPS) al clúster (ej: Nginx Ingress Controller, Traefik).
* **GitOps:**  Uso de Git como fuente de verdad para la infraestructura y las aplicaciones (ej: Argo CD, Flux).