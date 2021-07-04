# Getting Started with Terraform

Terraform is the most popular language for defining and provisioning infrastructure as code (IaC).

In this tutorial, you will learn how to install Terraform, write a Terraform configuration file, and then use it to create and destroy infrastructure.

## Prerequisites

For this tutorial you will need:
- [Docker Engine](https://docs.docker.com/engine/install) or [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running
- To know how to launch and interact with your system's [command line interface](https://www.w3schools.com/whatis/whatis_cli.asp) (CLI)

## Install Terraform

To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html) and download the appropriate zipped archive for your system.

Once downloaded, unzip the package and move the Terraform binary file to a directory on your system's [`PATH`](https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them).

## Verify the installation
To check that Terraform has been installed correctly, run the following command to display the version.

[CLI command](#cli-command)

### CLI command
```shell
$ terraform -v
Terraform v1.0.1
on darwin_amd64
```

## Create a project directory
To keep things organized, we recommend that you create a new directory for the project on your system and then navigate to it.

[CLI command](#cli-command-1)

### CLI command

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

## Create the configuration file

Create a file named `main.tf` for your Terraform configuration code.

[CLI command](#cli-command-2)

### CLI command

```shell
$ touch main.tf
```

Paste the following lines into the file and save it.

[CLI command](#cli-command-3)

### CLI command

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

## Initialize Terraform
Initialize Terraform with the `init` command. Terraform will fetch any required plugins and set up your project.

[CLI command](#cli-command-4)

### CLI command

```shell
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of kreuzwerker/docker...
- Installing kreuzwerker/docker v2.13.0...
- Installed kreuzwerker/docker v2.13.0 (self-signed, key ID 24E54F214569A8A5)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

## Create the infrastructure
Provision the resources with the `apply` command. Terraform will print a message at the bottom of the output asking for confirmation that you want to continue. Type `yes` and press `ENTER`.

The command will take a few minutes to run and display a message once Terraform has finished creating the resources.

[CLI command](#cli-command-5)

### CLI command

```shell
$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + entrypoint       = (known after apply)
      + env              = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = (known after apply)
      + init             = (known after apply)
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = "json-file"
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + remove_volumes   = true
      + restart          = "no"
      + rm               = false
      + security_opts    = (known after apply)
      + shm_size         = (known after apply)
      + start            = true
      + stdin_open       = false
      + tty              = false

      + healthcheck {
          + interval     = (known after apply)
          + retries      = (known after apply)
          + start_period = (known after apply)
          + test         = (known after apply)
          + timeout      = (known after apply)
        }

      + labels {
          + label = (known after apply)
          + value = (known after apply)
        }

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id          = (known after apply)
      + latest      = (known after apply)
      + name        = "nginx:latest"
      + output      = (known after apply)
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Still creating... [10s elapsed]
docker_image.nginx: Creation complete after 14s [id=sha256:4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 2s [id=e8b7ee4827fd0e90cd6cc949badd49aed09d3ec1f76f4cf66ee696e55fcbe3b9]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

## Destroy the infrastructure

Destroy the infrastructure with the `destroy` command. Again, Terraform will print a message at the bottom of the output asking for confirmation that you want to continue. Type `yes` and press `ENTER`.

[CLI command](#cli-command-6)

### CLI command

```shell
$ terraform destroy
docker_image.nginx: Refreshing state... [id=sha256:4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399nginx:latest]
docker_container.nginx: Refreshing state... [id=e8b7ee4827fd0e90cd6cc949badd49aed09d3ec1f76f4cf66ee696e55fcbe3b9]

Note: Objects have changed outside of Terraform

Terraform detected the following changes made outside of Terraform since the last "terraform apply":

  # docker_container.nginx has been changed
  ~ resource "docker_container" "nginx" {
      + dns               = []
      + dns_opts          = []
      + dns_search        = []
      + group_add         = []
        id                = "e8b7ee4827fd0e90cd6cc949badd49aed09d3ec1f76f4cf66ee696e55fcbe3b9"
      + links             = []
      + log_opts          = {}
        name              = "training"
      + sysctls           = {}
      + tmpfs             = {}
        # (31 unchanged attributes hidden)

        # (1 unchanged block hidden)
    }

Unless you have made equivalent changes to your configuration, or ignored the relevant attributes using ignore_changes, the following plan may include actions to
undo or respond to these changes.

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env               = [] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "e8b7ee4827fd" -> null
      - id                = "e8b7ee4827fd0e90cd6cc949badd49aed09d3ec1f76f4cf66ee696e55fcbe3b9" -> null
      - image             = "sha256:4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399" -> null
      - init              = false -> null
      - ip_address        = "172.17.0.3" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.3"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - remove_volumes    = true -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - security_opts     = [] -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - stdin_open        = false -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null
      - tty               = false -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id          = "sha256:4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399nginx:latest" -> null
      - latest      = "sha256:4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:47ae43cdfc7064d28800bc42e79a429540c7c80168e8c8952778c0d5af1c09db" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=e8b7ee4827fd0e90cd6cc949badd49aed09d3ec1f76f4cf66ee696e55fcbe3b9]
docker_container.nginx: Destruction complete after 1s
docker_image.nginx: Destroying... [id=sha256:4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399nginx:latest]
docker_image.nginx: Destruction complete after 1s

Destroy complete! Resources: 2 destroyed.
```

## Next Steps
In this tutorial you installed Terraform, created a configuration file, and used it to create and destroy infrastructure. Using Terraform to define your infrastructure as code lets you manage it all in one centralized configuration file and allows you to perform consistent and repeatable infrastructure deployments. 

This configuration file can be committed to a version control system (VCS) like Github to be versioned, audited, and collaborated on with other people.

- For more information on the technical terms used when discussing Terraform, review the [Terraform Glossary](https://www.terraform.io/docs/glossary.html).
- To learn more about the the configuration language and all of its different functions, explore the tutorials on the [Terraform configuration language](https://learn.hashicorp.com/collections/terraform/configuration-language) page.
- To learn how to manage infrastructure on the public clouds, check out the Getting Started tutorials for [Amazon Web Services](https://learn.hashicorp.com/tutorials/terraform/aws-build?in=terraform/aws-get-started), [Google Cloud Platform](https://learn.hashicorp.com/tutorials/terraform/google-cloud-platform-build?in=terraform/gcp-get-started), and [Microsoft Azure](https://learn.hashicorp.com/tutorials/terraform/azure-build?in=terraform/azure-get-started).
