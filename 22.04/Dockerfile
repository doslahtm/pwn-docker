FROM ubuntu:22.04
LABEL email="tuckdo4484@gmail.com"
WORKDIR /root
ENV LC_CTYPE C.UTF-8
ARG DEBIAN_FRONTEND=noninteractive

RUN dpkg --add-architecture i386

RUN echo "deb http://mirror.kakao.com/ubuntu focal-security main restricted \
deb http://mirror.kakao.com/ubuntu focal-security universe \
deb http://mirror.kakao.com/ubuntu focal-security multiverse" >> /etc/apt/sources.list

RUN apt update -y
RUN apt install python3 python3-pip python3-dev python3-setuptools libssl-dev libffi-dev build-essential libc6-i386 libc6-dbg gcc-multilib make gcc netcat git curl wget gdb vim nano zsh ruby-full net-tools nmap socat tmux -y
RUN python3 -m pip install --upgrade pip
RUN python3 -m pip install pwntools
RUN gem install one_gadget seccomp-tools

RUN git clone https://github.com/pwndbg/pwndbg .pwndbg
RUN sed -i 's/raise\ Exception(\x27Cannot\ override\ non-whitelisted\ built-in\ command\ \x22%s\x22\x27\ %\ command_name)/pass/g' .pwndbg/pwndbg/commands/__init__.py   
RUN python3 -m pip install -r .pwndbg/requirements.txt
RUN git clone https://github.com/hugsy/gef .gef
RUN python3 -m pip install -r .gef/requirements.txt
RUN git clone https://github.com/scwuaptx/Pwngdb .pwngdb
RUN echo "source ~/.pwngdb/pwngdb.py\\nsource ~/.pwngdb/angelheap/gdbinit.py\\n\\npython\\nimport angelheap\\nangelheap.init_angelheap()\\nend\\n\\ndefine init-pwndbg\\nsource ~/.pwndbg/gdbinit.py\\nend\\n\\ndefine init-gef\\nsource ~/.gef/gef.py\\nend" > .gdbinit
RUN echo "#!/bin/sh\\nexec gdb -q -ex init-pwndbg \"\$@\"" > /usr/bin/gdb-pwndbg
RUN echo "#!/bin/sh\\nexec gdb -q -ex init-gef \"\$@\"" > /usr/bin/gdb-gef
RUN chmod +x /usr/bin/gdb-*

RUN mkdir .zsh
RUN git clone https://github.com/sindresorhus/pure .zsh/pure
RUN git clone https://github.com/zdharma-continuum/fast-syntax-highlighting .zsh/fast-syntax-highlighting
RUN echo "ZSH_THEME=""\\nsource /root/.zsh/fast-syntax-highlighting/fast-syntax-highlighting.plugin.zsh\\nfpath+=/root/.zsh/pure\\nautoload -U promptinit; promptinit\\nprompt pure" > .zshrc

RUN mkdir /root/pwn
WORKDIR /root/pwn

RUN echo "init-gef" >> ~/.gdbinit
RUN apt-get update
RUN apt-get install xinetd -y
RUN chmod 774 /tmp
RUN chmod -R 774 /var/tmp /dev /run
RUN chmod 1733 /tmp /var/tmp /dev/shm

CMD [ "/bin/zsh" ]