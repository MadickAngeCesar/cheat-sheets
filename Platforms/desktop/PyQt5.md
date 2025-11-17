# PyQt5 Cheat Sheet

## Table of Contents
1. [Installation & Setup](#installation--setup)
2. [Basic Application Structure](#basic-application-structure)
3. [Widgets](#widgets)
4. [Layouts](#layouts)
5. [Signals & Slots](#signals--slots)
6. [Dialogs & Windows](#dialogs--windows)
7. [Menus & Toolbars](#menus--toolbars)
8. [Events](#events)
9. [Styling (QSS)](#styling-qss)
10. [Model-View Architecture](#model-view-architecture)
11. [Threading](#threading)
12. [Database Integration](#database-integration)
13. [Graphics & Animation](#graphics--animation)
14. [Custom Widgets](#custom-widgets)
15. [Deployment](#deployment)

---

## Installation & Setup

```bash
# Install PyQt5
pip install PyQt5

# Install Qt Designer (optional)
pip install pyqt5-tools

# Install additional modules
pip install PyQt5-sip PyQtWebEngine
```

### Basic Imports
```python
from PyQt5.QtWidgets import (QApplication, QMainWindow, QWidget, 
                              QPushButton, QLabel, QVBoxLayout)
from PyQt5.QtCore import Qt, pyqtSignal, pyqtSlot, QThread, QTimer
from PyQt5.QtGui import QIcon, QFont, QPixmap, QPalette, QColor
import sys
```

---

## Basic Application Structure

### Minimal Application
```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("My App")
        self.setGeometry(100, 100, 800, 600)
        
        label = QLabel("Hello PyQt5!", self)
        self.setCentralWidget(label)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
```

### Application with QWidget
```python
class MyWidget(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
    
    def initUI(self):
        self.setWindowTitle('My Widget')
        self.setGeometry(300, 300, 400, 300)
        
        layout = QVBoxLayout()
        self.setLayout(layout)
```

---

## Widgets

### Common Widgets

```python
from PyQt5.QtWidgets import *

# Labels
label = QLabel("Text")
label.setText("New Text")
label.setAlignment(Qt.AlignCenter)

# Buttons
button = QPushButton("Click Me")
button.setEnabled(True)
button.clicked.connect(self.on_click)

# Line Edit (Text Input)
line_edit = QLineEdit()
line_edit.setPlaceholderText("Enter text...")
line_edit.setText("Default text")
text = line_edit.text()

# Text Edit (Multi-line)
text_edit = QTextEdit()
text_edit.setPlainText("Multi-line text")
text_edit.setHtml("<h1>HTML Content</h1>")

# Combo Box (Dropdown)
combo = QComboBox()
combo.addItems(["Option 1", "Option 2", "Option 3"])
combo.currentTextChanged.connect(self.on_combo_changed)

# Checkbox
checkbox = QCheckBox("Enable feature")
checkbox.setChecked(True)
is_checked = checkbox.isChecked()

# Radio Button
radio = QRadioButton("Option A")
radio.setChecked(True)

# Spin Box
spinbox = QSpinBox()
spinbox.setRange(0, 100)
spinbox.setValue(50)

# Slider
slider = QSlider(Qt.Horizontal)
slider.setRange(0, 100)
slider.valueChanged.connect(self.on_slider_change)

# Progress Bar
progress = QProgressBar()
progress.setValue(75)
progress.setRange(0, 100)

# List Widget
list_widget = QListWidget()
list_widget.addItems(["Item 1", "Item 2", "Item 3"])
list_widget.currentItemChanged.connect(self.on_item_changed)

# Table Widget
table = QTableWidget(5, 3)  # rows, columns
table.setHorizontalHeaderLabels(['Col1', 'Col2', 'Col3'])
table.setItem(0, 0, QTableWidgetItem("Data"))

# Tree Widget
tree = QTreeWidget()
tree.setHeaderLabels(['Name', 'Value'])
item = QTreeWidgetItem(['Parent', '1'])
child = QTreeWidgetItem(['Child', '2'])
item.addChild(child)
tree.addTopLevelItem(item)
```

---

## Layouts

### Layout Types

```python
# Vertical Box Layout
v_layout = QVBoxLayout()
v_layout.addWidget(widget1)
v_layout.addWidget(widget2)
v_layout.addStretch()  # Add flexible space

# Horizontal Box Layout
h_layout = QHBoxLayout()
h_layout.addWidget(widget1)
h_layout.addWidget(widget2)
h_layout.addSpacing(10)  # Add fixed space

# Grid Layout
grid = QGridLayout()
grid.addWidget(widget, row, col)
grid.addWidget(widget, row, col, rowspan, colspan)

# Form Layout
form = QFormLayout()
form.addRow("Name:", QLineEdit())
form.addRow("Email:", QLineEdit())

# Stacked Layout
stack = QStackedLayout()
stack.addWidget(page1)
stack.addWidget(page2)
stack.setCurrentIndex(0)

# Nested Layouts
main_layout = QVBoxLayout()
sub_layout = QHBoxLayout()
sub_layout.addWidget(btn1)
sub_layout.addWidget(btn2)
main_layout.addLayout(sub_layout)

# Layout Properties
layout.setSpacing(10)
layout.setContentsMargins(10, 10, 10, 10)
layout.setAlignment(Qt.AlignCenter)
```

---

## Signals & Slots

### Basic Connection
```python
# Connect signal to slot
button.clicked.connect(self.on_button_clicked)

# Slot definition
def on_button_clicked(self):
    print("Button clicked!")

# Lambda functions
button.clicked.connect(lambda: self.process_data("arg"))

# Multiple connections
button.clicked.connect(self.handler1)
button.clicked.connect(self.handler2)

# Disconnect
button.clicked.disconnect(self.handler1)
```

### Custom Signals
```python
from PyQt5.QtCore import pyqtSignal, QObject

class MyClass(QObject):
    # Define custom signals
    message_signal = pyqtSignal(str)
    data_signal = pyqtSignal(int, str)
    finished = pyqtSignal()
    
    def __init__(self):
        super().__init__()
    
    def do_something(self):
        # Emit signals
        self.message_signal.emit("Hello")
        self.data_signal.emit(42, "data")
        self.finished.emit()

# Usage
obj = MyClass()
obj.message_signal.connect(lambda msg: print(msg))
obj.data_signal.connect(self.handle_data)
```

### Decorator Syntax
```python
@pyqtSlot()
def on_button_clicked(self):
    print("Clicked")

@pyqtSlot(int)
def on_value_changed(self, value):
    print(f"Value: {value}")
```

---

## Dialogs & Windows

### Message Boxes
```python
from PyQt5.QtWidgets import QMessageBox

# Information
QMessageBox.information(self, "Title", "Message")

# Warning
QMessageBox.warning(self, "Warning", "Warning message")

# Critical
QMessageBox.critical(self, "Error", "Error message")

# Question with Yes/No
reply = QMessageBox.question(self, "Confirm", 
                              "Are you sure?",
                              QMessageBox.Yes | QMessageBox.No)
if reply == QMessageBox.Yes:
    print("Yes clicked")

# Custom MessageBox
msg = QMessageBox()
msg.setIcon(QMessageBox.Information)
msg.setWindowTitle("Custom Dialog")
msg.setText("Main message")
msg.setInformativeText("Additional info")
msg.setDetailedText("Detailed information")
msg.setStandardButtons(QMessageBox.Ok | QMessageBox.Cancel)
result = msg.exec_()
```

### File Dialogs
```python
from PyQt5.QtWidgets import QFileDialog

# Open File
filename, _ = QFileDialog.getOpenFileName(
    self, "Open File", "", 
    "Text Files (*.txt);;All Files (*)"
)

# Open Multiple Files
filenames, _ = QFileDialog.getOpenFileNames(
    self, "Select Files", "", "Images (*.png *.jpg)"
)

# Save File
filename, _ = QFileDialog.getSaveFileName(
    self, "Save File", "", "Python Files (*.py)"
)

# Select Directory
directory = QFileDialog.getExistingDirectory(
    self, "Select Directory"
)
```

### Input Dialogs
```python
from PyQt5.QtWidgets import QInputDialog

# Get Text
text, ok = QInputDialog.getText(self, "Input", "Enter name:")

# Get Integer
value, ok = QInputDialog.getInt(self, "Input", "Enter age:", 25, 0, 100)

# Get Double
value, ok = QInputDialog.getDouble(self, "Input", "Enter price:", 
                                     0.0, 0.0, 1000.0, 2)

# Get Item from List
items = ["Option 1", "Option 2", "Option 3"]
item, ok = QInputDialog.getItem(self, "Select", "Choose:", items)
```

### Custom Dialogs
```python
from PyQt5.QtWidgets import QDialog, QDialogButtonBox

class CustomDialog(QDialog):
    def __init__(self, parent=None):
        super().__init__(parent)
        self.setWindowTitle("Custom Dialog")
        
        layout = QVBoxLayout()
        
        self.input = QLineEdit()
        layout.addWidget(QLabel("Enter value:"))
        layout.addWidget(self.input)
        
        # Standard buttons
        buttons = QDialogButtonBox(
            QDialogButtonBox.Ok | QDialogButtonBox.Cancel
        )
        buttons.accepted.connect(self.accept)
        buttons.rejected.connect(self.reject)
        layout.addWidget(buttons)
        
        self.setLayout(layout)
    
    def get_value(self):
        return self.input.text()

# Usage
dialog = CustomDialog(self)
if dialog.exec_() == QDialog.Accepted:
    value = dialog.get_value()
```

### Multiple Windows
```python
class SecondWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Second Window")

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.second_window = None
    
    def open_second_window(self):
        if self.second_window is None:
            self.second_window = SecondWindow()
        self.second_window.show()
```

---

## Menus & Toolbars

### Menu Bar
```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.create_menu()
    
    def create_menu(self):
        menubar = self.menuBar()
        
        # File Menu
        file_menu = menubar.addMenu('&File')
        
        # Actions
        new_action = QAction('&New', self)
        new_action.setShortcut('Ctrl+N')
        new_action.setStatusTip('Create new file')
        new_action.triggered.connect(self.new_file)
        file_menu.addAction(new_action)
        
        open_action = QAction('&Open', self)
        open_action.setShortcut('Ctrl+O')
        open_action.triggered.connect(self.open_file)
        file_menu.addAction(open_action)
        
        file_menu.addSeparator()
        
        exit_action = QAction('E&xit', self)
        exit_action.setShortcut('Ctrl+Q')
        exit_action.triggered.connect(self.close)
        file_menu.addAction(exit_action)
        
        # Edit Menu
        edit_menu = menubar.addMenu('&Edit')
        
        # Submenu
        recent_menu = file_menu.addMenu('Recent Files')
        recent_menu.addAction('File1.txt')
        recent_menu.addAction('File2.txt')
```

### Toolbar
```python
def create_toolbar(self):
    toolbar = self.addToolBar('Main Toolbar')
    toolbar.setMovable(False)
    toolbar.setToolButtonStyle(Qt.ToolButtonTextBesideIcon)
    
    # Add actions
    new_action = QAction(QIcon('new.png'), 'New', self)
    new_action.triggered.connect(self.new_file)
    toolbar.addAction(new_action)
    
    toolbar.addSeparator()
    
    # Add widgets
    self.search_box = QLineEdit()
    self.search_box.setPlaceholderText('Search...')
    toolbar.addWidget(self.search_box)
    
    toolbar.addWidget(QPushButton('Search'))
```

### Status Bar
```python
def create_statusbar(self):
    self.statusBar().showMessage('Ready', 3000)  # 3 seconds
    
    # Permanent widget
    self.status_label = QLabel('Status: OK')
    self.statusBar().addPermanentWidget(self.status_label)
```

### Context Menu (Right-Click)
```python
def contextMenuEvent(self, event):
    menu = QMenu(self)
    
    copy_action = menu.addAction("Copy")
    paste_action = menu.addAction("Paste")
    menu.addSeparator()
    delete_action = menu.addAction("Delete")
    
    action = menu.exec_(self.mapToGlobal(event.pos()))
    
    if action == copy_action:
        self.copy()
    elif action == paste_action:
        self.paste()
```

---

## Events

### Mouse Events
```python
def mousePressEvent(self, event):
    if event.button() == Qt.LeftButton:
        print(f"Left click at {event.pos()}")
    elif event.button() == Qt.RightButton:
        print("Right click")

def mouseReleaseEvent(self, event):
    print("Mouse released")

def mouseMoveEvent(self, event):
    print(f"Mouse moved to {event.pos()}")
    
def mouseDoubleClickEvent(self, event):
    print("Double clicked")

def wheelEvent(self, event):
    delta = event.angleDelta().y()
    print(f"Wheel: {delta}")
```

### Keyboard Events
```python
def keyPressEvent(self, event):
    if event.key() == Qt.Key_Escape:
        self.close()
    elif event.key() == Qt.Key_Return:
        print("Enter pressed")
    elif event.key() == Qt.Key_Space:
        print("Space pressed")
    
    # Check modifiers
    if event.modifiers() == Qt.ControlModifier:
        if event.key() == Qt.Key_S:
            self.save()
    
    # Get text
    text = event.text()

def keyReleaseEvent(self, event):
    print("Key released")
```

### Window Events
```python
def closeEvent(self, event):
    reply = QMessageBox.question(
        self, 'Confirm Exit',
        'Are you sure you want to quit?',
        QMessageBox.Yes | QMessageBox.No
    )
    
    if reply == QMessageBox.Yes:
        event.accept()
    else:
        event.ignore()

def resizeEvent(self, event):
    print(f"Window resized to {event.size()}")

def moveEvent(self, event):
    print(f"Window moved to {event.pos()}")

def showEvent(self, event):
    print("Window shown")

def hideEvent(self, event):
    print("Window hidden")
```

### Paint Events
```python
from PyQt5.QtGui import QPainter, QPen

def paintEvent(self, event):
    painter = QPainter(self)
    painter.setPen(QPen(Qt.red, 2))
    painter.drawLine(0, 0, 100, 100)
    painter.drawRect(50, 50, 100, 100)
    painter.drawEllipse(200, 50, 100, 100)
```

### Event Filters
```python
def eventFilter(self, obj, event):
    if obj == self.target_widget:
        if event.type() == QEvent.KeyPress:
            print(f"Key pressed: {event.key()}")
            return True  # Event handled
    return super().eventFilter(obj, event)

# Install filter
self.target_widget.installEventFilter(self)
```

---

## Styling (QSS)

### Basic Styling
```python
# Inline style
widget.setStyleSheet("background-color: blue; color: white;")

# Application-wide style
app.setStyleSheet("""
    QPushButton {
        background-color: #4CAF50;
        color: white;
        border: none;
        padding: 10px;
        border-radius: 5px;
    }
    QPushButton:hover {
        background-color: #45a049;
    }
    QPushButton:pressed {
        background-color: #3d8b40;
    }
""")

# Load from file
with open('style.qss', 'r') as f:
    app.setStyleSheet(f.read())
```

### Advanced Styling
```python
stylesheet = """
    QMainWindow {
        background-color: #2b2b2b;
    }
    
    QLabel {
        color: #ffffff;
        font-size: 14px;
        font-family: Arial;
    }
    
    QLineEdit {
        background-color: #3c3c3c;
        color: #ffffff;
        border: 1px solid #555555;
        border-radius: 3px;
        padding: 5px;
    }
    
    QLineEdit:focus {
        border: 1px solid #0078d7;
    }
    
    QTableWidget {
        background-color: #2b2b2b;
        alternate-background-color: #3c3c3c;
        gridline-color: #555555;
    }
    
    QTableWidget::item:selected {
        background-color: #0078d7;
    }
    
    QScrollBar:vertical {
        border: none;
        background: #2b2b2b;
        width: 10px;
    }
    
    QScrollBar::handle:vertical {
        background: #555555;
        border-radius: 5px;
    }
"""
```

### Dynamic Styling
```python
# Set object name for specific styling
button.setObjectName("primaryButton")

# Style specific object
stylesheet = """
    #primaryButton {
        background-color: blue;
    }
"""

# Property-based styling
button.setProperty("type", "danger")
stylesheet = """
    QPushButton[type="danger"] {
        background-color: red;
    }
"""
```

---

## Model-View Architecture

### List Model
```python
from PyQt5.QtCore import QAbstractListModel, Qt

class MyListModel(QAbstractListModel):
    def __init__(self, data=None):
        super().__init__()
        self._data = data or []
    
    def rowCount(self, parent=None):
        return len(self._data)
    
    def data(self, index, role=Qt.DisplayRole):
        if role == Qt.DisplayRole:
            return self._data[index.row()]
        return None
    
    def add_item(self, item):
        self.beginInsertRows(QModelIndex(), len(self._data), len(self._data))
        self._data.append(item)
        self.endInsertRows()

# Usage
model = MyListModel(['Item 1', 'Item 2', 'Item 3'])
list_view = QListView()
list_view.setModel(model)
```

### Table Model
```python
from PyQt5.QtCore import QAbstractTableModel

class TableModel(QAbstractTableModel):
    def __init__(self, data):
        super().__init__()
        self._data = data
        self._headers = ['Column 1', 'Column 2', 'Column 3']
    
    def rowCount(self, parent=None):
        return len(self._data)
    
    def columnCount(self, parent=None):
        return len(self._data[0]) if self._data else 0
    
    def data(self, index, role=Qt.DisplayRole):
        if role == Qt.DisplayRole:
            return str(self._data[index.row()][index.column()])
        return None
    
    def headerData(self, section, orientation, role=Qt.DisplayRole):
        if role == Qt.DisplayRole and orientation == Qt.Horizontal:
            return self._headers[section]
        return None
    
    def flags(self, index):
        return Qt.ItemIsEnabled | Qt.ItemIsSelectable | Qt.ItemIsEditable
    
    def setData(self, index, value, role=Qt.EditRole):
        if role == Qt.EditRole:
            self._data[index.row()][index.column()] = value
            self.dataChanged.emit(index, index)
            return True
        return False

# Usage
data = [
    [1, 'Alice', 25],
    [2, 'Bob', 30],
    [3, 'Charlie', 35]
]
model = TableModel(data)
table_view = QTableView()
table_view.setModel(model)
```

### Tree Model
```python
class TreeItem:
    def __init__(self, data, parent=None):
        self._data = data
        self._parent = parent
        self._children = []
    
    def appendChild(self, item):
        self._children.append(item)
    
    def child(self, row):
        return self._children[row] if row < len(self._children) else None
    
    def childCount(self):
        return len(self._children)
    
    def data(self, column):
        return self._data[column] if column < len(self._data) else None
    
    def parent(self):
        return self._parent
    
    def row(self):
        if self._parent:
            return self._parent._children.index(self)
        return 0

class TreeModel(QAbstractItemModel):
    def __init__(self, data):
        super().__init__()
        self._root = TreeItem(['Name', 'Value'])
        self.setupModelData(data, self._root)
    
    def index(self, row, column, parent=QModelIndex()):
        if not self.hasIndex(row, column, parent):
            return QModelIndex()
        
        parent_item = parent.internalPointer() if parent.isValid() else self._root
        child_item = parent_item.child(row)
        
        if child_item:
            return self.createIndex(row, column, child_item)
        return QModelIndex()
    
    def parent(self, index):
        if not index.isValid():
            return QModelIndex()
        
        child_item = index.internalPointer()
        parent_item = child_item.parent()
        
        if parent_item == self._root:
            return QModelIndex()
        
        return self.createIndex(parent_item.row(), 0, parent_item)
    
    def rowCount(self, parent=QModelIndex()):
        parent_item = parent.internalPointer() if parent.isValid() else self._root
        return parent_item.childCount()
    
    def columnCount(self, parent=QModelIndex()):
        return 2
    
    def data(self, index, role=Qt.DisplayRole):
        if not index.isValid() or role != Qt.DisplayRole:
            return None
        item = index.internalPointer()
        return item.data(index.column())
```

---

## Threading

### QThread Basics
```python
from PyQt5.QtCore import QThread, pyqtSignal

class WorkerThread(QThread):
    progress = pyqtSignal(int)
    finished = pyqtSignal(str)
    
    def __init__(self):
        super().__init__()
        self._running = True
    
    def run(self):
        """Override this method with your task"""
        for i in range(100):
            if not self._running:
                break
            time.sleep(0.1)
            self.progress.emit(i + 1)
        
        self.finished.emit("Task completed!")
    
    def stop(self):
        self._running = False

# Usage
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.thread = None
    
    def start_task(self):
        self.thread = WorkerThread()
        self.thread.progress.connect(self.update_progress)
        self.thread.finished.connect(self.task_finished)
        self.thread.start()
    
    def update_progress(self, value):
        self.progress_bar.setValue(value)
    
    def task_finished(self, message):
        print(message)
        self.thread = None
```

### QRunnable with QThreadPool
```python
from PyQt5.QtCore import QRunnable, QThreadPool, pyqtSlot

class Worker(QRunnable):
    def __init__(self, fn, *args, **kwargs):
        super().__init__()
        self.fn = fn
        self.args = args
        self.kwargs = kwargs
    
    @pyqtSlot()
    def run(self):
        self.fn(*self.args, **self.kwargs)

# Usage
threadpool = QThreadPool()
print(f"Max threads: {threadpool.maxThreadCount()}")

def task(n):
    for i in range(n):
        print(f"Working... {i}")
        time.sleep(1)

worker = Worker(task, 5)
threadpool.start(worker)
```

### Worker with Signals (Better Pattern)
```python
from PyQt5.QtCore import QObject, QThread, pyqtSignal

class Worker(QObject):
    finished = pyqtSignal()
    error = pyqtSignal(str)
    progress = pyqtSignal(int)
    result = pyqtSignal(object)
    
    def __init__(self, task_func, *args, **kwargs):
        super().__init__()
        self.task_func = task_func
        self.args = args
        self.kwargs = kwargs
    
    def run(self):
        try:
            result = self.task_func(*self.args, **self.kwargs, 
                                   progress_callback=self.progress.emit)
            self.result.emit(result)
        except Exception as e:
            self.error.emit(str(e))
        finally:
            self.finished.emit()

# Usage
class MainWindow(QMainWindow):
    def start_worker(self):
        self.thread = QThread()
        self.worker = Worker(self.long_running_task)
        
        self.worker.moveToThread(self.thread)
        
        self.thread.started.connect(self.worker.run)
        self.worker.finished.connect(self.thread.quit)
        self.worker.finished.connect(self.worker.deleteLater)
        self.thread.finished.connect(self.thread.deleteLater)
        
        self.worker.progress.connect(self.update_progress)
        self.worker.result.connect(self.handle_result)
        self.worker.error.connect(self.handle_error)
        
        self.thread.start()
    
    def long_running_task(self, progress_callback=None):
        for i in range(100):
            time.sleep(0.05)
            if progress_callback:
                progress_callback(i + 1)
        return "Task completed"
```

---

## Database Integration

### SQLite with PyQt5
```python
from PyQt5.QtSql import QSqlDatabase, QSqlQuery, QSqlTableModel

class DatabaseManager:
    def __init__(self, db_name='app.db'):
        self.db = QSqlDatabase.addDatabase('QSQLITE')
        self.db.setDatabaseName(db_name)
        
        if not self.db.open():
            print("Error: Could not open database")
    
    def create_table(self):
        query = QSqlQuery()
        query.exec_("""
            CREATE TABLE IF NOT EXISTS users (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT NOT NULL,
                email TEXT UNIQUE,
                age INTEGER
            )
        """)
    
    def insert_user(self, name, email, age):
        query = QSqlQuery()
        query.prepare("INSERT INTO users (name, email, age) VALUES (?, ?, ?)")
        query.addBindValue(name)
        query.addBindValue(email)
        query.addBindValue(age)
        return query.exec_()
    
    def get_all_users(self):
        query = QSqlQuery("SELECT * FROM users")
        users = []
        while query.next():
            users.append({
                'id': query.value(0),
                'name': query.value(1),
                'email': query.value(2),
                'age': query.value(3)
            })
        return users
    
    def close(self):
        self.db.close()

# Using QSqlTableModel with QTableView
class DatabaseWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        
        self.model = QSqlTableModel()
        self.model.setTable('users')
        self.model.setEditStrategy(QSqlTableModel.OnFieldChange)
        self.model.select()
        
        # Set headers
        self.model.setHeaderData(0, Qt.Horizontal, "ID")
        self.model.setHeaderData(1, Qt.Horizontal, "Name")
        self.model.setHeaderData(2, Qt.Horizontal, "Email")
        
        self.table = QTableView()
        self.table.setModel(self.model)
        self.setCentralWidget(self.table)
```

---

## Graphics & Animation

### QPainter Drawing
```python
from PyQt5.QtGui import QPainter, QPen, QBrush, QColor, QFont
from PyQt5.QtCore import Qt, QRect, QPoint

class Canvas(QWidget):
    def paintEvent(self, event):
        painter = QPainter(self)
        painter.setRenderHint(QPainter.Antialiasing)
        
        # Draw line
        painter.setPen(QPen(Qt.red, 3))
        painter.drawLine(10, 10, 100, 100)
        
        # Draw rectangle
        painter.setPen(QPen(Qt.blue, 2))
        painter.setBrush(QBrush(QColor(100, 150, 200)))
        painter.drawRect(50, 50, 100, 80)
        
        # Draw ellipse
        painter.setPen(QPen(Qt.green, 2))
        painter.drawEllipse(200, 50, 100, 100)
        
        # Draw polygon
        points = [QPoint(300, 100), QPoint(350, 50), QPoint(400, 100)]
        painter.drawPolygon(*points)
        
        # Draw text
        painter.setPen(QPen(Qt.black))
        painter.setFont(QFont('Arial', 16))
        painter.drawText(QRect(50, 150, 200, 50), Qt.AlignCenter, "Hello PyQt5")
        
        # Draw image
        pixmap = QPixmap('image.png')
        painter.drawPixmap(0, 200, pixmap)
```

### QGraphicsView and QGraphicsScene
```python
from PyQt5.QtWidgets import QGraphicsScene, QGraphicsView, QGraphicsRectItem
from PyQt5.QtCore import QRectF
from PyQt5.QtGui import QBrush, QPen

class GraphicsExample(QMainWindow):
    def __init__(self):
        super().__init__()
        
        self.scene = QGraphicsScene()
        self.view = QGraphicsView(self.scene)
        self.setCentralWidget(self.view)
        
        # Add items
        rect = QGraphicsRectItem(0, 0, 100, 100)
        rect.setBrush(QBrush(Qt.blue))
        rect.setPen(QPen(Qt.red))
        rect.setFlag(QGraphicsRectItem.ItemIsMovable)
        rect.setFlag(QGraphicsRectItem.ItemIsSelectable)
        self.scene.addItem(rect)
        
        # Add ellipse
        ellipse = self.scene.addEllipse(150, 0, 100, 100, 
                                        QPen(Qt.green), QBrush(Qt.yellow))
        ellipse.setFlag(QGraphicsRectItem.ItemIsMovable)
        
        # Add text
        text = self.scene.addText("Draggable Text", QFont('Arial', 16))
        text.setPos(0, 150)
        text.setFlag(QGraphicsRectItem.ItemIsMovable)
        
        # Add line
        self.scene.addLine(0, 0, 200, 200, QPen(Qt.black, 2))
```

### Property Animation
```python
from PyQt5.QtCore import QPropertyAnimation, QEasingCurve

class AnimatedButton(QPushButton):
    def __init__(self, text, parent=None):
        super().__init__(text, parent)
        self.setup_animation()
    
    def setup_animation(self):
        self.animation = QPropertyAnimation(self, b"geometry")
        self.animation.setDuration(1000)
        self.animation.setStartValue(QRect(0, 0, 100, 50))
        self.animation.setEndValue(QRect(200, 200, 100, 50))
        self.animation.setEasingCurve(QEasingCurve.OutBounce)
    
    def start_animation(self):
        self.animation.start()

# Fade animation
from PyQt5.QtWidgets import QGraphicsOpacityEffect

effect = QGraphicsOpacityEffect()
widget.setGraphicsEffect(effect)

fade_animation = QPropertyAnimation(effect, b"opacity")
fade_animation.setDuration(1000)
fade_animation.setStartValue(1.0)
fade_animation.setEndValue(0.0)
fade_animation.start()
```

### Sequential and Parallel Animations
```python
from PyQt5.QtCore import QSequentialAnimationGroup, QParallelAnimationGroup

# Sequential
seq_group = QSequentialAnimationGroup()
seq_group.addAnimation(animation1)
seq_group.addAnimation(animation2)
seq_group.start()

# Parallel
par_group = QParallelAnimationGroup()
par_group.addAnimation(animation1)
par_group.addAnimation(animation2)
par_group.start()
```

---

## Custom Widgets

### Basic Custom Widget
```python
class CustomWidget(QWidget):
    # Custom signals
    valueChanged = pyqtSignal(int)
    
    def __init__(self, parent=None):
        super().__init__(parent)
        self._value = 0
        self.initUI()
    
    def initUI(self):
        self.setMinimumSize(200, 100)
    
    @property
    def value(self):
        return self._value
    
    @value.setter
    def value(self, val):
        if self._value != val:
            self._value = val
            self.valueChanged.emit(val)
            self.update()  # Trigger repaint
    
    def paintEvent(self, event):
        painter = QPainter(self)
        painter.fillRect(self.rect(), Qt.white)
        # Custom drawing
        painter.drawText(self.rect(), Qt.AlignCenter, 
                        f"Value: {self._value}")
    
    def mousePressEvent(self, event):
        if event.button() == Qt.LeftButton:
            self.value += 1
```

### Custom Button with Hover Effect
```python
class HoverButton(QPushButton):
    def __init__(self, text, parent=None):
        super().__init__(text, parent)
        self.setStyleSheet("""
            QPushButton {
                background-color: #4CAF50;
                color: white;
                border: none;
                padding: 10px;
                border-radius: 5px;
            }
        """)
    
    def enterEvent(self, event):
        self.setStyleSheet("""
            QPushButton {
                background-color: #45a049;
                color: white;
                border: none;
                padding: 10px;
                border-radius: 5px;
            }
        """)
    
    def leaveEvent(self, event):
        self.setStyleSheet("""
            QPushButton {
                background-color: #4CAF50;
                color: white;
                border: none;
                padding: 10px;
                border-radius: 5px;
            }
        """)
```

### Composite Custom Widget
```python
class LoginWidget(QWidget):
    loginAttempted = pyqtSignal(str, str)
    
    def __init__(self, parent=None):
        super().__init__(parent)
        self.initUI()
    
    def initUI(self):
        layout = QVBoxLayout()
        
        # Username
        self.username = QLineEdit()
        self.username.setPlaceholderText("Username")
        layout.addWidget(QLabel("Username:"))
        layout.addWidget(self.username)
        
        # Password
        self.password = QLineEdit()
        self.password.setPlaceholderText("Password")
        self.password.setEchoMode(QLineEdit.Password)
        layout.addWidget(QLabel("Password:"))
        layout.addWidget(self.password)
        
        # Button
        login_btn = QPushButton("Login")
        login_btn.clicked.connect(self.on_login)
        layout.addWidget(login_btn)
        
        self.setLayout(layout)
    
    def on_login(self):
        self.loginAttempted.emit(
            self.username.text(), 
            self.password.text()
        )
    
    def clear(self):
        self.username.clear()
        self.password.clear()
```

---

## Deployment

### PyInstaller
```bash
# Install PyInstaller
pip install pyinstaller

# Create executable (basic)
pyinstaller --onefile --windowed main.py

# With icon and name
pyinstaller --onefile --windowed --icon=app.ico --name=MyApp main.py

# With additional files
pyinstaller --onefile --windowed --add-data "resources;resources" main.py

# Spec file customization
pyi-makespec --onefile --windowed main.py
# Edit main.spec then run:
pyinstaller main.spec
```

### Spec File Example
```python
# main.spec
# -*- mode: python ; coding: utf-8 -*-

block_cipher = None

a = Analysis(
    ['main.py'],
    pathex=[],
    binaries=[],
    datas=[('resources', 'resources'), ('config.ini', '.')],
    hiddenimports=['PyQt5.QtPrintSupport'],
    hookspath=[],
    hooksconfig={},
    runtime_hooks=[],
    excludes=[],
    win_no_prefer_redirects=False,
    win_private_assemblies=False,
    cipher=block_cipher,
    noarchive=False,
)

pyz = PYZ(a.pure, a.zipped_data, cipher=block_cipher)

exe = EXE(
    pyz,
    a.scripts,
    a.binaries,
    a.zipfiles,
    a.datas,
    [],
    name='MyApp',
    debug=False,
    bootloader_ignore_signals=False,
    strip=False,
    upx=True,
    upx_exclude=[],
    runtime_tmpdir=None,
    console=False,
    disable_windowed_traceback=False,
    target_arch=None,
    codesign_identity=None,
    entitlements_file=None,
    icon='app.ico'
)
```

### Resource Management
```python
import sys
import os

def resource_path(relative_path):
    """Get absolute path to resource, works for dev and PyInstaller"""
    try:
        # PyInstaller creates a temp folder and stores path in _MEIPASS
        base_path = sys._MEIPASS
    except Exception:
        base_path = os.path.abspath(".")
    
    return os.path.join(base_path, relative_path)

# Usage
icon_path = resource_path('resources/icon.png')
config_path = resource_path('config.ini')
```

### Auto-py-to-exe (GUI Tool)
```bash
pip install auto-py-to-exe
auto-py-to-exe
```

### cx_Freeze Alternative
```python
# setup.py
from cx_Freeze import setup, Executable

build_exe_options = {
    "packages": ["PyQt5"],
    "include_files": ["resources/"],
    "excludes": []
}

setup(
    name="MyApp",
    version="1.0",
    description="My PyQt5 Application",
    options={"build_exe": build_exe_options},
    executables=[Executable("main.py", base="Win32GUI", icon="app.ico")]
)

# Build: python setup.py build
```

---

## Advanced Tips & Best Practices

### Application Configuration
```python
from PyQt5.QtCore import QSettings

class AppSettings:
    def __init__(self):
        self.settings = QSettings('MyCompany', 'MyApp')
    
    def save_geometry(self, widget):
        self.settings.setValue('geometry', widget.saveGeometry())
    
    def restore_geometry(self, widget):
        geometry = self.settings.value('geometry')
        if geometry:
            widget.restoreGeometry(geometry)
    
    def save_value(self, key, value):
        self.settings.setValue(key, value)
    
    def get_value(self, key, default=None):
        return self.settings.value(key, default)
```

### Drag and Drop
```python
class DragDropWidget(QWidget):
    def __init__(self):
        super().__init__()
        self.setAcceptDrops(True)
    
    def dragEnterEvent(self, event):
        if event.mimeData().hasUrls():
            event.accept()
        else:
            event.ignore()
    
    def dropEvent(self, event):
        files = [u.toLocalFile() for u in event.mimeData().urls()]
        for file in files:
            print(f"Dropped: {file}")
```

### Clipboard Operations
```python
from PyQt5.QtWidgets import QApplication

clipboard = QApplication.clipboard()

# Copy text
clipboard.setText("Hello World")

# Get text
text = clipboard.text()

# Copy image
clipboard.setPixmap(QPixmap("image.png"))

# Get image
pixmap = clipboard.pixmap()
```

### Timers
```python
from PyQt5.QtCore import QTimer

# Single shot timer
QTimer.singleShot(1000, self.callback)  # 1 second

# Repeating timer
self.timer = QTimer()
self.timer.timeout.connect(self.update_ui)
self.timer.start(100)  # Every 100ms

# Stop timer
self.timer.stop()
```

### System Tray Icon
```python
from PyQt5.QtWidgets import QSystemTrayIcon, QMenu

class TrayApp(QMainWindow):
    def __init__(self):
        super().__init__()
        self.create_tray_icon()
    
    def create_tray_icon(self):
        self.tray_icon = QSystemTrayIcon(self)
        self.tray_icon.setIcon(QIcon('icon.png'))
        
        # Create menu
        tray_menu = QMenu()
        show_action = tray_menu.addAction("Show")
        show_action.triggered.connect(self.show)
        quit_action = tray_menu.addAction("Quit")
        quit_action.triggered.connect(QApplication.quit)
        
        self.tray_icon.setContextMenu(tray_menu)
        self.tray_icon.show()
        
        # Show message
        self.tray_icon.showMessage(
            "App Name",
            "Application started",
            QSystemTrayIcon.Information,
            2000
        )
```

### Logging
```python
import logging
from PyQt5.QtCore import QObject, pyqtSignal

class QTextEditLogger(logging.Handler, QObject):
    appendPlainText = pyqtSignal(str)
    
    def __init__(self, parent):
        super().__init__()
        QObject.__init__(self)
        self.widget = QTextEdit(parent)
        self.widget.setReadOnly(True)
        self.appendPlainText.connect(self.widget.appendPlainText)
    
    def emit(self, record):
        msg = self.format(record)
        self.appendPlainText.emit(msg)

# Usage
log_handler = QTextEditLogger(self)
logging.getLogger().addHandler(log_handler)
logging.getLogger().setLevel(logging.INFO)
```

### Virtual Keyboard Support
```python
from PyQt5.QtCore import Qt

# Enable virtual keyboard
widget.setAttribute(Qt.WA_InputMethodEnabled, True)

# Handle input method events
def inputMethodEvent(self, event):
    # Handle virtual keyboard input
    pass
```

---

## Common Patterns & Snippets

### Singleton Window
```python
class SingletonWindow(QMainWindow):
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### Progress Dialog
```python
from PyQt5.QtWidgets import QProgressDialog

progress = QProgressDialog("Processing...", "Cancel", 0, 100, self)
progress.setWindowModality(Qt.WindowModal)

for i in range(100):
    if progress.wasCanceled():
        break
    # Do work
    progress.setValue(i + 1)

progress.close()
```

### Splash Screen
```python
from PyQt5.QtWidgets import QSplashScreen

pixmap = QPixmap('splash.png')
splash = QSplashScreen(pixmap)
splash.show()

# Show messages during loading
splash.showMessage("Loading modules...", Qt.AlignBottom | Qt.AlignCenter)
app.processEvents()

# Close after loading
splash.finish(main_window)
```

### Validation
```python
from PyQt5.QtGui import QValidator, QIntValidator, QDoubleValidator, QRegExpValidator
from PyQt5.QtCore import QRegExp

# Integer validation
int_validator = QIntValidator(0, 100)
line_edit.setValidator(int_validator)

# Double validation
double_validator = QDoubleValidator(0.0, 100.0, 2)
line_edit.setValidator(double_validator)

# Regex validation (email)
regex = QRegExp(r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}')
email_validator = QRegExpValidator(regex)
line_edit.setValidator(email_validator)
```

---

## Resources

- **Official Documentation**: https://doc.qt.io/qt-5/
- **PyQt5 Reference**: https://www.riverbankcomputing.com/static/Docs/PyQt5/
- **Qt Designer**: Visual UI designer tool
- **Qt Creator**: Full IDE for Qt development
- **Community**: Stack Overflow, Qt Forum, Reddit r/QtFramework

---

**Happy Coding with PyQt5! 🚀**
