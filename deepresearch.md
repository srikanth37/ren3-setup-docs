## DeepResearch Configuration (Server Service)

The **DeepResearch** feature in ReN3 delivers comprehensive research capabilities through an interactive chat interface. This powerful tool allows users to conduct complex information gathering and analysis operations using natural language.

### Core Capabilities

- **Intelligent Query Processing**: Users can ask complex questions requiring multi-step research
- **Multi-source Analysis**: Analyze information from attached documents (PDF, DOCX, images) and web URLs simultaneously
- **Web Research Integration**: Access to web search functionality and site browsing capabilities
- **Export Flexibility**: Support for multiple export formats including DOCX, PDF, CSV, JSON, TXT, HTML, and Markdown

### Use Cases

- Compile comprehensive research reports from multiple sources
- Extract and analyze information from uploaded documents
- Perform targeted web research and content evaluation
- Generate structured outputs in preferred formats

### Configuration Parameters

| Parameter               | Type    | Required    | Description                                                                                                       |
| ----------------------- | ------- | ----------- | ----------------------------------------------------------------------------------------------------------------- |
| `allow_deep_research`   | boolean | Yes         | Enable the DeepResearch functionality.                                                                            |
| `use_offline_resources` | boolean | Optional    | Set to `true` to allow operation in air-gapped or offline environments.                                           |
| `google_search_api_key` | string  | Conditional | Required for enabling web search functionality.                                                                   |
| `google_search_cx`      | string  | Conditional | Required for enabling web search functionality.                                                                   |
| `restricted_tools`      | array   | Optional    | List of tools to restrict. Leave empty (`[]`) to allow all research tools including web search and site browsing. |

### Example Configuration

```json
{
    "allow_deep_research": true,
    "use_offline_resources": false,
    "google_search_api_key": "YOUR_GOOGLE_API_KEY",
    "google_search_cx": "YOUR_GOOGLE_CX_ID",
    "restricted_tools": []
}
```

### Google Search Integration
To enable web search in DeepResearch, set both `google_search_api_key` and `google_search_cx` in your configuration. Get the API key from the [Google Cloud Console](https://console.cloud.google.com/apis/credentials) and the CX ID from the [Google Custom Search Engine](https://cse.google.com/cse/) dashboard. With these, DeepResearch can perform web searches in the chat interface.

Apply this configuration in the **Server Service** configuration file to activate DeepResearch functionality with the specified capabilities.

---