# Life City BIM Tab · página de descarga

Página de venta y descarga del add-in **Life City BIM Tab** para Autodesk Revit 2024 / 2025 / 2026.
Publicada con GitHub Pages con la identidad visual de [lifecity.com.co](https://www.lifecity.com.co/).

## Cómo funciona el pago

1. El botón **Pagar con Wompi** lleva al link de pago `https://checkout.wompi.co/l/EtzxPB`.
2. Al aprobarse, Wompi devuelve al comprador a esta página con `?id=<transacción>` en la URL.
3. La página consulta la API pública de Wompi (`production.wompi.co/v1/transactions/<id>`).
   Si el estado es `APPROVED`, habilita el botón de descarga y lo recuerda en el navegador.
4. Quien cerró la pestaña puede pegar el ID de su comprobante para desbloquear la descarga.

> **Falta un paso en el panel de Wompi:** hay que configurar la *URL de redirección* del link
> de pago apuntando a esta página, para que el paso 2 ocurra automáticamente. Sin eso, el
> comprador tiene que pegar el ID a mano.

## Configuración

Todo lo ajustable está en el bloque `const LC = {...}` al final de `index.html`:
link de Wompi, endpoint de la API, ruta del instalador y clave de recordatorio.

## Aviso sobre la protección del archivo

El instalador vive en una carpeta de nombre aleatorio y solo se enlaza tras verificar el pago,
pero **una página estática no puede proteger un archivo de verdad**: la ruta queda en el código
del navegador y el repositorio es público. Sirve para compradores honestos, no contra alguien
decidido. Para un control real hay que servir el archivo desde un backend que valide la
transacción, o enviarlo por correo tras el pago.
