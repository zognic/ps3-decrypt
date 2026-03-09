# ps3-decrypt

An interactive command-line tool to decrypt PS3 ISO files using [ps3decrs](https://github.com/Redrrx/ps3dec).

DKEY keys are automatically fetched from the [aldostools database](https://ps3.aldostools.org/dkey.html) and cached locally. No external Python dependencies required — stdlib only.




https://github.com/user-attachments/assets/6e809602-5b15-4db1-9512-f44745352627



---

## Features

- Interactive menu listing all ISO files in a directory
- Automatic DKEY lookup: local cache first, then aldostools (fetched once per session)
- Visual indicators in the file list:
  - `🔑` key cached locally
  - `🌐` key available on aldostools
  - Decrypted ISOs displayed in dimmed green
- Disk space check before each decryption
- MD5 / Redump verification (`c <n>` command)
- Batch mode (`all`) with skip logic for already-decrypted files
- Optional deletion of original ISO after successful decryption
- Auto `chmod +x` on the `ps3decrs` binary if needed

---

## Requirements

- Python 3.10+
- [ps3decrs](https://github.com/Redrrx/ps3dec) binary

No third-party Python packages needed.

---

## Installation

```bash
git clone https://github.com/zognic/ps3-decrypt.git
cd ps3-decrypt
```

Place the `ps3decrs` binary in one of the following locations:

| Priority | Path |
|----------|------|
| 1 | Current working directory |
| 2 | `/userdata/roms/ps3/` (Batocera default) |
| 3 | System `PATH` |

---

## Usage

```bash
# Run in the current directory (ISOs must be in the same folder)
python3 ps3-decrypt

# Or specify a directory
python3 ps3-decrypt /path/to/ps3/isos
```

### Commands

| Input | Action |
|-------|--------|
| `1`–`n` | Decrypt the selected ISO |
| `c <n>` | Verify MD5 checksum against Redump database |
| `a` / `all` | Decrypt all pending ISOs (batch mode) |
| `l` / Enter | Refresh the ISO list |
| `q` | Quit |

---

## Key Management

Keys are stored in a `keys/` subfolder next to your ISO files.

```
/your/ps3/isos/
├── ps3decrs
├── ps3-decrypt
├── Game Title (USA).iso
└── keys/
    └── Game Title (USA).dkey
```

If a key is not found locally, the script fetches the aldostools page once, caches it for the session, and saves the matching `.dkey` file to `./keys/` for future use.

You can also add keys manually:

```
keys/Game Title (USA).dkey
```

The file should contain the 32-character hex DKEY on a single line.

---

## Batch Mode

Pressing `a` or typing `all` will:

1. Skip any ISO whose filename ends with `_decrypted.iso` (already processed)
2. Skip any ISO for which a `<filename>.iso_decrypted.iso` file already exists
3. Ask once whether to delete original ISOs after decryption
4. Process all remaining ISOs in sequence

---

## Redump Verification

```
> c 2
```

Computes the MD5 of the selected ISO and compares it against the aldostools database (which mirrors Redump checksums). A match confirms the ISO is an unmodified, bit-perfect dump.

---

## Batocera

This tool is designed to work natively in a [Batocera](https://batocera.org) environment:

- Default binary path: `/userdata/roms/ps3/ps3decrs`
- Default ISO path: `/userdata/roms/ps3/`
- Run via SSH or a terminal session

---

## References

- [ps3decrs](https://github.com/Redrrx/ps3dec) — PS3 ISO decryption binary by Redrrx
- [aldostools DKEY database](https://ps3.aldostools.org/dkey.html) — PS3 decryption keys
- [Redump](http://redump.org) — disc preservation database
- [Redump All Keys](http://redump.org/dkeys/ps3/) - Redump also provides an up-to-date zip file of every key here
---

## License

MIT — see [LICENSE](LICENSE)
