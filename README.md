# ⚠️️ DISCLAIMER ⚠️️

This project is...
* not actively maintained
* not actively supported and is not guaranteed to be compatible with all versions of Clair
* intended for local usage only and does not follow the best practices for production usage of Clair

# analyze-local-images

This is a basic tool that enables quick analysis of local Docker images with [Clair](https://github.com/coreos/clair).

## Install

First install [Go](https://golang.org/doc/install) and [glide](https://glide.sh) and then run the following commands:

    $ git clone https://github.com/coreos/analyze-local-images.git $HOME/analyze-local-images-gopath/src/github.com/coreos/analyze-local-images
    $ export GOPATH=$HOME/analyze-local-images-gopath
    $ cd $HOME/analyze-local-images-gopath/src/github.com/coreos/analyze-local-images
    $ glide install
    $ go install github.com/coreos/analyze-local-images

## Usage

### Clair is a local process or inside of a container running on my current machine

```
analyze-local-images <Docker Image>
```


### Clair is running inside of container on a local VM (e.g. Docker For Mac)

```
analyze-local-images -endpoint "http://<CLAIR-IP-ADDRESS>:6060" -my-address "<MY-IP-ADDRESS>" <Docker Image>
```

Clair needs filesystem access to the image files.
If you run Clair locally, this tool will store the files in the system's temporary folder and Clair will find them there.
It means if Clair is running in Docker, the host's temporary folder must be mounted in the Clair's container.
If you run Clair remotely, this tool will run a small HTTP server to let Clair downloading them.
It listens on the port 9279 and allows a single host: Clair's IP address, extracted from the `-endpoint` parameter.
The `my-address` parameters defines the IP address of the HTTP server that Clair will use to download the images.
With boot2docker, these parameters would be `-endpoint "http://192.168.99.100:6060" -my-address "192.168.99.1"`.

As it runs an HTTP server and not an HTTP**S** one, be sure to **not** expose sensitive data and container images.
