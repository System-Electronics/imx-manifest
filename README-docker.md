i.MX Repo Manifest README for Docker

Steps to build an image with docker packages on mx8 machines:
----------------------------------------------------------------
$: mkdir <release>
$: cd <release>
To download the 5.10.35-2.0.0 release with docker
$: repo init -u https://source.codeaurora.org/external/imx/imx-manifest -b imx-linux-hardknott -m imx-5.10.35-2.0.0_docker.xml
$: repo sync

Setup the build folder:
-----------------------------------------
Note: Docker is supported only on mx8 machines.

$ EULA=1 MACHINE=imx8qmmek DISTRO=fsl-imx-xwayland source ./fsl-setup-release.sh -b bld-dir

Add meta-virtualization layer in bblayers.conf:

$ echo "BBLAYERS += \" \${BSPDIR}/sources/meta-virtualization \"" >> conf/bblayers.conf

Add docker to the image in conf/local.conf:

DISTRO_FEATURES_append = " virtualization"
IMAGE_INSTALL_append = " docker"

Build an image:
---------------
bitbake <image recipe>

Some image recipes:
imx-image-core - core image with basic graphics and no multimedia
imx-image-multimedia - image with multimedia and graphics
imx-image-full - image with multimedia and machine learning and Qt

Basic steps to run Docker on the target:
-------------------------------------------
To retrieve a system information about Docker, use the command on i.MX Board Terminal

$ docker info

Pull the basic GNU/Linux Ubuntu 18.04 image:

$ docker pull ubuntu:18.04

Launch basic Docker image:

$ docker run -it --net=host ubuntu:18.04
