Bootstrap: docker
From: archlinux

%runscript
    echo "scgenome"

%environment
    export PATH=/opt/miniconda3/bin:$PATH

%post
    echo "scgenome"

    # set time zone
    ln -s /usr/share/zoneinfo/UTC /etc/localtime

    # set locale
    echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen
    locale-gen
    echo 'LANG=en_US.UTF-8' > /etc/locale.conf

    # set the package mirror server
    echo 'Server = https://mirrors.kernel.org/archlinux/$repo/os/$arch' > \
        /etc/pacman.d/mirrorlist
    # add fail-over servers
    echo 'Server = https://archlinux.honkgong.info/$repo/os/$arch' >> \
        /etc/pacman.d/mirrorlist

    # install arch packages
    pacman -Syu --noconfirm wget
    pacman -Syu --noconfirm unzip
    pacman -Syu --noconfirm --needed base-devel
    pacman -Syu --noconfirm git
    pacman -Syu --noconfirm xorg-server

    # install miniconda
    wget https://repo.anaconda.com/miniconda/Miniconda3-py37_4.8.2-Linux-x86_64.sh
    bash Miniconda3-py37_4.8.2-Linux-x86_64.sh -bfp /opt/miniconda3
    rm -f Miniconda3-py37_4.8.2-Linux-x86_64.sh
    export PATH=/opt/miniconda3/bin:$PATH

    # install scgenome and dependencies
    wget https://codeload.github.com/shahcompbio/scgenome/zip/master -O scgenome.zip
    unzip scgenome.zip
    cd scgenome-master
    pip install numpy cython
    pip install -r requirements.txt
    python setup.py install

    # Remove the packages downloaded to Pacman cache dir.
    pacman -Syu --noconfirm pacman-contrib
    paccache -r -k0
