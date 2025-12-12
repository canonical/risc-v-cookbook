# RISC-V Image Cookbook

> ⚠️ **This repo is archived** ⚠️
>
> * New repo: https://github.com/canonical/ubuntu-hw-support
> * Docs at: https://canonical-ubuntu-hardware-support.readthedocs-hosted.com/

---

The RISC-V Image Cookbook is meant to help users to spin Ubuntu based images
for hardware that is not yet supported by the Ubuntu distribution.

It will guide you through these steps of your project:

* Collect requirements
* Set up Launchpad
* Build packages
* Build the image
* Test the image

## Project homepage

The project Git repository is located at [github.com/canonical/risc-v-cookbook](https://github.com/canonical/risc-v-cookbook/).

The generated documentation is hosted at [canonical-risc-v-cookbook.readthedocs-hosted.com](https://canonical-risc-v-cookbook.readthedocs-hosted.com/en/latest/).

The continuous integration results are available at [risc-v-cookbook/actions](https://github.com/canonical/risc-v-cookbook/actions/).

## Building

    cd docs/
    make html
    make spellcheck
    make linkcheck
    make vale

Serve the HTML pages on [http://127.0.0.1:8000](http://127.0.0.1:8080).

    make serve

## How to contribute

See `docs/contributing.rst`.

## License

[CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/)
