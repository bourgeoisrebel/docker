# docker

## Installed in this image

- Azure CLI 2.0.66
- Terraform 0.12.12
- Az Powershell Module 2.2.0 

## Build
To build from docker file, navigate to directory containing docker file then:

``` docker build . ```

To pull latest version of the image:

``` docker pull bourgeoisrebel/azadmin ```

## To run interactively:

CLI:

``` docker run -it -v $PWD:/git bourgeoisrebel/azadmin bash ```

Powershell: 

``` docker run -it -v $PWD:/git bourgeoisrebel/azadmin pwsh ```

**See https://github.com/bourgeoisrebel/docker for centos version. This helps deal with security certs in a proxied environment**
