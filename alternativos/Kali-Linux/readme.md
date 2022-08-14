# Create a Kali Linux Image With Docker

## Objectives

The current project will create a working image of Kali Linux with the followin packets:

- metasploit-framework
- nmap
- powersploit
- python- pip
- python3
- vim
- sudo
- curl
- wget
- sudo
- nano
- git
- net-tools
- whois
- netcat-traditional
- pciutils
- usbutils
- kali-tools-top10
- exploitdb metasploit-framework
- man-db
- dirb
- nikto
- iputils-ping
- wpscan
- uniscan
- lsof
- apktool
- dex2jar
- strace
- binwalk
- openssh-server

## Required Apps

1. Docker
2. GIT

## Files Description

- user // Home directory volume
- .env // Environment variables (username and password) rename file env.example to .env
- create_user.sh // Bash script to generate user
- docker-compose.yml // Docker Compose configuration to ease the container management
- dockerfile // Script with all the setup commands to generate Kali image
- entrypoint.sh // Bash script to start SSH Server
- ssh_config // SSH host configuration
- sshd_config // SSH Server configuration

## Steps

1. Clone the repository and go to the project folder

```
git clone https://github.com/mdorozcog/kali_docker.git kali_docker
cd kali_docker
```

2. Customise settings. Inside the "dockerfile" add or remove packages according to your needs.

3. Create docker image and run container in the background.

```
docker-compose up -d
```

1. Log into your new container

```
ssh kali@127.0.0.1 -p 2222
su
#Enter your SU password. kali by default


And that's it! Now you have a working Kali Linux container.

To stop the container, simply run

```
docker-compose stop
```

To start the container again, you don't need to execute docker-compose up, instead you have to execute `docker-compose start`

Everytime you want to log in into your container, do it via SSH

```
ssh kalig@127.0.0.1 -p 2222
```
