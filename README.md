# RISC-V Image Cookbook

The RISC-V Image Cookbook is meant to help users to spin Ubuntu based images
for hardware that is not yet supported by the Ubuntu distribution.

It will guide you through these steps of your project:

* Collect requirements
* Setup Launchpad
* Build packages
* Build the image
* Test the image

## Project homepage

The project git repository is located at
https://github.com/canonical/risc-v-cookbook/.

The generated documentation is hosted at
https://canonical-risc-v-cookbook.readthedocs-hosted.com/en/latest/.

The continuous integration results are available at
https://github.com/canonical/risc-v-cookbook/actions/.

## Building

    cd docs/
    make html
    make spellcheck
    make linkcheck
    make wokecheck

Serve the HTML pages on http://127.0.0.1:8080

    make serve

## How to contribute

See doc/contributing.rst

## License

CC-BY-SA-4.0 (https://creativecommons.org/licenses/by-sa/4.0/)
