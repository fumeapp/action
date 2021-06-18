# Fume Action

[![Latest Version](https://img.shields.io/github/release/fumeapp/action.svg?style=flat-square)](https://github.com/fumeapp/action/releases)

This Github Action provides a way to directly use Fume within your CI/CD pipeline.

## Requirements

To use this Github Action, you will need an active [Fume](https://fume.app) subscription.

## Usage

### 1. Setting up a Github Secret
In order to authenticate with Fume from Github Actions, we will need to add a `FUME_TOKEN` [secret](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets#creating-encrypted-secrets) to your repository.\
To do so, you may do the following:
1. On GitHub, navigate to the main page of the repository you intend to use this action on.
2. Under your repository name, click `Settings`.
3. In the left sidebar, click `Secrets`.
4. Click `Add a new secret`.
5. For the name of your secret, enter `FUME_TOKEN`.
6. For the value itself, enter your Fume token. You may generate one in your  [Fume Sessions](https://fume.app/session).
7. Click `Add secret`.
![Example of the Project Settings Secrets page](/images/project-settings-secrets.png)

### 2. Setting up our GitHub Action

Next, let's head over to the `Actions` page, and create a new workflow.\
To keep things simple, let's set up an action that deploys to environments `production` or `staging` when code is pushed to branches of the same name:

```yaml
name: Deploy with fume

on:
  push:
    branches: [ staging, production ]

jobs:
  fume:
    name: Deploy with Fume
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: fumeapp/action@master
      with:
        token: ${{ secrets.FUME_TOKEN }}
        # this passes the name of the branch
        environment: ${GITHUB_REF##*/}
```

Place this in `.github/workflows/deploy-fume.yml`

#### Explanation

The above does a few things:
1. It does a git checkout of your NuxtJS|NestJS App (your repository) using the `actions/checkout` action.
2. It executes the `fume` CLI command, passing in the arguments given. In our example, this means it runs `fume deploy production`.

If you would like to find out more regarding the syntax used by GitHub Actions, you can take a look at [this page](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#onevent_nametypes).

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email acidjazz@gmail.com instead of using the issue tracker.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
