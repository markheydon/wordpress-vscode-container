# Wordpress Visual Studio Code Container
A simple Visual Studio Code Docker container setup for WordPress development.  See [Developing inside a Container](https://code.visualstudio.com/docs/remote/containers) for more information on using containers in VS Code.

## What's included?

The files in this project are designed to be dropped-in to a VS Code workspace as-is to provide a simple, working, container for WordPress development.  In other words, this repository is not designed to be cloned and used directly.

The configuration files have not been tested with all possible variations of WordPress development, but I personally use them for plugin development including WP CLI projects.

Without modification, the supplied configuration provices the following.

- Main VS Code container running the official Docker Hub wordpress image.
- Mapped folder `./wordpress` to the container WordPress folder (`/var/www/html`).
- Additional container running MariaDB.
- PHP Composer installed.
- WP CLI installed.
- XDebug enabled via port 9000.
- WP_DEBUG enabled in wp-config.php file.

## Getting Started

These instructions should get you a working container environment to test and debug your WordPress project.

1. Install the [Visual Studio Code Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) if you don't have it installed already in VS Code.
1. Additionally, if you haven't already, follow the [Getting Started](https://code.visualstudio.com/docs/remote/containers#_getting-started) steps in the VS Code documenation.
1. Download the latest zip release file from this repository and unzip to a temporary folder.
1. Copy the folders `.vscode` and `.devcontainer` from the download into the root of your VS Code workspace folder, i.e. the top level where your project code files are in VS Code.  Amend as needed for your project.
1. Although the files can be used as-is, you may want to at least update the 'name' in the `.devcontainer/devcontainer.json` file to match that of your project.
1. At this point, you can open the VS Code command pallette and use the **Remote-Containers: Reopen Folder in Container** command to fire up the container.

Once the container is built and up and running, you should be able to access the WordPress instance via `http://localhost:8080`.  The installer should appear on first run.

## Opening in Dev Container

[![Open in Dev Container](https://img.shields.io/badge/Open%20in-Dev%20Container-blue)](vscode://vscode-remote/containers?url=https://github.com/noeoedi/wordpress-vscode-container)

## Making Changes

If you want to amend the supplied Dockerfile or docker-compose.yml to your own liking, do so but don't forget to use the **Remote-Containers: Rebuild Container** command in VS Code command to rebuild the VS Code container.

## Optional 'composer.json' File

If you aren't using Composer in your project already, feel free to copy in the `composer.json` file supplied.  This enables the PHP Code Sniffer with the WordPress standard installed.  Update the file as required for your project, again at least the 'name' should be changed and remove the 'type' or update it to 'project'.  Run the `Composer: Update` command from the command pallette to install/enable.

If you are using Composer already, then 'require' the following to enable PHP Code Sniffer if you haven't installed/enabled it already.

```
squizlabs/php_codesniffer:^3.5
dealerdirect/phpcodesniffer-composer-installer:*
wp-coding-standards/wpcs:*
```


## Use for Developing Plugins

The easiest way to include these files in a Plugin respository, is to follow the [WordPress Plugin Handbook - Best Practices Folder Structure](https://developer.wordpress.org/plugins/plugin-basics/best-practices/#folder-structure) and have a folder at root level named the same as your plugin.  Then, in the docker-compose.yml add something such as the following under the `- ../wordpress:/var/www/html` line under `volumes`...

```
    volumes:
      - ..:/workspace:cached
      - ../wordpress:/var/www/html
      - ../plugin-name:/var/www/html/wp-content/plugins/plugin-name
```

This will map the root level `plugin-name` folder to the right place in the container.  Update the `.vscode/launch.json` file `pathMappings` section to enable debugging of the same.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

