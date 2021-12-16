![Built With Stencil](https://img.shields.io/badge/-Built%20With%20Stencil-16161d.svg?logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDE5LjIuMSwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMCkgIC0tPgo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHZpZXdCb3g9IjAgMCA1MTIgNTEyIiBzdHlsZT0iZW5hYmxlLWJhY2tncm91bmQ6bmV3IDAgMCA1MTIgNTEyOyIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI%2BCjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI%2BCgkuc3Qwe2ZpbGw6I0ZGRkZGRjt9Cjwvc3R5bGU%2BCjxwYXRoIGNsYXNzPSJzdDAiIGQ9Ik00MjQuNywzNzMuOWMwLDM3LjYtNTUuMSw2OC42LTkyLjcsNjguNkgxODAuNGMtMzcuOSwwLTkyLjctMzAuNy05Mi43LTY4LjZ2LTMuNmgzMzYuOVYzNzMuOXoiLz4KPHBhdGggY2xhc3M9InN0MCIgZD0iTTQyNC43LDI5Mi4xSDE4MC40Yy0zNy42LDAtOTIuNy0zMS05Mi43LTY4LjZ2LTMuNkgzMzJjMzcuNiwwLDkyLjcsMzEsOTIuNyw2OC42VjI5Mi4xeiIvPgo8cGF0aCBjbGFzcz0ic3QwIiBkPSJNNDI0LjcsMTQxLjdIODcuN3YtMy42YzAtMzcuNiw1NC44LTY4LjYsOTIuNy02OC42SDMzMmMzNy45LDAsOTIuNywzMC43LDkyLjcsNjguNlYxNDEuN3oiLz4KPC9zdmc%2BCg%3D%3D&colorA=16161d&style=flat-square)

# How to set up semantic-relese with Stencil
## Set up `NPM_TOKEN`
Go to your repository's `Settings` -> `Secrets` -> `New repository secret`. 
In the `Name` field put `NPM_TOKEN`. Then in the `Value` past your npm token.


## Set up `GitHub Actions`
Go to your repository's `Actions` tab and press `set up a workflow yourself`:

<img width="1177" alt="image" src="https://user-images.githubusercontent.com/15261018/146361996-6561154d-70b9-4943-a7cf-644da3b57a4f.png">

Then copy and paste the below workflow set up

```yml
name: Release
'on':
  push:
    branches:
      - master
      - next
      - beta
      - '*.x'
jobs:
  release:
    name: release
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          cache: npm
          node-version: 16
      - run: npm ci
      - run: npm run test
      - run: npm run build
      - run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}

```
Press `Start commit` button to commit. 

And you're done! Next time you want to release a new version of your Stencil component, prefix your commit with `feat(desc):` or just `feat:` and a new minor version will be semanticly-released. 

If you want to release a fix, prefix your commit with `fix(desc):` or `fix:` and a fix to your Stencil component will be semanticly-released. 

`GITHUB_TOKEN` is a [default environment variable](https://docs.github.com/en/actions/learn-github-actions/environment-variables) and is going to be grabbed automatically so no need to set it up (apart from generating it in the first place). If you don't have your `NPM_TOKEN` and `GITHUB_TOKEN` yet, look below for a quick tutorial on how to generate them.

# Generate `NPM_TOKEN`
 
You can find out how to generate `NPM_TOKEN` [here](https://docs.npmjs.com/creating-and-viewing-access-tokens). Tip: for GitHub Actions you need to generate `Automation` token.

# Generate `GITHUB_TOKEN`
Go to your GitHub profile `Settings` -> `Developer settings` -> `Personal access tokens` and click `Generate new token` button. 

<img width="1219" alt="image" src="https://user-images.githubusercontent.com/15261018/146362633-beb85809-d0fc-4c37-b803-3e515871182e.png">

Give it some name in the `Note` field (I chose `semantic-release-token`) and tick everything in the  `repo` field (that's the only field you'll need).

<img width="1230" alt="image" src="https://user-images.githubusercontent.com/15261018/146362945-b58adf2f-7294-4e9a-87cd-78286ad9430c.png">

then press `Generate token` button. You'll be presented with your token. Save it in a safe place.
