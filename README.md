# action-style-quality
GitHub action that yells if style formatting and linting is not up to snuff!

# Requirements

[npm >= 7](https://github.blog/2021-02-02-npm-7-is-now-generally-available/)

Install [D(A)FT](https://github.com/reload/daft):

```sh
npm install @reloaddk/daft --save-dev
```

or the minimally required packages:

```sh
npm install stylelint prettier --save-dev
```

The action will make use of your installed version of the required tools.
So make sure to have them specified in your `package.json` and available from
`node_modules/.bin`

# Usage

## Root configuration

```yaml
name: Style Quality

on: pull_request

jobs:
  quality:
    name: Style Quality
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Setup Node
      uses: actions/setup-node@v2
      with:
        node-version: '16'
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Style Quality
      uses: reload/action-style-quality@1.0.0
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Subdirectory (theme) configuration

```yaml
name: Style Quality

on: pull_request

jobs:
  quality:
    name: Style Quality
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Setup Node
      uses: actions/setup-node@v2
      with:
        node-version: '16'
        cache: 'npm'
        cache-dependency-path: ./web/themes/custom/custom-theme/package-lock.json

    - name: Install dependencies
      run: npm ci
      working-directory: ./web/themes/custom/custom-theme

    - name: Style Quality
      uses: reload/action-style-quality@1.0.0
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        working_directory: ./web/themes/custom/custom-theme/scss
```

## Change targeted files

```yaml
jobs:
  quality:
    name: Style Quality
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Setup Node
      uses: actions/setup-node@v2
      with:
        node-version: '16'
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Style Quality
      uses: reload/action-style-quality@1.0.0
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        file_extensions: '.css,' # Default: '.css,.sass,.scss,.less'
```

## Override stylelint and prettier targets

```yaml
jobs:
  quality:
    name: Style Quality
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Setup Node
      uses: actions/setup-node@v2
      with:
        node-version: '16'
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Style Quality
      uses: reload/action-style-quality@1.0.0
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        # Both of the specified targets have their origin at whatever
        # 'working_directory' is set to.
        working_directory: './src' # Default: ''
        prettier_target: './code/**/*' # Default: './**/*'
        stylelint_target: './code/**/*' # Default: './**/*'
```
