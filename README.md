# Multi-Platform ABS LongCoding Decoder (EBC 460)

Open the Websitehttps://phnahes.github.io/vw_ebc460_abs_decoder/

This project is a **comprehensive web-based decoder for hexadecimal LongCoding values** used in Volkswagen (VW) ABS modules, specifically modules with ID **460 (EBC)**.

**🚀 MAJOR UPDATE**: Now supports multiple platforms with enhanced decoding based on detailed Drive2.ru research:
- **PQ25 Platform**: 19 bytes (18 if counted from zero) - Rapid/Toledo early models  
- **PQ26 Platform**: 20 bytes (19 if counted from zero) - Rapid/Toledo later models
- **Premium Units**: 24 bytes (23 if counted from zero) - Future support
- **Auto-detection** of platform based on byte count

It also optionally supports validation of **specific characters in the VIN** (Vehicle Identification Number), when available, for deeper cross-reference with ABS module coding.

---
## Enhanced Byte Decoding

Each byte in the LongCoding string represents a specific configuration, capability, or VIN-related encoding for the ABS module. The system now provides **platform-specific decoding** with detailed bit-level analysis:

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
- **LongCoding PQ25 (19 bytes)**: `10 E3 48 06 81 F0 00 54 08 9F 12 0D 81 FB 00 40 02 00`
- **LongCoding PQ26 (20 bytes)**: `43 F8 28 76 89 7F 00 D8 C2 2A 14 2B 91 24 00 79 4F 08 00 C1`

**Real Example from Drive2.ru/VCDS:**
- **VIN**: `9BWAB45U2F5123456` (Rapid PQ26)
- **LongCoding**: `43 F8 28 76 89 7F 00 D8 C2 2A 14 2B 91 24 00 79 4F 08 00 C1`
- **Platform**: Auto-detected as **PQ26** (20 bytes)
- **Decoded Results**:
  - Byte 0 (43): **Rapid Liftback/Spaceback PQ26 [L0L]** 
  - Byte 1 (F8): **7th VIN char: N (PQ26)**
  - Byte 2 (28): **Rear: Drum 200mm [1KL/1KH] | Front: 256mm [1ZG]**
  - Byte 3 (76): **8th VIN char: H (PQ26)**
  - Byte 15 (79): **Starting torque: CWVA 1.6MPI + Start-Stop: Enabled + Steering: Left [L0L]**
  - Byte 16 (4F): **HHC: Enabled | TPMS: RDKS+/iTPMS+ [7K1] | ESC: Active, ASR off, ESC sport (Sport)**
  - Byte 19 (C1): **MKB/MCB: Enabled | ESC Menu: Enabled | RDKS+/TPMS+: Enabled**

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


---

## ⚠️ Disclaimer

This tool is provided **for educational and informational purposes only**.

- I am **not responsible** for any use of this tool in real vehicle systems.
- I am **not involved** in modifying or tampering with any vehicle modules or configurations.
- Use this tool at your own risk.

---

## 🚀 **MAJOR UPDATE - Enhanced Features**

Based on comprehensive Drive2.ru research, this version includes:

### **✅ Multi-Platform Support**
- **Auto-detection** of platform (PQ25/PQ26/Premium) based on byte count
- **Platform-specific decoding** with different logic for different vehicle families
- Support for **18-24 byte** Long Coding strings

### **🔧 Advanced Byte Decoding** 
- **Bit-level analysis** for bytes 2, 4, 15, 16, 17, 19
- **Platform-specific configurations**: PQ25/PQ26 (Rapid/Toledo) vs up!/Mii/Citigo families
- **Enhanced checksum validation** with visual ✓/✗ indicators

### **📊 New Byte Support**
- **Byte 15**: Initial torque, Start-Stop system, DSR protocol, steering wheel position
- **Byte 16**: Hill Hold Control, advanced TPMS types (7K0/7K6/7K9/7K1), ESC/ASR modes
- **Byte 17**: Detailed EDS coefficients, platform-specific radar/sensor configurations  
- **Byte 18**: ABS sensor type configuration (always 00, reserved for PLA)
- **Byte 19**: PQ26-only features (MKB/MCB, ESC menu control, RDKS+)

### **🎯 Expanded Platform Coverage**
- **20+ platform codes** including all PQ25/PQ26 variants
- **Brake configuration** with precise bit decoding (rear brake type + front disc diameter)
- **Chassis options** with different decoding logic per platform family
- **Engine-specific** torque and EDS coefficient mapping

**Sources**: Detailed research from Drive2.ru community articles on EBC 460 Long Coding + Official VCDS/VASYA .lbl file (6R0-614-517.lbl) with precise platform-specific VIN and configuration codes.

## 🔧 **VCDS .LBL FILE INTEGRATION**

The system now includes **official VCDS/VASYA label file data** (6R0-614-517.lbl) providing:

### **Enhanced VIN Validation**
- **Platform-specific VIN codes**: Separate PQ25 vs PQ26 mappings for bytes 5,7,9,11,13
- **Precise character mapping**: 20+ new VIN character codes per byte
- **Cross-platform compatibility**: Legacy codes maintained for other units

### **Detailed Configuration Codes**  
- **Brake systems**: Added missing "06" code for "Disc 230mm [1KX]"
- **ESC configurations**: 11 detailed ESC/ASR modes vs previous 3 basic ones
- **EDS engine mappings**: Precise engine-specific EDS coefficients  
- **Front Assist**: Accurate radar system detection codes
- **Drivetrain types**: Platform-specific ESP/MABS configurations

### **Technical Accuracy**
- **Checksum validation**: Enhanced bit-mirror validation logic
- **Security access codes**: Included for professional use
- **Part number mapping**: Direct correlation with VW part numbers
- **Option code references**: Complete PR code integration
