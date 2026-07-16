# Brantlab website

A custom Hugo website for Brantlab IT Consulting.

## Preview locally

```sh
hugo server
```

Then open `http://localhost:1313`.

## Deploy

Run `hugo --minify` and publish the generated `public/` directory. The included GitHub Actions workflow builds and deploys pushes to `main` or `master` automatically.

The site uses a custom Hugo layout and does not require an external theme or Node dependencies.
