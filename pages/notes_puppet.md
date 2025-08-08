# Puppet
Puppet Client configuration
```
/etc/puppetlabs/puppet/puppet.conf
[main]
srv_domain = <puppet-server-here>

[agent]
environment = <Env-to-use>
```

Check puppet client has finished applying catalogue
```
journalctl -u puppet -f  # Shall show "Applied catalog"
```
test puppet agent
```
puppet agent --test
```
If is running
```
sudo cat `sudo puppet agent --configprint agent_disabled_lockfile`
```
Run Puppet agent
```
/opt/puppetlabs/bin/puppet agent -t 
```
Disable puppet agent
```
puppet agent -t --disable "Puppet Agent Down for Maintenance"
```
Run Puppet dry run
```
puppet agent -t --agent_disabled_lockfile=/none --no-use_srv_records --server r10k.org --environment <your-dev-here> --noop

```
Run Puppet from a r10k environment
```
puppet agent -t --agent_disabled_lockfile=/none --no-use_srv_records --server r10k.org --environment <your-dev-here>

```
Enable Puppet agent
```
/opt/puppetlabs/bin/puppet agent --enable
```
Start puppet agent service
```
echo "START=yes" > /etc/default/puppet 
/opt/puppetlabs/bin/puppet resource service puppet ensure=running enable=true 
/opt/puppetlabs/bin/puppet resource service pxp-agent ensure=running enable=true 
```
Clear pupppet agent on host
```
delete /etc/puppetlabs/puppet/ssl.
# run puppet agent to request a new cert
```
## Puppet certs on ca server
```
# all certs
sudo puppetserver ca list --all

# needed signature
sudo puppetserver ca list

# sign cert
sudo puppetserver ca sign  --certname <hostname>
 
# delete cert
sudo puppetserver ca clean --certname <hostname>
```
## facter
```
# facter -j | jq '.hostname'
# facter -j | jq '.os.distro'
# facter -j | jq '.dmi'

```
