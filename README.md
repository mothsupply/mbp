# Moth Binary Protocol (MBP)

Protocole binaire ultra-rapide, zéro allocation, `#![no_std]` pour le framework Mothc.

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

