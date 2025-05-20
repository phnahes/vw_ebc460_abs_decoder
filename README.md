# 18-Byte ABS LongCoding Decoder (460 EBC)

This project is a **web-based decoder for 18-byte hexadecimal LongCoding values** used in Volkswagen (VW) ABS modules, specifically modules with ID **460 (EBC)**.

It also optionally supports validation of **specific characters in the VIN** (Vehicle Identification Number), when available, for deeper cross-reference with ABS module coding.

---
## Byte Decoding

Each of the 18 bytes in the LongCoding string represents a specific configuration, capability, or VIN-related encoding for the ABS module. Below is a summary of their known purposes:

| Byte # | Description |
|--------|-------------|
| 0 | **Platform/Model ID** (e.g., VW up!, Gol, Skoda Rapid) |
| 1 | **7th VIN Character** (cross-validated if VIN is provided) |
| 2 | **Brake Configuration**: Rear + Front brake setup (drum/disc sizes) |
| 3 | **8th VIN Character** (cross-validated if VIN is provided) |
| 4 | **Chassis Options**: Suspension type, transmission family, and steering |
| 5 | **13th VIN Character** (cross-validated if VIN is provided) |
| 6 | **Drivetrain Type**: ESP/ABS configuration |
| 7 | **14th VIN Character** (cross-validated if VIN is provided) |
| 8 | **Checksum (from Byte 0)**: Reverse-bit checksum for validation |
| 9 | **15th VIN Character** (cross-validated if VIN is provided) |
| 10 | **Checksum (from Byte 2)** |
| 11 | **16th VIN Character** (cross-validated if VIN is provided) |
| 12 | **Engine, Start/Stop, Steering** |
| 13 | **17th VIN Character** (cross-validated if VIN is provided) |
| 14 | **Checksum (from Byte 6)** |
| 15 | **EDS & Emergency Braking Config** |
| 16 | **Checksum (from Byte 8)** |
| 17 | **Unknown / Reserved / Diagnostic filler** |

🧠 Some bytes also include **bit-level parsing**, especially:
- Byte `10` → ESC, HHC, TPMS
- Byte `11` → EDS type and Emergency Braking
- Bytes `08`, `0A`, `0C`, `0E`, `10`, `12`, `14`, `0F` → Checksum bytes (reverse bit logic)

--- 
## Sources & References
The decoding logic is based on research and community knowledge, with major references including:
- **[Drive2.ru - EBC460 Longcoding Reference - 1](https://www.drive2.ru/l/598621135357094086/)**
- **[Drive2.ru - EBC460 Longcoding Reference - 2](https://www.drive2.ru/l/618100358232697346/)**

---

## 🔧 How It Works

The decoder parses each byte of a given LongCoding (hexadecimal) and maps it to its corresponding meaning using predefined lookup tables. It also supports:

- VIN digit validation (7th to 17th digit)
- Checksum verification (via reverse bits)
- Byte-specific contextual decoding (e.g., brake type, transmission, suspension)
- Bitwise decoding for certain bytes (e.g., HHC, TPMS, ESC, EDS)
- Responsive, mobile-friendly interface
- Visual feedback with ✅ or ❌ for VIN matches

---

## 💡 Example Input

- **VIN (optional)**: `9BWAB45U2F5123456`
- **LongCoding**: `10 E3 48 06 81 F0 00 54 08 9F 12 0D 81 FB 00 40 02 00`

---

## 📋 VIN Validation Rules

The tool extracts and validates the following VIN characters:

| Byte | Meaning         | Extracted From VIN |
|------|------------------|--------------------|
| 1    | 7th character    | vin[6]             |
| 3    | 8th character    | vin[7]             |
| 5    | 13th character   | vin[12]            |
| 7    | 14th character   | vin[13]            |
| 9    | 15th character   | vin[14]            |
| 11   | 16th character   | vin[15]            |
| 13   | 17th character   | vin[16]            |

Each of these is compared to byte-specific values in the lookup table to confirm whether the byte encoding is consistent with the provided VIN.

---

## ✅ Features

- 🧩 **Byte-level descriptions**: Each byte of the LongCoding is mapped using bitwise and direct value decoding.
- ✔️ **VIN cross-check**: Visually confirms if the LongCoding matches specific VIN digits.
- 🔄 **Checksum validation**: Uses reverse bit logic to validate integrity of select bytes.
- 🛠️ **Standalone**: No backend — runs 100% in-browser (pure HTML + JavaScript).
- 🌐 **Open source**: MIT-licensed and hosted on GitHub.

---

## 🖥️ How to Use

1. Open the Website[https://phnahes.github.io/vw_ebc460_abs_decoder/] in your browser.
2. Paste the 18-byte hex LongCoding into the input field (spaces optional).
3. (Optional) Provide the full 17-character VIN.
4. Click **Decode**.
5. Review the decoded table and VIN validation results.


## 📄 License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).


---

## ⚠️ Disclaimer

This tool is provided **for educational and informational purposes only**.

- I am **not responsible** for any use of this tool in real vehicle systems.
- I am **not involved** in modifying or tampering with any vehicle modules or configurations.
- Use this tool at your own risk.