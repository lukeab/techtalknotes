# Hashicorp Internal use of Hashicorp tools- 18 Jan 2017
### Patterns in use:
~~Terraform apply == infrastructure up in one go~~ doesn't work that way ultimately

tfe-vault example environment -*Is it on github?*
```
$>tree
-README.md
-packer/
  - template.json
-terraform
 -environments
  -staging
  -production


```
* HC dont use terraform provisioners, as they can't self heal.
* Self discovery of environment by context, not terraform provisioners, ASG instances' cant figure it's * self out.
* dont use `terraform plan apply` patter directly, use `terraform plan --out {planfile}` then `terraform apply planfile {planfile}...` adds specificity to what you apply, dont apply days after last plan run, as changes underneath in env could screw it up.
* use IAM policies using HCL markup.
* maximum lifetime of instance in aws 5 days.
* packer builds dont need base ami id as query can find latest build.
* To reduce network price, just use `"spotprice-auto_product setting":"Linux/Unix (Amazon VPC)"` every time <- double check this value
* use RAMfs to store vault tokens, based off instace unique id.
* Don't use Docker anymore, everything installed through debs.
* firstrun/boot polling consul raft endpoint to check when up and joined cluster.
    * systemd runs this and tests for it for other services to start.
    `After=consul-online.target`
* generating config vs static in git. instancebootstrap.sh or openpvpnBootstrap.sh which gets data from environment and generates config to install into consul, discover vpc, asg members cluster, etc..
* use systemd "dropin" feature to add init script before normal systemd service bundled with package.
* Vault secure introduction. cool feature
* very little cloud init use
* use surf encryption, mostly just as a way to key datacenters as seperate, ni dns txt record, not secret, just a key mechanism.
* Consul boostrap checks for consul member list instances vs asg list instnaces, any not in asg but in consul get `consul force-leave` to ensure dead instances dont stay in cluster and affect quorum.
* Dont use consul exec, it's bad, fillsup kv log, and exposes user/instance rights across the network.. baaaad.

## App monitoring:
instance - app metrics vs new relic
they like that it's much nicer visually, and possibly feature wise.

## Questions
1. ~~why choose TC versus Jenkins, GoCD?~~
2. Upgrading Consul in ASG
  * package upgrade on the box, eg .deb test and roll back if it's not working (see zfs rooted boxes and snapshots.)
3. For mixing Cloudformation and terraform, see `terraform import aws_vpc vpcid123` or using data sources `.cloudformation_stack`
4. missing api's in terraform can be polyfilled by calling cloudformation from terraform.
5. ~~Get rid of planfiles?~~ Statefiles not likely to go away, unless someone comes up with something that is like planfiles but different. Need to make process less painful.
