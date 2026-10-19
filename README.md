# mdx

A small CLI around [Microsoft MarkItDown](https://github.com/microsoft/markitdown)
that converts documents and URLs to Markdown, with optional image description and OCR
through a local MLX server or the EVREN LLM API (any OpenAI-compatible endpoint).

- Plain conversion runs fully offline (pdf, docx, pptx, xlsx, html, csv, ...).
- `-l` uses a local MLX VLM server. If the server is down, mdx starts it and stops it
  again when done. If it was already running, mdx leaves it alone.
- `-e` uses the EVREN API.
- `mdx status` and `mdx changedefaultmodel` show and change the settings.

## Requirements

- [uv](https://docs.astral.sh/uv/) (dependencies are resolved automatically on first run)
- For `-l`: a local MLX server on port 8080 and helper commands `mlx-start`, `mlx-stop`,
  `mlx-status` in your PATH (only needed for auto start/stop)
- For `-e`: an EVREN API key in the `EVRENAPI` environment variable

## Install

    mkdir -p ~/bin
    curl -fsSL https://raw.githubusercontent.com/mustafacoshkun/mdx/main/mdx -o ~/bin/mdx
    chmod +x ~/bin/mdx

Make sure `~/bin` is in your PATH.

## Usage

    mdx report.pdf -o report.md
    mdx slides.pptx -l -o slides.md
    mdx scan.pdf -l --ocr -o scan.md
    mdx *.docx -e -o ./md_out/
    mdx https://example.com/article

    mdx help
    mdx status
    mdx changedefaultmodel [evren|mlx]

## Settings

Resolved in this order: command-line flag, environment variable, config file
(`~/.config/mdx/config.json`), built-in default.

| Variable           | Meaning                                   |
| ------------------ | ----------------------------------------- |
| EVRENAPI           | EVREN API key                             |
| EVREN_BASE_URL     | EVREN base URL                            |
| EVREN_VISION_MODEL | EVREN model for image description         |
| MLX_BASE_URL       | local server URL (default 127.0.0.1:8080) |
| MLX_MODEL          | local model id                            |
| MLX_START          | profile passed to `mlx-start`             |

## Privacy

`-e` sends document content (images) to the EVREN API. Use plain mode or `-l` for
sensitive documents.

## License

MIT
