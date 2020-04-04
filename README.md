# Symfony Docker

A [Docker](https://www.docker.com/)-based installer and runtime for the [Symfony](https://symfony.com) web framework, with full [HTTP/2](https://symfony.com/doc/current/weblink.html) and HTTPS support.

## Getting Started

1. Run `docker-compose up` (the logs will be displayed in the current shell)
2. Open `https://localhost` in your favorite web browser and [accept the auto-generated TLS certificate](https://stackoverflow.com/a/15076602/1352334)
3. **Enjoy!**

## Selecting a Specific Symfony Version

Use the `SYMFONY_VERSION` environment variable to select a specific Symfony version.

For instance, use the following command to install Symfony 3.4:

`SYMFONY_VERSION=3.4.* docker-compose up --build`

To install a non-stable version of Symfony, use the `STABILITY` environment variable during the build.
The value must be [a valid Composer stability option](https://getcomposer.org/doc/04-schema.md#minimum-stability)) .

For instance, use the following command to use the `master` branch of Symfony:

```bash
STABILITY=dev docker-compose up --build
```

## Debugging
This version of the image is shipped with Xdebug.

Xdebug is enabled by default and you can listen to the port using PHP Storm a Jetbrains product which can be downloaded from [here](https://www.jetbrains.com/phpstorm/).

Using this instruction will connect your IDE and Docker together:
##
#### Configuring your docker
##### (1) You should update your Docker to version above 18.0
##### (2) You should expouse the daemon which listen to the port 2375

For MacOS and Windows:

Go to the Docker Dashboard -> Settings -> General
Select <code>Expose daemon on tcp://localhost:2375 without TLS</code>
And restart you Docker Application(Don't just Apply & Restart)

For Linux:

Just start the service using <code>systemctl start docker</code> and if you want more configuration edit <code>daemon.json</code>
located in <code>/etc/docker/</code>
##
#### Add Project Configuration to IDE

##### (1) Open "Run/Debug Configurations" using the menu
- if there is an configuration then just check the parameters through the following steps.

##### (2) Add a new Environment and select "docker-compose"
##### (3.1) Name it something
##### (3.2) On the server part click on "Create New"
##### (3.2.1) Name it after the <code>docker-compose.override.yaml</code> key which is <code>PHP_IDE_CONFIG</code>
##### (3.2.2) Select TCP socket and make sure the value is <code>tcp://localhost:2375 </code> or any port that you docker daemon is listening
##### (3.2.3) In the "Path Mapping" you should give your project location in the docker and inside your PC
   ######Example: my virtual path is <code>/srv/app</code> and my machine path is <code>C:\Users\Code\symfony</code>
##### (3.2.4) Hit OK!
##### (4) In Compose file select <code>docker-compose.yml</code> and <code>docker-compose.override.yaml</code>
##### (5) Hit Save!

##
#### Configuring Xdebug Port in PHPstorm

- Default that I put was 5902 but you can change it to any port which is available.
- You can change the Xdebug port by changing <code>remote_port</code> in <code>docker-compose.override.yaml</code> 
##### (1) In PHP Storm open Setting -> Language and Framework -> PHP -> Servers
##### (2.1) Add a new server
##### (2.2) Name it after the <code>PHP_IDE_CONFIG</code> and your host should be the IP in the configuration.
 - if you have <code>host.docker.internal</code> you should enter <code>localhost</code>
 ##### (2.3) Port must be the <code>remote_port</code>
 ##### (2.4) debugger <code>Xdebug</code>
 ##### (2.5) Choose you binary console file located inside project <code>bin/console</code>
 ##### (2.6) Hit Save and Rule.
 
 - to become 100% sure that everything is worked out please rebuild the project by using this two command
 <br>
 <code>docker-compose up --build</code>
 <br>
 <code>docker-compose up -d</code>
   





  

## Credits

Created by [Kévin Dunglas](https://dunglas.fr), co-maintained by [Maxime Helias](https://twitter.com/maxhelias) and sponsored by [Les-Tilleuls.coop](https://les-tilleuls.coop).<br>
Xdebug instruction by [Farid Golchin](http://igolchin.com)

------

## Homestead Installation

0.a. Install virtual box:  
https://www.virtualbox.org/wiki/Downloads

0.b. Install Vagrant for your system  
https://www.vagrantup.com/downloads.html 

1. on Mac or Linux run `php vendor/bin/homestead make` or 
on Windows run `vendor\\bin\\homestead make`  
https://laravel.com/docs/7.x/homestead#per-project-installation

2. modify `Homestead.yaml`

an example is: (please modify `Path-to-project)
```yaml
ip: 192.168.10.10
memory: 2048
cpus: 2
provider: virtualbox
authorize: ~/.ssh/id_rsa.pub
keys:
    - ~/.ssh/id_rsa
folders:
    -
        map: /Path-to-project/base-symfony-docker
        to: /home/vagrant/code
sites:
    -
        map: web2.test
        to: /home/vagrant/code/public
databases:
    - homestead
features:
    -
        mariadb: false
    -
        ohmyzsh: false
    -
        webdriver: false
name: base-symfony-docker
hostname: base-symfony-docker
```
3. run `vagrant up`