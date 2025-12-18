# StatelessChatUI

Minimalist, stateless chat interface for LLM APIs with native support for extended thinking and streaming. Perfect for debugging, testing, and portable scenarios.

## Concept

StatelessChatUI operates as a pure client-side application without backend infrastructure. All conversation logic remains in the browser, with API calls executed directly from the client. The interface implements complete control over conversation state with granular manipulability via integrated JSON editor.

The architecture eliminates server dependencies entirely – the HTML document functions as an autonomous unit requiring only network access to the target API. This reduces deployment complexity to serving a single static file.

## Features

**Core Capabilities**
- Streaming-based response processing with delta accumulation
- Native interpretation of `<thinking>` tags and `reasoning`/`reasoning_content` content blocks
- Complete message history control via integrated JSON editor
- CORS-compatible direct connection to OpenAI-compatible APIs
- File/Vision support for multimodal LLM
- Import/export mechanisms for conversation persistence (JSON/JSONL)

**UI/UX Design**
- Dark-mode-first with minimal visual redundancy
- Collapsible extended thinking display (reasoning steps remain hidden until explicitly requested)
- Markdown rendering with syntax highlighting
- Token rate metrics during streaming operations
- Auto-scroll logic with manual override option

**Model Management**
- Dynamic loading of available models via `/v1/models` endpoint
- Temperature control (0.0 – 2.0)
- Max token limitation
- System prompt configuration with conversation persistence

**Developer Tools**
- Live JSON editor for direct state access
- Validation, beautification, minification of message history
- Append/replace modes for JSON-based message injection
- Drag-and-drop import

## Technical Specification

- **Dependencies:** None. Pure vanilla JavaScript.
- **Size:** Single-file deployment, ~47KB uncompressed
- **Browser Requirements:** Modern ES6+ support, Fetch API, Dialog element
- **API Compatibility:** OpenAI Chat Completion v1 spec

## Usage

### Local Execution

The simplest approach is opening the HTML file directly in your browser:

```bash
# Direct filesystem execution (no server required)
open chat.html
# or double-click the file in your file manager
```

For scenarios requiring CORS compatibility or advanced testing, optional web server deployment:

```bash
# Option 1: Python HTTP Server
python -m http.server 8000

# Option 2: Node.js HTTP Server
npx http-server -p 8000
```

### API Configuration

Set base URL and API key in the header section:

1. **Base URL:** API endpoint (e.g., `https://api.openai.com/v1`, `http://localhost:11434/v1`)
2. **API Key:** Optional depending on provider (OpenAI: required, local inference: often not required)
3. **Model Selection:** Either manual input or automatic loading via "Load Models"

The app automatically detects o1 models and disables system prompts according to API constraints.

### Conversation Management

**JSON Editor Access:**
- Open "Raw JSON Editor" below the chat log
- Manipulate message history directly
- Use "Apply (Replace)" for complete state override
- Use "Apply (Append)" for message injection

**Import/Export:**
- Export: Download as timestamped JSON
- Import: Drag & drop or file picker
- Format: Standard OpenAI messages array or custom wrapper with top-level `system`

## Extended Thinking Support

The interface interprets three thinking formats:

1. **XML Tags:** `<thinking>...</thinking>` (legacy format, some models)
2. **Content Blocks:** `{type: "reasoning_content", ...}` (structured, primary format)
3. **Named Content Blocks:** `{type: "thinking", ...}` (deprecated but supported)

Reasoning steps appear in collapsible detail elements above the final response. The logic aggregates thinking fragments during streaming and renders them incrementally.

## API Compatibility

**Tested with:**
- OpenAI Compatible Public API Services
- Local inference (llama.cpp, Ollama, LM Studio)

**Requirements:**
- `/v1/chat/completions` endpoint with SSE streaming
- CORS headers or proxy configuration
- Optional: `/v1/models` for model discovery

## Use Cases

**Rapid Prototyping:** Immediate test environment for prompt engineering without setup overhead.

**Conversation Analysis:** JSON editor enables surgical message manipulation for debugging context handling and response variance.

**Local Inference:** Direct integration with local inference servers without cloud dependencies.

**Research Tool:** Complete conversation exports for systematic evaluation of model behavior.

## Limitations

- No server-side persistence – conversations exist only in browser state
- CORS constraints require either permissive API headers or proxy setup
- No multi-modal support (text-only)
- No function calling integration
- Token counting is approximate (based on streaming metrics, not tokenizer-accurate)

## Development

The project follows a single-file philosophy. All functionality resides in `chat.html`:

- CSS: Inline `<style>` block (lines 7-129)
- HTML: Body structure (lines 130-220)
- JavaScript: Inline `<script>` block (lines 221-1070)

No build pipeline, no bundling, no dependencies. Pure vanilla.

## License

Apache 2.0