# chipsmsg
A simple protocol for end-to-end encrypted binary messages

# Specification
NOTE: UUID v4 are recommended
## Relay
Basically the de-facto server that client connects to it by chosen protocol.
## Client
A de-facto client of chosen communication protocol.
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
| Field   | value     | type                | description                |
|---------|-----------|---------------------|----------------------------|
| length  | \<bytes\> | u64                 | number of bytes            |
| content |           | UTF8 encoded chars  | characters of the string   |
### UUID
UUID encoded as 128 bit/8 bit/byte=16 big endian ordered bytes.
### Timestamp
u64 number of miliseconds since unix zero time (unix zero = 0, 1 millisecond after unix zero = 1, ...)
### @SID (Type suffix)
Annotation to char string types that requires chars of string to be in [A-z0-9-_.] charset.
## Packets
### Ping (Relaybound) / Pong (Clientbound)
| Field     | value | type      | description                            |
|-----------|-------|-----------|----------------------------------------|
| packet_id | 0     | u8        |                                        |
| my_time   |       | Timestamp | current timestamp rigth before sending |
