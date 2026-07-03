# Certificado Educabot — Taller Valijas Adaptativas

Certificado web **personalizado y estático** para los participantes del Taller
Introductorio sobre el uso de las Valijas Adaptativas (Educabot · Ministerio de
Educación · Gobierno del Chubut).

Es una sola página autocontenida: recibe el nombre por la URL y dibuja el
certificado en el navegador, con botones para descargar en PNG / PDF y compartir
en redes.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | La página del certificado (autocontenida: fondo, fuente y lógica embebidos). |
| `preview.jpg` | Imagen fija para la tarjeta social (WhatsApp/Facebook/X al pegar el link). |

## Cómo funciona

El nombre se pasa por parámetros en la URL:

```
https://<DOMINIO>/?nombre=Ana&apellido=Czubaj
```

- Si `nombre`/`apellido` vienen vacíos, deriva el nombre del parámetro `email`
  (parte antes del `@`). Ej: `?email=ana.czubaj@gmail.com` → "Ana Czubaj".
- Si tampoco hay email, muestra "Participante".
- El nombre **se achica automáticamente** si es muy largo, para no pisar el
  listón/medalla de la derecha.

### Uso con Brevo (mailing)

En el botón del email, pegar:

```
https://<DOMINIO>/?nombre={{ contact.NOMBRE }}&apellido={{ contact.APELLIDO }}
```

## Cómo hostearlo

Es estático, sirve en cualquier hosting (GitHub Pages, Netlify, Vercel, servidor
propio). Con **GitHub Pages**:

1. Subí `index.html` y `preview.jpg` a la raíz de un repo.
2. **Settings → Pages → Source: `main` / `/root`**.
3. Queda publicado en `https://<usuario>.github.io/<repo>/`.

### ⚠️ Al mover a otro dominio, actualizar 3 cosas en `index.html`

Las meta tags del preview están fijas y hay que apuntarlas al dominio nuevo
(si no, la tarjeta social sigue mostrando el preview del dominio anterior):

```html
<meta property="og:image"  content="https://<DOMINIO>/preview.jpg">
<meta name="twitter:image" content="https://<DOMINIO>/preview.jpg">
```

Y `preview.jpg` tiene que quedar junto al `index.html`.

---

Generado con el generador de certificados de Educabot. La lógica de
derivar-nombre-desde-email y el auto-shrink del nombre son ajustes manuales
sobre el export del generador — si se re-exporta el HTML, hay que reaplicarlos.
