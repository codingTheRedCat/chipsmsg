# chipsmsg
A simple protocol for end-to-end encrypted binary messages

# Specification
NOTE: UUID v4 are recommended
## Relay
A node that exchanges messages between targets/relays.
## Target
A logical receiver of messages. Has an address UUID that identifies them.
## Types
### u\<bits\> i\<bits\>
integers unsigned/signed(2's complement) little endian
### char\<bytes\>
static length ascii string \<bytes\> character long. Orginal character order.
### char
variable length, length-prefixed ascii string \<bytes (runtime)\> character long.
| Field   | value     | type          | description                |
|---------|-----------|---------------|----------------------------|
| length  | \<bytes\> | u64           | number of characters/bytes |
| content |           | char\<bytes\> | characters of the string   |
### utf8
variable length, length-prefixed unicode uft-8 encoded string \<bytes (runtime)\> bytes long.
| Field   | value     | type                | description                |
|---------|-----------|---------------------|----------------------------|
| length  | \<bytes\> | u64                 | number of bytes            |
| content |           | UTF8 encoded chars  | characters of the string   |
### UUID
UUID encoded as 128 bit/8 bit/byte=16 big endian ordered bytes.
### Timestamp
u64 number of miliseconds since unix zero time (unix zero = 0, 1 millisecond after unix zero = 1, ...)
### vlist\[\<T\>\]
variable length, length-prefixed, <n (runtime)> elements long array of elements of specified type T.
| Field   | value     | type                | description                |
|---------|-----------|---------------------|----------------------------|
| length  | \<n\>     | u64                 | number of elements         |
| content |           | <T>                 | elements one after another |
### @SID (Type suffix)
Annotation to char string types that requires chars of string to be in [A-z0-9-_.] charset.
## Packets
### Ping (Relaybound) / Pong (Clientbound)
| Field     | value | type      | description                            |
|-----------|-------|-----------|----------------------------------------|
| packet_id | 0     | u8        |                                        |
| my_time   |       | Timestamp | current timestamp rigth before sending |
