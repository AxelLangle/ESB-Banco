 
# shared/
 
Código compartido entre todos los módulos del monorepo.
Ningún equipo debe duplicar estas constantes o utilidades en su propio módulo.
 
## Contenido
 
| Archivo | Descripción |
|---------|-------------|
| `constants/limits.js` | Límites operacionales de todos los servicios |
| `constants/networks.js` | Redes y procesadores permitidos |
| `constants/currencies.js` | Monedas aceptadas por servicio |
| `utils/index.js` | Helpers comunes (formateo, fechas, UUID) |
| `types/index.d.ts` | Tipos TypeScript del sobre canónico |
 
## Cómo importar
 
```js
// Desde cualquier módulo del monorepo
const { RETIRO, DEPOSITO } = require("shared/constants/limits");
const { RETIRO_NACIONAL_NETWORKS } = require("shared/constants/networks");
```
 
## Reglas
 
1. Para agregar o modificar constantes, abrir PR con base en `develop`.
2. Los PRs a shared/ requieren aprobación de Tech Leads.
   Revisores: @Bryan @Jaxe020406 @MaxCrg @axellangle
3. No agregar lógica de negocio aquí — solo constantes, tipos y utilidades puras.

