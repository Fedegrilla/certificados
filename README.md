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

## Agregar participantes nuevos (equipo — requiere la clave privada)

Cada link es válido solo si está **firmado** con la clave privada
(`cert_private_key.pem`, que NO está en este repo y hay que guardar en secreto).
La clave pública embebida en `index.html` solo sirve para *verificar*, no para
*firmar*. Por eso sumar gente nueva = firmarle el nombre.

Herramienta: `firmar_certificados.py` (queda junto a la clave privada, fuera del repo).

1. Completar `nuevos_contactos.csv` (una fila por persona):
   ```
   EMAIL,NOMBRE,APELLIDO
   ana@mail.com,Ana,Czubaj
   ```
2. Correr:  `python firmar_certificados.py`
3. Se genera `nuevos_links.csv` (con la columna `LINK` firmada) → importar en Brevo
   mapeando `LINK` a un atributo. En el botón del mail: `{{ contact.LINK }}`.

**Cómo saber si un link es válido:** abrilo. Muestra el nombre = válido;
dice "Certificado no válido" = firma ausente o incorrecta. Nadie puede fabricar
un link válido sin la clave privada.

> Si cambia el dominio del hosting, editar la línea `BASE = "..."` del script.

---

La verificación de firma y el auto-ajuste del nombre son ajustes manuales sobre
el export del generador — si se re-exporta el HTML, hay que reaplicarlos.
