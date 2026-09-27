# Life City MCP para Revit · página de descarga

Página de descarga del add-in **gratuito** Life City MCP para Autodesk Revit 2024 / 2025 / 2026.
Publicada con GitHub Pages con la identidad visual de [lifecity.com.co](https://www.lifecity.com.co/).

El sitio ya no vende nada: `index.html` ofrece un único archivo, gratis y sin pasarela de pago.

## Qué se publica

| Ruta | Qué es |
| --- | --- |
| `index.html` | Landing del puente MCP gratuito |
| `descarga/LifeCityMCP_Revit_Setup_1.0.0.exe` | Instalador gratuito (el único enlazado) |

El instalador se regenera con `installer/build-mcp-installer.ps1` y se copia a `descarga/`.

## Archivos heredados de la etapa de pago

Siguen en el repositorio pero **ya no se enlazan desde ninguna página**:

- `gracias/index.html` — página de retorno de Wompi que verificaba la transacción y entregaba
  el add-in completo. Sigue funcionando si se le pasa `?id=<transacción>`.
- `descarga/53d2ba7f4ad7954d/LifeCityBIM_Tab_Setup_1.0.0.exe` — instalador del add-in completo
  (cinco paneles), el producto de COP 415.000.

Se conservan por si el add-in completo se sigue vendiendo con el link de Wompi
`https://checkout.wompi.co/l/PqmRlA`, que redirige a `gracias/`. Si esa venta se descarta,
ambos se pueden borrar sin tocar nada más.

> **Aviso:** una página estática no puede proteger un archivo. La ruta del instalador de pago
> queda en un repositorio público, así que la carpeta de nombre aleatorio solo disuade a
> curiosos. Para control real hay que servirlo desde un backend que valide la transacción.

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
