# pdf2word
this script converts pdf to editable word file
this Python script extracts UTF-8 text and images from a PDF file and saves it to an editable .docx file using the `pdfminer.six` and `python-docx` libraries:

```python
from pdfminer.high_level import extract_text, extract_pages
from pdfminer.layout import LTImage
from docx import Document
from docx.shared import Inches

# Open PDF file and extract text to string
with open('example.pdf', 'rb') as pdf_file:
    pdf_text = extract_text(pdf_file)
    
# Create new Word document
doc = Document()

# Add text to Word document
doc.add_paragraph(pdf_text)

# Extract images from PDF and add to Word document
for page_layout in extract_pages(pdf_file):
    for element in page_layout:
        if isinstance(element, LTImage):
            # Get image data
            image_data = element.stream.get_rawdata()
            
            # Add image to Word document
            doc.add_picture(image_data, width=Inches(element.width/72), height=Inches(element.height/72))

# Save Word document
doc.save('example.docx')
```

Here's how the script works:

1. The script opens the PDF file in read-binary mode using the `open` function with the `'rb'` mode argument.
2. The script uses the `extract_text` function from the `pdfminer.high_level` module to extract the text from the PDF file to a string.
3. The script creates a new Word document using the `Document` class from the `python-docx` library.
4. The script adds the PDF text to the Word document using the `add_paragraph` method of the `Document` object.
5. The script uses the `extract_pages` function from the `pdfminer.high_level` module to extract the layout information for each page in the PDF file.
6. The script loops through each layout element on each page and checks if it is an image using the `isinstance` function and the `LTImage` class from the `pdfminer.layout` module.
7. If the layout element is an image, the script gets the image data using the `get_rawdata` method of the `LTImage` object and adds the image to the Word document using the `add_picture` method of the `Document` object.
8. The script saves the Word document to a file using the `save` method of the `Document` object.

Note that this script assumes that you have installed the `pdfminer.six` and `python-docx` libraries, which can be installed using pip, the Python package installer. You can install these libraries by running `pip install pdfminer.six python-docx` in your terminal or command prompt. Additionally, note that the script may not work correctly for all PDF files, especially those with complex layouts or formatting. In those cases, you may need to use additional libraries or tools to extract text and images from the PDF file.
