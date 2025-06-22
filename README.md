## **Overview**
CAS (Cool App System) is a professional presentation software designed for church LED panels and other display uses. It provides advanced features for creating, editing and presenting slides with rich media integration.

**Key Features:**
- Multiple themed slide groups
- Advanced text and background editing
- Fullscreen LED panel display
- Video and image background support
- PDF and URL content extraction
- AI-powered presenter mode
- Typewriter text animation effects
- Export/Import functionality
- Modern dark theme interface

## **System Architecture**

### **Core Components**
1. **EditorWindow** - Main application window with editing interface
2. **DisplayWindow** - Full-screen presentation output window
3. **Slide/SlideGroup** - Data models for presentation content
4. **HymnExtractor** - PDF to slides converter and URLs provided by a giant cloud database + you provide your URL
5. **LyricsExtractor** - Web content scraper
6. **NeuralNetwork** - AI model for presenter mode
7. **EncryptionModule** - Data security for project files

### **Technical Stack**
- Python 3.x
- PyQt5 for GUI
- OpenCV for video processing
- PyTorch for AI capabilities
- pdfplumber for PDF extraction
- BeautifulSoup for web scraping
- Cryptography for file encryption

## **Installation**
Requires Python 3.8+ with the following packages:

```bash
pip install PyQt5 opencv-python torch pdfplumber beautifulsoup4 cryptography
```

For speech recognition in presenter mode:
```bash
pip install SpeechRecognition pyaudio
```

## **User Interface**

### **Main Editor Window**
![Editor Window Layout]
1. **Menu Bar** - File operations, view controls and tools
2. **Theme Tabs** - Organize presentations by theme
3. **Slide Editor** - Text, font and appearance controls
4. **Slide Grid** - Visual thumbnail overview of slides
5. **Display Controls** - Presentation navigation buttons

### **Display Window**
- Fullscreen or windowed output
- Draggable text positioning
- Video/image background support
- Keyboard navigation (arrow keys)
- Typewriter text animation

## **Core Features**

### **1. Slide Management**
- Create unlimited slides per theme
- Duplicate existing slides
- Reorder slides via drag-and-drop
- Apply styling to multiple slides

**Slide Properties:**
- Text content with rich formatting
- Custom fonts, sizes and colors
- Background images/videos
- Text alignment and positioning
- Typewriter effect speed control

### **2. Multimedia Support**
**Background Types:**
- Solid color gradients
- Image files (PNG, JPG, etc.)
- Video files (MP4, AVI, etc.)
- Web video URLs (YouTube, etc.)

**Video Controls:**
- Play/pause/stop
- Loop playback
- Fullscreen toggle
- URL loading

### **3. Presentation Tools**
**Display Options:**
- Resolution settings
- Fullscreen mode
- Always-on-top
- Multiple display support

**Navigation:**
- Keyboard shortcuts (arrows, Esc)
- On-screen controls
- Remote control via network

### **4. Content Import**
**PDF Extractor:**
- Processes PDF hymn books
- Auto-splits into slides
- Preserves numbering
- Outputs to JSON/cas format

**URL Extractor:**
- Scrapes lyrics from web
- Handles multiple URLs
- Cleans and formats text
- Creates presentation-ready slides

### **5. AI Presenter Mode**
**Features:**
- Listens for spoken keywords
- Matches phrases to slide content
- Auto-advances presentation
- Trains on current material
- Visual feedback cues

**How It Works:**
1. Extract last N words from slides
2. Train neural network model
3. Listen for matching speech patterns
4. Advance when match confidence is high

### **6. Project Management**
**File Operations:**
- New/Open/Save projects (.cas)
- Import multiple projects
- Auto-save recovery
- Version control

**Security:**
- AES-256 encryption
- Unique project keys
- Tamper protection
- Secure data storage

## **Advanced Features**

### **Custom Scripting**
```python
# Example: Create slides programmatically
slide = Slide()
slide.text = "Welcome to CAS"
slide.font_family = "Arial"
slide.font_size = 42
slide.background_image = "bg.png"
```

### **Multi-Monitor Support**
```
[Display]
primary = 1920x1080
secondary = 800x600
arrangement = right
```

### **Network Control**
- HTTP API for remote management
- WebSocket for realtime updates
- Multi-client sync capability

## **Performance Optimization**

**Recommended Specs:**
- CPU: Intel i5 or equivalent
- GPU: DirectX 11 compatible
- RAM: 8GB+ for large presentations
- Storage: SSD recommended

**Optimization Tips:**
- Use compressed media formats
- Limit simultaneous videos
- Pre-render complex slides
- Disable animations when not needed

## **Troubleshooting**

### **Common Issues**
**Problem:** Videos don't play  
**Solution:** Install codecs or convert to MP4/H.264

**Problem:** PDF import fails  
**Solution:** Ensure PDF is text-based (not scanned)

**Problem:** Presenter mode not responding  
**Solution:** Check microphone permissions

### **Error Recovery**
- Automatic backup creation
- Crash reporting
- Safe mode options
- Reset preferences

## **License & Copyright**
© 2025 Tech Solutions By Gabriel N  

**Third-Party Components:**
- Qt Framework (LGPL)
- OpenCV (Apache 2.0)
- PyTorch (BSD-style)

## **Appendices**

### **Keyboard Shortcuts**
| Key | Action |
|-----|--------|
| F11 | Toggle fullscreen |
| Esc | Exit fullscreen |
| ←↑ | Previous slide |
| →↓ | Next slide |
| Ctrl+N | New project |
| Ctrl+O | Open project |
| Ctrl+S | Save project |

### **File Formats**
| Extension | Purpose |
|-----------|---------|
| .cas | Encrypted project file |
| .json | Import/export format |
| .pth | AI model weights |

### **Version History**
- v1.0 - Initial release
- v1.1 - Added PDF extractor
- v1.2 - AI presenter mode
- v1.2.5 - Performance improvements
- v1.2.6 - Bug fixes and UI polish

## **Support**
For assistance, contact:  
support@techbygabriel.com

**Resources:**
- [User Manual PDF]
- [Video Tutorials]
- [Community Forum]

---

✅ Complete Documentation Generated

- Comprehensive system documentation
- Covers all major features and components
- Includes installation, usage and troubleshooting
- Organized into logical sections

