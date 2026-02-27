# TH9800_CAT Project Memory

## Project Overview
- TYT TH-9800 ham radio CAT (Computer Aided Transceiver) control application
- Python GUI app using DearPyGui for UI, pyserial-asyncio for serial communication
- GitHub fork: https://github.com/ukbodypilot/TH9800_CAT
- Main files: `TH9800_CAT.py` (main app), `TH9800_Enums.py` (enums/constants)

## Architecture
- See [architecture.md](architecture.md) for detailed notes

## Bugs Fixed (branch: fix/multiple-bugs)
1. **Missing `sys` import** (line 6) - `sys.platform` used at line 196 without import
2. **match/case OR pattern** (line 826) - `case (0x40|0xC0):` parens forced bitwise eval to `0xC0`; fixed to `case 0x40 | 0xC0:`
3. **List append** (line 1044) - `enabled_icons += "H"` changed to `+= ["H"]`
4. **exit() not called** (line 2065) - `exit` referenced but not invoked

## Known Hardware Issues
- DTR toggle button code is correct (identical logic to working RTS toggle) but DTR line may not respond due to hardware — user confirmed fixed in hardware

## Git Setup
- Added `.gitignore` (excludes `__pycache__/`, `venv/`, `*.pyc`)
- Remote origin: https://github.com/ukbodypilot/TH9800_CAT.git
