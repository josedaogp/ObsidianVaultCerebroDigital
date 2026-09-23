# 🌰 Cheatsheet de CocoaPods

Este cheatsheet cubre los aspectos esenciales de CocoaPods, el gestor de dependencias para proyectos de Swift y Objective-C en plataformas Apple (iOS, macOS, watchOS, tvOS).

## 🔍 Conceptos Básicos

*   **CocoaPods:** Gestor de dependencias a nivel de aplicación para proyectos de Swift y Objective-C.  Simplifica la integración y gestión de bibliotecas de terceros (pods).
*   **Pod:** 📦 Unidad básica de código gestionada por CocoaPods (una biblioteca o framework).  Un pod tiene un nombre, versión, y puede tener dependencias de otros pods.
*   **Podfile:** 📄 Archivo de texto (en Ruby) que define las dependencias de tu proyecto.  Se encuentra en la raíz del proyecto Xcode.
*   **`pod install`:** ⬇️ Comando que instala las dependencias especificadas en el `Podfile`.  Crea el archivo `Podfile.lock` y el workspace de Xcode (`.xcworkspace`).
*   **`pod update`:** ⬆️ Comando que actualiza las dependencias a las últimas versiones compatibles (según las restricciones del `Podfile`).
*   **`Podfile.lock`:** 🔒 Archivo que registra las versiones *exactas* de los pods instalados.  Garantiza que todos los miembros del equipo usen las mismas versiones.  *Debe* estar versionado en Git.
*   **Workspace (`.xcworkspace`):** 📂 Archivo de Xcode generado por CocoaPods que contiene tu proyecto original y el proyecto `Pods`.  *Siempre* debes usar el workspace después de instalar CocoaPods.
* **Podspec:** Archivo que describe un Pod.
* **Repositorio de Specs (Specs Repo):** Repositorio central de CocoaPods donde se encuentran las especificaciones de los pods.

## 🚀 Instalación

CocoaPods se instala como una gema de Ruby.

```bash
sudo gem install cocoapods  # Instalar CocoaPods globalmente
```

*   **Recomendación:**  Usar un gestor de versiones de Ruby (como RVM o rbenv) para evitar conflictos con la versión de Ruby del sistema.
*  **Bundler:** Si usas Bundler en tu proyecto, puedes añadir `gem 'cocoapods'` a tu `Gemfile` y ejecutar `bundle install`.

## ⚙️ Configuración de un Proyecto con CocoaPods

1.  **Navega al directorio de tu proyecto Xcode:**

    ```bash
    cd ruta/a/tu/proyecto
    ```

2.  **Crea un `Podfile` (si no existe):**

    ```bash
    pod init
    ```
    *   Esto crea un `Podfile` básico.

3.  **Edita el `Podfile`:**  Añade las dependencias (pods) que necesitas.

    ```ruby
    # Podfile

    # Plataforma y versión mínima (iOS, macOS, etc.)
    platform :ios, '13.0'

    # Ignorar todas las advertencias de todos los pods
    # inhibit_all_warnings!

    # Usar frameworks en lugar de bibliotecas estáticas (requerido para Swift)
    use_frameworks!

    # Target de tu aplicación
    target 'MiApp' do
      # Pods para tu aplicación
      pod 'Alamofire', '~> 5.0'  # Pod Alamofire, versión ~> 5.0 (ej. 5.1, 5.2, pero no 6.0)
      pod 'SnapKit', '>= 5.0.0' # Versión >= 5.0.0
      pod 'Kingfisher' # Sin especificar versión (usará la última)

       # Pods solo para pruebas
       target 'MiAppTests' do
         inherit! :search_paths #Hereda los pods del target principal
         pod 'Quick'
         pod 'Nimble'
       end
    end

    # Pods para un target específico (ej. una extensión)
    # target 'MiExtension' do
    #   pod 'MiOtraLibreria'
    # end

    # Post install hook (opcional)
    # post_install do |installer|
    #   # ...
    # end
    ```

    *   **`platform`:**  Especifica la plataforma y la versión mínima de despliegue.
    *   **`use_frameworks!`:**  Indica que se usen frameworks (recomendado para Swift).  Si usas bibliotecas estáticas, omite esta línea.
    *   **`target`:**  Define un *target* de Xcode (ej. tu aplicación, una extensión, tests).
    *   **`pod 'Nombre', 'Versión'`:**  Especifica un pod y, opcionalmente, una versión.
        *   `'~> 5.0'`:  Versión optimista (ej. 5.1, 5.2, pero no 6.0).
        *   `'>= 5.0.0'`:  Versión mayor o igual a 5.0.0.
        *   `'5.0'` : Versión exacta
        *   Sin versión:  Última versión.
    *   **`pod 'MiPod', :path => '../MiPod'`:**  Usar un pod local (para desarrollo).
    *   **`pod 'MiPod', :git => 'https://github.com/usuario/MiPod.git'`:** Usar un pod desde un repositorio Git.
    *  **`inherit! :search_paths`**: Hereda los pods del target padre.

4.  **Instala los pods:**

    ```bash
    pod install
    ```
    *   Este comando descarga e instala los pods, crea el archivo `Podfile.lock`, y genera el workspace de Xcode (`.xcworkspace`).

5.  **Abre el workspace (`.xcworkspace`) en Xcode:**  A partir de ahora, *siempre* usa el workspace, no el proyecto original (`.xcodeproj`).

## 📝 Comandos de CocoaPods

*   **`pod install`:**  Instala los pods definidos en el `Podfile`.  Se debe ejecutar después de crear o modificar el `Podfile`.  Genera el `Podfile.lock`.
*   **`pod update`:**  Actualiza los pods a las últimas versiones compatibles (según las restricciones del `Podfile`).
*   **`pod update [POD_NAME]`:** Actualiza un pod específico.
*   **`pod outdated`:**  Muestra los pods que tienen versiones más recientes disponibles.
*   **`pod search [QUERY]`:**  Busca pods.
*   **`pod repo update`:**  Actualiza el repositorio local de specs de CocoaPods (obtiene las últimas especificaciones de los pods).
*   **`pod init`:** Crea un `Podfile` básico.
*   **`pod deintegrate`:**  Elimina la integración de CocoaPods de tu proyecto.
*   **`pod clean`:**  Limpia la caché de CocoaPods.
*   **`pod env`**: Muestra información del entorno.

## 🗂️ `Podfile` Avanzado

*   **Múltiples targets:**

    ```ruby
    target 'MiApp' do
      pod 'Alamofire'
    end

    target 'MiExtension' do
      pod 'OtraLibreria'
    end
    ```

*   **Abstract targets:**

    ```ruby
      abstract_target 'Base' do
          pod 'Alamofire'
          target 'App1'
          target 'App2'
      end
    ```

*   **Especificar un subspec:**

    ```ruby
    pod 'Firebase/Analytics'  # Solo el subspec Analytics de Firebase
    ```

*   **Usar un repositorio de specs privado:**

    ```ruby
    source 'https://github.com/mi-organizacion/Specs.git'  # Añadir un source (además del oficial)

    pod 'MiPodPrivado', :source => 'https://github.com/mi-organizacion/Specs.git'
    ```

*   **Hooks (`pre_install`, `post_install`):**  Permiten ejecutar scripts antes o después de la instalación de los pods.
* **Inhibir warnings:**
   *  `inhibit_all_warnings!`: Globalmente
   *  `pod 'MiPod', :inhibit_warnings => true`: Para un pod específico.

## 📦 Crear un Pod (Podspec)

1.  **Crear el archivo `.podspec`:**

    ```bash
    pod spec create MiPod
    ```
    *   Esto crea un archivo `MiPod.podspec` con una plantilla.

2.  **Editar el `.podspec`:**

    ```ruby
    Pod::Spec.new do |s|
      s.name             = 'MiPod'
      s.version          = '0.1.0'
      s.summary          = 'Una breve descripción de MiPod.'
      s.description      = <<-DESC
    Una descripción más larga de MiPod.
                           DESC
      s.homepage         = 'https://github.com/mi-usuario/MiPod'
      s.license          = { :type => 'MIT', :file => 'LICENSE' }
      s.author           = { 'Mi Nombre' => 'mi@email.com' }
      s.source           = { :git => 'https://github.com/mi-usuario/MiPod.git', :tag => s.version.to_s }

      s.ios.deployment_target = '13.0'  # Plataforma y versión mínima

      s.source_files = 'MiPod/Classes/**/*'  # Archivos fuente del pod

      # s.resource_bundles = {  # Recursos (opcional)
      #   'MiPod' => ['MiPod/Assets/*.png']
      # }

       s.dependency 'Alamofire', '~> 5.0' # Dependencias del pod
       s.swift_version = '5.0' # Versión de Swift
    end
    ```

3.  **Validar el `.podspec`:**

    ```bash
    pod lib lint
    ```

4.  **Publicar el pod (opcional):**

    *   **Repositorio de specs privado:**  Agrega el `.podspec` a tu repositorio de specs privado.
    *   **CocoaPods Trunk (repositorio público):**
        ```bash
        pod trunk register mi@email.com 'Mi Nombre' --description='Mi descripción' # Registrarse
        pod trunk push MiPod.podspec  # Publicar el pod
        ```

## ✨ Otros Aspectos Importantes

*   **`Podfile.lock`:**  *Siempre* debe estar versionado en Git.  Garantiza que todos los desarrolladores usen las mismas versiones de los pods.
*   **Workspace (`.xcworkspace`):**  Úsalo *siempre* después de instalar CocoaPods.
*   **Actualizar CocoaPods:**  `sudo gem update cocoapods` (o `bundle update cocoapods` si usas Bundler).
*   **Frameworks vs. Static Libraries:**  CocoaPods puede integrar los pods como frameworks (recomendado para Swift) o como bibliotecas estáticas.
*  **Usar la última versión estable de Xcode y CocoaPods.**
* **Repositorios Git:** CocoaPods se integra bien con Git.

Este cheatsheet cubre los aspectos más importantes de CocoaPods, desde la instalación y configuración hasta la creación y publicación de pods. ¡Guárdalo y consúltalo a menudo!