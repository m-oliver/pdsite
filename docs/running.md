% Running pdsite

`pdsite` uses the contents of the working directory to generate HTML files. The types of files it takes as input, the location of the output HTML, the HTML theme used, and other site-wide variables are defined via a configuration file (`.pdsite.yml`) in the current directory.

### Setting up .pdsite.yml

`pdsite` will not run in a directory unless a valid configuration file is present. To quickly create a valid file, use the `init` command:

```
$ pdsite init
```

This will create a default `.pdsite.yml` YAML file in your current directory. You can then customize it as desired. Any data defined in the config file is available to the template engine during the build process, so you can use this file to define site-wide variables. For example, the config file used to generate this site looked like:

```yaml
# pdsite configuration

theme: default
inputextension: .md
outputfolder: .html
sitepath: 

# Site-wide template variables

bootswatch: lumen 
sitename: "pdsite"
pagetitle-suffix: "Pandoc-backed static site generator"
footer: '<a class="navbar-link" href="https://github.com/GordStephen/pdsite">GitHub Repo</a> | <a class="navbar-link" href="https://github.com/GordStephen/pdsite/issues">Report an Issue</a>'
```

The variable `sitepath` can be used to specify a path prefix in case that the root of the `pdsite` document tree is located in a subdirectory of the web server root.

### Building your site

Once a config file is defined, `pdsite` can convert the current directory into a linked website according to its folder and file structure(hidden files are excluded). For example, this site was generated by the following file structure:

```
├── CNAME 
├── index.md
├── installing.md
├── running.md
└── themes
    ├── choosing-themes.md
    └── creating-themes.md
```

In that example, the input directory contains a number of Markdown documents (including some in a subfolder) and a "CNAME" file that needs to be copied in to the output directory.

Converting arbitrarily-deep nested files to HTML is supported, although the built-in themes only support generating navigation menus for up to two levels of folders (e.g. `/category/subcategory/page`). A [custom theme](/themes/creating-themes) could support automatically linking to deeper files.

Any (non-hidden) files in the directory structure that aren't recognised as content to be converted (like the "CNAME" file above) are copied over to the output directory without any processing.

To generate the HTML website, just run the `build` command:

```sh
$ pdsite build
Creating new build folder... done.
Building site... done.
```
We can check the results in the specified output folder:

```sh
$ tree .html
.html
├── CNAME 
├── index.html
├── installing
│   └── index.html
├── running
│   └── index.html
├── styles.css
└── themes
    ├── choosing-themes
    │   └── index.html
    └── creating-themes
        └── index.html
```

The Markdown files have all be converted to HTML, with new directories created where appropriate to allow for simple pretty URLs. Non-Markdown files have been directly copied without any changes (and some new files have also been copied in as part of the theme).

Alternatively, you could use the `serve` command, which first builds the site and then launches a server so you can see the results in a browser.

```sh
$ pdsite serve
Clearing old build folder... done.
Building site... done.
fix_ug: uid=1000 euid=1000 / gid=1000 egid=1000 / gids: 1000 10 95 
http server started
  ipv6  : yes
  ssl   : no
  node  : localhost.localdomain
  ipaddr: ::
  port  : 8000
  export: /the/current/path/.html
  user  : theuser 
  group : thegroup 
```

Visiting `localhost:8000` in your browser will display the site output. Auto-regeneration of the site when an input file changes is not yet supported, so you'll need to stop the server and re-run the command to see any modifications.

