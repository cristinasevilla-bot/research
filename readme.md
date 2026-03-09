# SalónReach 💇‍♀️ — CON PROXY SEGURO

Herramienta de IA para generar mensajes de Instagram DM personalizados para vender un bot de WhatsApp a salones de belleza.

## Qué hace

1. Introduces el perfil de Instagram de un salón (handle o URL)
2. Añades señales de dolor observadas (opcional)
3. La IA analiza el perfil y genera **2 variantes de mensaje** personalizadas listas para copiar y enviar

## Stack

- HTML/CSS/JS puro — sin frameworks, sin build step
- Anthropic Claude API (claude-sonnet-4-20250514)

## Deploy en Render (gratis)

### 1. Sube a GitHub

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/salon-reach.git
git push -u origin main
```

### 2. Deploy en Render

1. Ve a [render.com](https://render.com) → New → **Static Site**
2. Conecta tu repo de GitHub
3. Configuración:
   - **Build Command**: *(dejar vacío)*
   - **Publish Directory**: `.`
4. Click **Deploy**

### 3. Añadir tu API key de Anthropic

La API key se inyecta automáticamente en claude.ai. Si lo despliegas en tu propio servidor necesitarás un backend proxy para no exponer la key.

**Opción rápida con Render Web Service (Node.js proxy):**

Crea un `server.js`:

```js
const express = require('express');
const app = express();
app.use(express.json());
app.use(express.static('.'));

app.post('/api/messages', async (req, res) => {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify(req.body)
  });
  const data = await response.json();
  res.json(data);
});

app.listen(3000);
```

Y en Render → Environment → añade `ANTHROPIC_API_KEY` como variable de entorno.

## Mejoras futuras

- [ ] Scraping automático de bio desde handle de Instagram
- [ ] Historial de mensajes generados
- [ ] A/B tracking de qué variante convierte más
- [ ] Exportar lista de salones contactados + estado
