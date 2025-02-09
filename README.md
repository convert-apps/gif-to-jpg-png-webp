#  gif to jpg/png/webp converter

Here's a detailed explanation of converting GIF to JPG/PNG/WEBP using JavaScript and HTML:

```html
<!DOCTYPE html>
<html>
<head>
    <title>GIF to Image Converter</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/libgif-js/0.5.0/libgif.min.js"></script>
</head>
<body>
    <input type="file" id="gifInput" accept="image/gif">
    <div id="preview"></div>
    <canvas id="canvas" style="display: none;"></canvas>
    <div id="output"></div>

    <script>
        const gifInput = document.getElementById('gifInput');
        const previewDiv = document.getElementById('preview');
        const canvas = document.getElementById('canvas');
        const outputDiv = document.getElementById('output');
        const ctx = canvas.getContext('2d');

        // Handle GIF file selection
        gifInput.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (!file) return;

            // Clear previous outputs
            previewDiv.innerHTML = '';
            outputDiv.innerHTML = '';

            // Create GIF parser instance
            const superGif = new SuperGif({ gif: file });
            
            superGif.load({
                // Success callback
                callback: function() {
                    // Get first frame (for static conversion)
                    const frame = this.frames[0];
                    
                    // Set canvas dimensions
                    canvas.width = this.get_width();
                    canvas.height = this.get_height();

                    // Draw frame on canvas
                    ctx.drawImage(frame.data, 0, 0);

                    // Create preview
                    previewDiv.innerHTML = '<h3>Original GIF Preview:</h3>';
                    const img = document.createElement('img');
                    img.src = URL.createObjectURL(file);
                    previewDiv.appendChild(img);

                    // Create conversion buttons
                    createConversionButtons();
                }
            });
        });

        function createConversionButtons() {
            const formats = ['jpg', 'png', 'webp'];
            
            formats.forEach(format => {
                const btn = document.createElement('button');
                btn.textContent = `Convert to ${format.toUpperCase()}`;
                btn.onclick = () => convertToFormat(format);
                outputDiv.appendChild(btn);
            });
        }

        function convertToFormat(format) {
            // Quality parameter (0-1) for formats that support it
            const quality = format === 'png' ? undefined : 0.92;

            // Convert canvas to data URL
            const dataUrl = canvas.toDataURL(`image/${format}`, quality);

            // Create download link
            const link = document.createElement('a');
            link.download = `converted.${format}`;
            link.href = dataUrl;
            link.textContent = `Download ${format.toUpperCase()}`;

            // Display result
            outputDiv.appendChild(link);
        }
    </script>
</body>
</html>
```

**Key Components Explained:**

1. **HTML Structure:**
   - File input for GIF selection
   - Preview containers for original and converted images
   - Hidden canvas element for frame processing
   - Output area for conversion results

2. **JavaScript Workflow:**
   - File input listener for GIF selection
   - libgif.js library for GIF frame extraction
   - Canvas element for frame rendering
   - Conversion to different formats using canvas.toDataURL()

3. **Conversion Process:**
   - **JPG:** Lossy compression, supports quality parameter
   - **PNG:** Lossless compression, supports transparency
   - **WEBP:** Modern format with both lossy/lossless options

**Important Notes:**
1. This example converts only the first frame of the GIF
2. For animated GIF conversion, you would need to:
   - Iterate through all frames
   - Create multiple images or a sprite sheet
   - Consider creating ZIP archives for multiple outputs
3. Quality parameters:
   - JPG: 0 (low quality) to 1 (high quality)
   - WEBP: Similar range with better compression
4. Transparency:
   - JPG doesn't support transparency (converts to black/white background)
   - PNG and WEBP support alpha transparency

**Browser Compatibility:**
- Modern browsers (Chrome, Firefox, Edge)
- WEBP support requires newer browsers (Chrome 32+, Firefox 65+)

**Enhancement Ideas:**
1. Add quality controls
2. Support multi-frame conversion
3. Add background color options for JPG
4. Implement batch processing
5. Add image size/resolution controls

**Dependencies:**
- libgif.js (for GIF frame parsing)
- Modern browser with Canvas support

To use this code:
1. Host it on a web server
2. Include the libgif.js dependency
3. Test with different GIF files

This implementation provides a basic but functional GIF conversion solution that can be extended based on specific needs.

- https://github.com/convert-apps/avif-to-jpg-png-webp
- https://github.com/convert-apps/webp-to-png-jpg-gif
- https://github.com/convert-apps/jpg-to-png-webp-jpeg
- https://github.com/convert-apps/png-to-jpg-webp-ico
- https://github.com/convert-apps/gif-to-jpg-png-webp
