# QR Code Generator

This project is a Tkinter-based application for generating QR codes from URLs. The application allows users to enter a URL and an optional filename to generate and save the QR code as an SVG file.

## Features

- **URL Input:** Enter a URL to generate a QR code.
- **Filename Input:** Optionally enter a filename for the generated QR code.
- **QR Code Generation:** Generate and save QR codes as SVG files.
- **User-Friendly Interface:** Simple and intuitive Tkinter GUI.

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/your-username/qr-code-generator.git
    ```
2. Navigate to the project directory:
    ```sh
    cd qr-code-generator
    ```
3. Install the required dependencies:
    ```sh
    pip install pyqrcode pypng
    ```

## Usage

1. Run the main application script:
    ```sh
    python main.py
    ```
2. Enter the URL in the provided text box.
3. Optionally, enter a filename.
4. Click the "GENERATE QR CODE" button to generate and save the QR code.

## Code Explanation

- `QRCodeApp`: Main application class inheriting from `tk.Tk` that sets up the UI and handles events.
- `generate_qr_code()`: Method to handle the generation of QR codes when the button is clicked.
- `QRCode`: Helper class to create and save the QR code using the `pyqrcode` library.
  - `createCode(url, filename)`: Generates a QR code from the given URL and saves it as an SVG file.

## Example

```python
import tkinter as tk
from tkinter import messagebox
import pyqrcode
import re

class QRCodeApp(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("QR Code Generator")
        self.geometry("400x250")
        self.configure(bg="#f0f0f0")

        self.label = tk.Label(self, text="Enter URL:", bg="#f0f0f0", font=("Arial", 14))
        self.label.pack(pady=10)

        self.url_entry = tk.Entry(self, width=50, font=("Times New Roman", 12))
        self.url_entry.pack(pady=10)

        self.file_label = tk.Label(self, text="Enter filename (optional):", bg="#f0f0f0", font=("Arial", 14))
        self.file_label.pack(pady=5)

        self.file_entry = tk.Entry(self, width=50, font=("Times New Roman", 12))
        self.file_entry.pack(pady=10)

        self.generate_button = tk.Button(self, text="GENERATE QR CODE", command=self.generate_qr_code, bg="#4CAF50",
                                         fg="white", font=("Arial", 12))
        self.generate_button.pack(pady=10)

    def generate_qr_code(self):
        url = self.url_entry.get()
        filename = self.file_entry.get()

        if url:
            try:
                qr = QRCode()
                qr.createCode(url, filename)
                messagebox.showinfo("Success", "QR Code generated successfully!")
            except Exception as e:
                messagebox.showerror("Error", f"Failed to generate QR Code: {e}")
        else:
            messagebox.showwarning("Input Error", "Please enter a valid URL.")

class QRCode:
    def createCode(self, url: str, filename: str = ''):
        newurl = pyqrcode.create(url)
        domain = url.split('//')[-1].split('/')[0]
        domain = re.sub(r'^(www\.)?|(\.com|\.org|\.pl|\.net)$', '', domain)

        if filename:
            file_path = f'{filename}.svg'
        else:
            file_path = f'QRCode-{domain}.svg'

        newurl.svg(file_path, scale=8)
        self.qrcode = file_path

if __name__ == '__main__':
    app = QRCodeApp()
    app.mainloop()
