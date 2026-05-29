---
name: markitdown
description: Convert documents, PDFs, Office files, images, audio, HTML, and URLs to Markdown using the markitdown library. Trigger this skill whenever the user wants to convert or extract text from a file into Markdown format.
---

# Markitdown

Use the `markitdown` Python library to convert files and URLs to Markdown.

## When to use

- User provides a PDF, Word (.docx), Excel (.xlsx), PowerPoint (.pptx), HTML, image, or audio file and wants its content extracted or converted
- User asks to "convert to markdown", "extract text from", or "read this file"
- User pastes a URL and wants the page content as Markdown

## How to use

### Basic file conversion

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("path/to/file.pdf")
print(result.text_content)
```

### URL conversion

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("https://example.com")
print(result.text_content)
```

### Azure AI Content Understanding (for complex documents, images, audio)

Use this when basic conversion is insufficient — e.g., scanned PDFs, images with text, or audio transcription.

```python
from markitdown import MarkItDown
from azure.ai.contentunderstanding import ContentUnderstandingClient
from azure.identity import DefaultAzureCredential

client = ContentUnderstandingClient(
    endpoint="https://<your-resource>.cognitiveservices.azure.com/",
    credential=DefaultAzureCredential()
)

md = MarkItDown(content_understanding_client=client)
result = md.convert("path/to/scanned.pdf")
print(result.text_content)
```

## Supported formats

| Format | Extension |
|--------|-----------|
| PDF | .pdf |
| Word | .docx |
| Excel | .xlsx |
| PowerPoint | .pptx |
| Images | .png, .jpg, .jpeg, .gif, .webp |
| Audio | .mp3, .wav, .m4a |
| HTML | .html, .htm |
| URLs | http/https |
| Text/CSV/JSON/XML | various |
| ZIP | .zip (recurses into contents) |

## Rules

- Always use `result.text_content` to access the converted Markdown string
- For large files, convert first and summarize rather than printing everything raw
- If Azure credentials are not available, fall back to the basic `MarkItDown()` client without `content_understanding_client`
- When converting a URL, respect the site's content — do not scrape private or paywalled pages
