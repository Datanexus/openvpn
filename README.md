# openvpn
openvpn overlay

## configuration
If any pairs don't apply, comment them out.
### amazon web services
Certain _key:value_ pairs can be configured per project. Others should remain fixed for all projects.

These values should remain unchanged.

    cloud: aws
Replace TENANT with the name of the AWS_PROFILE environment variable. 
    
    tenant: TENANT
Replace PROJECT_NAME with the name of the project hosting the instances. 

    project: PROJECT_NAME
Replace DOMAIN with a `production` or `development` (or another value of your choosing).

    domain: DOMAIN
These values should remain unchanged.
    
    application: openvpn
Replace REGION with your desired AWS region such as `us-east-1` or `us-west-2`.

    region: REGION    
Replace USER with `centos` for CentOS or `redhat` for RHEL.

    user: USER
Replace AMI with the AMI ID in the region you wish to deploy; Centos 7.3+ or RHEL 7.3 +are supported. If commented out or removed, CentOS 7.4 will be booted.

    image: AMI
Replace SIZE with the size of the root or data volume in GB, e.g, `20` for 20gig or `2000` for 2TB. `11` is a reasonable value for the root volume.

    root_volume: SIZE
Replace CIDR_BLOCK and SUBNET with the values in the VPC, e.g., 172.31.0.0/16, 172.31.1.0/24, 172.31.2.0/24. The internal subnet is private and non-routable, the external subnet will outbound routing.
    
    cidr_block: CIDR
    internal_subnet: SUBNET
    external_subnet: SUBNET

### osp
Certain _key:value_ pairs can be configured per project. Others should remain fixed for all projects.

These values should remain unchanged.

    cloud: osp
Replace TENANT with the name of the configure OSP tenant. 

    tenant: TENANT
Replace PROJECT_NAME with the name of the project hosting the instances. 
    
    project: PROJECT
Replace DOMAIN with a `production` or `development` (or another value of your choosing).

    domain: DOMAIN
These values should remain unchanged.

    application: openvpn
Replace REGION with your configured OSP region.

    region: OSP_REGION
Replace USER with `centos` for CentOS or `redhat` for RHEL.

    user: USER
Replace IMAGE_UUID with the UUID of the image you wish to boot.

    image: IMAGE_UUID
Replace SIZE with  configured virtual machine size.

    type: TYPE
Replace SIZE with the size of the root or data volume in GB, e.g, `20` for 20gig or `2000` for 2TB. `11` is a reasonable value for the root volume. If booting from an image, this is ignored.

    root_volume: SIZE
Replace POOL with any configured block pool, e.g., ScaleIO or Ceph.

    block_pool: POOL
Replace  SUBNET with the configured values, e.g., 172.31.1.0/24, 172.31.2.0/24. The internal subnet is private and non-routable, the external subnet will outbound routing.
    
    internal_subnet: SUBNET
    external_subnet: SUBNET
Replace  FLOAT with the floating IP pool, e.g., external. 
    
    float_pool: FLOAT
Replace SUBNET_UUID with the UUID's of the internal and external subnets.

    internal_uuid: SUBNET_UUID
    external_uuid: SUBNET_UUID
Replace ZONE with the configured OSP zone.

    zone: ZONE 
(if needed) Replace USER, PASSWORD, DOMAIN, PORT with the necessary values.

    http_proxy: http://USER:PASSWORD@DOMAIN:PORT

### SSL configuration
The OpenVPN overlay expects a number of additional SSL related configuration variables defined in `security.yml`.

The number of days the certificates should be valid for, e.g., 365 for one year.

    duration: DAYS

A random password for the certificate stores. Not used other than initial configuration.

    storepass: PASSWORD

A random password for the key password. Not used other than initial configuration.

    keypass: PASSWORD

The configuration settings for the certificates.

    certs:
      CN: COMMON NAME
      OU: ORGANIZATIONAL UNIT
      O: ORGANIZATION
      ST: STATE
      L: LOCALITY
      C: COUNTRY
   
A working example of the above would be:

    duration: 365
    storepass: L%6CwdnRWEmAaa3jWqd(FAgu
    keypass: pnH}hHk8r9mNhDubpZ*TMqNm
    certs:
      CN: OpenVPN
      OU: Engineering
      O: DataNexus
      ST: CO
      L: Denver
      C: US
      
## deployment
### amazon web services
You will  want to set your AWS_PROFILE to your preferred credentials.

Assuming `aws.yml` is the name of the inventory configuration file.  To deploy the openvpn overlay on top of an existing jumphost:

    ./deploy aws.yml security.yml

Expected running times:

* approximately 7 minutes 

### openstack
Assuming `osp.yml` is the name of the inventory configuration file.  To deploy the postgresql overlay:

    ./deploy osp.yml security.yml

Expected running times:

* standlone approximately 7 minutes 

## testing
Add the `datanexus-client.conf` configuration file to your VPN client (for example [tunnelblick](https://tunnelblick.net/)) and connect and log into the jumphost.

