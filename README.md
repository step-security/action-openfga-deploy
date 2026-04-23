[![StepSecurity Maintained Action](https://raw.githubusercontent.com/step-security/maintained-actions-assets/main/assets/maintained-action-banner.png)](https://docs.stepsecurity.io/actions/stepsecurity-maintained-actions)

# OpenFGA Github Action - Deploy

This action can be used to deploy your authorization model to an OpenFGA store.

## Parameter

| Parameter          | Required/Optional | Description                                                                                                            |
|--------------------|-------------------|------------------------------------------------------------------------------------------------------------------------|
| `api-url`          | Required          | The OpenFGA server to import the Authorization Model into                                                              |
| `store-id`         | Required          | The store to import the Authorization Model into                                                                       |
| `api-token`        | Optional          | When using pre-shared key auth, specify the shared secret to use for the request                                       |
| `api-token-issuer` | Optional          | When using client credentials auth, specify the token issuer to use for the request                                    |
| `client-id`        | Optional          | When using client credentials auth, specify the client ID to use for the request                                       |
| `client-secret`    | Optional          | When using client credentials auth, specify the client secret to use for the request                                   |
| `api-audience`     | Optional          | When using client credentials auth, specify the API audience to use for the request                                    |
| `api-scopes`       | Optional          | When using client credentials auth, specify the API scopes to use for the request                                      |
| `model-file-path`  | Optional          | The file path of the model to be imported                                                                              |
| `model`            | Optional          | The model to be imported if no file path is provided. Characters should be escaped. Used if `model-file-path` is empty |
| `format`           | Optional          | Authorization model input format. Can be "fga" or "json", defaults to auto-detecting from the file extension           |
| `fga_cli_version`  | Optional          | Version tag of openfga/cli to use (default "latest")                                                                   |

## Outputs

| Output                   | Description                                |
|--------------------------|--------------------------------------------|
| `authorization_model_id` | The ID of the deployed Authorization Model |

## Example

```yaml
name: Deploy Action

on:
  workflow_dispatch:

jobs:
  deploy:
    name: Deploy Authorization Model
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - name: Deploy using a file
        uses: step-security/action-openfga-deploy@v0
        with:
          api-url: http://localhost:8080
          api-token: ${{ secrets.KEY }}
          store-id: ${{ env.STORE_ID }}
          model-file-path: ./example/model.fga
      - name: Deploy using a string
        uses: step-security/action-openfga-deploy@v0
        with:
          api-url: http://localhost:8080
          api-token: ${{ secrets.KEY }}
          store-id: ${{ env.STORE_ID }}
          format: json
          model: '{\"schema_version\":\"1.1\",\"type_definitions\":\[\{\"type\":\"user\"\},\{\"type\":\"document\",\"relations\":\{\"reader\":\{\"this\":\{\}\},\"writer\":\{\"this\":\{\}\},\"owner\":\{\"this\":\{\}\}\},\"metadata\":\{\"relations\":\{\"reader\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\},\"writer\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\},\"owner\":\{\"directly_related_user_types\":\[\{\"type\":\"user\"\}\]\}\}\}\}\]\}'
```
