# Kali Linux latest with useful tools by tsumarios
FROM kalilinux/kali-rolling

ARG SSH_USER
ARG SSH_PASSWORD
# Set working directory to /root
WORKDIR /root

# Update
RUN apt -y update && DEBIAN_FRONTEND=noninteractive apt -y dist-upgrade && apt -y autoremove && apt clean

# Install common and useful tools
RUN apt -y install curl wget sudo vim nano git net-tools whois netcat-traditional pciutils usbutils openssh-server -y


# Install useful languages
RUN apt -y install python3-pip golang nodejs npm

# Install Kali Linux "Top 10" metapackage and a few cybersecurity useful tools
RUN DEBIAN_FRONTEND=noninteractive apt -y install kali-tools-top10 exploitdb metasploit-framework powersploit man-db dirb nikto iputils-ping wpscan uniscan lsof apktool dex2jar  strace binwalk

# Config services ssh
RUN echo "root:${SSH_PASSWORD}" | chpasswd
COPY ssh_config /etc/ssh/ssh_config
COPY sshd_config /etc/ssh/sshd_config

COPY create_user.sh /usr/local/bin/create_user.sh
RUN chmod +x /usr/local/bin/create_user.sh
RUN /usr/local/bin/create_user.sh
COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

# Install Tor and proxychains, then configure proxychains with Tor
RUN apt -y install tor proxychains
COPY config-files/proxychains.conf /etc/proxychains.conf

# Install ZSH shell with custom settings and set it as default shell
RUN apt -y install zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
COPY config-files/.zshrc .

# Clean up
RUN apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD tail -f /dev/null
#ENTRYPOINT ["/bin/zsh"]
