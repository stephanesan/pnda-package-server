# pnda-package-server

Due to the limited resources I have on my laptop, I built the [PNDA packages](https://github.com/pndaproject/pnda-package-server-docker) directly on the package server.
Next are the steps I undertook to get this working on openstack:

Upload the debian cloud image under the name ["Debian 8.6.0"](https://github.com/stephanesan/pnda-package-server/blob/master/pack_server_env.yaml#L4).

```
wget http://cdimage.debian.org/cdimage/openstack/8.6.0/debian-8.6.0-openstack-amd64.qcow2
openstack image create --name="Debian 8.6.0" --disk-format=qcow2 --container-format=bare --property architecture=x86_64 --progress --file debian-8.0.0-openstack-amd64.qcow2
```

Create a key pair named ["sl_disc"](https://github.com/stephanesan/pnda-package-server/blob/master/pack_server_env.yaml#L5) in order to access the server (in case something goes wrong, user name will be debian) and save the private key to sl_disc.pem.
```
openstack keypair create sl_disc > sl_disc.pem
```

Clone thsi repo and provision the heat stack from this repo:
```
git clone https://github.com/stephanesan/pnda-package-server.git
cd pnda-package-serve
openstack stack create -e pack_server_env.yaml -t ./pack_server_template.yaml pack_server
```

# Note
In case something went wrong
Get the public IP of the server 
```
openstack stack output show  me-pkg-server instance_ip
```
Ssh into the server using the private key saved above.
```
ssh debian@your.server.ip -i sl_disc.pem
```
