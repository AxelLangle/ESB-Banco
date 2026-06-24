# ESB Bancario
 
> Enterprise Service Bus para operaciones bancarias: retiro, depósito y transferencia.
> Arquitectura de monorepo multi-equipo sobre Node.js 20.
 
## Equipos y responsables
 
| Módulo              | Equipo                        | Revisores GitHub              |
|---------------------|-------------------------------|-------------------------------|
| esb-core            | Equipo ESB                    | @axellangle @Bryan            |
| apps/app-movil      | Equipo App Móvil              | @Irving @Roberto              |
| apps/cajero-atm     | Equipo Cajero ATM             | @JoselineCasiano              |
| apps/cajero-red     | Equipo Red Interbancaria      | @JECHAPE08                    |
| services/retiro     | Equipo Retiro                 | @LuisEnrique                  |
| services/deposito   | Equipo Depósito               | @DanaeMariel                  |
| services/transferencia | Equipo Transferencia       | @Jaxe020406                   |
| shared / docs / infra | Tech Leads                 | @Bryan @Jaxe020406 @MaxCrg @axellangle |
 
## Estructura del monorepo
 
```
esb-bancario/
├── .github/          # CI/CD, CODEOWNERS, PR template
├── docs/             # Contratos, arquitectura, branching
├── esb-core/         # Núcleo del bus: adapters, routing, seguridad
├── apps/             # Apps consumidoras (app-movil, cajero-atm, cajero-red)
├── services/         # Servicios de negocio (retiro, deposito, transferencia)
├── shared/           # Constantes, tipos y helpers compartidos
└── infra/            # Docker, Kubernetes, variables de entorno
```
 
## Setup rápido (desarrollo local)
 
**Requisitos:** Node.js >= 20, npm >= 10, Docker Desktop
 
```bash
# 1. Clonar
git clone https://github.com/<org>/esb-bancario.git
cd esb-bancario
 
# 2. Variables de entorno
cp infra/env/.env.example .env
# Edita .env con tus valores locales
 
# 3. Instalar dependencias (todos los workspaces)
npm install
 
# 4. Levantar base de datos y servicios
docker compose -f infra/docker/docker-compose.yml up -d
 
# 5. Correr tests
npm test
```
 
## Documentación clave
 
| Documento | Ruta |
|-----------|------|
| Sobre canónico (spec) | docs/canonical-envelope.md |
| Diagrama de arquitectura | docs/architecture.md |
| Estrategia de ramas | docs/branching-strategy.md |
| Schema JSON del sobre | esb-core/canonical/envelope.schema.json |
| Variables de entorno | infra/env/.env.example |
 
## Convención de commits
 
```
feat(modulo): descripción corta
fix(modulo): descripción corta
docs(modulo): descripción corta
chore(modulo): descripción corta
```
 
## Ramas protegidas
 
- `main` — producción, solo merges desde `release/*`
- `develop` — integración continua, punto de partida para features
 
Ver docs/branching-strategy.md para el flujo completo.

