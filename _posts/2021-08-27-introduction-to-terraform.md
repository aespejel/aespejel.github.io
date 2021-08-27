---
layout: single
classes: wide
title:  "Introducción a Terraform"
excerpt: "En este post trato de explicar qué es Terraform y los comandos más útiles para empezar a usarlo."
categories: terraform
tags: 
  - Terraform
  - AWS
  - IaC
  - iniciación
toc: true
toc_label: "Secciones:"
---

En este post vamos a hablar de Terraform, una herramienta creada por Hashicorp y que [hace poco ha alcanzado su primera versión estable 1.0][Announcing HashiCorp Terraform 1.0 General Availability].
No nos dejemos engañar por este dato, aunque haya alcanzado su primera versión estable hace poco, es una herramienta considerada madura y que muchas empresas la usan ya desde hace años para desplegar infraestructura en la nube.

Ya hay muchísimos artículos muy completos sobre iniciación a Terraform, uno de los mejores (y habitualmente actualizado) es la [documentación de introducción][Introduction] de la propia web de Terraform.
Así pues no vamos a extendernos demasiado en este artículo y solamente vamos a destacar algunos de los puntos fuertes de Terraform, por qué nos gusta y por qué es necesario usar una herramienta de infraestructura como código.

## Algunas de las cosas que caracterizan a Terraform
### Infraestructura como código!
Nos ofrece un lenguaje de alto nivel con el que declarar nuestra infraestructura, de forma que podamos versionarla y reutilizarla. Gracias a este tipo de herramientas, se pasa de la gestión manual de los recursos a la “programación de infraestructura”, lo que es mucho menos propenso a errores humanos.

Al final del día, si sigues repitiendo lo mismo una y otra vez, eventualmente te vas a equivocar o los resultados no van a ser perfectamente homogéneos. Tener tu infraestructura como código evita esto porque solo necesitas ejecutar un comando para desplegar tu infraestructura, sin importar lo compleja que sea.

Por ejemplo, si tuviéramos que desplegar 100 instancias con la misma configuración, lo más probable es que fallásemos al crear algunas de ellas (imaginad crear 100 recursos teniendo que hacer clic en varias checkboxes, nombrarlos siguiendo un patrón, etc. Yo mismo muy probablemente fallaría un par de veces durante el proceso, se me olvidaría marcar una de las checkboxes o me saltaría una iteración en el patrón del nombre del recurso). Con Terraform, esto es mucho menos probable que suceda porque podemos hacer una configuración y desplegar ese recurso configurado 100 veces con un bucle. Si creamos una configuración específica para un entorno utilizando las mejores prácticas, parametrizando valores con variables, etc., hacer lo mismo para otro entorno sería fácil porque solo necesitaríamos adaptar los valores de las variables.

### Declarativo!
Una de las características principales de Terraform es que es declarativo. Es esto bueno? Malo? Qué es ser declarativo?
Respecto a esto, hay varios paradigmas de la programación que nos permiten clasificar cómo es un lenguaje de programación en función de algunas de sus características. Así pues y limitándonos a lo que nos interesa en este post, un lenguaje de programación puede ser declarativo o imperativo.
Dependiendo de lo familiarizado que se esté con el mundo del desarrollo, las diferencias entre declarativo e imperativo pueden ser difusas, pero para aquellos que vengan de nuevas trataré de poner un ejemplo relacionado con una de las cosas que más me gustan en el mundo, la comida:
Voy a un restaurante de comida rápida y me pido una hamburguesa de forma:
* Declarativa: hola (mundo), quisiera una hamburguesa al punto, con queso, cebolla y pepinillos.
* Imperativa: hola (mundo), quisiera que picasen 150g de carne de vacuno, 2 veces, calentasen una sartén con unas gotas de aceite y pusieran la carne con forma de disco de unos 12 cm de diámetro en la sartén a una temperatura constante de… os hacéis a la idea, verdad? (Acabo de darme cuenta de que pedir de forma imperativa en el mundo real suena bastante maleducado).

Para la infraestructura como código el hecho de usar un lenguaje declarativo, en mi opinión, es una ventaja. Podemos ir pidiendo o declarando los recursos como si de una carta de un restaurante se tratase, sin preocuparnos en gran medida de lo que tiene que ocurrir para que finalmente los tengamos. 
Si un recurso está correctamente declarado en nuestra configuración, tras ejecutar terraform, ese recurso será creado tal y como lo hayamos descrito (con cebolla, queso, pepinillos…).
Si más adelante lo borramos de nuestra configuración, tras ejecutar terraform el recurso será borrado de la infraestructura. Es decir, lo que haya en nuestra configuración se va a traducir en infraestructura. Si añadimos cosas se añadirá infraestructura, si borramos cosas se borrará infraestructura.

El hecho de ser declarativo tiene algunos inconvenientes (que trataré de cubrir en futuros posts) aunque en mi opinión y basándome en mi experiencia personal con Terraform, el hecho de ser declarativo es mucho más una ventaja que un inconveniente.

Para finalizar este apartado os dejo por aquí un enlace a la [descripción de la wikipedia de lo que es lenguaje declarativo][Declarative programming - Wikipedia], así como este [otro post][The declarative vs imperative Infrastructure as Code discussion is flawed] hablando de este tópico centrado en Terraform, Cloudformation y el CDK, que me ha resultado muy interesante.

### Gestiona dependencias!
Cuando vamos a desplegar recursos, como una instancia en una VPC específica, parece obvio que antes de desplegar la instancia tendremos que desplegar la VPC. Desgraciadamente según incrementamos en número de recursos a desplegar y la complejidad de la arquitectura, cada vez es más complicado saber cuál es el orden correcto y muy probablemente fallaremos en nuestro primer intento tratando de desplegar un recurso que necesita que esté presente otro que aún no ha sido desplegado. Terraform gestiona dependencias, tanto implícitas como explícitas (ya hablaré de esto en futuros posts), haciendo mucho más sencillo desplegar los recursos en el orden correcto.

### Open source!
Terraform está licenciado como [Mozilla Public License v2.0][Terraform license] y al ser open source nos permite contribuir y extenderlo, así como usarlo de forma gratuita.

## Guía rápida para usar Terraform
Para ponernos manos a la obra lo único que tenemos que hacer es ir al sitio [web de descargas][Download Terraform]
del binario de Terraform, descargarlo y colocar ese binario en algún directorio dentro de nuestro [PATH][What are PATH and other environment variables].
A partir de aquí, ya podremos ejecutar el cli de Terraform.

Para además poder desplegar infraestructura en alguna nube, necesitaremos que Terraform sea capaz de identificarse de alguna manera en esa nube con permisos suficientes para poder desplegar lo que hayamos configurado.
Vamos a utilizar [AWS][AWS] para nuestros ejemplos y la forma más sencilla de configurar ese acceso de Terraform a AWS es configurando el sistema para poder usar el [cli de AWS][Configuring the AWS CLI].
Una vez tengamos la configuración hecha tal y como se explica ahí, solo tendremos que añadir el [proveedor de AWS][Hashicorp AWS] a nuestra configuración de Terraform (si no sabes qué es un proveedor de Terraform, no te preocupes porque lo explicaremos en futuros posts). Hay más formas de hacer que Terraform se identifique en AWS, pero de momento usaremos esta que acabamos de explicar, que es la más sencilla.

Terraform identificará cualquier archivo con extensión .tf dentro de nuestro directorio como parte de la configuración, así que para la configuración del proveedor podemos usar el archivo provider.tf y suponiendo que usamos la última versión de Terraform (o al menos una versión superior a la 0.13) la configuración quedaría de la siguiente manera:
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
}
```

### 4 comandos que necesitas saber
Con estos 4 comandos ya podemos desplegar infraestructura y eliminarla, siempre y cuando tengamos configurado correctamente Terraform y la posibilidad de que acceda a un proveedor de infraestructura (AWS en nuestros ejemplos).

#### terraform init
El comando [terraform init][Command: init] es el usado para inicializar nuestro entorno de terraform. Se comprueba la configuración de proveedores, se descargan módulos… Es similar a cuando hacemos git init para comenzar a usar git en un proyecto.
Es lo primero que tenemos que hacer para probar una nueva configuración que hayamos creado o que nos hayamos descargado de nuestro control de versiones.
Después de una ejecución exitosa de terraform init, deberíamos ver un mensaje similar a este:

```
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v3.55.0...
- Installed hashicorp/aws v3.55.0 (self-signed, key ID XXXXXXXXXXXXXXXX)

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

#### terraform plan
El comando [terraform plan][Command: plan] creará un plan de ejecución, esto nos mostrará los recursos que se tratarán de crear, modificar o eliminar en caso de ejecutar el comando terraform apply sobre la configuración actual. Terraform hace un seguimiento de todo lo que ha sido creado con la configuración en el "[estado][state]", de esta forma, durante un terraform plan Terraform podrá comparar lo que ya se ha desplegado previamente con lo que debería estar desplegado tras la ejecución de terraform apply. El estado además se usa para prevenir cambios a recursos desplegados por otras personas o herramientas, ya que solo realizará cambios sobre recursos que aparezcan en el estado (hablaré más de gestión del estado de Terraform en futuros posts).
Es uno de los puntos más importantes, ya que nos permite corregir posibles errores antes de que se hayan ejecutado.
Por otro lado, horas o incluso días de corrección de errores nos pueden ahorrar valiosos minutos de revisar el plan. Vosotros veréis!
Para este ejemplo, vamos a usar los siguientes dos archivos de configuración (que al tener la extensión .tf terraform interpretará como una única configuración):

* provider.tf:
```
provider "aws" {
  region                  = "us-west-2"
  shared_credentials_file = "~/.aws/credentials"
  profile                 = "our_aws_user"
}
```
* main.tf:
```
resource "aws_instance" "app_server" {
  ami           = "ami-0af6e2b3ada249943"
  instance_type = "t2.micro"
}
```

Y el resultado de ejecutar terraform plan será (hemos recortado las partes menos relevantes): 
```
$ terraform plan

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.app_server will be created
  + resource "aws_instance" "app_server" {
      + ami                                  = "ami-0af6e2b3ada249943"
...
      + instance_type                        = "t2.micro"
...
    }

Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```
De momento no entenderemos gran parte de lo que nos dice, pero es importante ver el número de ítems que se van a añadir, cambiar y destruir en nuestra infraestructura. Esto ya nos va a dar una pista de si lo que va a hacer terraform al ejecutar terraform apply es realmente lo que estábamos buscando.

#### terraform apply
Con [terraform apply][Command: apply] finalmente haremos los cambios descritos en nuestra configuración. Antes de aplicar estos cambios, Terraform hará un plan y pedirá confirmación para proceder:
```
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.app_server will be created
  + resource "aws_instance" "app_server" {
      + ami                                  = "ami-0af6e2b3ada249943"
...
      + instance_type                        = "t2.micro"}
...
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```
Y tras responder "yes":
```
  Enter a value: yes

aws_instance.app_server: Creating...
aws_instance.app_server: Still creating... [10s elapsed]
aws_instance.app_server: Still creating... [20s elapsed]
aws_instance.app_server: Still creating... [30s elapsed]
aws_instance.app_server: Still creating... [40s elapsed]
aws_instance.app_server: Creation complete after 45s [id=i-06c589b58eb588561]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```
Nuestra configuración es aplicada y nuestros recursos (una instancia en este ejemplo) desplegados.

#### terraform destroy
Con [terraform destroy][Command: destroy] toda la infraestructura que ha sido desplegada con nuestra configuración será eliminada. Antes de hacerlo se hace un plan, se muestran los resultados y se pide confirmación:
```
$ terraform destroy

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # aws_instance.app_server will be destroyed
  - resource "aws_instance" "app_server" {
      - ami                                  = "ami-0af6e2b3ada249943" -> null
...
      - id                                   = "i-06c589b58eb588561" -> null
      - instance_type                        = "t2.micro" -> null
...
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value:
```
Y tras responder "yes":
```
  Enter a value: yes

aws_instance.app_server: Destroying... [id=i-06c589b58eb588561]
aws_instance.app_server: Still destroying... [id=i-06c589b58eb588561, 10s elapsed]
aws_instance.app_server: Still destroying... [id=i-06c589b58eb588561, 20s elapsed]
aws_instance.app_server: Still destroying... [id=i-06c589b58eb588561, 30s elapsed]
aws_instance.app_server: Destruction complete after 36s

Destroy complete! Resources: 1 destroyed.
```
La configuración que se había desplegado anteriormente, es destruida.


[Announcing HashiCorp Terraform 1.0 General Availability]: https://www.hashicorp.com/blog/announcing-hashicorp-terraform-1-0-general-availability
[Introduction]: https://www.terraform.io/intro/index.html
[Declarative programming - Wikipedia]: https://en.wikipedia.org/wiki/Declarative_programming
[The declarative vs imperative Infrastructure as Code discussion is flawed]: https://aws-blog.de/2020/02/the-declarative-vs-imperative-infrastructure-as-code-discussion-is-flawed.html
[Terraform license]: https://github.com/hashicorp/terraform/blob/main/LICENSE
[Download Terraform]: https://www.terraform.io/downloads.html
[What are PATH and other environment variables]: https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them
[AWS]: https://aws.amazon.com/
[Configuring the AWS CLI]: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html
[Hashicorp AWS]: https://registry.terraform.io/providers/hashicorp/aws/latest/docs
[Command: init]: https://www.terraform.io/docs/cli/commands/init.html
[Command: plan]: https://www.terraform.io/docs/cli/commands/plan.html
[state]: https://www.terraform.io/docs/language/state/index.html
[Command: apply]: https://www.terraform.io/docs/cli/commands/apply.html
[Command: destroy]: https://www.terraform.io/docs/cli/commands/destroy.html
