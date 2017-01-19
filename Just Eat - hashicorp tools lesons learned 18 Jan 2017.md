# Just Eat - hashicorp tools lesons learned 18 Jan 2017

## Consul
cluster per env, 
git2consul -> seed key/vals from github
expand keys: true
acl and consul service per instance

####FeatureConfig
config/features/featurename/...

#### Envconfig
config/environment/monitoring/... [port,ip]
Settings
/config/settings/>>??

### Consul Template
packer buils consul-template in.
worked well mostly
replacing static files == greater effort locallly

##### Cons
changing env settings required apppool recycle.

### Consul ACLs


###Centralised data
 * launched consul cluster as Just Eat source of truth
 * Accounts, envs
 * reliable

### Consul Locks
Unique instance value
 * instnace name
 * instance location
 * ...
 * Reliable

####
very easy to setup
very reliable

## Vault
Not using it yet... yet

###approach
consul for config, vault for secretes. 
backed by consul, key value simple to use
Instances generate instnace token, mapped to acl on vault to use in  generating credentials

###Lessons
 * Vault instnaces didn't forward requests,, they seem to now (latest release)
 * look after keys
 * Read docs carefully


#####QUESTIONS
~~Have you opted for paid HC services/commercial relationship do you have with Hashcorp? ~~ not yet
~~Using vault consul vagrant ... Not using terraform for AWS?~~ still using cloud formation, on its way.
How are you monitoring? Consul service monitoring? What do you back on to? zabbicx/nagios/sensu?

