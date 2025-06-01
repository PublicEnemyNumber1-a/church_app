import sys
import json
import os
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
from PyQt5.QtGui import *


class Slide:
    def __init__(self):
        self.text = ""
        self.font_family = "Arial"
        self.font_size = 24
        self.font_color = "#FFFFFF"
        self.background_image = ""
        self.text_alignment = Qt.AlignCenter
        # Text positioning properties
        self.text_x = 0.5  # Relative position (0-1)
        self.text_y = 0.5  # Relative position (0-1)
        self.text_width = 1.0  # Relative width (0-1)
        self.text_height = 1.0  # Relative height (0-1)


class SlideGroup:
    def __init__(self, name="New Group"):
        self.name = name
        self.slides = [Slide()]


class DisplayWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("LED Panel Display")
        self.setGeometry(100, 100, 600, 600)
        self.setWindowFlags(Qt.Window | Qt.FramelessWindowHint | Qt.WindowStaysOnTopHint)

        self.old_pos = None
        self.text_dragging = False
        self.drag_start_pos = None
        self.current_slide = None

        # Estilo visual
        self.setStyleSheet("""
            QWidget {
                background-color: #0a0a0a;
                color: #00ffff;
                font-family: 'Segoe UI', Arial;
            }
        """)

        self.display_label = QLabel(self)
        self.display_label.setAlignment(Qt.AlignCenter)
        self.display_label.setWordWrap(True)
        self.display_label.setStyleSheet("color: white; padding: 20px; background: rgba(0, 0, 0, 0.3); border-radius: 10px;")

        # Digitação
        self._typing_timer = QTimer(self)
        self._typing_timer.timeout.connect(self._update_typing_effect)
        self._typing_index = 0
        self._typing_text = ""

    def mousePressEvent(self, event):
        if event.button() == Qt.LeftButton:
            if self.display_label.geometry().contains(event.pos()):
                self.text_dragging = True
                self.drag_start_pos = event.pos()
                self.setCursor(Qt.ClosedHandCursor)
            else:
                self.old_pos = event.globalPos()

    def mouseMoveEvent(self, event):
        if self.text_dragging and self.current_slide:
            delta = event.pos() - self.drag_start_pos
            current_rect = self.display_label.geometry()
            new_x = max(0, min(self.width() - current_rect.width(), current_rect.x() + delta.x()))
            new_y = max(0, min(self.height() - current_rect.height(), current_rect.y() + delta.y()))
            self.current_slide.text_x = new_x / (self.width() - current_rect.width()) if self.width() > current_rect.width() else 0.5
            self.current_slide.text_y = new_y / (self.height() - current_rect.height()) if self.height() > current_rect.height() else 0.5
            self.display_label.move(new_x, new_y)
            self.drag_start_pos = event.pos()
        elif event.buttons() == Qt.LeftButton and self.old_pos and not self.text_dragging:
            delta = QPoint(event.globalPos() - self.old_pos)
            self.move(self.x() + delta.x(), self.y() + delta.y())
            self.old_pos = event.globalPos()

    def mouseReleaseEvent(self, event):
        if event.button() == Qt.LeftButton:
            self.text_dragging = False
            self.setCursor(Qt.ArrowCursor)

    def show_typing_effect(self, slide, interval=50):
        self.current_slide = slide

        # Fundo
        if slide.background_image and os.path.exists(slide.background_image):
            pixmap = QPixmap(slide.background_image)
            scaled_pixmap = pixmap.scaled(self.size(), Qt.KeepAspectRatioByExpanding, Qt.SmoothTransformation)
            palette = QPalette()
            palette.setBrush(QPalette.Window, QBrush(scaled_pixmap))
            self.setPalette(palette)
            self.setAutoFillBackground(True)
        else:
            self.setStyleSheet("background-color: #0a0a0a;")
            self.setAutoFillBackground(False)

        # Fonte
        font = QFont(slide.font_family, slide.font_size)
        font.setBold(True)
        self.display_label.setFont(font)
        self.display_label.setStyleSheet(f"""
            color: {slide.font_color}; 
            padding: 20px; 
            background: rgba(0, 0, 0, 0.0);
            border-radius: 10px;
        """)
        self.display_label.setText("")

        # Posicionamento
        self.display_label.adjustSize()
        available_width = max(0, self.width() - self.display_label.width())
        available_height = max(0, self.height() - self.display_label.height())
        x = int(available_width * slide.text_x)
        y = int(available_height * slide.text_y)
        self.display_label.move(x, y)
        self.display_label.show()

        # Digitação
        self._typing_text = slide.text
        self._typing_index = 0
        self._typing_timer.start(interval)

    def _update_typing_effect(self):
        if self._typing_index < len(self._typing_text):
            current_text = self._typing_text[:self._typing_index + 1]
            self.display_label.setText(current_text)
            self._typing_index += 1
        else:
            self._typing_timer.stop()

    def display_slide(self, slide):
        self.current_slide = slide

        if slide.background_image and os.path.exists(slide.background_image):
            pixmap = QPixmap(slide.background_image)
            scaled_pixmap = pixmap.scaled(self.size(), Qt.KeepAspectRatioByExpanding, Qt.SmoothTransformation)
            palette = QPalette()
            palette.setBrush(QPalette.Window, QBrush(scaled_pixmap))
            self.setPalette(palette)
            self.setAutoFillBackground(True)
        else:
            self.setStyleSheet("background-color: #0a0a0a;")
            self.setAutoFillBackground(False)

        font = QFont(slide.font_family, slide.font_size)
        font.setBold(True)
        self.display_label.setFont(font)
        self.display_label.setStyleSheet(f"""
            color: {slide.font_color}; 
            padding: 20px; 
            background: rgba(0, 0, 0, 0.0);
            border-radius: 10px;
        """)
        self.display_label.setText(slide.text)

        self.display_label.adjustSize()
        available_width = max(0, self.width() - self.display_label.width())
        available_height = max(0, self.height() - self.display_label.height())
        x = int(available_width * slide.text_x)
        y = int(available_height * slide.text_y)
        self.display_label.move(x, y)
        self.display_label.show()

    def resizeEvent(self, event):
        super().resizeEvent(event)
        if self.current_slide:
            self.display_slide(self.current_slide)

    def keyPressEvent(self, event):
        if event.key() == Qt.Key_Escape and self.isFullScreen():
            self.showNormal()
        else:
            super().keyPressEvent(event)


class EditorWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.slide_groups = [SlideGroup("Main Presentation")]
        self.current_group_index = 0
        self.current_slide_index = 0
        self.display_window = DisplayWindow()
        self.project_file = ""
        
        # Initialize UI components that are referenced during setup
        self.slide_counter = QLabel("Slide 1 of 1")
        self.preview_label = QLabel()
        
        self.init_ui()
        self.apply_futuristic_style()
        self.update_slide_display()
        
    def keyPressEvent(self, event):
        """Handle keyboard events for slide navigation"""
        if event.key() == Qt.Key_Up:
            self.previous_slide()
        elif event.key() == Qt.Key_Down:
            self.next_slide()
        elif event.key() == Qt.Key_Left:
            self.previous_slide()
        elif event.key() == Qt.Key_Right:
            self.next_slide()
        elif event.key() == Qt.Key_Escape:
            if self.display_window.isFullScreen():
                self.display_window.showNormal()
        else:
            super().keyPressEvent(event)
        
    def init_ui(self):
        self.setWindowTitle("Church Presentation Editor - Futuristic Edition")
        self.setGeometry(200, 200, 1200, 800)
        
        # Central widget
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        # Main layout
        main_layout = QHBoxLayout()
        central_widget.setLayout(main_layout)
        
        # Left panel - Editor
        left_panel = QVBoxLayout()
        left_widget = QWidget()
        left_widget.setLayout(left_panel)
        left_widget.setMaximumWidth(450)
        
        # Tab widget for slide groups
        self.tab_widget = QTabWidget()
        self.tab_widget.setTabsClosable(True)
        self.tab_widget.tabCloseRequested.connect(self.close_tab)
        self.tab_widget.currentChanged.connect(self.tab_changed)
        left_panel.addWidget(self.tab_widget)
        
        # Tab management buttons
        tab_btn_layout = QHBoxLayout()
        self.add_tab_btn = QPushButton("+ Add Theme")
        self.rename_tab_btn = QPushButton("Rename Theme")
        self.add_tab_btn.clicked.connect(self.add_new_tab)
        self.rename_tab_btn.clicked.connect(self.rename_current_tab)
        tab_btn_layout.addWidget(self.add_tab_btn)
        tab_btn_layout.addWidget(self.rename_tab_btn)
        left_panel.addLayout(tab_btn_layout)
        
        # Initialize tabs
        self.setup_tabs()
        
        # Right panel - Preview and Controls
        right_panel = QVBoxLayout()
        right_widget = QWidget()
        right_widget.setLayout(right_panel)
        
        # Preview
        preview_group = QGroupBox("🖥️ PREVIEW")
        preview_layout = QVBoxLayout()
        
        self.preview_label = QLabel()
        self.preview_label.setMinimumSize(450, 300)
        self.preview_label.setStyleSheet("""border: 3px solid #00ffff; background-color: #1a1a1a; border-radius: 10px;""")
        self.preview_label.setAlignment(Qt.AlignCenter)
        self.preview_label.setWordWrap(True)
        self.preview_label.setScaledContents(False)
        preview_layout.addWidget(self.preview_label)
        
        preview_group.setLayout(preview_layout)
        right_panel.addWidget(preview_group)
        
        # Display controls
        display_group = QGroupBox("🎮 DISPLAY CONTROLS")
        display_layout = QVBoxLayout()
        
        # Display window controls
        display_btn_layout = QHBoxLayout()
        self.show_display_btn = QPushButton("🚀 Launch Display")
        self.hide_display_btn = QPushButton("🔒 Hide Display")
        self.show_display_btn.clicked.connect(self.show_display_window)
        self.hide_display_btn.clicked.connect(self.hide_display_window)
        display_btn_layout.addWidget(self.show_display_btn)
        display_btn_layout.addWidget(self.hide_display_btn)
        display_layout.addLayout(display_btn_layout)
        
        # Slideshow controls
        slideshow_layout = QHBoxLayout()
        self.prev_btn = QPushButton("⬅️ Previous")
        self.next_btn = QPushButton("➡️ Next")
        self.prev_btn.clicked.connect(self.previous_slide)
        self.next_btn.clicked.connect(self.next_slide)
        slideshow_layout.addWidget(self.prev_btn)
        slideshow_layout.addWidget(self.next_btn)
        display_layout.addLayout(slideshow_layout)
        
        # Initialize slide_counter BEFORE it's used
        self.slide_counter = QLabel("Slide 1 of 1")
        self.slide_counter.setAlignment(Qt.AlignCenter)
        display_layout.addWidget(self.slide_counter)
        
        display_group.setLayout(display_layout)
        right_panel.addWidget(display_group)
        
        main_layout.addWidget(left_widget)
        main_layout.addWidget(right_widget)
        
        # Menu bar
        self.create_menu_bar()

    def choose_font_color(self):
        color = QColorDialog.getColor()
        if color.isValid():
            if self.current_group_index < len(self.slide_groups):
                current_group = self.slide_groups[self.current_group_index]
                if self.current_slide_index < len(current_group.slides):
                    widgets = self.get_current_tab_widgets()
                    if widgets:
                        if widgets['apply_font_all_cb'].isChecked():
                            # Apply to all slides in current group
                            for slide in current_group.slides:
                                slide.font_color = color.name()
                        else:
                            # Apply to current slide only
                            current_group.slides[self.current_slide_index].font_color = color.name()
                        
                        # Update the color button appearance
                        widgets['font_color_btn'].setStyleSheet(
                            f"background-color: {color.name()}; border: 2px solid #00ffff; border-radius: 5px; min-height: 30px;"
                        )
                        self.update_slide_display()
        
    def setup_tabs(self):
        for i, group in enumerate(self.slide_groups):
            tab_widget = self.create_tab_content()
            self.tab_widget.addTab(tab_widget, group.name)
        
        if self.slide_groups:
            self.load_tab_data(0)
    
    def create_tab_content(self):
        tab_widget = QWidget()
        tab_layout = QVBoxLayout()
        tab_widget.setLayout(tab_layout)
        
        # Slide management
        slide_group = QGroupBox("📋 SLIDES")
        slide_layout = QVBoxLayout()
        
        # Slide list
        slide_list = QListWidget()
        slide_list.itemClicked.connect(self.slide_selected)
        slide_layout.addWidget(slide_list)
        
        # Slide control buttons
        slide_btn_layout = QHBoxLayout()
        add_slide_btn = QPushButton("➕ Add")
        delete_slide_btn = QPushButton("🗑️ Delete")
        add_slide_btn.clicked.connect(self.add_slide)
        delete_slide_btn.clicked.connect(self.delete_slide)
        slide_btn_layout.addWidget(add_slide_btn)
        slide_btn_layout.addWidget(delete_slide_btn)
        slide_layout.addLayout(slide_btn_layout)
        
        slide_group.setLayout(slide_layout)
        tab_layout.addWidget(slide_group)
        
        # Text editing
        text_group = QGroupBox("✏️ TEXT EDITOR")
        text_layout = QVBoxLayout()
        
        text_edit = QTextEdit()
        text_edit.textChanged.connect(self.text_changed)
        text_layout.addWidget(text_edit)
        
        text_group.setLayout(text_layout)
        tab_layout.addWidget(text_group)
        
        # Font settings
        font_group = QGroupBox("🎨 FONT SETTINGS")
        font_layout = QVBoxLayout()
        
        # Font family
        font_family_layout = QHBoxLayout()
        font_family_layout.addWidget(QLabel("Font:"))
        font_combo = QFontComboBox()
        font_combo.currentFontChanged.connect(self.font_changed)
        font_family_layout.addWidget(font_combo)
        font_layout.addLayout(font_family_layout)
        
        # Font size
        font_size_layout = QHBoxLayout()
        font_size_layout.addWidget(QLabel("Size:"))
        font_size_spin = QSpinBox()
        font_size_spin.setRange(8, 200)
        font_size_spin.setValue(24)
        font_size_spin.valueChanged.connect(self.font_changed)
        font_size_layout.addWidget(font_size_spin)
        font_layout.addLayout(font_size_layout)
        
        # Font color
        font_color_layout = QHBoxLayout()
        font_color_layout.addWidget(QLabel("Color:"))
        font_color_btn = QPushButton()
        font_color_btn.setStyleSheet("background-color: white; border: 2px solid #00ffff; border-radius: 5px; min-height: 30px;")
        font_color_btn.clicked.connect(self.choose_font_color)
        font_color_layout.addWidget(font_color_btn)
        font_layout.addLayout(font_color_layout)
        
        # Apply to all slides checkbox
        apply_font_all_cb = QCheckBox("Apply to all slides in this theme")
        font_layout.addWidget(apply_font_all_cb)
        
        font_group.setLayout(font_layout)
        tab_layout.addWidget(font_group)
        
        # Background settings
        bg_group = QGroupBox("🖼️ BACKGROUND")
        bg_layout = QVBoxLayout()
        
        # Background image
        bg_img_btn = QPushButton("🌄 Choose Image")
        bg_img_btn.clicked.connect(self.choose_background_image)
        bg_layout.addWidget(bg_img_btn)
        
        # Current background display
        bg_preview = QLabel("No background selected")
        bg_preview.setStyleSheet("""
            border: 2px solid #666; 
            padding: 10px; 
            border-radius: 5px;
            background-color: #2a2a2a;
        """)
        bg_preview.setMaximumHeight(80)
        bg_preview.setAlignment(Qt.AlignCenter)
        bg_layout.addWidget(bg_preview)
        
        # Apply to all slides checkbox
        apply_bg_all_cb = QCheckBox("Apply to all slides in this theme")
        bg_layout.addWidget(apply_bg_all_cb)
        
        # Remove background button
        remove_bg_btn = QPushButton("🗑️ Remove Background")
        remove_bg_btn.clicked.connect(self.remove_background)
        bg_layout.addWidget(remove_bg_btn)
        
        bg_group.setLayout(bg_layout)
        tab_layout.addWidget(bg_group)
        
        return tab_widget
    
    def apply_futuristic_style(self):
        self.setStyleSheet("""
            QMainWindow {
                background-color: #0d1117;
                color: #e6edf3;
            }
            
            QWidget {
                background-color: #0d1117;
                color: #e6edf3;
                font-family: 'Segoe UI', 'Arial';
            }
            
            QGroupBox {
                font-weight: bold;
                border: 2px solid #30363d;
                border-radius: 8px;
                margin-top: 1ex;
                padding-top: 15px;
                background-color: #161b22;
            }
            
            QGroupBox::title {
                subcontrol-origin: margin;
                left: 10px;
                padding: 0 8px 0 8px;
                color: #00d4aa;
                font-size: 12px;
            }
            
            QPushButton {
                background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                          stop: 0 #238636, stop: 1 #1f7a32);
                border: 1px solid #2ea043;
                border-radius: 6px;
                color: white;
                font-weight: bold;
                padding: 8px 16px;
                min-height: 20px;
            }
            
            QPushButton:hover {
                background: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1,
                                          stop: 0 #2ea043, stop: 1 #238636);
                border: 1px solid #46954a;
            }
            
            QPushButton:pressed {
                background: #1f7a32;
            }
            
            QTextEdit {
                background-color: #21262d;
                border: 2px solid #30363d;
                border-radius: 6px;
                color: #e6edf3;
                padding: 8px;
                font-size: 11px;
            }
            
            QTextEdit:focus {
                border: 2px solid #00d4aa;
            }
            
            QListWidget {
                background-color: #21262d;
                border: 2px solid #30363d;
                border-radius: 6px;
                color: #e6edf3;
                padding: 4px;
            }
            
            QListWidget::item {
                padding: 8px;
                border-radius: 4px;
                margin: 2px;
            }
            
            QListWidget::item:selected {
                background-color: #00d4aa;
                color: #0d1117;
                font-weight: bold;
            }
            
            QListWidget::item:hover {
                background-color: #30363d;
            }
            
            QTabWidget::pane {
                border: 2px solid #30363d;
                border-radius: 8px;
                background-color: #161b22;
                margin-top: -1px;
            }
            
            QTabBar::tab {
                background-color: #21262d;
                border: 2px solid #30363d;
                border-bottom: none;
                border-radius: 6px 6px 0 0;
                padding: 8px 16px;
                color: #e6edf3;
                font-weight: bold;
                margin-right: 2px;
            }
            
            QTabBar::tab:selected {
                background-color: #00d4aa;
                color: #0d1117;
                border: 2px solid #00d4aa;
            }
            
            QTabBar::tab:hover {
                background-color: #30363d;
            }
            
            QComboBox, QSpinBox {
                background-color: #21262d;
                border: 2px solid #30363d;
                border-radius: 6px;
                color: #e6edf3;
                padding: 6px;
                min-height: 20px;
            }
            
            QComboBox:focus, QSpinBox:focus {
                border: 2px solid #00d4aa;
            }
            
            QComboBox::drop-down {
                border: none;
                width: 20px;
            }
            
            QComboBox::down-arrow {
                image: none;
                border-left: 5px solid transparent;
                border-right: 5px solid transparent;
                border-top: 5px solid #e6edf3;
            }
            
            QCheckBox {
                color: #e6edf3;
                spacing: 8px;
            }
            
            QCheckBox::indicator {
                width: 18px;
                height: 18px;
            }
            
            QCheckBox::indicator:unchecked {
                background-color: #21262d;
                border: 2px solid #30363d;
                border-radius: 3px;
            }
            
            QCheckBox::indicator:checked {
                background-color: #00d4aa;
                border: 2px solid #00d4aa;
                border-radius: 3px;
                image: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIGhlaWdodD0iMTYiIGZpbGw9IiMwZDExMTciIHZpZXdCb3g9IjAgMCAxNiAxNiI+PHBhdGggZD0ibTEwLjk3IDQuOTctLjAyLjAyLTMuNDcgMy40Ny0xLjQ4LTEuNDhhLjc1Ljc1IDAgMCAwLTEuMDggMS4wNGwuMDIuMDJMMSA4bC4wMS4wMWEuNzUuNzUgMCAwIDAgMS4wNi0uMDJsMS40Ny0xLjQ3IDMuNDctMy40N2EuNzUuNzUgMCAwIDAtMS4wNi0xLjA2WiIvPjwvc3ZnPg==);
            }
            
            QLabel {
                color: #e6edf3;
            }
            
            QMenuBar {
                background-color: #161b22;
                color: #e6edf3;
                border-bottom: 1px solid #30363d;
            }
            
            QMenuBar::item {
                background: transparent;
                padding: 4px 8px;
            }
            
            QMenuBar::item:selected {
                background-color: #00d4aa;
                color: #0d1117;
                border-radius: 4px;
            }
            
            QMenu {
                background-color: #21262d;
                border: 1px solid #30363d;
                border-radius: 6px;
                color: #e6edf3;
            }
            
            QMenu::item {
                padding: 8px 24px;
                border-radius: 4px;
                margin: 2px;
            }
            
            QMenu::item:selected {
                background-color: #00d4aa;
                color: #0d1117;
            }
        """)
    
    def get_current_tab_widgets(self):
        current_tab = self.tab_widget.currentWidget()
        if not current_tab:
            return None
        
        # Find widgets in the current tab
        slide_list = current_tab.findChild(QListWidget)
        text_edit = current_tab.findChild(QTextEdit)
        font_combo = current_tab.findChild(QFontComboBox)
        font_size_spin = current_tab.findChild(QSpinBox)
        
        # Find the font color button (look for buttons and get the right one)
        buttons = current_tab.findChildren(QPushButton)
        font_color_btn = None
        for btn in buttons:
            if "background-color:" in btn.styleSheet():
                font_color_btn = btn
                break
        
        # Find background preview label
        bg_preview = None
        labels = current_tab.findChildren(QLabel)
        for label in labels:
            if label.maximumHeight() == 80 or "background" in label.text().lower():
                bg_preview = label
                break
        
        checkboxes = current_tab.findChildren(QCheckBox)
        apply_font_all_cb = checkboxes[0] if len(checkboxes) > 0 else None
        apply_bg_all_cb = checkboxes[1] if len(checkboxes) > 1 else None
        
        return {
            'slide_list': slide_list,
            'text_edit': text_edit,
            'font_combo': font_combo,
            'font_size_spin': font_size_spin,
            'font_color_btn': font_color_btn,
            'bg_preview': bg_preview,
            'apply_font_all_cb': apply_font_all_cb,
            'apply_bg_all_cb': apply_bg_all_cb
        }
    
    def choose_background_image(self):
        file_path, _ = QFileDialog.getOpenFileName(self, "Choose Background Image", "", "Images (*.png *.jpg *.jpeg *.bmp *.gif)")
        if file_path:
            if self.current_group_index < len(self.slide_groups):
                current_group = self.slide_groups[self.current_group_index]
                widgets = self.get_current_tab_widgets()
                
                if widgets and widgets['apply_bg_all_cb']:
                    if widgets['apply_bg_all_cb'].isChecked():
                        # Apply to all slides in current group
                        for slide in current_group.slides:
                            slide.background_image = file_path
                    else:
                        # Apply to current slide only
                        if self.current_slide_index < len(current_group.slides):
                            current_group.slides[self.current_slide_index].background_image = file_path
                    
                    self.update_background_preview(file_path)
                    self.update_slide_display()
    
    def add_new_tab(self):
        name, ok = QInputDialog.getText(self, 'New Theme', 'Enter theme name:')
        if ok and name:
            new_group = SlideGroup(name)
            self.slide_groups.append(new_group)
            
            tab_widget = self.create_tab_content()
            self.tab_widget.addTab(tab_widget, name)
            self.tab_widget.setCurrentIndex(len(self.slide_groups) - 1)
            
            self.current_group_index = len(self.slide_groups) - 1
            self.current_slide_index = 0
            self.load_tab_data(self.current_group_index)
    
    def rename_current_tab(self):
        current_index = self.tab_widget.currentIndex()
        if current_index >= 0:
            current_name = self.slide_groups[current_index].name
            name, ok = QInputDialog.getText(self, 'Rename Theme', 'Enter new name:', text=current_name)
            if ok and name:
                self.slide_groups[current_index].name = name
                self.tab_widget.setTabText(current_index, name)
    
    def close_tab(self, index):
        if len(self.slide_groups) > 1:
            reply = QMessageBox.question(self, 'Close Theme', 
                                       f'Are you sure you want to close "{self.slide_groups[index].name}"?',
                                       QMessageBox.Yes | QMessageBox.No)
            if reply == QMessageBox.Yes:
                del self.slide_groups[index]
                self.tab_widget.removeTab(index)
                
                if index <= self.current_group_index:
                    self.current_group_index = max(0, self.current_group_index - 1)
                
                self.load_tab_data(self.current_group_index)
    
    def tab_changed(self, index):
        if index >= 0:
            self.current_group_index = index
            self.current_slide_index = 0
            self.load_tab_data(index)
    
    def load_tab_data(self, group_index):
        if 0 <= group_index < len(self.slide_groups):
            widgets = self.get_current_tab_widgets()
            if widgets:
                self.update_slide_list()
                self.load_slide_data()
                self.update_slide_display()
    
    def update_slide_list(self):
        widgets = self.get_current_tab_widgets()
        if not widgets or self.current_group_index >= len(self.slide_groups):
            return
        
        slide_list = widgets['slide_list']
        slide_list.clear()
        
        current_group = self.slide_groups[self.current_group_index]
        for i, slide in enumerate(current_group.slides):
            preview_text = slide.text[:30] + "..." if len(slide.text) > 30 else slide.text
            if not preview_text.strip():
                preview_text = f"Slide {i+1}"
            slide_list.addItem(preview_text)
        
        if self.current_slide_index < len(current_group.slides):
            slide_list.setCurrentRow(self.current_slide_index)
        
        # Update counter
        total_slides = len(current_group.slides)
        self.slide_counter.setText(f"Slide {self.current_slide_index + 1} of {total_slides}")
    
    def slide_selected(self, item):
        widgets = self.get_current_tab_widgets()
        if widgets:
            slide_list = widgets['slide_list']
            self.current_slide_index = slide_list.row(item)
            self.load_slide_data()
            self.update_slide_display()
    
    def load_slide_data(self):
        widgets = self.get_current_tab_widgets()
        if not widgets or self.current_group_index >= len(self.slide_groups):
            return
        
        current_group = self.slide_groups[self.current_group_index]
        if self.current_slide_index < len(current_group.slides):
            slide = current_group.slides[self.current_slide_index]
            
            widgets['text_edit'].setText(slide.text)
            widgets['font_combo'].setCurrentFont(QFont(slide.font_family))
            widgets['font_size_spin'].setValue(slide.font_size)
            
            # Update font color button
            widgets['font_color_btn'].setStyleSheet(
                f"background-color: {slide.font_color}; border: 2px solid #00ffff; border-radius: 5px; min-height: 30px;"
            )
            
            self.update_background_preview(slide.background_image)
            
            # Update counter
            total_slides = len(current_group.slides)
            self.slide_counter.setText(f"Slide {self.current_slide_index + 1} of {total_slides}")
    
    def text_changed(self):
        if self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            if self.current_slide_index < len(current_group.slides):
                widgets = self.get_current_tab_widgets()
                if widgets:
                    current_group.slides[self.current_slide_index].text = widgets['text_edit'].toPlainText()
                    self.update_slide_list()
                    self.update_slide_display()
    
    def font_changed(self):
        if self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            widgets = self.get_current_tab_widgets()
            if widgets:
                # Get current font values from the widgets
                font_family = widgets['font_combo'].currentFont().family()
                font_size = widgets['font_size_spin'].value()
                
                if widgets['apply_font_all_cb'].isChecked():
                    # Apply to all slides in current group
                    for slide in current_group.slides:
                        slide.font_family = font_family
                        slide.font_size = font_size
                else:
                    # Apply to current slide only
                    if self.current_slide_index < len(current_group.slides):
                        current_group.slides[self.current_slide_index].font_family = font_family
                        current_group.slides[self.current_slide_index].font_size = font_size
                
                self.update_slide_display()
    
    def update_background_preview(self, image_path):
        widgets = self.get_current_tab_widgets()
        if widgets and widgets['bg_preview']:
            if image_path and os.path.exists(image_path):
                pixmap = QPixmap(image_path)
                scaled_pixmap = pixmap.scaled(200, 60, Qt.KeepAspectRatio, Qt.SmoothTransformation)
                widgets['bg_preview'].setPixmap(scaled_pixmap)
                widgets['bg_preview'].setText("")
            else:
                widgets['bg_preview'].clear()
                widgets['bg_preview'].setText("No background selected")
    
    def remove_background(self):
        widgets = self.get_current_tab_widgets()
        if widgets and self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            if widgets['apply_bg_all_cb'].isChecked():
                for slide in current_group.slides:
                    slide.background_image = ""
            else:
                if self.current_slide_index < len(current_group.slides):
                    current_group.slides[self.current_slide_index].background_image = ""
            
            self.update_background_preview("")
            self.update_slide_display()
    
    def add_slide(self):
        if self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            current_group.slides.append(Slide())
            self.current_slide_index = len(current_group.slides) - 1
            self.update_slide_list()
            self.load_slide_data()
            self.update_slide_display()
    
    def delete_slide(self):
        if self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            if len(current_group.slides) > 1 and self.current_slide_index < len(current_group.slides):
                del current_group.slides[self.current_slide_index]
                if self.current_slide_index >= len(current_group.slides):
                    self.current_slide_index = len(current_group.slides) - 1
                self.update_slide_list()
                self.load_slide_data()
                self.update_slide_display()
    
    def update_slide_display(self):
        if self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            if self.current_slide_index < len(current_group.slides):
                slide = current_group.slides[self.current_slide_index]
                
                # Update preview with background
                if slide.background_image and os.path.exists(slide.background_image):
                    pixmap = QPixmap(slide.background_image)
                    scaled_pixmap = pixmap.scaled(self.preview_label.size(), Qt.KeepAspectRatioByExpanding, Qt.SmoothTransformation)
                    
                    # Create a new pixmap with text overlay
                    preview_pixmap = QPixmap(self.preview_label.size())
                    preview_pixmap.fill(Qt.black)
                    
                    painter = QPainter(preview_pixmap)
                    
                    # Draw background image
                    painter.drawPixmap(0, 0, scaled_pixmap)
                    
                    # Draw text
                    font = QFont(slide.font_family, max(8, slide.font_size // 4))
                    font.setBold(True)
                    painter.setFont(font)
                    painter.setPen(QColor(slide.font_color))
                    
                    # Add semi-transparent background for text
                    text_rect = self.preview_label.rect()
                    painter.fillRect(text_rect, QColor(0, 0, 0, 100))
                    
                    # Convert int to Qt.AlignmentFlag if needed
                    if isinstance(slide.text_alignment, int):
                        alignment = Qt.AlignmentFlag(slide.text_alignment)
                    else:
                        alignment = slide.text_alignment
                    
                    painter.drawText(text_rect, alignment | Qt.TextWordWrap, slide.text)
                    painter.end()
                    
                    self.preview_label.setPixmap(preview_pixmap)
                else:
                    # No background image, just text
                    self.preview_label.clear()
                    font = QFont(slide.font_family, max(8, slide.font_size // 4))
                    font.setBold(True)
                    self.preview_label.setFont(font)
                    self.preview_label.setStyleSheet(f"""
                        color: {slide.font_color}; 
                        padding: 10px; 
                        border: 3px solid #00ffff;
                        background-color: #1a1a1a;
                        border-radius: 10px;
                    """)
                    self.preview_label.setText(slide.text)
                
                # Update display window if visible
                if self.display_window.isVisible():
                    self.display_window.display_slide(slide)
    
    def show_display_window(self):
        self.display_window.show()
        self.display_window.raise_()
        self.display_window.activateWindow()
        self.update_slide_display()
    
    def hide_display_window(self):
        self.display_window.hide()
    
    def previous_slide(self):
        if self.current_slide_index > 0:
            self.current_slide_index -= 1
            widgets = self.get_current_tab_widgets()
            if widgets:
                widgets['slide_list'].setCurrentRow(self.current_slide_index)
            self.load_slide_data()
            self.update_slide_display()
    
    def next_slide(self):
        if self.current_group_index < len(self.slide_groups):
            current_group = self.slide_groups[self.current_group_index]
            if self.current_slide_index < len(current_group.slides) - 1:
                self.current_slide_index += 1
                widgets = self.get_current_tab_widgets()
                if widgets:
                    widgets['slide_list'].setCurrentRow(self.current_slide_index)
                self.load_slide_data()
                self.update_slide_display()
    
    def create_menu_bar(self):
        menubar = self.menuBar()
        
        # File menu
        file_menu = menubar.addMenu('📁 File')
        
        new_action = QAction('🆕 New Project', self)
        new_action.setShortcut('Ctrl+N')
        new_action.triggered.connect(self.new_project)
        file_menu.addAction(new_action)
        
        open_action = QAction('📂 Open Project', self)
        open_action.setShortcut('Ctrl+O')
        open_action.triggered.connect(self.open_project)
        file_menu.addAction(open_action)
        
        file_menu.addSeparator()
        
        save_action = QAction('💾 Save Project', self)
        save_action.setShortcut('Ctrl+S')
        save_action.triggered.connect(self.save_project)
        file_menu.addAction(save_action)
        
        save_as_action = QAction('💾 Save Project As...', self)
        save_as_action.setShortcut('Ctrl+Shift+S')
        save_as_action.triggered.connect(self.save_project_as)
        file_menu.addAction(save_as_action)
        
        file_menu.addSeparator()
        
        exit_action = QAction('🚪 Exit', self)
        exit_action.setShortcut('Ctrl+Q')
        exit_action.triggered.connect(self.close)
        file_menu.addAction(exit_action)
        
        # View menu
        view_menu = menubar.addMenu('👁️ View')
        
        fullscreen_action = QAction('🖥️ Toggle Display Fullscreen', self)
        fullscreen_action.setShortcut('F11')
        fullscreen_action.triggered.connect(self.toggle_display_fullscreen)
        fullscreen_action.triggered.connect(self.toggle_display_fullscreen)
        view_menu.addAction(fullscreen_action)

        exit_fullscreen_action = QAction('🧯 Exit Fullscreen Only', self)
        exit_fullscreen_action.triggered.connect(self.exit_display_fullscreen_only)
        view_menu.addAction(exit_fullscreen_action)
        
        # Help menu
        help_menu = menubar.addMenu('❓ Help')
        
        about_action = QAction('ℹ️ About', self)
        about_action.triggered.connect(self.show_about)
        help_menu.addAction(about_action)

        typing_effect_action = QAction('⌨️ Show Typing Effect', self)
        typing_effect_action.triggered.connect(self.show_typing_effect_slide)
        view_menu.addAction(typing_effect_action)
    
    def toggle_display_fullscreen(self):
        self.display_window.setWindowState(Qt.WindowNoState)
        self.display_window.setWindowFlags(Qt.Window | Qt.FramelessWindowHint | Qt.WindowStaysOnTopHint)
        self.display_window.show()  # reaplica com os flags novos
        QApplication.processEvents()

        if self.display_window.isFullScreen():
            self.display_window.showNormal()
        else:
            self.display_window.showFullScreen()

    def show_typing_effect_slide(self):
        if self.current_group_index < len(self.slide_groups):
            group = self.slide_groups[self.current_group_index]
            if self.current_slide_index < len(group.slides):
                slide = group.slides[self.current_slide_index]
                self.display_window.show()
                self.display_window.raise_()
                self.display_window.activateWindow()
                self.display_window.show_typing_effect(slide)



    def exit_display_fullscreen_only(self):
        self.display_window.setWindowState(Qt.WindowNoState)
        self.display_window.setWindowFlags(Qt.Window | Qt.FramelessWindowHint | Qt.WindowStaysOnTopHint)
        self.display_window.showNormal()
        self.display_window.setGeometry(100, 100, 600, 600)
        QApplication.processEvents()

    
    def show_about(self):
        QMessageBox.about(self, "About Church Presenter", 
                         """
                         <h2>🎬 Church LED Panel Presenter</h2>
                         <p><b>Futuristic Edition v1.0</b></p>
                         <p>Professional presentation software for church LED panels</p>
                         <p>Features:</p>
                         <ul>
                         <li>Multiple themed slide groups</li>
                         <li>Advanced text and background editing</li>
                         <li>Fullscreen LED panel display</li>
                         <li>Professional dark theme interface</li>
                         </ul>
                         <p>Created by Gabriel N 💙</p>
                         """)
    
    def new_project(self):
        reply = QMessageBox.question(self, 'New Project', 
                                   'Create new project? Unsaved changes will be lost.',
                                   QMessageBox.Yes | QMessageBox.No)
        if reply == QMessageBox.Yes:
            self.slide_groups = [SlideGroup("Main Presentation")]
            self.current_group_index = 0
            self.current_slide_index = 0
            self.project_file = ""
            
            # Clear and recreate tabs
            self.tab_widget.clear()
            self.setup_tabs()
            
            self.setWindowTitle("Church Presentation Editor - Futuristic Edition")
    
    def save_project(self):
        if not self.project_file:
            self.save_project_as()
        else:
            self.save_to_file(self.project_file)
    
    def save_project_as(self):
        file_path, _ = QFileDialog.getSaveFileName(
            self, "Save Project", "", "Church Presentation Files (*.cpp)"
        )
        
        if file_path:
            self.project_file = file_path
            self.save_to_file(file_path)
            self.setWindowTitle(f"Church Presentation Editor - {os.path.basename(file_path)}")
    
    def save_to_file(self, file_path):
        try:
            project_data = {
                'slide_groups': [],
                'current_group': self.current_group_index,
                'current_slide': self.current_slide_index,
                'version': '2.0'
            }
            
            for group in self.slide_groups:
                group_data = {
                    'name': group.name,
                    'slides': []
                }
                
                for slide in group.slides:
                    slide_data = {
                        'text': slide.text,
                        'font_family': slide.font_family,
                        'font_size': slide.font_size,
                        'font_color': slide.font_color,
                        'background_image': slide.background_image,
                        'text_alignment': int(slide.text_alignment)
                    }
                    group_data['slides'].append(slide_data)
                
                project_data['slide_groups'].append(group_data)
            
            with open(file_path, 'w', encoding='utf-8') as f:
                json.dump(project_data, f, indent=2, ensure_ascii=False)
            
            QMessageBox.information(self, "Success", "🎉 Project saved successfully!")
            
        except Exception as e:
            QMessageBox.critical(self, "Error", f"❌ Failed to save project: {str(e)}")
    
    def open_project(self):
        file_path, _ = QFileDialog.getOpenFileName(
            self, "Open Project", "", "Church Presentation Files (*.cpp)"
        )
        
        if file_path:
            self.load_from_file(file_path)
    
    def load_from_file(self, file_path):
        try:
            with open(file_path, 'r', encoding='utf-8') as f:
                project_data = json.load(f)
            
            # Load slide groups
            self.slide_groups = []
            for group_data in project_data.get('slide_groups', []):
                group = SlideGroup(group_data.get('name', 'Unnamed Group'))
                group.slides = []
                
                for slide_data in group_data.get('slides', []):
                    slide = Slide()
                    slide.text = slide_data.get('text', '')
                    slide.font_family = slide_data.get('font_family', 'Arial')
                    slide.font_size = slide_data.get('font_size', 24)
                    slide.font_color = slide_data.get('font_color', '#FFFFFF')
                    slide.background_image = slide_data.get('background_image', '')
                    # Fix the alignment issue - ensure it's a Qt.AlignmentFlag
                    alignment_value = slide_data.get('text_alignment', Qt.AlignCenter)
                    if isinstance(alignment_value, int):
                        slide.text_alignment = Qt.AlignmentFlag(alignment_value)
                    else:
                        slide.text_alignment = alignment_value
                    group.slides.append(slide)
                
                # Ensure each group has at least one slide
                if not group.slides:
                    group.slides.append(Slide())
                
                self.slide_groups.append(group)
            
            # Ensure we have at least one group
            if not self.slide_groups:
                self.slide_groups = [SlideGroup("Main Presentation")]
            
            # Set current indices
            self.current_group_index = project_data.get('current_group', 0)
            if self.current_group_index >= len(self.slide_groups):
                self.current_group_index = 0
            
            self.current_slide_index = project_data.get('current_slide', 0)
            current_group = self.slide_groups[self.current_group_index]
            if self.current_slide_index >= len(current_group.slides):
                self.current_slide_index = 0
            
            self.project_file = file_path
            self.setWindowTitle(f"Church Presentation Editor - {os.path.basename(file_path)}")
            
            # Recreate tabs
            self.tab_widget.clear()
            self.setup_tabs()
            self.tab_widget.setCurrentIndex(self.current_group_index)
            
            QMessageBox.information(self, "Success", "🎉 Project loaded successfully!")
            
        except Exception as e:
            QMessageBox.critical(self, "Error", f"❌ Failed to load project: {str(e)}")
            # Create default project if loading fails
            self.new_project()

    def keyPressEvent(self, event):
        # Keyboard shortcuts for slideshow control
        if event.key() == Qt.Key_Left or event.key() == Qt.Key_Up:
            self.previous_slide()
        elif event.key() == Qt.Key_Right or event.key() == Qt.Key_Down:
            self.next_slide()
        elif event.key() == Qt.Key_Escape:
            if self.display_window.isFullScreen():
                self.display_window.showNormal()
        else:
            super().keyPressEvent(event)

def main():
    app = QApplication(sys.argv)
    
    # Set application properties
    app.setApplicationName("Church LED Panel Presenter")
    app.setApplicationVersion("2.0")
    app.setOrganizationName("Church Tech Solutions By Gabriel N")
    
    # Set application style
    app.setStyle('Fusion')
    
    # Create and show the editor window
    editor = EditorWindow()
    editor.show()
    
    sys.exit(app.exec_())

if __name__ == '__main__':
    main()
