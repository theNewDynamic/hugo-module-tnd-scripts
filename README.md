
# TND Scripts Hugo Module

(intro)

## Requirements

Requirements:
- Go 1.14
- Hugo 0.61.0


## Installation

If not already, [init](https://gohugo.io/hugo-modules/use-modules/#initialize-a-new-module) your project as Hugo Module:

```
$: hugo mod init {repo_url}
```

Configure your project's module to import this module:

```yaml
# config.yaml
module:
  imports:
  - path: github.com/theNewDynamic/hugo-module-tnd-scripts
```

## Usage

### Registered scripts.
Without any configuration, the Module will consider `/assets/js/index.js` a registered script of name `main`.
In order to add more registrered scripts, user should reference them under the `scripts` key of the module configuration with the following keys:
__name__: The name of the script. Will be use to call it through the `tnd-scripts/tags` partial`
__path__: The `path` relative to the project's assets directory.
__target__: The `target`environment for the generated script. Defaults to `es2015`
__format__: The output `format`for the generated script. Either `iief` (immediately-invoked function expression), `csj` for CommonJS or `esm` for ECMAScript module. Defaults to `iief`.
__params__: Params will be passed to the script and available from it using the `import { params } from '@params'` statement.
Params can either be set of keys/value pairs or a string pointing to a returning partial and formated as such: `partial func/GetScriptParams`.

```
tnd_scripts:
  scripts:
    - name: main
      path: /js/index.js
    - name: carousel
      path: /js/carousel.jsx
      params:
        slides: [...]
    - name: shop
      path: /js/shop.jsx
      params: partial GetShopScriptData
```

### tnd-scripts/tags

By calling the `tnd-scripts/tags` partial, the module will print a `<script>` tag for each registered scripts.
If the context is a slice of strings, only the registered scripts whose name is included will be called:

#### Examples

##### Call all scripts
```
<head>
{{ partial "tnd-scripts/tags" "anything" }}
</head>
```
Will print:

```html
<head>
  <script crossorigin="anonymous" defer="defer" src="/js/index.js" type="text/javascript"></script>
  <script crossorigin="anonymous" defer="defer" src="/js/carousel.js" type="text/javascript"></script>
</head>
```

###### Call selected scripts

```
<head>
{{ partial "tnd-scripts/tags" (slice "carousel") }}
</head>
```
Will print:

```html
<head>
  <script crossorigin="anonymous" defer="defer" src="/js/carousel.js" type="text/javascript"></script>
</head>
```

## Global Settings

### env_keys

The module takes a list of environment variable keys to be replaced by their found value.

```yaml
params:
  tnd_scripts:
    env_keys:
      - API_KEY
      - SERVICE_URL
      ...
```

In order for your scripts to proper define variables when bundling, the module needs to know the list of environment variable keys which need to be fetched and properly replaced so that

```js
let public_api_key = process.env.API_KEY
let endpoint = process.env.SERVICE_URL
```

be processed as

```js
let public_api_key = "the_value_of_API_KEY"
let endpoint = "the_value_of_SERVICE_URL"
```

## theNewDynamic

This project is maintained and loved by [thenewDynamic](https://www.thenewdynamic.com).
