# WordPress Visual Studio Code Container

A drop-in [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers) setup for WordPress development.

This repository is **not** designed to be cloned and used as a project. Copy the `.devcontainer` and `.vscode` folders into your existing WordPress plugin or theme workspace, then reopen the folder in a container.

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

1. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) in VS Code.
2. If you have not used Dev Containers before, read the [Getting Started](https://code.visualstudio.com/docs/devcontainers/containers#_getting-started) guide.
3. Download the latest release ZIP from this repository, or copy `.devcontainer` and `.vscode` from a checkout.
4. Place those folders in the root of your VS Code workspace (where your project files live).
5. Update the `name` in `.devcontainer/devcontainer.json` to match your project if you want.
6. Open the Command Palette and run **Dev Containers: Reopen in Container**.

On first run, open `http://localhost:8080` and complete the WordPress installer.

## Making changes

If you edit `.devcontainer/Dockerfile` or `.devcontainer/docker-compose.yml`, rebuild with **Dev Containers: Rebuild Container**.

## Optional Composer and PHPCS

An optional `composer.json` is included to install PHP CodeSniffer with WordPress Coding Standards.

If you are not already using Composer in your project, copy in the supplied `composer.json` and `phpcs.xml`, update the package `name`, then run:

```bash
composer install
```

Composer dependencies are also installed automatically when the dev container starts, if `composer.json` is present.

Useful commands:

```bash
composer lint
composer lint:fix
```

If you already have Composer, add these packages instead:

```
squizlabs/php_codesniffer:^3.13
dealerdirect/phpcodesniffer-composer-installer:^1.1
wp-coding-standards/wpcs:^3.3
```

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
