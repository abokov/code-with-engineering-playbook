

# Threat description
By design containers are running on top of host and this bring a lot of benefits as well as potential risks which needs to be mentioned:
* Docker host should be secured itself - it's obvious, but must-have thing - we won't cover this topic 'how secure my Linux', but have to mention this point.
* We should limit system calls and other potentially risky actions on container.

## Best practices for host
* Host _and_ Docker engine should be securely configured which includes authenticated access, secure communication. 
You can refer to [Docker Bench Security](https://github.com/docker/docker-bench-security) for this.
* Use minimal container specific hosts ( CoreOS, RancherOS )
* Keep updated you container orchestration engine accordinly.
* Use whitelist for system calls like chmod, mount, kill - see Seccomp [profile](https://raw.githubusercontent.com/moby/moby/master/profiles/seccomp/default.json)

## Resources 
* [CSE Threat model details](SecurityThreatModelDetails.md) 

## Q&A 
