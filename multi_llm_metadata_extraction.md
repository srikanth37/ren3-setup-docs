# Configuring Multiple LLMs for Metadata Extraction

This guide explains how to enable and configure multiple language models (LLMs) for metadata extraction in your application. By following these steps, you can leverage different LLMs for various extraction scenarios, including both text and vision-based processing.

## Overview

The application supports flexible metadata extraction using multiple LLMs. This is controlled by two main settings:
- `show_multillm` in the server `.env` file
- `metadata_extraction_model_profiles` in the ingestor `config.json`

When `show_multillm` is enabled, the system uses the LLMs defined in `metadata_extraction_model_profiles` for metadata extraction, allowing you to specify different models and options for various use cases.

## Step 1: Enable Multi-LLM Support

In your server's `.env` file, set the following flag:

```
show_multillm=true
```

This enables the use of multiple LLMs for metadata extraction.

## Step 2: Configure Metadata Extraction Model Profiles

In the ingestor's `config.json`, define the `metadata_extraction_model_profiles` array. Each entry specifies a model and optional settings for how it should be used during metadata extraction.

### Example Configuration

```json
"metadata_extraction_model_profiles": [
  {"model_name": "Azure - OpenAI GPT4o"},
  {"model_name": "Azure - OpenAI GPT4.1"},
  {"model_name": "Claude 3.5 Sonnet", "options": {"manual_json_schema": true, "metadata_extraction_size": 5}},
  {"model_name": "Claude 3.7 Sonnet", "options": {"manual_json_schema": true, "metadata_extraction_size": 5}},
  {"model_name": "Llama 3.3 70B Versatile", "options": {"max_input_tokens": 4000, "manual_json_schema": true, "metadata_extraction_size": 20}},
  {"model_name": "Llama 3.3 70B", "options": {"max_input_tokens": 10000, "manual_json_schema": true, "metadata_extraction_size": 20}},
  {"model_name": "Azure - OpenAI GPT4o", "options": {"mode": "vision", "metadata_extraction_size": 20}},
  {"model_name": "Azure - OpenAI GPT4.1", "options": {"mode": "vision", "metadata_extraction_size": 20}},
  {"model_name": "Claude 3.5 Sonnet", "options": {"mode": "vision", "manual_json_schema": true, "metadata_extraction_size": 5}},
  {"model_name": "Claude 3.7 Sonnet", "options": {"mode": "vision", "manual_json_schema": true, "metadata_extraction_size": 5}}
]
```

- `model_name`: The display name of the model as defined in your LLM configuration.
- `options`: (Optional) Additional parameters for the model, such as:
  - `mode`: Specifies the processing mode (`"vision"` for image-based extraction, `"text"` for text-based extraction).
  - `metadata_extraction_size`: Number of snippets to use for extraction.
  - `manual_json_schema`: Whether to use a manual JSON schema for output.
  - Other model-specific options (e.g., `max_input_tokens`).

## How It Works

- **When `show_multillm` is set to `false`:**  
  The ingestor uses the model specified by `default_pref_ingestion_model` for all metadata extraction tasks, based on user-configured document types.

- **When `show_multillm` is set to `true`:**  
  The system uses the models listed in `metadata_extraction_model_profiles`. For each extraction task, it selects up to three models that support the required mode (`vision` for images, `text` or `ocr` for non-image files) according to your workspace settings.

  You can further customize each model's behavior by specifying options in the profile entry.

## Customizing Model Selection

- **Mode Selection:**  
  The `mode` option allows you to specify whether a model should be used for vision (image) or text extraction. Only models supporting the required mode will be considered for a given file type.

- **Additional Options:**  
  You can pass other attributes in the `options` object to fine-tune extraction, such as:
  - `metadata_extraction_size`
  - `manual_json_schema`
  - `max_input_tokens`
  - Any other supported model-specific parameters

## Summary

By enabling multi-LLM support and configuring `metadata_extraction_model_profiles`, you can optimize metadata extraction workflows by leveraging the strengths of different language models for various content types and extraction requirements.
