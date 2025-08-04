# WhatsApp Pizzería Bot con LangChain y Qdrant (Docker Ready)

Este proyecto implementa un agente conversacional conectado a la API oficial de WhatsApp (Meta) para toma de pedidos en una pizzería, utilizando una base de conocimiento vectorial con LangChain y Qdrant.

---

## 🧱 Estructura del proyecto

```
.
├── docker-compose.yml
├── Dockerfile
├── .env.example
├── requirements.txt
├── webhook/
│   └── app.py               # Webhook Flask para WhatsApp API
├── langchain_rag/
│   └── rag_processor.py     # Lógica LangChain + Qdrant
└── documents/
    └── menu.txt             # Base de conocimiento (documento de ejemplo)
```

---

## 🚀 ¿Qué tecnologías se usan?

- [WhatsApp Cloud API (Meta)](https://developers.facebook.com/docs/whatsapp)
- [LangChain](https://www.langchain.com/)
- [OpenAI (Embeddings + LLM)](https://platform.openai.com/)
- [Qdrant Vector DB](https://qdrant.tech/)
- [Docker + Docker Compose](https://docs.docker.com/compose/)
- [Flask](https://flask.palletsprojects.com/)

---

## ⚙️ Configuración

### 1. Clona este repositorio y entra a la carpeta

```bash
git clone <REPO_URL>
cd whatsapp-meta-rag-bot-qdrant-docker
```

### 2. Crea el archivo `.env`

```bash
cp .env.example .env
```

Edita `.env` y coloca tus claves:

```env
VERIFY_TOKEN=tu_token_verificacion
ACCESS_TOKEN=tu_token_de_meta
PHONE_NUMBER_ID=tu_numero_meta
OPENAI_API_KEY=tu_clave_openai
QDRANT_HOST=qdrant
QDRANT_PORT=6333
```

### 3. Levanta los servicios

```bash
docker compose up --build
```

Esto iniciará:
- Qdrant (puerto 6333)
- Webhook Flask (puerto 5000)

---

## 📌 Configura el Webhook en Meta (WhatsApp API)

1. Usa [ngrok](https://ngrok.com/) para exponer tu puerto 5000:
   ```bash
   ngrok http 5000
   ```

2. Copia la URL pública de ngrok y configúrala en tu panel de WhatsApp Business como:
   ```
   https://<ngrok_subdomain>.ngrok.io/webhook
   ```

3. Usa tu `VERIFY_TOKEN` para completar la verificación.

---

## 🧠 ¿Cómo funciona?

1. Un cliente envía un mensaje por WhatsApp.
2. El webhook lo recibe, procesa el texto con LangChain + OpenAI.
3. Qdrant devuelve la información más relevante desde `menu.txt`.
4. El bot responde automáticamente por WhatsApp.

---

## ✏️ Editar base de conocimiento

Edita el archivo `documents/menu.txt` con tu contenido y luego reinicia los contenedores para revectorizar:

```bash
docker compose down
docker compose up --build
```

---

## 🧪 Probar localmente

Puedes enviar una prueba desde el simulador de WhatsApp Cloud API o usando Postman hacia `/webhook`.

---

## 🛠 Próximos pasos

- Registrar pedidos en base de datos
- Añadir autenticación por cliente
- Soporte multi-documento o colecciones por categoría

---

## 📩 Soporte

Para asistencia técnica, consulta la [documentación de WhatsApp Cloud API](https://developers.facebook.com/docs/whatsapp/) o contacta soporte de OpenAI si es necesario.
