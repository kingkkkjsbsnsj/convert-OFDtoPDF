# OFD Batch Converter

An OFD document batch conversion solution based on the [ofdrw](https://gitee.com/ofdrw/ofdrw) open-source library, combined with Microsoft Power Automate Desktop (PAD) for full automation.

## 📋 Overview

This is a **wrapper project that invokes an existing OFD converter**. It does not include source code and only provides:
- Launch script (one-click to run the converter)
- Power Automate Desktop automation flow configuration
- Usage documentation

### Core Components

| Component | Description | Source |
|-----------|-------------|--------|
| **OFD Converter** | `ofd-to-image-converter-1.0.0.jar` | Built from ofdrw 2.3.5 (obtain separately) |
| **Launch Script** | `start.bat` | Provided by this project |
| **Automation Flow** | PAD flow configuration | Provided by this project |

## 🏗️ Project Structure

```
ofd-batch-converter/
├── start.bat                    # Launch script (starts OFD converter GUI)
├── README.md                    # This file
├── GET_JAR.md                   # Instructions for obtaining the converter jar
├── LICENSE                      # Apache 2.0 license
├── .gitignore                   # Git ignore rules
└── pad-flow/
    └── workflow.md              # PAD automation flow documentation
```

## 📦 Prerequisites

### 1. Obtain the OFD Converter

You need to obtain `ofd-to-image-converter-1.0.0.jar` separately and place it in the same directory as `start.bat`, or update the path in `start.bat`.

See [GET_JAR.md](GET_JAR.md) for detailed instructions.

### 2. Environment Requirements

| Dependency | Version |
|------------|---------|
| **JDK** | 21+ (ofdrw 2.3.5 supports JDK 1.8+, but JDK 21 recommended) |
| **Power Automate Desktop** | Latest (only needed for batch mode) |

## 🚀 Quick Start

### 1. Place the JAR File

Place `ofd-to-image-converter-1.0.0.jar` in the project root directory, or update the path in `start.bat`.

### 2. Launch the Converter

Double-click `start.bat` to start the GUI:

```bash
start.bat
```

Or from command line:

```bash
java -jar ofd-to-image-converter-1.0.0.jar
```

### 3. Manual Conversion with GUI

1. Click "Select File" or "Select Folder" to import OFD files
2. Choose output format (PNG / JPG / BMP)
3. Adjust PPM quality parameter (default: 15)
4. Click "Convert" to start processing

### 4. Batch Conversion with PAD

For fully automated batch processing (unzip → convert → cleanup → notify), see [pad-flow/workflow.md](pad-flow/workflow.md) for PAD flow configuration.

## 🔁 Automation Flow Overview

The PAD flow consists of the following stages:

```
┌──────────────┐    ┌────────────┐    ┌────────────────┐    ┌─────────────┐    ┌──────────┐
│  Get ZIP     │ →  │  Extract   │ →  │  Launch        │ →  │  GUI        │ →  │ Cleanup  │
│  Files (1)   │    │  ZIP (2-4) │    │  Converter (6) │    │  Auto (7-12)│    │+Notify   │
│              │    │            │    │                │    │             │    │(15-26)  │
└──────────────┘    └────────────┘    └────────────────┘    └─────────────┘    └──────────┘
```

1. **Get Files** — Retrieve all `.zip` files from source directory
2. **Batch Extract** — Unzip each archive to working directory
3. **Discover Folders** — Get list of extracted subfolders
4. **Launch Engine** — Start OFD converter GUI
5. **GUI Automation** — RPA fills paths and clicks convert button
6. **Cleanup** — Delete intermediate OFD source files and PNG temp files
7. **Notify** — Show completion dialog

## ⚠️ Notes

- Before running the PAD batch flow, update **hardcoded paths** to match your environment
- The converter GUI component names (`JText`, `JText2`, `JPush Button`) must not be changed, otherwise the PAD flow will fail
- JDK 21 is recommended for best compatibility

## 📄 License

This project is licensed under [Apache License 2.0](LICENSE).

The [ofdrw](https://gitee.com/ofdrw/ofdrw) library used by the OFD converter is also Apache 2.0 licensed.

## 🙏 Acknowledgments

- [ofdrw](https://gitee.com/ofdrw/ofdrw) — OFD document read/write/convert library
- [Apache PDFBox](https://pdfbox.apache.org/) — PDF processing library
