Se está desarrollando un plugin para Obsidian, la aplicación de toma de notas.
El propósito del plugin es permitir a los usuarios de Obsidian gestionar notas de reuniones de manera eficiente.
El objetivo final es facilitar la creación de notas, agilizando sobre todo el linkado de la nota. La idea parte de que la creación de la nota partirá de otra nota. Entonces, estando en cualquier nota, se podrá seleccionar un comando de la paleta de comandos para crear una nota para una determinada categoría. Entonces, el usuario seleccionará la subcarpeta de la categoría que haya seleccionado y le pondrá un nombre. El plugin creará automáticamente la nota donde haya indicado, y además, escribirá la nota nueva linkada en la nota en la que esté.
En la configuración del plugin, el usuario podrá crear una lista de categorías. Por ejemplo, Reuniones, Personas, Proyectos, etc. Cada categoría, será una carpeta en la raíz del vault.
A su vez, cada categoría puede tener subcarpetas. Por ejemplo, en la categoría (y carpeta) Reuniones, puede haber subcarpetas como "Dailys" o "Decisiones".
El plugin creará un comando de creación de notas por cada categoría. Por ejemplo:
"Crear Reunion"
"Crear Persona"
"Crear Proyecto"
