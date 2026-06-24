 
# Estrategia de Ramas — ESB Bancario
 
> Propietarios: @Bryan @Jaxe020406 @MaxCrg @axellangle
 
## Modelo base
 
GitHub Flow extendido con rama `develop` como punto de integración.
Nadie hace push directo a `develop` ni a `main` — todo entra por PR.
 
## Ramas protegidas
 
| Rama | Propósito | Merge desde |
|------|-----------|-------------|
| `main` | Producción | Solo desde `release/*` |
| `develop` | Integración continua | Desde `feature/*` y `bugfix/*` |
| `release/vX.Y.Z` | Estabilización pre-producción | Desde `develop` |
| `hotfix/TICKET-desc` | Fix crítico en producción | Desde `main`, vuelve a `main` y `develop` |
 
## Prefijos por módulo
 
### Apps consumidoras
```
feature/app-movil/TICKET-descripcion
bugfix/app-movil/TICKET-descripcion
feature/cajero-atm/TICKET-descripcion
bugfix/cajero-atm/TICKET-descripcion
feature/cajero-red/TICKET-descripcion
bugfix/cajero-red/TICKET-descripcion
```
 
### ESB Core
```
feature/esb-core/TICKET-descripcion
feature/esb-core/adapter-rest
feature/esb-core/adapter-iso8583
feature/esb-core/adapter-swift
feature/esb-core/security-ofac
feature/esb-core/monitoring-dashboard
```
 
### Servicios de negocio
```
feature/svc-retiro/nacional
feature/svc-retiro/internacional
feature/svc-retiro/sin-tarjeta
feature/svc-deposito/efectivo
feature/svc-deposito/cuenta
feature/svc-deposito/cheque
feature/svc-transferencia/spei
feature/svc-transferencia/interna
feature/svc-transferencia/swift
feature/svc-transferencia/programada
```
 
## Flujo de trabajo de una feature
 
```bash
# 1. Crear rama desde develop
git checkout develop
git pull origin develop
git checkout -b feature/app-movil/AM-12-auth-jwt
 
# 2. Desarrollar con commits semánticos
git commit -m "feat(app-movil): implementar validación JWT RS256"
 
# 3. Push y abrir PR hacia develop
git push origin feature/app-movil/AM-12-auth-jwt
# Abrir PR en GitHub → base: develop
 
# 4. El CI corre automáticamente (lint + tests)
# 5. Mínimo 1 aprobación del equipo dueño
# 6. Merge squash → la rama feature se elimina
```
 
## Convención de commits
 
```
feat(modulo):     nueva funcionalidad
fix(modulo):      corrección de bug
refactor(modulo): sin cambio funcional
docs(modulo):     solo documentación
chore(modulo):    build, deps, config
```
 
## Reglas importantes
 
1. El PR siempre apunta a `develop`, nunca directamente a `main`.
2. El sobre canónico (esb-core/canonical/) requiere aprobación adicional del Equipo ESB.
3. Las ramas `feature/*` se eliminan después del merge.
4. Los hotfixes se bifurcan desde `main` y hacen merge a `main` Y a `develop`.

