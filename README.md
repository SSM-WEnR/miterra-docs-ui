# Miterra Documentation Website User Interface Bundle
This project provides the UI for the Miterra Model Documentation site. It provides both the visual theme and user interactions for the docs site. The UI bundle that this project produces is designed to be used with [Antora](https://antora.org/). It bundles the HTML templates (layouts, partials, and helpers), CSS, JavaScript, fonts, and site-wide images. The rest of the material for the docs site comes from the content repositories.

**Table of Contents**
- [Usage](#usage)
- [Development Quickstart](#development-quickstart)
  - [Prerequisites](#prerequisites)
    - [git](#git)
    - [Node.js](#nodejs)
  - [Clone and Initialize the UI Project](#clone-and-initialize-the-ui-project)
  - [Change the UI](#change-the-ui)
  - [Preview the UI](#preview-the-ui)
  - [Package for Use with Antora](#package-for-use-with-antora)
    - [Source Maps](#source-maps)
- [Releases](#releases)
- [Copyright and License](#copyright-and-license)
  - [Software](#software)
  - [Branding and design](#branding-and-design)


## Usage
To use this UI with Antora, add the following configuration to the playbook file for your site:
```yaml
ui:
  bundle:
    url: https://github.com/SSM-WEnR/miterra-site-ui/releases/latest/download/miterra-site-ui-bundle.zip
  snapshot: true
```
The UI bundle is cached and Antora won’t download the UI bundle again if the `url` value remains the same. In order for Antora to fetch the latest UI bundle, the `--fetch` option must be used and the `snapshot` key must be set to true to bypass the cache.


## Development Quickstart
This section offers a basic tutorial to teach you how to set up the UI project, preview it locally, and bundle it for use with Antora. A more comprehensive tutorial can be found in the documentation at docs.antora.org.

### Prerequisites
To preview and bundle the UI, you need the following software on your computer:

* [git](https://git-scm.com) (command: `git`)
* [Node.js](https://nodejs.org) (commands: `node`, `npm`, and `npx`)

#### git
First, make sure you have git installed. Open Terminal, and type the command:
```shell
$ git --version
```

If you get an error meessage or nothing happened, you don't have git installed. [Download and install](https://git-scm.com/downloads) the git package for your system.

#### Node.js
Next, make sure that you have Node.js installed (which also provides npm).
```shell
$ node --version
```

If this command fails with an error, you don't have Node.js installed.
If the command doesn't report an active LTS version of Node.js (e.g., v16.16.0), it means you don't have a suitable version of Node.js installed. Follow [Node.js download instructions](https://nodejs.org/en/download) to install a supported version of Node.js.


### Clone and Initialize the UI Project
Change the current directory to the location where you want to store the project. Then, clone the UI project using the following git command:
```
$ git clone https://github.com/SSM-WEnR/miterra-site-ui.git
```

After the project is successfully cloned, change the current directory to the project folder:
```shell
$ cd miterra-site-ui
```
Stay in this project folder when executing all subsequent commands.

Use npm to install the project’s dependencies inside the project by executing the following command:
```shell
$ npm ci
```
This command installs the dependencies listed in `package.json` into the `node_modules/` folder inside the project. This folder does not get included in the UI bundle and should not be committed to the source control repository.


### Change the UI
You can make changes freely to the UI project to make it suits your specific needs. All relevant files are located under the `src/` folder.

- `css/` contains all CSS files for the site.
- `font/` contains all font files used in the site. These font files are provided as a failsafe, as the site will first try to load fonts from CDN services, and will fall back to these fonts if they fail to load.
- `helpers/` contains all JavaScript files that are used to extend the Handlebars template engine.
-  `img/` contains all images used in the site.
-  `js/` contains all JavaScript files used in the site.
-  `layouts/` contains the Handlebars templates.
-  `partials/` contains the Handlebars partials.
-  `static/` contains the [static UI files](https://docs.antora.org/antora-ui-default/add-static-files/) that are copied directly to the site when built by Antora.

Antora uses the Handlebars templates to combine the converted AsciiDoc content and other UI elements to make the pages in the site. You can find out more about Handlebars templates and how to use them with Antora in the [Handlebars documentation](https://handlebarsjs.com/guide/) and [Antora documentation](https://docs.antora.org/antora-ui-default/templates/).


### Preview the UI
The UI project is configured to preview offline. The files in the `preview-site-src/` folder provide the sample content that allow you to see the UI in action. In this folder, you’ll primarily find pages written in AsciiDoc. These pages provide a representative sample of content from the real site.

To build the UI and preview it in a local web server, run the preview command:
```shell
$ npx gulp preview
```
You’ll see a URL listed in the output of this command:
```
[12:00:00] Starting server...
[12:00:00] Server started http://localhost:5252
[12:00:00] Running server
```
Navigate to this URL to preview the site locally.

While this command is running, any changes you make to the source files will be instantly reflected in the browser. Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to stop the preview server and end the continuous build.


### Package for Use with Antora
If you need to package the UI so you can use it to generate the documentation site locally, run the following command:
```shell
$ npx gulp bundle
```
If any errors are reported by lint, you’ll need to fix them.

When the command completes successfully, the UI bundle will be available at `build/ui-bundle.zip`. You can point Antora at this bundle using the `--ui-bundle-url` command-line option, or modify the `ui.bundle.url` key in the playbook file (see [Usage](#usage)).

#### Source Maps
The build consolidates all the CSS and client-side JavaScript into combined files, `site.css` and `site.js`, respectively, in order to reduce the size of the bundle. Source maps correlate these combined files with their original sources.

This “source mapping” is accomplished by generating additional map files that make this association. These map files sit adjacent to the combined files in the build folder. The mapping they provide allows the debugger to present the original source rather than the obfuscated file, an essential tool for debugging.

In preview mode, source maps are enabled automatically, so there’s nothing you have to do to make use of them. If you need to include source maps in the bundle, you can do so by setting the `SOURCEMAPS` environment varible to true when you run the bundle command:
```shell
$ SOURCEMAPS=true npx gulp bundle
```
In this case, the bundle will include the source maps, which can be used for debuggging your production site.


## Releases
The build and release steps are executed by GitHub Actions and is defined in the file `.github/workflows/build-release.yml`. The action is either manually started, or automatically triggered by the push event on the `main` branch, if the head commit message starts with the string `[Release]` (case sensitive). Once started, the action will package the UI bundle, and create a release on GitHub using [calendar versioning](https://calver.org/) (`YYYY.MM.x`, `x` is the sequence of release within a month).


## Copyright and License

### Software
This project is a derivative of [Asciidoctor Docs UI](https://github.com/asciidoctor/asciidoctor-docs-ui), [Antora Default UI](https://gitlab.com/antora/antora-ui-default), and [Rudder Documentation](https://github.com/Normation/rudder-doc). The software assets in this repository (CSS files, JavaScript files, Handlebars templates, utility icons, etc.) mostly come from Asciidoctor Docs UI. As such, use of the software is granted under the terms of the [Mozilla Public License 2.0 (MPL 2.0)](https://www.mozilla.org/en-US/MPL/2.0). See [LICENSE](./LICENSE) to find the full license text.

### Branding and design
Miterra Docs UI contains branding materials (logos, colors, iconography, etc.) of the Miterra model, its copyright holder, and third-parties. Rights to those materials are retained by their respective copyright holders.

