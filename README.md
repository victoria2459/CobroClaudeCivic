# Claude Team · Billing Compartido

Dashboard para repartir el costo de una suscripción Claude Team entre múltiples organizaciones. Desarrollado por Civic House para uso compartido con Fonselp.

## ¿Qué hace?

- Muestra los miembros del equipo agrupados por organización
- Lista todas las facturas con el reparto exacto (quién paga qué)
- Tiene un botón para registrar aprobaciones de uso adicional, con historial
- Los links van directo a las Google Sheets con el detalle completo

## Deploy

Está deployado en Vercel conectado a este repo. Cualquier push a `main` actualiza el sitio automáticamente.

## Archivos

```
index.html    → todo el dashboard (HTML + CSS + JS en un solo archivo)
vercel.json   → configuración de Vercel para sitio estático
README.md     → este archivo
```

## Cómo agregar una nueva organización

Cuando se sume un nuevo equipo al Claude Team, hay que hacer tres cambios en `index.html`:

### 1. Agregar la org en el resumen (cards superiores)

Buscá el bloque `<div class="summary">` y agregá una card nueva:

```html
<div class="card" style="border-top: 3px solid #854F0B;">
  <div class="card-label">Nombre Org</div>
  <div class="card-amount" style="color:#854F0B;">$0.00</div>
  <div class="card-sub">N miembros · X%</div>
  <div class="bar-wrap"><div style="height:100%;background:#854F0B;border-radius:2px;width:X%"></div></div>
</div>
```

### 2. Agregar el bloque de miembros

Buscá `<div class="members-grid">` y agregá un bloque nuevo:

```html
<div class="org-block">
  <div class="org-header" style="background:#FAEEDA;">
    <span class="org-name" style="color:#854F0B;">Nombre Org</span>
    <span class="org-count">N miembros · $X/mes base</span>
  </div>
  <!-- una .member-row por cada persona -->
  <div class="member-row">
    <div class="avatar" style="background:#FAC775;color:#854F0B;">AB</div>
    <span class="member-name">Nombre</span>
    <span class="seat-badge standard">Standard</span>
  </div>
</div>
```

### 3. Agregar la org como opción en el selector de aprobaciones

Buscá `<select id="beneficiary">` y agregá un `<optgroup>`:

```html
<optgroup label="Nombre Org">
  <option>Persona 1</option>
  <option>Persona 2</option>
</optgroup>
```

### 4. Actualizar las facturas futuras

El array `INVOICES` en el JS tiene todas las facturas. Cada entrada tiene esta forma:

```js
{ date:"27 abr", num:"0026", type:"seats", desc:"descripción", total:200.00, civic:40.00, fonselp:160.00 }
```

Cuando haya una nueva org, agregás un campo más (ej: `nuevaorg: 0`) en cada entrada futura, y actualizás los totales del resumen.

## Colores por organización

| Org | Color principal | Light |
|-----|----------------|-------|
| Civic House | `#185FA5` | `#E6F1FB` |
| Fonselp | `#0F6E56` | `#E1F5EE` |
| Próxima org | `#854F0B` (amber) | `#FAEEDA` |
| Siguiente | `#993556` (pink) | `#FBEAF0` |

## Google Sheets vinculadas

- [Facturas y reparto](https://docs.google.com/spreadsheets/d/1z8cRMq_avEwkHKjDlxBlylP40xLHMrTXVCTaKib9ipE)
- [Registro de aprobaciones](https://docs.google.com/spreadsheets/d/1qDa9TwLQkAGGG-yS7OfCTksOqLc9J71SsDpxIqYVD3s)

## Roadmap

- [ ] Conectar el botón de aprobación directamente a Google Sheets via API
- [ ] Notificación por mail/Slack cuando se aprueba un uso adicional
- [ ] Vista por organización con login simple
- [ ] Importación automática de nuevas facturas
