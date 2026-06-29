# Agentic AI Blog Writing Agent

An end-to-end AI blog writing system built with LangGraph, Gemini, and Streamlit.

This project can:
- Route topics into closed-book, hybrid, or open-book writing modes
- Fetch and filter web evidence (via Tavily) for research-grounded writing
- Plan a structured blog outline
- Generate section drafts in parallel worker nodes
- Merge sections into a final markdown article
- Optionally generate and place technical images into the article

## Project Structure

- `blog_writing_agent_backend.py`: LangGraph workflow (router, research, planner, workers, reducer)
- `blog_writing_agent_frontend.py`: Streamlit app for generating, previewing, and exporting blogs
- `requirement.txt`: Python dependencies
- `.env`: API keys and environment configuration

## Requirements

- Python 3.11+
- Google Gemini API key
- (Optional but recommended) Tavily API key for web research mode

## Installation

1. Create and activate a virtual environment:

```powershell
python -m venv myenv
.\myenv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
pip install -r requirement.txt
```

## Environment Variables

Create or update `.env` in the project root:

```env
GOOGLE_API_KEY=your_google_api_key
TAVILY_API_KEY=your_tavily_api_key
```

Notes:
- `GOOGLE_API_KEY` is required for text generation and image generation.
- `TAVILY_API_KEY` is used for research retrieval. Without it, research returns empty results and the app falls back to non-web evidence behavior.

## Run the App

Start the Streamlit UI:

```powershell
streamlit run blog_writing_agent_frontend.py
```

Then open the local URL shown in your terminal (usually `http://localhost:8501`).

## How It Works

1. Router decides mode:
- `closed_book`: no web research needed
- `hybrid`: uses research for up-to-date references
- `open_book`: strongly research-driven (news/recency focused)

2. Research node (optional):
- Runs Tavily queries
- Normalizes and deduplicates evidence
- Applies recency filtering in open-book mode

3. Orchestrator:
- Produces a structured `Plan` with 5–9 section tasks

4. Worker fanout:
- Each task is drafted independently
- Sections are citation-aware based on mode and task flags

5. Reducer + images:
- Merges all sections into one markdown document
- Plans image placeholders
- Generates images and injects final markdown image blocks

## Output

- Final article is saved as `your_blog_title.md` in the project root.
- Images are saved in `images/` and referenced in markdown.
- Frontend supports:
  - Markdown preview with local image rendering
  - Download markdown only
  - Download images zip
  - Download full bundle zip

## Useful Commands

Check configured git remotes:

```powershell
git remote -v
```

Set `origin` remote URL:

```powershell
git remote set-url origin https://github.com/saurabhmethani/Agentic-AI-Blog-Writing-Agent.git
```

## Troubleshooting

- Missing API key errors:
  - Ensure `.env` exists and keys are valid.
  - Restart Streamlit after changing `.env`.

- No research evidence returned:
  - Verify `TAVILY_API_KEY` is set.
  - Check internet/network access.

- Image generation fails:
  - Verify `GOOGLE_API_KEY` and quotas.
  - The app gracefully inserts a fallback block into markdown when image generation fails.

