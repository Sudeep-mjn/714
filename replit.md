# PDF Data Extraction and Auto-Fill Application

## Overview

This application extracts structured data from uploaded PDF documents and automatically fills that information into a standardized editable PDF template. The system processes up to 10 source PDFs at a time, extracts relevant field data using keyword matching, and generates filled versions of a template form for download.

## User Preferences

Preferred communication style: Simple, everyday language.

## Recent Changes (July 10, 2025)

✓ Fixed corrupted EditablePdf.pdf template by creating new editable PDF with reportlab
✓ Updated Streamlit app to display extracted data in the interface during processing
✓ Added extracted data view in results section with expandable sections
✓ Enhanced error handling for PDF form field filling
✓ Created test PDF sample with realistic data for testing extraction
✓ Improved keyword matching patterns to handle various field separators (: - _ =)
✓ Added debug logging for form field detection and mapping
✓ Fixed citizenship number extraction patterns to extract only clean numbers
✓ Removed duplicate family member extraction that was causing father/grandfather name issues
✓ Enhanced family name validation to prevent duplicates
✓ Improved in-law family relationship extraction patterns (daughter-in-law, father-in-law, mother-in-law)
✓ Added comprehensive pattern variations for better name extraction accuracy

## System Architecture

### Frontend Architecture
- **Framework**: Streamlit web application
- **Interface**: Simple file upload interface with progress feedback
- **State Management**: Streamlit session state for tracking processed files and completion status
- **File Handling**: Direct file upload with size and count validation (max 10 files)

### Backend Architecture
- **Processing Pipeline**: Three-stage process (upload → extract → fill)
- **Text Extraction**: PyMuPDF (fitz) for reading PDF content
- **Form Filling**: PyPDF2 for manipulating editable PDF forms
- **Data Flow**: In-memory processing with no persistent storage

### Data Processing Strategy
- **Extraction Method**: Keyword-based text parsing (no AI/ML)
- **Field Mapping**: Static dictionary mappings between extracted data and form fields
- **Template Handling**: Single static editable PDF template (`EditablePdf.pdf`)

## Key Components

### 1. PDFProcessor (`pdf_processor.py`)
**Purpose**: Extracts structured data from source PDF documents
- Uses PyMuPDF for text extraction across all pages
- Implements keyword-based field identification
- Supports 20+ common form fields (name, DOB, address, etc.)
- Returns cleaned, structured data dictionary

### 2. FieldMapper (`field_mapper.py`)
**Purpose**: Maps extracted data to editable PDF form fields
- Uses PyPDF2 for form field manipulation
- Maintains mapping between extracted fields and PDF form field names
- Handles multiple field name variations for the same data type
- Generates filled PDF as bytes for download

### 3. Main Application (`app.py`)
**Purpose**: Streamlit web interface and orchestration
- File upload validation and management
- Processing workflow coordination
- Results display and download functionality
- Session state management for multi-step process

## Data Flow

1. **Upload Phase**: User uploads 1-10 PDF files via Streamlit interface
2. **Validation Phase**: File count and type validation
3. **Processing Phase**: 
   - Extract text from each PDF using PyMuPDF
   - Parse text using keyword matching to identify fields
   - Map extracted data to template form fields
4. **Generation Phase**: Create filled PDF for each source document
5. **Download Phase**: Present individual filled PDFs for download

## External Dependencies

### Core Libraries
- **PyMuPDF (fitz)**: PDF text extraction and reading
- **PyPDF2**: PDF form manipulation and writing
- **Streamlit**: Web application framework

### Processing Dependencies
- **tempfile**: Temporary file handling during processing
- **base64**: File encoding for download functionality
- **io**: In-memory file operations
- **re**: Regular expression processing for text cleaning

## Deployment Strategy

### Runtime Requirements
- Python environment with PDF processing libraries
- Static template file (`EditablePdf.pdf`) must be present in root directory
- No database or persistent storage required

### Processing Constraints
- Maximum 10 PDF files per batch
- Only digital PDFs supported (no OCR)
- Template form fields must have predetermined names
- All processing occurs in-memory

### Output Format
- Individual filled PDFs named `<original_filename>_output.pdf`
- Direct download of each file (no ZIP packaging)
- Files generated on-demand without server storage

### Architecture Benefits
- **Simplicity**: No external databases or complex infrastructure
- **Portability**: Self-contained application with minimal dependencies
- **Deterministic**: Keyword-based parsing ensures consistent results
- **Scalable**: Stateless processing allows for easy horizontal scaling

### Limitations
- Static template approach limits flexibility
- Keyword matching may miss variations in field naming
- No persistent data storage or user accounts
- Limited to digital PDFs only