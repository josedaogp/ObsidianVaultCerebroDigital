Resumen del Proyecto: Asistente de Compras con IA de Amazon

Dirigido a Stakeholders:

Hemos desarrollado un innovador asistente de compras virtual, impulsado por Inteligencia Artificial, diseñado para transformar la experiencia de compra en Amazon. Esta plataforma ofrece a los usuarios una interfaz de chat intuitiva donde pueden describir en lenguaje natural los productos que necesitan. Nuestro sistema inteligente procesa estas solicitudes y proporciona recomendaciones de productos personalizadas y al instante.

Funcionalidades Clave que Aportan Valor:

Interacción Conversacional Inteligente: Los usuarios chatean con un asistente virtual que comprende sus necesidades, similar a hablar con un experto en ventas.
Recomendaciones de Productos Altamente Personalizadas: La IA analiza la consulta del usuario y, conectándose a un motor de recomendación backend, sugiere los artículos más adecuados de Amazon. Esto aumenta la probabilidad de conversión y la satisfacción del cliente.
Descubrimiento Proactivo de Productos: La página principal muestra secciones dinámicas como "Productos Destacados" y "Ofertas del Día", incentivando la exploración y destacando promociones o artículos populares.
Experiencia de Usuario Optimizada: Un diseño moderno, atractivo y fácil de usar, totalmente adaptado a dispositivos móviles y de escritorio, asegura una navegación fluida.
El Papel Crucial de la Base de Datos para el Éxito y la Escalabilidad:

Para que nuestro asistente de compras funcione de manera eficiente, ofrezca valor real y pueda crecer, una base de datos robusta es fundamental en varias áreas:

Catálogo Extenso y Actualizado de Productos: El corazón de nuestro sistema de recomendación (el motor de IA en el backend) depende de una base de datos masiva y constantemente actualizada con todos los productos de Amazon. Esta base de datos debe contener detalles como nombres, descripciones, precios, imágenes, valoraciones, y número de reseñas para que las recomendaciones sean precisas y relevantes.
Gestión Dinámica de Contenido Promocional: Las secciones "Productos Destacados" y "Ofertas del Día" no deben ser estáticas. Para maximizar su impacto, esta información debe residir en una base de datos. Esto permitiría al equipo de marketing o producto actualizar estos listados fácilmente y en tiempo real, sin necesidad de modificar el código de la aplicación, adaptándose a campañas o tendencias.
Historial de Conversaciones y Personalización (Potencial Futuro): Para ofrecer una experiencia aún más personalizada y permitir a los usuarios, por ejemplo, retomar conversaciones anteriores o recibir recomendaciones basadas en su historial, necesitaríamos almacenar las interacciones del chat. Esto requeriría una base de datos para guardar los mensajes y asociarlos (potencialmente) a perfiles de usuario.
Análisis de Datos y Mejora Continua: Almacenar las consultas de los usuarios y la efectividad de las recomendaciones (datos que residirían en una base de datos) nos proporcionaría información valiosísima para entender mejor las necesidades de los clientes, optimizar los algoritmos de IA y mejorar continuamente la oferta de productos y la experiencia general.
En esencia, este proyecto no solo busca mejorar la forma en que los clientes encuentran productos, sino también crear una plataforma inteligente que pueda adaptarse, aprender y crecer, siendo la base de datos un pilar fundamental para esta visión.

Dirigido al Equipo de Product Owners:

El proyecto actual consiste en el frontend de una aplicación de chat que actúa como un asistente de compras para productos de Amazon, utilizando un backend de IA para las recomendaciones.

Funcionalidades Implementadas en el Frontend:

Interfaz de Chat Principal:
Los usuarios ingresan sus consultas sobre productos en un área de texto.
La conversación (mensajes del usuario y del bot) se muestra en un formato de chat.
Se incluye un mensaje de bienvenida inicial y placeholders dinámicos para guiar al usuario.
Se gestionan visualmente los estados de carga (mientras se espera la respuesta del bot) y los mensajes de error.
Los mensajes del bot que contienen URLs de productos se formatean como enlaces clicables directos a Amazon.
Integración con el Servicio de Recomendación (Backend):
Al enviar un mensaje, se realiza una petición POST al endpoint http://mtgvpn.ddns.net:8000/recomendar-articulo.
Este endpoint recibe la pregunta del usuario y devuelve la respuesta generada por la IA.
Visualización de Productos (Actualmente Mockups):
Existen secciones para "Productos Destacados" y "Ofertas del Día".
Los datos para estos productos (featuredProducts, dailyDeals) están actualmente hardcodeados en el frontend.
Se utiliza un componente ProductCard para mostrar detalles del producto (nombre, precio, imagen, valoración, etc.).
Experiencia de Usuario (UX) y Diseño:
Componentes reutilizables como Header y Footer.
Una sección de bienvenida que explica el propósito y cómo usar el chat.
Funcionalidades UX como scroll automático al último mensaje, scroll a la sección de chat en móviles, y textarea que se autoajusta en altura.
Identificación de Funcionalidades que Requieren o se Beneficiarían de una Base de Datos:

Mensajes de Chat (Message):

Entidad de Datos: Message { id: string, text: string, sender: "user" | "bot", timestamp: Date }.
Necesidad de Base de Datos: Actualmente, los mensajes se almacenan en el estado del componente React (useState), lo que significa que se pierden si el usuario recarga la página o cierra la sesión. Para persistir el historial de conversaciones (por ejemplo, si se implementan cuentas de usuario) o para análisis de patrones de consulta, estos mensajes deberían almacenarse en una base de datos.
Impacto para el Producto: Permitiría a los usuarios revisar sus consultas anteriores. Para el negocio, permitiría analizar las interacciones para mejorar el bot y entender mejor las necesidades del cliente.
Productos (Product):

Entidad de Datos: Product { id: string, name: string, price: number, originalPrice?: number, image: string, rating: number, reviews: number }.
Necesidad de Base de Datos (Backend): Es crucial entender que el servicio backend (/recomendar-articulo) ya depende intrínsecamente de una base de datos masiva de productos para poder buscar y generar recomendaciones relevantes. Esta es la fuente principal de la información de productos que el bot utiliza.
Necesidad de Base de Datos (Para contenido dinámico en Frontend):
Productos Destacados (featuredProducts): Actualmente es un array estático. Para que el equipo de producto/marketing pueda actualizar estos productos dinámicamente (ej. según temporada, stock, popularidad) sin requerir un nuevo despliegue de frontend, esta lista debería ser obtenida de una base de datos (posiblemente a través de un CMS o una API interna).
Ofertas del Día (dailyDeals): Misma situación que los productos destacados. Necesitan ser gestionables y actualizables desde una fuente de datos externa.
Impacto para el Producto: Mayor flexibilidad para curar el contenido de la página principal, realizar campañas promocionales efectivas y asegurar que la información de los productos destacados/ofertas sea siempre actual.
Perfiles de Usuario (Potencial Futuro - User):

Entidad de Datos (Ejemplo): User { userId: string, preferences: object, savedItems: Product[], chatHistoryIds: string[] }.
Necesidad de Base de Datos: Si se decide implementar funcionalidades como cuentas de usuario, guardar preferencias de búsqueda, listas de deseos, o asociar historiales de chat a usuarios específicos, se requerirá una base de datos para almacenar esta información.
Impacto para el Producto: Permitiría una personalización mucho más profunda, mejor retención de usuarios y funcionalidades avanzadas.
Feedback sobre Recomendaciones (Potencial Futuro - Feedback):

Entidad de Datos (Ejemplo): Feedback { queryText: string, recommendedProductId: string, userRating: number, comment?: string }.
Necesidad de Base de Datos: Si se implementa un sistema para que los usuarios califiquen la utilidad de las recomendaciones, estos datos de feedback necesitarían ser almacenados para análisis y para el reentrenamiento/ajuste del modelo de IA.
Impacto para el Producto: Ciclo de mejora continua para la calidad de las recomendaciones, basado en la retroalimentación directa del usuario.