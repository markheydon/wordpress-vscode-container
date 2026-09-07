# WordPress Visual Studio Code Container

A drop-in [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers) setup for WordPress development.

This repository is **not** designed to be cloned and used as a project. Copy the files into your existing WordPress plugin or theme workspace, then reopen the folder in a container. See [Getting started](#getting-started) for the full set of files.

## Looking for a full project template?

If you want a GitHub template with plugin or theme scaffolding, automated setup, and distribution workflows, use one of these instead:

- [wordpress-plugin-dev](https://github.com/markheydon/wordpress-plugin-dev) — WordPress plugin development template
- [avada-child-theme-dev](https://github.com/markheydon/avada-child-theme-dev) — Avada child theme development template

This repo stays intentionally smaller: a generic container you can drop into any WordPress codebase.

## What's included?

Without modification, the supplied configuration provides:

- WordPress container based on `wordpress:php8.4-apache`
- MariaDB 11.4 with a persistent database volume
- Workspace mounted at `/workspace`
- WordPress files mapped to `./wordpress` (`/var/www/html` in the container)
- PHP Composer and WP-CLI installed
- Xdebug 3 enabled (listen on port 9003)
- `WP_DEBUG` enabled via `WORDPRESS_DEBUG`
- Direct filesystem writes via `FS_METHOD` for local development

The configuration has not been tested with every possible WordPress setup, but it is intended for plugin and theme development, including WP-CLI workflows.

## Getting started

Follow these steps in order. Later sections are extras (PHP version, plugin volume mapping, debugging), not a second copy of the setup.

1. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) in VS Code.
2. If you have not used Dev Containers before, read the [Getting Started](https://code.visualstudio.com/docs/devcontainers/containers#_getting-started) guide.
3. Download the latest release ZIP from this repository, or copy the files from a checkout.
4. Place `.devcontainer` and `.vscode` in the root of your VS Code workspace (where your project files live).
5. Optionally add PHP CodeSniffer with WordPress Coding Standards. Composer is not required for the container to run; skip this step if you do not want it.
   - If the project does **not** already use Composer, copy `composer.json` and `phpcs.xml` into that same workspace root.
   - If the project **already** uses Composer, do not overwrite `composer.json`. Add these packages instead:

     ```
     squizlabs/php_codesniffer:^3.13
     dealerdirect/phpcodesniffer-composer-installer:^1.1
     wp-coding-standards/wpcs:^3.3
     ```

     Copy `phpcs.xml` if you do not already have a PHPCS config.
6. Update project names to match your workspace:
   - `name` in `.devcontainer/devcontainer.json`
   - `name` in `composer.json`, if you copied that file in the previous step
7. If this project is not on PHP 8.4, change the image tag **before** the first build (see [PHP version](#php-version)).
8. Open the Command Palette and run **Dev Containers: Reopen in Container**.
9. Wait for the container to finish starting. If `composer.json` is present, Composer dependencies install automatically.
10. Open `http://localhost:8080` and complete the WordPress installer.

If you added Composer, you can check coding standards inside the container with:

```bash
composer lint
composer lint:fix
```

## PHP version

The container is pinned to **PHP 8.4** (`wordpress:php8.4-apache`). That is a current, well-supported default, not a match for every existing plugin or theme.

If you drop these files into a project that already targets another PHP release, change the image before you build. In [`.devcontainer/Dockerfile`](.devcontainer/Dockerfile):

```dockerfile
ARG WORDPRESS_IMAGE=wordpress:php8.4-apache
```

Use the matching official tag, for example `wordpress:php8.3-apache` or `wordpress:php8.5-apache`. See [WordPress Docker tags](https://hub.docker.com/_/wordpress) and [WordPress PHP compatibility](https://make.wordpress.org/core/handbook/references/php-compatibility-and-wordpress-versions/).

If you copied `composer.json` / `phpcs.xml` (or already have them), update the Composer `php` requirement and the PHPCS `testVersion` so they match the image you chose.

Then rebuild with **Dev Containers: Rebuild Container**. Confirm with `php -v` inside the container.

## Making changes

If you edit `.devcontainer/Dockerfile` or `.devcontainer/docker-compose.yml`, rebuild with **Dev Containers: Rebuild Container**.

## Use for developing plugins

The easiest approach is to follow the [WordPress Plugin Handbook folder structure](https://developer.wordpress.org/plugins/plugin-basics/best-practices/#folder-structure) and keep your plugin at the workspace root. Then add a volume mapping in `.devcontainer/docker-compose.yml` under the `wordpress` service:

```yaml
    volumes:
      - ..:/workspace:cached
      - ../wordpress:/var/www/html
      - ../plugin-name:/var/www/html/wp-content/plugins/plugin-name
```

Update `.vscode/launch.json` `pathMappings` if you want to debug plugin files separately:

```json
"pathMappings": {
    "/var/www/html/": "${workspaceFolder}/wordpress",
    "/var/www/html/wp-content/plugins/plugin-name/": "${workspaceFolder}/plugin-name"
}
```

## Debugging

Xdebug 3 is configured to listen on port **9003**. Use the **Listen for Xdebug** launch configuration in VS Code.

## Contributing

Please read [`.github/CONTRIBUTING.md`](.github/CONTRIBUTING.md) for contribution guidance.

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
