## Edge Gateway for the IT/OT convergence - NIOT-E-TIJCX-GB

For platform details read on [here](https://www.netiot.com/netiot/netiot-edge/).

### Passive fieldbus raw data access in a container

The image provided hereunder deploys a container which contains all necessary libraries to grant access to the raw frame data of the fieldbus interface when it is running in so-called "listener" or "passive" mode.
Note, that using the passive fieldbus interface requires an appropriate license on the NIOT-E-TIJCX-GB edge gateway.

Base of this image builds a tagged version of [ubuntu](https://hub.docker.com/r/library/ubuntu/tags/14.04/) with enabled [SSH](https://en.wikipedia.org/wiki/Secure_Shell).

#### Container prerequisites

##### Port mapping

For remote login to the container across SSH the container's SSH port `22` needs to be mapped to any free host port.

##### Privileged mode

Only the privileged mode option lifts the enforced container limitations to allow usage of the passive fieldbus interface in a container.

#### Getting started

STEP 1. Open the gateway's landing page under `https://<gateways's ip address>`. 

STEP 2. Click the Docker tile to open the [Portainer.io](http://portainer.io/) Docker management user interface. 

STEP 3. Enter the following parameters under **Containers > Add Container**

* **Image**: `hilschernetiotedge/passive-fieldbus`

* **Restart policy**: `always`

* **Port mapping**: `Host "22" (any unused one) -> Container "22"` 

* **Runtime > Privileged mode** : `On`

* **Runtime > Devices > add device** `Host "/dev/netanalyzer_0" -> Container "/dev/netanalyzer_0"`

STEP 4. Press the button **Actions > Start container**

Pulling the image from Docker Hub may take up to 5 minutes average. 

#### Accessing

The container starts the SSH service automatically. 
Login to it with an SSH client such as [putty](http://www.putty.org/) using the gateway's IP address along with the mapped SSH port. Use the credentials `root` as user and `root` as password when asked and you are logged in as root.

Access to passive filedbus raw frame data is enabled via the popular libpcap library. The required special libpcap version is already provided within the Docker image.
See the [libpcap project](http://www.tcpdump.org/) for details about the libpcap API.

#### Give it a try

As the passive fieldbus interface is available as a standard libpcap capture device, popular network analyzers such as [tshark](https://www.wireshark.org/) are able to use the capture interface directly.
`tshark` and `dumpcap` are already provided within the given Docker image.

Login via SSH and start `dumpcap -D` to list all available capture devices.
You should see following output:
```
1. eth0
2. any
3. lo (Loopback)
4. nflog
5. nfqueue
6. sit0
7. cifXANALYZER_0
```
Where `cifXANALYZER_0` represents the passive fieldbus interface.

To start a capture type `dumpcap -i cifXANALYZER_0` or `tshark -i cifXANALYZER_0` respectively

### GitHub sources
The image is built from the GitHub project [passive-fieldbus](https://github.com/hilschernetiotedge/passive-fieldbus). It complies with the [Dockerfile](https://docs.docker.com/engine/reference/builder/) method to build a Docker image [automated](https://docs.docker.com/docker-hub/builds/). 

View the license information for the software in the Github project. As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained). As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
   
[![N|Solid](http://www.hilscher.com/fileadmin/templates/doctima_2013/resources/Images/logo_hilscher.png)](http://www.hilscher.com)  Hilscher Gesellschaft fuer Systemautomation mbH  www.hilscher.com
