# TRANSMIXR Technical Documentation

This repository contains the source code for technical documentation and
results of the [TRANSMIXR EU research project](https://transmixr.eu). The site
can be compiled using the [Hugo](https://gohugo.io) static site generator and
is available to browse at https://transmixr.github.io

## Building

In order to build the site, first install the [Hugo](https://gohugo.io) static
site generator for your operating system. Then, the site can be built by
invoking the following command in the root directory of this repository:

    hugo

This will build the site and place the results into the folder `public/`. To
deploy the site, run a HTTP server in the `public/` folder, or copy its
contents to the root of your HTTP server of choice. For development, you can
use Hugo's builtin development server:

    hugo server
