# Sequence Diagram: SD-UC-04 Record blood usage

| Field | Value |
|---|---|
| Use case | UC-04 Record blood usage |
| Version | v1 |
| FRs | FR-02-01, FR-02-06; BR-09 |

```mermaid
sequenceDiagram
  autonumber
  actor S as Hospital Staff
  participant P as NBSP Web App
  participant B as NBSP Backend
  participant D as NBSP Database
  S->>P: enter blood type, product, quantity, usage date, status (Used/Discarded) and reason
  P->>B: submit usage record
  B->>D: read hospital lots for that blood type and product
  D-->>B: lots with quantity and expiry date
  B->>B: validate fields and check available quantity
  alt valid and enough units (FR-02-01)
    B->>D: deduct lots earliest expiry first (BR-09) and append usage history
    D-->>B: saved
    B-->>P: success with updated inventory
    P-->>S: show confirmation and updated inventory
  else invalid or not enough units (FR-02-06)
    B-->>P: reason, nothing changed
    P-->>S: show the reason, keep the entered values for correction
  end
```
