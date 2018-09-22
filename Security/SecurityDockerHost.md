
# Docker host and kernel security


## Threat description
Basically every container running on a host shares a common kernel. All containers are isolated from one another, but in case if the host OS is compromised, all containers running on it are at risk. Same thing happened if container is using a vulnerable library, it could be exploited to gain access to the underlying host.
By design containers are running on top of host and this bring a lot of benefits as well as potential risks which needs to be mentioned:
* Docker host should be secured itself - it's obvious, but must-have thing - we won't cover this topic 'how secure my Linux', but have to mention this point.
* We should limit system calls and other potentially risky actions on container.


## Best practices for host
* Host _and_ Docker engine should be securely configured which includes authenticated access, secure communication. 
You can refer to [Docker Bench Security](https://github.com/docker/docker-bench-security) for this.
* Use minimal container specific hosts ( CoreOS, RancherOS )
* Keep updated you container orchestration engine accordinly.
* Active use Seccomp ( Secure Computing ) linux kernel module - for exampel use whitelist for system calls like chmod, mount, kill - see Seccomp [profile](https://raw.githubusercontent.com/moby/moby/master/profiles/seccomp/default.json)

### Host - seccomp policies
Based on json format they do specify custom actions when referenced system calls are used. For example policy below prevent execution of mkdir and chown.
<code>
  {
	"defaultAction": "SCMP_ACT_ALLOW",
	"syscalls": [
		{
		  "name": "mkdir",
		  "action": "SCMP_ACT_ERRNO"
		},

		{
		  "name": "chown",
		  "action": "SCMP_ACT_ERRNO"
		}
	]
}
</code>

## Container runtime threats

### Unix sockets

## Priviliged containers

## Ports: binding, exposing and 

### Volume mounting




## Examples

[Seccomp](https://docs.docker.com/engine/security/seccomp/) allows you to filter which actions can be performed by the container, you may try this:

<code>
# docker run -it alpine sh
/ # whoami
root
/ # mount /dev/sda1 /tmp
mount: permission denied (are you root?)
/ # swapoff -a
swapoff: /dev/sda2: Operation not permitted  
</code>
You can use seccomp profiles to manage your container/host system calls priviligies - as example of [seccomp profile](https://raw.githubusercontent.com/moby/moby/master/profiles/seccomp/default.json) you may use mentioned before
and to apply this profile you can use --security-opt tag
<code>
  # docker container run --rm -it --security-opt seccomp=./default.json alpine sh
/ # chmod +r /usr
chmod: /usr: Operation not permitted
</code>


## References

Threat description, examples and best practices based on these sources:
* (https://sysdig.com/blog/7-docker-security-vulnerabilities/)
* (https://www.stackrox.com/post/2017/08/hardening-docker-containers-and-hosts-against-vulnerabilities-a-security-toolkit/)


## Resources 
* [CSE Threat model details](SecurityThreatModelDetails.md) 

## Q&A 
