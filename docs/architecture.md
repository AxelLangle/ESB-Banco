 
# Arquitectura del Sistema — ESB Bancario
 
> Versión: 1.0  |  Propietarios: @axellangle @Bryan @Jaxe020406 @MaxCrg
 
## Visión general
 
El sistema sigue una arquitectura de Enterprise Service Bus (ESB):
las aplicaciones consumidoras NO se comunican directamente con los servicios
de negocio. Todo el tráfico pasa por el bus, que normaliza, enruta, valida
seguridad y orquesta las llamadas.
 
## Flujo de un mensaje (extremo a extremo)
 
```
Consumidor          Inbound Adapter      ESB Core              Servicio
──────────          ───────────────      ────────              ────────
                                                               
App Móvil ─REST──►  rest-json/        ┐
Cajero ATM─ISO8583►  iso8583/         ├─► Canonical  ─► CBR Router ─► retiro/
Cajero Red─SWIFT──►  swift-mt/        ┘   Envelope       │            deposito/
                                                          │            transferencia/
                                           Orchestrator ◄─┘
                                                │
                                           Security Module
                                           (JWT / PIN / OFAC / Fraud ML)
```
 
## Componentes principales
 
### 1. Inbound Adapters (`esb-core/adapters/`)
Transforman el protocolo nativo al sobre canónico JSON.
- `rest-json/` — para App Móvil
- `iso8583/`  — para Cajero ATM
- `swift-mt/` — para Red Interbancaria
 
### 2. Módulo Canónico (`esb-core/canonical/`)
Define y valida el sobre canónico. Es el contrato central del sistema.
Ver docs/canonical-envelope.md.
 
### 3. CBR Router (`esb-core/routing/router.js`)
Content-Based Routing: lee el campo `header.target_service` del sobre
y determina a qué servicio de negocio debe dirigirse el mensaje.
 
### 4. Orchestrator (`esb-core/orchestration/orchestrator.js`)
Coordina llamadas a múltiples servicios cuando una operación lo requiere
(ej. validar saldo antes de ejecutar retiro).
 
### 5. Security Module (`esb-core/security/`)
Corre en paralelo para todas las transacciones:
- JWT RS256 / PIN Block MFA — autenticación
- Fraud ML Scorer — score antifraude 0.0-1.0
- OFAC Checker — screening listas sancionados
 
### 6. Servicios de Negocio (`services/`)
Microservicios independientes por dominio:
- `retiro/` — nacional, internacional, sin-tarjeta
- `deposito/` — efectivo, cuenta, cheque
- `transferencia/` — spei, interna, swift, programada
 
## Decisiones de diseño
 
| Decisión | Justificación |
|----------|---------------|
| Monorepo | Facilita compartir tipos y constantes entre equipos |
| Sobre canónico único | Un solo contrato elimina acoplamiento entre adapters y servicios |
| CBR en ESB Core | El router es el único que conoce la topología de servicios |
| Seguridad transversal | PCI-DSS exige validación en cada transacción, independiente del servicio |
 
## Tecnologías base
 
| Capa | Tecnología |
|------|------------|
| Runtime | Node.js 20 |
| Protocolo interno | JSON over HTTP |
| Base de datos | PostgreSQL 16 |
| Contenedores (dev) | Docker Compose |
| Orquestación (prod) | Kubernetes |
| CI/CD | GitHub Actions |

