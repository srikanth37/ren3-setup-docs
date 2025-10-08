# LLM Models Configuration Guide

This document provides instructions for configuring custom (self-hosted or private) language model integrations within the application. These configurations must be applied to both the server and ingestor `config.json` files.

## Table of Contents
- [Overview](#overview)
- [Configuration Parameters](#configuration-parameters)
- [Configuration Examples](#configuration-examples)
- [Setup Instructions](#setup-instructions)

## Overview

The application supports integration with various language models, including self-hosted models and cloud service providers. The `on_premise_model` section in the configuration files allows you to define these integrations.

## Configuration Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `url` | String | The endpoint URL for the model API (required for `onpremise` type) |
| `apiKey` | String | Authentication key for accessing the model API |
| `region` | String | Geographic region for cloud service providers (required for `bedrock` type) |
| `name` | String | Unique display name for the model configuration |
| `llm` | String | Provider type: `openai`, `claude`, `mistral-groq`, `bedrock`, `onpremise` |
| `icon` | String | Visual indicator in the UI (options: `openai`, `claude-color`, `meta-color`, `mistral-color`, `gemma-color`, `deepseek-color`) |
| `model` | String | Specific model identifier provided by the LLM service |
| `max_snippets` | Integer | Maximum number of text snippets to process per request |
| `use_max_snippets` | Boolean | Whether to enforce the `max_snippets` limit |
| `max_fragments` | Integer | Maximum number of text fragments to process per request (0 means no limit) |
| `process_fragments_in_batches` | Integer | Number of fragments to process in parallel (0 processes all at once) |
| `max_input_tokens` | Integer | Maximum number of input tokens to send to the LLM |
| `max_output_tokens` | Integer | Maximum number of output tokens to request from the LLM |
| `max_image_files` | Integer | Number of images to process per request |
| `max_con_sub_calls` | Integer | Number of concurrent image LLM calls allowed per request |
| `local` | Boolean | Whether the LLM is hosted locally (`true`) or in the cloud (`false`) |
| `metadata_extraction_size` | Integer | Number of text snippets to use for metadata extraction (0 means use all) |
| `manual_json_schema` | Boolean | Whether to use manual JSON or structured output |
| `input` | Array | Types of input supported by the LLM (e.g., `["vision", "text"]`) |
| `capabilities` | Array | Capabilities of the LLM (e.g., `["metadata_extraction", "agentic", "deepresearch"]`) |

## Configuration Examples

### Example 1: Self-hosted Llama 3.2 Vision Model

```json
{
  "url": "http://localhost:11434/v1/chat/completions",
  "apiKey": "your-api-key-here",
  "name": "ReN3 Llama 3.2 Vision:11b Q4",
  "llm": "onpremise",
  "icon": "meta-color",
  "model": "llama3.2-vision:11b",
  "max_snippets": 16,
  "use_max_snippets": true,
  "max_fragments": 0,
  "process_fragments_in_batches": 10,
  "max_input_tokens": 16000,
  "max_output_tokens": 8192,
  "max_image_files": 1,
  "max_con_sub_calls": 1,
  "local": true,
  "metadata_extraction_size": 20,
  "manual_json_schema": true,
  "input": ["vision", "text"],
  "capabilities": ["metadata_extraction", "agentic"]
}
```

This configuration sets up a locally hosted Llama 3.2 Vision model with multimodal capabilities (text and image processing).

### Example 2: AWS Bedrock Claude Integration

```json
{
  "region": "ap-southeast-1",
  "name": "Bedrock - Claude 4 Sonnet",
  "llm": "bedrock",
  "icon": "claude-color",
  "model": "apac.anthropic.claude-sonnet-4-20250514-v1:0",
  "max_snippets": 48,
  "use_max_snippets": true,
  "max_fragments": 0,
  "process_fragments_in_batches": 10,
  "max_input_tokens": 100000,
  "max_output_tokens": 8192,
  "max_image_files": 3,
  "max_con_sub_calls": 3,
  "local": true,
  "metadata_extraction_size": 20,
  "manual_json_schema": true,
  "input": ["vision", "text"],
  "capabilities": ["metadata_extraction", "agentic"]
}
```

This configuration integrates with Claude 4 Sonnet via AWS Bedrock in the Asia Pacific (Singapore) region.

## Setup Instructions

### Adding Models to Configuration

1. Open the configuration files:
   - Server: `[installation_path]/server/config.json`
   - Ingestor: `[installation_path]/ingestor/config.json`

2. Locate the `on_premise_model` array in each file (create it if it doesn't exist).

3. Add your model configuration object with the appropriate parameters as shown in the examples above.

4. Save the configuration files and restart the respective services for changes to take effect.

### API Key Management for Supported LLM Providers

For the following LLM providers: **OpenAI, Claude, Mistral, and Mistral-Groq**, you have two options for API key management:

- **Using ReN3-Provided API Keys:**  
  If you wish to use API keys managed by ReN3, please contact the ReN3 support team. The team will handle the necessary configuration steps for your environment and assist with integration.

- **Using Your Own API Keys:**  
  If you have your own API keys for these providers, you can configure them directly in the relevant model configuration section.  
  - Set the `apiKey` field to your personal or organizational API key.
  - Ensure that your API key has the required permissions and quota for the intended usage.

**Note:** Always keep your API keys secure and do not share them publicly.

### Setting Up Authentication

#### For Self-Hosted Models:
1. Determine the API endpoint for your self-hosted model (e.g., `http://localhost:11434/v1/chat/completions` for locally hosted Ollama).
2. Generate or configure an API key if your model server requires authentication.
3. Update the `url` and `apiKey` fields in your configuration accordingly.

#### For AWS Bedrock:

1. Ensure you have an AWS account with access to the Amazon Bedrock service.
2. Bedrock access can be provided in two ways:
    - **Attach IAM Policy to Instance (Recommended):** Attach the following IAM policy to the IAM role associated with your EC2 instance to grant the necessary permissions for Bedrock model access:
      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
           {
              "Sid": "VisualEditor0",
              "Effect": "Allow",
              "Action": [
                "bedrock:InvokeModel",
                "bedrock:InvokeModelWithResponseStream",
                "bedrock:ListFoundationModels"
              ],
              "Resource": "*"
           }
        ]
      }
      ```
    - **Environment Variables:** Alternatively, you can set AWS credentials and region using environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_REGION`) on the host machine.
3. Specify the correct `region` where your Bedrock models are available in your configuration.


### Setting Default LLMs for Users

You can specify default language models for users by setting the following parameters in either the server `.env` file or the relevant `config.json`:

- `default_pref_model`:  
  The default model to be used for general (text-based) tasks.  
  Example:  
  ```
  default_pref_model=Azure - OpenAI GPT4o
  ```

- `default_pref_vision_model`:  
  The default model to be used for vision (image-based) tasks.  
  Example:  
  ```
  default_pref_vision_model=Claude 3.5 Sonnet
  ```

Set these values to the `name` of the desired model configuration as defined in your LLM configuration section. This ensures that users have a consistent default model for both text and vision scenarios unless they choose otherwise.

### Troubleshooting Common Issues

- **Connection Errors**: Verify the model endpoint URL is correct and accessible from the server.
- **Authentication Failures**: Confirm API keys are correctly set and have not expired.
- **Performance Issues**: Adjust batch sizes and token limits if experiencing timeouts or memory errors.
