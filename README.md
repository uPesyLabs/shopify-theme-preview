# Create Shopify Theme Preview

This GitHub action creates a preview of a pull request for a Shopify store. Adding a simple comment in the corresponding pull request will trigger this action and creates links for a preview of the store and the shopify editor. This makes it easier to see the changes made in a pull request before they are merged.


## Inputs
| Name | Description | Example | Required |
| ---- | ----------- | ------- | :------: |
| `shopify_flag_store` | Your Store URL | `your-store.myshopify.com` | &check; |
| `shopify_cli_theme_token` | Password generated from [Theme Access App](https://shopify.dev/themes/tools/theme-access) | `shptka_7e95eace43t00be7f9f8612325212805` | &check; |
| `build_step` | Command used as build step | `npm i` | &cross; |
| `dir` | Directory to preview | `dist` | &cross; |
| `timezone` | Timezone to use (default is UTC) | `Europe/Berlin` | &cross; |


## Example usage

### Create a preview link and add it to the comment
This Action can be triggered by adding the `!preview` keyword to a pull request comment. It will then replace the entire comment with a table that includes a preview and editor link. This keyword can be changed to filter for a different string in the pull request comments. Remember to generate a `shopify_cli_theme_token` for the repository and pass it to the input of this action, along with the `shopify_flag_store`, which is your store URL. 

For more information on how to set up the `shopify_cli_theme_token` follow this [Theme Access App Guide](https://shopify.dev/themes/tools/theme-access)

### Optional inputs
Optionally you can add the inputs `build_step` and `dir` to the composite action. These inputs are used if there are specific requirements to preview the theme like a custom build step or changing the route to a different path before the preview is deployed.

### preview.yml

```yaml
run-name: Create Theme Preview by @${{ github.actor }}

permissions: 
  pull-requests: write

on:
  issue_comment:      
    types: [created]    
jobs:                   
  deploy:
    name: Preview
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, '!preview')
    steps:
      - uses: Brand-Boosting-GmbH/shopify-theme-preview@v1.0.6
        with:
          shopify_flag_store: 'your-store.myshopify.com'
          shopify_cli_theme_token: 'shopify_cli_theme_token'
          build_step: 'npm i && npm run build'              // optional
          dir: 'dist'                                       // optional
          timezone: 'Europe/Berlin'                         // optional

```

<p>To trigger the action write a comment with the defined keyword:</p>

![image](https://user-images.githubusercontent.com/77160493/206173680-5e960d83-807d-4205-9d25-b962e6a30091.png)

<p>After the action finished loading, the comment will be replaced with a table that contains the links for the Shopify store preview:</p>

![image](https://user-images.githubusercontent.com/77160493/210612083-000e254e-9706-4a05-bac3-ebae7fe07bcf.png)


## Composite action steps

The action has the following steps:

1. Get the current date and time
2. Create a comment on the pull request for the loading state with a table containing the name of the store, the status of the preview creation, and the date and time of the action
3. Check out the current pull request
4. Set up Node.js and Ruby
5. Install the Shopify CLI
6. Optional build step via `build-step`-input
7. Use the Shopify CLI to create the preview and save the returned preview link in an output object (optional path via `dir`-input)
8. Update the table to display the preview links
9. If any of the previous steps fail, create a comment on the pull request with an error message

---
<div style="display: inline">
  <img width="280" alt="wort-bild-primary@2x" src="https://user-images.githubusercontent.com/77160493/206194969-10dc2ed8-476d-4639-865e-75c9028109a4.png">
  <div>
    <b>Brand Boosting GmbH</b> |
    <b>David Süßlin</b>
  </div>
</div>
