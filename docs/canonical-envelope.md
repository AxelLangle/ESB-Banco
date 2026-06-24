# Sobre Canónico — ESB Bancario
 
> Versión: 1.0  |  Propietario: Equipo ESB  |  Revisores: @axellangle @Bryan
 
## ¿Qué es el sobre canónico?
 
Todo mensaje que viaja por el bus, sin importar el protocolo de origen
(REST, ISO 8583 o SWIFT MT), se transforma a esta estructura unificada.
Los adaptadores inbound son los responsables de hacer esa transformación.
 
## Estructura base
 
```json
{
  "header": {
    "transaction_id": "UUID-V4",
    "timestamp":      "2026-06-20T15:44:07Z",
    "consumer_app":   "",
    "target_service": "",
    "protocol_source":""
  },
  "security_context": {
    "auth_method":      "",
    "pci_dss_compliant": true,
    "fraud_check": {
      "ml_score":    0.0,
      "ofac_cleared": false
    }
  },
  "payload": {},
  "business_rules": {}
}
```
 
## Campos del header
 
| Campo | Tipo | Valores permitidos | Descripción |
|-------|------|--------------------|-------------|
| transaction_id | string (UUID v4) | — | Identificador único de la transacción |
| timestamp | string (ISO 8601) | — | Fecha y hora UTC del mensaje |
| consumer_app | enum | APP_MOVIL, CAJERO_ATM, CAJERO_RED | Origen del mensaje |
| target_service | string | — | Servicio de destino (ej. retiro/nacional) |
| protocol_source | enum | REST_JSON, ISO_8583, SWIFT_MT | Protocolo de entrada |
 
## Campos de security_context
 
| Campo | Tipo | Valores permitidos | Descripción |
|-------|------|--------------------|-------------|
| auth_method | enum | JWT_RS256, PIN_BLOCK_MFA | Método de autenticación del consumidor |
| pci_dss_compliant | boolean | true / false | Cumplimiento PCI-DSS del canal |
| fraud_check.ml_score | number [0-1] | — | Score del modelo antifraude (0 = seguro) |
| fraud_check.ofac_cleared | boolean | true / false | Resultado del screening OFAC |
 
## Mapeo por aplicación consumidora
 
### App Móvil (iOS / Android)
- Protocolo entrada: REST, HTTPS, TLS 1.3
- consumer_app: `APP_MOVIL`
- protocol_source: `REST_JSON`
- auth_method: `JWT_RS256`
 
### Cajero ATM / Sucursal
- Protocolo entrada: ISO 8583, MPLS, TLS 1.3
- consumer_app: `CAJERO_ATM`
- protocol_source: `ISO_8583`
- auth_method: `PIN_BLOCK_MFA`
- Nota: el adapter traduce campos fijos ISO (ej. DE 4 → amount)
 
### Red Interbancaria
- Protocolo entrada: ISO 8583, SWIFT MT, MPLS
- consumer_app: `CAJERO_RED`
- protocol_source: `SWIFT_MT`
 
## Reglas de modificación
 
1. Ningún equipo puede modificar este documento ni el schema JSON sin abrir un PR.
2. El PR requiere aprobación de **ambos** revisores del Equipo ESB (@axellangle y @Bryan).
3. Cambios breaking (eliminar o renombrar campos) requieren versión mayor y plan de migración.
 
## Schema de validación
 
Ver: `esb-core/canonical/envelope.schema.json`

