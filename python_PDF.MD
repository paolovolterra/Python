---
title: PDF
---


# estrarre testo da PDF

```Python
from tika import parser
raw = parser.from_file("BNL_20220507.pdf")
raw = str(raw)
safe_text = raw.encode('utf-8', errors='ignore')
safe_text = str(safe_text).replace("\n", "").replace("\\", "")
print('--- safe text ---' )
print( safe_text )

```

# Preserve Whitespaces While Extracting PDF Text Using Python and Pytesseract
An alternative way to extract PDF text using pdf2image and pytesseract

PDF data could be tricky to deal with in a data science project. For example, you try to extract text from PDF for a Natural Language Processing (NLP) project, you might experience missing whitespace between words or separating whole words with random whitespaces. You can’t develop any meaningful NLP models without correct whitespace between words. In this article, I’m going to introduce an alternative way to extract text from PDF whiling preserving whitespaces: pdf2image and pytesseract.

There are numerous packages, (such as, PyPDF2, pdfPlumber, Textract) that can extract text from PDF. Each has its own strengths and weakness. One package might be better at handling tables, others are better at extracting text. But there is no one-size-fits-all solution.

Take the following PDF file for example, we would like to extract text from this paragraph. It looks straightforward, but it could become a headache if the whitespaces between words can’t be correctly specified.

## Issue 1: Missing Whitespaces

In the following code, “PyPDF2” package is used to extract the PDF. As you can see, the whitespaces are NOT preserved. The output would be useless if our machine learning model need to understand the context of the text.

	import PyPDF2
	file = open('examle.pdf', 'rb')
	pdfReader = PyPDF2.PdfFileReader(file)
	ocr_text = pdfReader.getPage(0).extractText()

	
## Issue 2: Random and Useless Whitespaces

In the following code, “pdfplumber” package is used. As you can see, the whitespaces are NOT correctly specified. And the random separation of whole words makes the output useless for NLP projects.

	import pdfplumber
	file = pdfplumber.open('examle.pdf')
	ocr_text = file.pages[0].extract_text()


## Preserving Meaningful Whitespaces using pdf2image and Pytesseract

Instead of relying on PDF structure to extract the underlying text, we can convert PDF into Image(s), then use an OCR engine (e.g., Tesseract) to extract text from the image(s).
### Required Libraries

    pdf2image: to convert a PDF file to image(s)
    pytesseract: to extract text from image(s)

### Install Libraries

	pip install pdf2image
	pip install pytesseract

### Download and Install additional software

We would need additional software to use the libraries.

    For pdf2image, we will have to download the poppler for windows users.
    For pytesseract, we will need to install Tesseract-OCR Engine.

### Import Libraries

	import pytesseract
	from pdf2image import convert_from_path

### Initialize pytesseract and pdf2image

After you download and install the software, you can add their executable paths into Environment Variables. Alternatively, you can directly include their paths in the program.

	poppler_path = '...\pdf2image_poppler\Release-22.01.0-0\poppler-22.01.0\Library\bin'
	pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'

Implementation is straightforward. “convert_from_path” is used to convert PDF into an image. and “pytesseract.image_to_string” is used to extract the text from the image. As you can see, in this example, whitespaces between words are correctly specified.

# convert PDF to image
images = convert_from_path('examle.pdf', poppler_path=poppler_path)
# Extract text from image
ocr_text = pytesseract.image_to_string(images[0])


# Handle Multiple Pages in PDF

If there are multiple pages in a PDF, we can simply use a loop function to combine text from all the pages.

	images = convert_from_path('example.pdf', poppler_path=poppler_path)
	ocr_text = ''
	for i in range(len(images)):        
		page_content = pytesseract.image_to_string(images[i])
		page_content = '***PDF Page {}***\n'.format(i+1) + page_content
		ocr_text = ocr_text + ' ' + page_content

# Not just PDF, Pytesseract Works for Image Files as well

Another advantage of using pytesseract instead other packages is it can directly extract text from an image file. If the resolution of a image is NOT good enough or there is noisy background in the image, we will need to pre-process the image using additional packages (e.g., cv2).

pytesseract.image_to_string('example.tif')
pytesseract.image_to_string('example.jpg')
pytesseract.image_to_string('example
