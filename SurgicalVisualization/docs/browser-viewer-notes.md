# Browser Viewer Notes — Three.js STL Pipeline

The browser viewer demonstrates cross-platform rendering parity using Three.js.

## Why a Local Server Is Required
Opening `index.html` via `file://` can block ES module imports and local file loading.  
Use a local server:

```bash
python -m http.server 8000
