# Debian package for MATLAB

## Description

Debian rules to generate a Debian package for [MATLAB](https://www.mathworks.com/products/matlab.html) for amd64 architecture. The MATLAB offline installer corresponding to version in changelog has to be provided in `installer_$VERSION/` to build the package.

The generated Debian package:
- installs MATLAB in `/opt/matlab`
- adds a link to `matlab` executable in `/usr/bin`
- adds icon and desktop menu entry in `/usr/share`
- adds license file if provided

A license file `license.lic` has to be provided to use MATLAB either next to the installer in order to be included in the package or need to be added in `/opt/matlab/licenses/` after the package is installed. To use a network license server the `license.lic` is simply:

    SERVER hostname-or-ip-of-license-server AAAAAAAAAAAA 27000
    USE_SERVER


Inspired by [MATLAB Arch Linux PKGBUILD](https://aur.archlinux.org/packages/matlab).

## Package build

1. Download the installer `matlab_R*_glnxa64.zip` from MATLAB website (note the update number) and unpack
2. Launch the installer, authenticate, and in *Advanced Options* select *I want to download without installing*
3. Select and download the products, the files are downloaded in `$HOME/Downloads/MathWorks/R*/*`
4. Define the downloaded version (with update patch):

        export VERSION=2022a6

5. Clone the git repository containing the packages rules in an empty directory:

        git clone -b ${VERSION:0:5} https://github.com/guillod/deb-matlab.git matlab-$VERSION
        cd matlab-$VERSION

6. Update the changelog to include patch revision if required with:

        dch -v $VERSION

7. Move all downloaded files into the `installer_$VERSION/` directory:

        mkdir installer_$VERSION
        mv $HOME/Downloads/MathWorks/R${VERSION:0:5}/*/* installer_$VERSION/

8. Put the file installation key obtained from MATLAB in a `fileInstallationKey` file in the current directory.
9. Optionally, put a `license.lic` file (network license server) in the current directory in order to include it in the package.
10. Generate the package for example with:

        debuild --no-lintian -i -b -uc -us

    or `sbuild` (note you can speed up the creation of the source archive using `--dpkg-source-opts="-Zgzip -z1"`).

## Installation

Once generated, the package `matlab_$VERSION_amd64.deb` can be installed for example with:

sudo apt install ../matlab_$VERSION_amd64.deb


## Contact

For comments, issues, bug-reports and requests, please use the issue tracker of the current repository. Otherwise the principal author can be reached at:

    Julien Guillod
    julien.guillod CHEZ sorbonne-universite.fr
    https://guillod.org/
    Department of Mathematics
    Sorbonne University
    France