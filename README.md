# msi Blog (Zola + Apollo Theme)

This site is built using [Zola](https://www.getzola.org/) with the [Apollo theme](https://www.getzola.org/themes/apollo/) and hosted on Cloudflare Pages.

## Notes / Gotchas

Some issues were encountered during setup and deployment. Documenting them here for reference:

* See the [discussion on Zola support for Cloudflare Pages](https://github.com/cloudflare/pages-build-image/issues/3#issuecomment-1646873666).
* Use a **build command to download and install Zola** during deployment on Cloudflare Workers: [reference comment](https://github.com/cloudflare/pages-build-image/issues/3#issuecomment-2732858605).
* **Disable preview deployments for PRs** and keep builds only for the `main`/production branch. ~~`$CF_PAGES_URL` may be empty in some builds, causing errors with `--base-url`.~~

## Running Locally

1. Install [Zola](https://www.getzola.org/documentation/getting-started/installation/).

2. Start a development server:

   ```bash
   zola serve
   ```

   Visit your site at [http://localhost:1111](http://localhost:1111).

3. Generate a production build:

   ```bash
   zola build
   ```

   The output will be in the `public` directory.
