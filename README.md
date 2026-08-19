# Moth Binary Protocol (MBP) 🦋

*MBP* est un protocole de communication binaire ultra-rapide, à zéro allocation, conçu spécifiquement pour l'architecture microservices du framework Mothc.
​Il remplace les formats lourds (comme JSON ou XML) par un flux d'octets strictement typé, prédictible et exécutable avec une complexité temporelle de \mathcal{O}(1). Ce protocole est la colonne vertébrale des échanges internes (RPC) du système e-commerce de Mothc.




## Structure de la Trame (8 octets + Payload)

| Offset | Champ | Type | Description |
| :--- | :--- | :--- | :--- |
| `0..2` | `message_id` | `u16` | Identifiant de l'action |
| `2..4` | `status` | `u16` | Code de statut / retour |
| `4..8` | `payload_len` | `u32` | Taille $N$ de la charge utile |
| `8..` | `payload` | `&[u8]` | Données brutes ($N$ octets) |

## Utilisation rapide

```rust
use mbp::MbpFrame;

let buffer: [u8; 12] = [
    0x00, 0x01,             // message_id = 1
    0x00, 0xC8,             // status = 200
    0x00, 0x00, 0x00, 0x04, // payload_len = 4
    0xDE, 0xAD, 0xBE, 0xEF  // payload
];

let frame = MbpFrame::parse(&buffer).unwrap();
assert_eq!(frame.message_id, 1);
assert_eq!(frame.payload, &[0xDE, 0xAD, 0xBE, 0xEF]);

