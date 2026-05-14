# How to Obtain the OFD Converter

This project is a **wrapper that invokes an existing converter**. You need to obtain `ofd-to-image-converter-1.0.0.jar` separately.

## Option 1: Build from ofdrw Official Repository (Recommended)

### 1. Clone the ofdrw Repository

```bash
git clone https://gitee.com/ofdrw/ofdrw.git
cd ofdrw
```

### 2. Explore the Examples

The ofdrw official repository contains conversion examples in:
- `ofdrw-converter/src/test/java/` directory

Key classes:
- `ImageExporter` — Export to images
- `ImageMaker` — Generate images page by page
- `ConvertHelper` — PDF conversion utility

### 3. Write Your Own GUI Tool

Reference the following code structure:

```java
package com.example;

import org.ofdrw.converter.export.ImageExporter;
import org.ofdrw.reader.DLOFDReader;
import javax.swing.*;
import java.awt.*;
import java.nio.file.Path;
import java.nio.file.Paths;

public class OFDToImageGUI extends JFrame {
    // GUI Components:
    // - JTextField: Input file path (variable name JText)
    // - JTextField: Output directory path (variable name JText2)
    // - JButton: Convert button (text "转换" / "Convert")
    // - JButton: OK button (text "确定" / "OK")

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new OFDToImageGUI().setVisible(true));
    }

    private void convert(String inputPath, String outputDir, double ppm) {
        try (ImageExporter exporter = new ImageExporter(
                Paths.get(inputPath),
                Paths.get(outputDir),
                "PNG",
                ppm)) {
            exporter.export();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 4. Maven Dependency

```xml
<dependencies>
    <dependency>
        <groupId>org.ofdrw</groupId>
        <artifactId>ofdrw-converter</artifactId>
        <version>2.3.5</version>
    </dependency>
</dependencies>
```

### 5. Package

Use `maven-shade-plugin` to create an executable fat JAR:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.5.1</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals><goal>shade</goal></goals>
            <configuration>
                <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>com.example.OFDToImageGUI</mainClass>
                    </transformer>
                </transformers>
            </configuration>
        </execution>
    </executions>
</plugin>
```

```bash
mvn clean package
```

## Option 2: Use JAR from Other Sources

If you already have a pre-built `ofd-to-image-converter-1.0.0.jar` from another source, you can use it directly.

## Placement

Place the JAR file in one of these locations:

1. **Same directory as start.bat** (Recommended)
   ```
   ofd-batch-converter/
   ├── start.bat
   ├── ofd-to-image-converter-1.0.0.jar  <-- Place here
   └── README.md
   ```

2. **Update path in start.bat**
   ```batch
   java -jar D:\your\path\ofd-to-image-converter-1.0.0.jar
   ```

## Runtime Dependencies

`ofd-to-image-converter-1.0.0.jar` requires these runtime dependencies (usually bundled in the fat JAR):

| Dependency | Version | Description |
|------------|---------|-------------|
| ofdrw-converter | 2.3.5 | OFD conversion core |
| ofdrw-core | 2.3.5 | OFD data structures |
| ofdrw-reader | 2.3.5 | OFD document parser |
| ofdrw-layout | 2.3.5 | Layout rendering engine |
| pdfbox | 2.0.25 | PDF processing (for PDF conversion) |

If the JAR doesn't bundle these dependencies, you'll need to add them to the classpath manually.
