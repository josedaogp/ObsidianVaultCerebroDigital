## Sección 3 (Tema 1)
10. Templates. Cómo hacer templates, una de las partes fundamentales
11.  Cadenas (Chain) (versión antigua que ya no se usa)
12. LCEL (Langchain expression language). Ya no se usa la LLMChain, se usa '|'. --> Nota: No usar langchain.prompts import PromptTemplate, usar langchain_core.import... porque está en ese paquete. Lo mismo para la clase anterior.
13. Esquema de la arquitectura de LangChain. Muy útil para entender de donde viene cada paquete y cosa que utilizamos.
---
16. COMIENZA CON PROYECTO **CHATBOT** CON STREAMLIT. #ProyectoCurso
	1. No es from **langchain.schema** import AIMessage, HumanMessage, SystemMessage, es from **langchain_core.messages** import AIMessage, HumanMessage, SystemMessage
17. Lectura qué es Streamlit.
18. Pasos iniciales con Streamlit y cómo configurar la página. Cómo arrancar un proyecto streamlit: "streamlit run script.py"
19. Introducción a la memoria de streamlit
20. Explica los tres tipos de mensajes: Los de la IA de respuesta (AIMessage), los del humano (HumanMessage) y los de sistema (SystemMessage) que vienen del paquete langchain.schema.
21. Campo de entrada del usuario
22. Generación de la respuesta del asistente
23. 
24. Explicación del código final con chain y cómo hacer que escriba poco a poco como lo hace chatgpt.
---
## Sección 4 (Tema 2)
26. Explica cómo funcionan los runnables (LCEL: LangChain Expression Language). Vemos de dónde viene el funcionamiento del operador | .
27. Explicación miniproyecto: Análisis de setnimientos con LangChain #ProyectoCurso 
28. Adición del **procesamiento en paralelo** al proyecto anterior. Sirve para procesar cosas a la vez
29. Cómo invocar .invoke de una misma cadena **por lotes**, varias veces. Se hace en vez de con .invoke con **.batch**. Si alguna falla no pasa nada, sigue con el resto.
30. Cómo provar un **PromptTemplate** antes de pasarlo a un llm, a ver si pone lo que queríamos que pusiera.
31. Cómo utilizar el **ChatPromptTemplate** 
32. Tarea en la que modificamos el proyecto de la sección anterior (ChatBot) para utilizar ChatPromptTemplate y hacer el template dinámico según la configuración
33. 
34. 
35. Enseña el **SystemMessagePromptTemplate** y el **HumanMessagePromptTemplate**, con el que podemos hacer plantillas de cada tipo, y luego juntarlas con **ChatPromptTemplate**
36. **Output parsers** --> Cómo hacer que la salida de tu modelo sea concretamente lo que quieres: un csv, un json, un objeto Pydantic... Aquí lo pone en contexto, todavía no lo explica.
37. Explicación rápida de Pydantic
38. Explica cómo hacer que el LLM de como respuesta un Output concreto, concretamente un objeto Pydantic con las propiedades que nosotros definamos. Utiliza **llm.with_structured_output(ClasePydanticModelo)**. Además, el resultado del .invoke será un propio objeto Pydantic, teniendo así todos los métodos de Pydantic como .model_dump_json(). En los modelos de Pydantic tenemos que utilizar el **Field**() para indicarle una descripción a cada propiedad, que es lo que *tomará el modelo para saber qué tiene que poner en cada propiedad*.
---
40. COMIENZA CON EL PROYRECTO DE LA SECCIÓN: **ANALIZADOR DE CVs** #ProyectoCurso 
41. Descarga del proyecto
42. **Estructura de ficheros y directorios.** *Útil para en general, crear apps IA*
43. Hace el modelo que utilizaremos como Output Parser del llm, de pydantic. Muestra los parámetros ge y le de Field para indicar valores mínimos y máximos respectivamente que el modelo debe devolver. Por ejemplo, si quieres que devuelva un número entre 0 y 100, el Field sería: Field("Descripción que quieras", ge=0 , le=100)
44. Explica como extraer el texto de un pdf (el que pasen con el cv). Usa PyPDF2. También ha comentado que es buena práctica siempre hacer un strip() del texto que le pasaremos al llm para ahorrar tokens.
45. Ha explicado los prompts (utiliza ChatPromptTemplate, SystemMessagePromptTemplate y HumanMessagePromptTemplate)
46. Creamos el llm como tal, con su Output parser y demás.
47. Explica la interfaz de la app
48. Ejecución de la app
---
## Sección 5 (Tema 3)
49. Veremos los RAGs y BBDD Vectoriales
50. Instalamos langchain-community para utilizar cosas de terceros.
51. **Document Loaders.** --> Lo que nos referimos a los "plugins", los modulos de la comunidad, que facilitan la carga de documentos. Ha explicado concretamente el **PyPDFLoader y WebBaseLoader**
52. Explica cómo cargar datos de **Google Drive** con el **GoogleDriveLoader** y como sacar una api para ello en Google Cloud Console.
53. Expone en un documento los Docuemnt Loaders más interesantes:
	1. WebBaseLoader
	2. PyPDFLoader
	3. DirectoryLoader - Procesamiento Masivo
	4. YoutubeLoader - Contenido Multimedia (Transcripciones de vídeos)
	5. UnstructuredHTMLLoader
	6. CSVLoader - Datos Tabulares
	7. SeleniumURLLoader - JavaScript y Contenido Dinámico (páginas dinámicas)
	8. GitLoader - Repositorios de Código
54. **Text Splitter** --> Por qué no le puedes pasar un libro entero a un llm. Para hacerlo, hay que dividir el libro. Muestra qué pasa si le metemos el quijote entero. Da un error. 
55. **Text Splitter** --> Muestra cómo conseguir "pasarle" el quijote al llm. Utiliza **RecursiveCharacterTextSplitter**
	1. Chunk Size: Tamaño del fragmento (3000 aprox)
	2. Chunk Overlap: Solapamiento. Para saber lo que había antes del fragmento. (200 aprox)
56. Explica qué son los **embeddings**. Es un vector de números que representan semánticamente a sus textos.
57. Hace un embedding de dos textos parecidos y no parecidos y muestra los vectores. 
	1. Primero crea un objeto de OpenAIEmbeddings, y luego con .embed_query te devuelve el vector.
58. Explica qué son las BBDD Vectoriales y pone ejemplos: Chroma y Pinecone.
59. Hace un ejemplo con ChromaDB
60. Explica los **retrievers** que es lo que se encarga de recuperar la información de una BBDD vectorial.
61. Explica el **MultiQueryRetriever** , un retriever que utiliza IA, concretamente un llm para variar la consulta y así obtener más similitudes.
62. Lectura donde se explica claramente qué es un **retriever** y algunos de los más interesantes.
63. Explica qué es RAG (Retrieval Augmented Generation, generación aumentada mediante recuperación), que se usa para dar contexto al LLM de algo en concreto que esté por ejemplo en una BBDD vectorial.
---
64. COMIENZA CON EL PROYECTO: RAG. SISTEMA DE ASISTENCIA LEGAL Y EVALUACIÓN DE CONTRATOS.
---
103. COMIENZA CON EL PROYECTO: CHAT MULTI-USUARIO CON **MEMORIA AVANZADA**
104. Descarga
105. Configuración de la aplicación --> fichero *config.py*
106. Definición del estado extendido y modelo Pydantic. --> fichero *memory_manager.py*
	1. Memoria persistir historial. Clases de langgraph
	2. Memoria transversal con bbdd vectorial
	3. Define el estado extendido que combina mensajes con memoria vectorial
107. Definiciónn de la clase de gestión de memoria --> fichero *memory_manager.py*
	1. Crea el constructor de ModernMemoryManager, que será la clase encargada de recoger la memoria vectorial y guardarla en la bbdd vectorial
108. Inicializar la bbdd vectorial --> fichero memory_manager.py , función "\_init_vector_db"
109. Sistema de extracción inteligente --> fichero *memory_manager.py* , función \_init_extraction_system()
110. Gestión múltiples chats para un mismo usuario . Usará JSON para parámetros no importantes como el título del chat, cuando se actualizó etc, y LangGraph para la persistencia de los mensajes del chat. Fichero *memory_manager.py* , función get_user_chats() y create_new_chat()
111. update_chat_metadata() , delete_chat() , get_chat_info() , \_generate_chat_title
112. Implementación de la memoria vectorial --> save_vector_memory() , search_vector_memory()
113. 