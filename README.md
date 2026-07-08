# Certificado Educabot — Primer Encuentro de Formación (Valijas Adaptativas)

Certificado web **personalizado y estático** para los participantes del Primer
Encuentro de Formación sobre el uso de las Valijas Adaptativas (Educabot ·
Ministerio de Educación · Gobierno del Chubut).

Es una sola página autocontenida: recibe un **link firmado** por persona, verifica
la firma en el navegador y dibuja el certificado, con botones para descargar en
PNG / PDF y compartir en redes.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | La página del certificado (autocontenida: fondo, fuente, lógica y clave pública embebidos). |
| `preview.jpg` | Imagen fija para la tarjeta social (WhatsApp/Facebook/X al pegar el link). |

## Seguridad — links firmados (importante)

Para evitar que cualquiera edite el nombre en la URL y se genere un certificado
falso, **el nombre va firmado criptográficamente** (ECDSA P-256). Cada persona
recibe un link único con esta forma:

```
https://<DOMINIO>/?d=<nombre-codificado>&s=<firma>
```

La página lleva embebida solo la **clave pública** y verifica la firma con
Web Crypto. Si alguien cambia el `d` (el nombre), la firma no valida y aparece
**"Certificado no válido"**. Solo los links generados con la **clave privada**
(que NO está en este repo) funcionan.

> Los links con `?nombre=...` (sin firma) ya **no** funcionan — es a propósito.

### Uso con Brevo (mailing)

Cada contacto tiene su link firmado cargado como atributo (ej. `LINK`). En el
botón del email:

```
{{ contact.LINK }}
```

## Cómo hostearlo

Es estático, sirve en cualquier hosting (Railway, GitHub Pages, Netlify, servidor
propio). Subí `index.html` y `preview.jpg` juntos a la raíz.

### ⚠️ Al mover a otro dominio, actualizar 2 cosas en `index.html`

```html
<meta property="og:image"  content="https://<DOMINIO>/preview.jpg">
<meta name="twitter:image" content="https://<DOMINIO>/preview.jpg">
```

Y `preview.jpg` tiene que quedar junto al `index.html`. (Los links firmados
funcionan en cualquier dominio — la firma es sobre el nombre, no sobre la URL.)

---

La verificación de firma y el auto-ajuste del nombre son ajustes manuales sobre
el export del generador — si se re-exporta el HTML, hay que reaplicarlos.
