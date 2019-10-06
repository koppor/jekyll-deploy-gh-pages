# A GitHub Action for Custom Jekyll Builds on GitHub Pages

> A GitHub Action for building and deploying a Jekyll repo back to its `gh-pages` branch **with `.nojekyll` to prevent GitHub rebuilding the page**.

**Why not just let GitHub Pages build it? Becaues this way we can use our own custom Jekyll plugins and build scripts.**

## Secrets

* `GITHUB_TOKEN`: Access key scoped to the repository, we need this to push the site files back to the repo. (specify in workflow)
  
## Environment Variables

* `GITHUB_ACTOR`: Username of repo owner or object intiating the action (GitHub Provides)
* `GITHUB_REPO`: Owner/Repository (GitHub Provides)

## Example

```yml
name: Jekyll Deploy

on: [push]

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - name: Build for GitHub Pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GITHUB_REPOSITORY: ${{ secrets.GITHUB_REPOSITORY }}
          GITHUB_ACTOR: ${{ secrets.GITHUB_ACTOR }}
        uses: BryanSchuetz/jekyll-deploy-gh-pages@master
      - name: Push to `gh-pages` branch
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: gh-pages
          force: true
          directory: build
```

Clones the repo, builds the site, and commits it back to the gh-pages branch of the repository.

## Caveats

* `destination:` should be set to `./build` in your _config.yml file — as God demands.
* Needs a `Gemfile`
* Be sure that any custom gems needed are included in your `Gemfile`.
* Pushed currently do not work on public repositories. We therefore rely on [GitHub Push](https://github.com/marketplace/actions/github-push).
