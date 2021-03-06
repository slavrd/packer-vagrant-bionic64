# Packer Bionic64 Vagrant box

A packer project that creates a Bionic64 Vagrant base box for VirtualBox provider.

## Prerequisites

* Install [packer](https://www.packer.io/downloads.html).
* Install [vagrant](https://www.vagrantup.com/downloads.html).
* Ruby version ~> 2.5.1 for running KitchenCI test.

## Building the box with Packer

The packer template is in `template.json`. In the `variables` section you can set parameters to customize the build. Help on setting, overriding variables in packer can be found [here](https://www.packer.io/docs/templates/user-variables.html#setting-variables).

Run `packer validate template.json` - to make basic template validation.

Run `packer build template.json` - to build the Vagrant box with packer.

## Testing with KitchenCI

The project includes a KitchenCI configuration to run basic tests against the box outputted from packer.

To run it you need to install the gems from the `Gemfile`. Its recommended to use ruby [`bundler`](https://bundler.io/).

Installing gems with bundler:

* `gem install bundler`
* `bundle install`

Running Kitchen tests:

* `bundle exec kitchen converge` - will build the test environment.
* `bundle exec kitchen verify` - will run the tests.
* `bundle exec kitchen destroy` - will destroy the test environment.
* `bundle exec kitchen test` - will perform the above steps with a single command.

**Caveat** - Kitchen will not remove the box from vagrant after running `kitchen destroy`. For the moment need to clean up manually by running `vagrant box remove ubuntu-1804-virtualbox-test`

## Uploading to Vagrant cloud

The project includes a script `vagrant_cloud_upload.sh` to upload the box to Vagrant cloud. It is basically a wrapper for the `vagrant cloud publish` [command](https://www.vagrantup.com/docs/cli/cloud.html#cloud-publish) so you can use it instead.

In the script there are variables that define the box name, description and the local box package path.

You need to set up Vagrant cloud access by setting the `VAGRANT_CLOUD_TOKEN` environment variable to your user token.

script usage: `./upload-vagrant-cloud.sh [box_version]`, box_version will default to `yy.mm.dd` if not set.

## Example

```bash
packer validate template.json # basic template validation.

packer build template.json   # build the box with packer.

bundle exec kitchen test # run tests

vagrant_cloud_upload.sh 18.04.3 # upload to Vagrant cloud
```
