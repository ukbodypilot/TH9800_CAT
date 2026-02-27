# TH9800_CAT Architecture

## Key Classes

### SerialProtocol (asyncio.Protocol)
- Handles serial connection lifecycle (connection_made, data_received, connection_lost)
- Manages RTS/DTR line control (set_rts, toggle_rts, set_dtr, toggle_dtr)
- Packet buffering with start byte detection (`\xAA\xFD`) and XOR checksum validation
- Receive/transmit queues for async packet processing

### SerialRadio
- Radio state management (VFO memory, icons, volume, squelch, PTT)
- Dual VFO (LEFT/RIGHT) with memory/VFO operating modes
- DearPyGui theme management for icon display
- Command execution (exe_cmd) builds TX packets and handles button press/release sequences

### SerialPacket
- Packet creation (create_tx_packet) and parsing (process_rx_packet)
- RX command dispatch via match/case on RADIO_RX_CMD enum values
- Display text, channel, icons, startup sequence handling
- Volume/squelch value conversion (vol_sq_to_packet)

### TCP (client/server)
- TCP server: password-protected, relays serial data to remote clients
- TCP client: connects to remote server, forwards packets bidirectionally
- Commands prefixed with `!` (e.g., `!rts`, `!dtr`, `!pass`, `!data`, `!exit`)

### CATController + RigctlServer
- Rigctl-compatible TCP server (default port 4532) for hamlib integration
- Supports: frequency get/set, mode, VFO switching, PTT, memory names, dump_state

## Threading Model
- Main thread: DearPyGui rendering loop
- Background thread: asyncio event loop (serial I/O, TCP, rigctl)
- GUI callbacks run in main thread, access serial transport directly (thread-safety caveat)

## Packet Format
- Start: `0xAA 0xFD`
- Length byte + payload + XOR checksum
- TX default packet: `[0x84, 0xFF, 0xFF, 0xFF, 0xFF, 0x81, 0xFF, 0xFF, 0x82, 0xFF, 0xFF, 0x00]`

## Dependencies
- dearpygui 2.0.0, pyserial 3.5, pyserial-asyncio 0.6
