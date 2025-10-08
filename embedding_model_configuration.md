# Embedding Model Configuration Guide

> **Note:**  
> If you want to use the OpenAI embedding service with an API key managed by ReN3, **do not set any of the embedding model environment variables** described below. Please contact the ReN3 support team, who will handle all necessary configuration steps for your environment.

This guide provides step-by-step instructions for configuring an embedding model in the application. The embedding model settings must be applied in both the server and ingestor `.env` files to ensure consistent behavior across services.

## Overview

Embedding models are used to convert text or other data into vector representations for tasks such as semantic search, similarity comparison, and retrieval-augmented generation. The application supports integration with on-premise or external embedding providers.

## Configuration Parameters

Add the following parameters to both your server and ingestor `.env` files:

| Variable | Description |
|----------|-------------|
| `on_premise_embeddings_provider` | The provider for embeddings. Set to `onpremise` for self-hosted embedding services. |
| `on_premise_embeddings_api` | The API endpoint for the embedding model. Example: `http://localhost:11434/api/embeddings` |
| `on_premise_embeddings_apikey` | API key for authenticating with the embedding service (leave blank if not required). |
| `on_premise_embeddings_model` | The name or identifier of the embedding model to use. Example: `bge-m3:567m` |
| `onpremise_embeddingapi_inputkey` | The request payload key for input text expected by the embedding API. Use `prompt` for models like `bge-m3:567m`, and `input` for providers such as OpenAI or Qwen. |
| `onpremise_embeddingapi_supportsbatch` | Indicates if the embedding API supports batch processing (`true` or `false`). |
| `embeddings_batch_size` | Number of items to process in a single batch request. Adjust based on your model and hardware capabilities. |

## Example Configuration

### On-Premise Embedding Model Example

```env
on_premise_embeddings_provider=onpremise
on_premise_embeddings_api=http://localhost:11434/api/embeddings
on_premise_embeddings_apikey=
on_premise_embeddings_model=bge-m3:567m
onpremise_embeddingapi_inputkey=prompt
onpremise_embeddingapi_supportsbatch=true
embeddings_batch_size=100
```

### OpenAI Embedding Model Example

```env
on_premise_embeddings_provider=openai
on_premise_embeddings_api=https://api.openai.com/v1/embeddings
on_premise_embeddings_apikey=<your-openai-api-key>
on_premise_embeddings_model=text-embedding-3-small
onpremise_embeddingapi_inputkey=input
onpremise_embeddingapi_supportsbatch=true
embeddings_batch_size=100
```

## Parameter Details

- **on_premise_embeddings_provider**:  
  Set this to `onpremise` to use a self-hosted embedding model. For other providers, specify the appropriate value as per your deployment.

- **on_premise_embeddings_api**:  
  The full URL to the embedding API endpoint. Ensure this endpoint is accessible from both the server and ingestor services.

- **on_premise_embeddings_apikey**:  
  If your embedding API requires authentication, provide the API key here. Leave blank if authentication is not needed.

- **on_premise_embeddings_model**:  
  Specify the exact model name or version as required by your embedding API.

- **onpremise_embeddingapi_inputkey**:  
  The key used in the API request payload for input text (commonly `prompt` or `text`).

- **onpremise_embeddingapi_supportsbatch**:  
  Set to `true` if the API can process multiple inputs in a single request; otherwise, set to `false`.

- **embeddings_batch_size**:  
  Defines the maximum number of items sent per batch request. Adjust this value based on your API's capabilities and available resources.

## Additional Notes

- Ensure that the embedding API endpoint is reachable from both the server and ingestor environments.
- If using authentication, keep your API key secure and do not expose it in public repositories.
- Batch size should be tuned according to your hardware and the embedding model's performance characteristics.

## Troubleshooting

- **Connection Issues:**  
  Verify the API endpoint URL and network connectivity between your services and the embedding provider.

- **Authentication Errors:**  
  Ensure the API key is correct and has the necessary permissions.

- **Performance Issues:**  
  Adjust `embeddings_batch_size` and confirm that batch processing is supported by your embedding API.

## Using ReN3-Provided OpenAI Embedding API Key

If you wish to use the OpenAI embedding service with an API key managed by ReN3, **do not set any of the embedding model environment variables** listed above. Instead, please contact the ReN3 support team. The team will handle all necessary configuration steps for your environment and ensure proper integration with the ReN3-managed OpenAI API key.

By following these steps, you can successfully configure and integrate an embedding model into your application for enhanced search and retrieval capabilities.
