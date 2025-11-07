<div align="center">

# 🤖 Manuelita Chatbot - Módulo 2: Agente Conversacional (v4)

**Un agente inteligente que decide la mejor manera de responder tus preguntas sobre la empresa Manuelita.**

</div>

---

## 🎯 **Propósito Principal**

Este proyecto implementa un **agente conversacional avanzado** que actúa como el cerebro de un chatbot. Su principal habilidad es el **enrutamiento inteligente**: analiza cada pregunta del usuario y decide si debe buscar la respuesta en una base de datos de documentos (`RAG`) o consultar información específica y estructurada como contactos o direcciones (`JSON`).

Esta versión (v4) soluciona errores críticos de versiones anteriores, logrando una mayor estabilidad y robustez.

---

## ✨ **Características Clave**

-   **🧠 Agente de Enrutamiento (ReAct)**: Utiliza un agente de LangChain que razona sobre la pregunta del usuario antes de actuar, eligiendo la herramienta más adecuada para la tarea.

-   **📚 Búsqueda Documental Híbrida (RAG)**:
    -   Combina búsqueda **semántica** (por significado) y por **palabra clave** (BM25) para encontrar la información más relevante en documentos `.md`.
    -   Utiliza un **Re-ranker** (`CrossEncoder`) para filtrar y ordenar los resultados, entregando solo los fragmentos de mayor calidad al modelo de lenguaje.

-   **🗂️ Búsqueda en Datos Estructurados**: Accede a un archivo `datos_estructurados.json` para obtener respuestas instantáneas y precisas a preguntas sobre:
    -   📞 Teléfonos y correos de contacto.
    -   🏢 Direcciones y sedes.
    -   ⏰ Horarios de atención.
    -   📄 NIT de la empresa.

-   **💬 Memoria Conversacional**: Recuerda el historial de la conversación para poder responder preguntas de seguimiento de forma coherente.

-   **🌐 Interfaz Web Amigable**: Construido con **Gradio** para una interacción fácil y visualmente agradable.

---

## 🛠️ **Arquitectura del Sistema**

El flujo de trabajo del agente sigue estos pasos:

1.  **[ Usuario ]** → Envía una pregunta a través de la interfaz de **Gradio**.

2.  **[ Agente ReAct ]** → Recibe la pregunta y analiza su intención.
    > *¿Es una pregunta general o busca un dato específico?*

3.  **[ Decisión ]** → El agente elige una de las dos herramientas disponibles:

    -   **Herramienta A: `busqueda_documental_manuelita`**
        -   *Activación*: Preguntas generales como "¿cuál es la historia de la empresa?" o "¿qué productos ofrecen?".
        -   *Acción*: Activa el pipeline de RAG para buscar en los documentos `.md`.

    -   **Herramienta B: `buscar_datos_especificos_manuelita`**
        -   *Activación*: Preguntas con palabras clave como "teléfono", "NIT", "dirección", "horario".
        -   *Acción*: Realiza una búsqueda rápida en el archivo `datos_estructurados.json`.

4.  **[ Respuesta ]** → La herramienta seleccionada devuelve la información encontrada.

5.  **[ Agente ReAct ]** → Formula una respuesta final en lenguaje natural y la presenta al usuario.

---

## 🔧 **Correcciones Clave en la Versión 4**

Esta versión se enfoca en la estabilidad y soluciona dos problemas fundamentales:

1.  **`ERROR: missing tool_names` (Solucionado)**
    -   **Problema**: El prompt del agente no recibía la lista de herramientas al inicializarse, causando un fallo.
    -   **Solución**: Se utilizó el método `.partial()` en el `PromptTemplate` para "inyectar" los nombres y descripciones de las herramientas antes de crear el agente.
        ```python
        # ¡LA CORRECCIÓN CRÍTICA!
        agent_prompt = agent_prompt.partial(
            tools="\n".join([f"{tool.name}: {tool.description}" for tool in tools]),
            tool_names=", ".join([tool.name for tool in tools]),
        )
        ```

2.  **Error de Indentación en `buscar_datos_especificos` (Solucionado)**
    -   **Problema**: La lógica de búsqueda se ejecutaba fuera del bloque `try...except`, lo que podía causar un error si el archivo `.json` no se encontraba.
    -   **Solución**: Se corrigió la indentación para asegurar que la lógica de búsqueda solo se ejecute si el archivo se carga correctamente.

---

## 🚀 **Cómo Ejecutar el Proyecto**

Sigue estos pasos para poner en marcha el agente conversacional en tu máquina local.

### **1. Prerrequisitos**

-   Python 3.8 o superior.
-   Una clave de API de **Google (Gemini)**.

### **2. Clonar el Repositorio**

```bash
git clone <div align="center">

# 🤖 Manuelita Chatbot - Módulo 2: Agente Conversacional (v4)

**Un agente inteligente que decide la mejor manera de responder tus preguntas sobre la empresa Manuelita.**

</div>

---

## 🎯 **Propósito Principal**

Este proyecto implementa un **agente conversacional avanzado** que actúa como el cerebro de un chatbot. Su principal habilidad es el **enrutamiento inteligente**: analiza cada pregunta del usuario y decide si debe buscar la respuesta en una base de datos de documentos (`RAG`) o consultar información específica y estructurada como contactos o direcciones (`JSON`).

Esta versión (v4) soluciona errores críticos de versiones anteriores, logrando una mayor estabilidad y robustez.

---

## ✨ **Características Clave**

-   **🧠 Agente de Enrutamiento (ReAct)**: Utiliza un agente de LangChain que razona sobre la pregunta del usuario antes de actuar, eligiendo la herramienta más adecuada para la tarea.

-   **📚 Búsqueda Documental Híbrida (RAG)**:
    -   Combina búsqueda **semántica** (por significado) y por **palabra clave** (BM25) para encontrar la información más relevante en documentos `.md`.
    -   Utiliza un **Re-ranker** (`CrossEncoder`) para filtrar y ordenar los resultados, entregando solo los fragmentos de mayor calidad al modelo de lenguaje.

-   **🗂️ Búsqueda en Datos Estructurados**: Accede a un archivo `datos_estructurados.json` para obtener respuestas instantáneas y precisas a preguntas sobre:
    -   📞 Teléfonos y correos de contacto.
    -   🏢 Direcciones y sedes.
    -   ⏰ Horarios de atención.
    -   📄 NIT de la empresa.

-   **💬 Memoria Conversacional**: Recuerda el historial de la conversación para poder responder preguntas de seguimiento de forma coherente.

-   **🌐 Interfaz Web Amigable**: Construido con **Gradio** para una interacción fácil y visualmente agradable.

---

## 🛠️ **Arquitectura del Sistema**

El flujo de trabajo del agente sigue estos pasos:

1.  **[ Usuario ]** → Envía una pregunta a través de la interfaz de **Gradio**.

2.  **[ Agente ReAct ]** → Recibe la pregunta y analiza su intención.
    > *¿Es una pregunta general o busca un dato específico?*

3.  **[ Decisión ]** → El agente elige una de las dos herramientas disponibles:

    -   **Herramienta A: `busqueda_documental_manuelita`**
        -   *Activación*: Preguntas generales como "¿cuál es la historia de la empresa?" o "¿qué productos ofrecen?".
        -   *Acción*: Activa el pipeline de RAG para buscar en los documentos `.md`.

    -   **Herramienta B: `buscar_datos_especificos_manuelita`**
        -   *Activación*: Preguntas con palabras clave como "teléfono", "NIT", "dirección", "horario".
        -   *Acción*: Realiza una búsqueda rápida en el archivo `datos_estructurados.json`.

4.  **[ Respuesta ]** → La herramienta seleccionada devuelve la información encontrada.

5.  **[ Agente ReAct ]** → Formula una respuesta final en lenguaje natural y la presenta al usuario.

---

## 🔧 **Correcciones Clave en la Versión 4**

Esta versión se enfoca en la estabilidad y soluciona dos problemas fundamentales:

1.  **`ERROR: missing tool_names` (Solucionado)**
    -   **Problema**: El prompt del agente no recibía la lista de herramientas al inicializarse, causando un fallo.
    -   **Solución**: Se utilizó el método `.partial()` en el `PromptTemplate` para "inyectar" los nombres y descripciones de las herramientas antes de crear el agente.
        ```python
        # ¡LA CORRECCIÓN CRÍTICA!
        agent_prompt = agent_prompt.partial(
            tools="\n".join([f"{tool.name}: {tool.description}" for tool in tools]),
            tool_names=", ".join([tool.name for tool in tools]),
        )
        ```

2.  **Error de Indentación en `buscar_datos_especificos` (Solucionado)**
    -   **Problema**: La lógica de búsqueda se ejecutaba fuera del bloque `try...except`, lo que podía causar un error si el archivo `.json` no se encontraba.
    -   **Solución**: Se corrigió la indentación para asegurar que la lógica de búsqueda solo se ejecute si el archivo se carga correctamente.

---

## 🚀 **Cómo Ejecutar el Proyecto**

Sigue estos pasos para poner en marcha el agente conversacional en tu máquina local.

### **1. Prerrequisitos**

-   Python 3.8 o superior.
-   Una clave de API de **Google (Gemini)**.

### **2. Clonar el Repositorio**

```bash
git clone https://github.com/tu-usuario/tu-repositorio.git
cd tu-repositorio
