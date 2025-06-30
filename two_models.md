# Using Two Azure OpenAI Models in JudgeGPT
---

## **Overview**

JudgeGPT allows you to configure two Azure OpenAI models:
1. **Primary Model**: The default model
2. **Alternative Model**: A secondary model used for users with a userID from a provided selection.

The application dynamically initializes and routes requests to the appropriate model based on the configuration in the [`.env`](.env) file and the logic in the [`app.py`](app.py) file. Additionally, the title for conversations is always computed using the primary model, regardless of user routing.

---

## **Configuration Steps**

### 1. **Environment Variables**
The [`.env`](.env) file contains the configuration for both models. Below are the key variables:

```properties
# Primary Model Configuration
AZURE_OPENAI_MODEL=gpt-4o-mini
AZURE_OPENAI_KEY=<primary_model_api_key>
AZURE_OPENAI_RESOURCE=<primary_model_resource_name>
AZURE_OPENAI_ENDPOINT=https://<primary_model_resource_name>.openai.azure.com/

# Alternative Model Configuration
AZURE_OPENAI_ALT_MODEL=gpt-4.1-mini
AZURE_OPENAI_KEY_2=<alt_model_api_key>
AZURE_OPENAI_RESOURCE_2=<alt_model_resource_name>
AZURE_OPENAI_ENDPOINT_2=https://<alt_model_resource_name>.openai.azure.com/

# User Routing for Alternative Model
AZURE_OPENAI_ALT_MODEL_USER_IDS=00000000-0000-0000-0000-000000000000,auth0|6855852b951d58e1da2c2179
```

- **Primary Model**:
  - `AZURE_OPENAI_MODEL`: Name of the primary model.
  - `AZURE_OPENAI_KEY`: API key for the primary model.
  - `AZURE_OPENAI_RESOURCE`: Azure resource name for the primary model.
  - `AZURE_OPENAI_ENDPOINT`: Endpoint for the primary model.

- **Alternative Model**:
  - `AZURE_OPENAI_ALT_MODEL`: Name of the alternative model.
  - `AZURE_OPENAI_KEY_2`: API key for the alternative model.
  - `AZURE_OPENAI_RESOURCE_2`: Azure resource name for the alternative model.
  - `AZURE_OPENAI_ENDPOINT_2`: Endpoint for the alternative model.

- **User Routing**:
  - `AZURE_OPENAI_ALT_MODEL_USER_IDS`: Comma-separated string of user IDs that should use the alternative model. Do NOT use double-quotes when setting the value in .env or in Azure App Service Environment Variables

### 2. **Single vs. Double Model Usage**
- **Single Model**:
  - To use only the primary model, leave `AZURE_OPENAI_ALT_MODEL` empty in the [`.env`](.env) file.

- **Double Model**:
  - To enable both models, set `AZURE_OPENAI_ALT_MODEL` and its related variables (`AZURE_OPENAI_KEY_2`, `AZURE_OPENAI_RESOURCE_2`, `AZURE_OPENAI_ENDPOINT_2`, `AZURE_OPENAI_ALT_MODEL_USER_IDS`) in the [`.env`](.env) file.

### 3. **User ID for Routing**
- The backend uses the `user_principal_id` from the authenticated user's details to determine which model to route the request to.
- If the `user_principal_id` is in the `AZURE_OPENAI_ALT_MODEL_USER_IDS` list, the request is routed to the alternative model. Otherwise, it is routed to the primary model.

---