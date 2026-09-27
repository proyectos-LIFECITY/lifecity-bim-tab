# Life City BIM Tab · página de descarga

Landing del add-in para Autodesk Revit 2024 / 2025 / 2026, publicada con GitHub Pages
con la identidad visual de [lifecity.com.co](https://www.lifecity.com.co/).

La página ofrece **dos opciones**:

| Opción | Precio | Entrega |
| --- | --- | --- |
| Puente **Claude MCP** | Gratis | Enlace directo, sin pasarela |
| **BIM Tab completo** (5 paneles, MCP incluido) | COP 415.000 | Link de Wompi + verificación de transacción |

## Archivos publicados

| Ruta | Qué es |
| --- | --- |
| `index.html` | Landing con los dos planes |
| `gracias/index.html` | Página de retorno de Wompi: verifica y entrega el instalador completo |
| `descarga/LifeCityMCP_Revit_Setup_1.0.0.exe` | Instalador gratuito (enlace directo) |
| `descarga/53d2ba7f4ad7954d/LifeCityBIM_Tab_Setup_1.0.0.exe` | Instalador completo (tras verificar el pago) |

## Cómo funciona el pago

1. El botón **Pagar con Wompi** lleva al link `https://checkout.wompi.co/l/PqmRlA`.
2. Al aprobarse, Wompi devuelve al comprador con `?id=<transacción>` en la URL.
3. Se consulta la API pública de Wompi (`production.wompi.co/v1/transactions/<id>`, permite CORS).
   Si el estado es `APPROVED`, se desbloquea la descarga y se recuerda en el navegador.
4. Quien cerró la pestaña puede pegar el ID de su comprobante para desbloquearla.

Tanto `index.html` como `gracias/index.html` aceptan el retorno y el ID pegado a mano.

### URL de redirección de Wompi

En el panel de Wompi, el link de pago debe redirigir a:

    https://proyectos-lifecity.github.io/lifecity-bim-tab/gracias/

> **Falta un paso en el panel de Wompi:** configurar esa *URL de redirección*. Sin eso el
> comprador paga pero no vuelve solo, y tiene que pegar el ID de su comprobante a mano.

## Configuración

Todo lo ajustable del plan de pago está en el bloque `const LC = {...}` al final de
`index.html` (y su equivalente en `gracias/index.html`): link de Wompi, endpoint de la API,
ruta del instalador y clave de recordatorio. El plan gratuito no pasa por ahí: su enlace
está directo en el HTML.

## Aviso sobre la protección del archivo de pago

El instalador completo vive en una carpeta de nombre aleatorio y solo se enlaza tras verificar
el pago, pero **una página estática no puede proteger un archivo de verdad**: la ruta queda en
el código del navegador y el repositorio es público. Sirve para compradores honestos, no contra
alguien decidido. Para control real hay que servirlo desde un backend que valide la transacción,
o enviarlo por correo tras el pago.

## Los dos productos

Ambos salen del mismo `src/LifeCityBIM.Tab/LifeCityBIM.Tab.csproj`:

| | Add-in completo | MCP gratuito |
| --- | --- | --- |
| Compilación | `dotnet build -c "Release 2026"` | `... -p:McpOnly=true` |
| Ensamblado | `LifeCityBIM.Tab.dll` | `LifeCityBIM.Mcp.dll` |
| Manifiesto | `LifeCityBIM.Tab.addin` | `LifeCityBIM.Mcp.addin` |
| Paneles | 5 | solo «Claude MCP» |
| Instalador | `build-installer.ps1` | `build-mcp-installer.ps1` |

Tienen `ClientId` y `AppId` distintos, así que pueden convivir instalados en el mismo Revit:
`App.cs` reutiliza el panel si ya existe en vez de intentar crearlo dos veces.
