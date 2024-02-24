Vagrant.configure("2") do |config|
  config.vm.box = "nome_da_box_debian_12"

  config.vm.provision "shell", inline: <<-SHELL
    # Atualiza pacotes e instala pré-requisitos
    sudo apt update && sudo apt upgrade -y
    sudo apt install -y curl software-properties-common apt-transport-https ca-certificates lsb-release gnupg

    # Instala Node.js e npm
    curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
    sudo apt-get install -y nodejs

    # Instala Python 3.11 e pip
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt update
    sudo apt install -y python3.11 python3.11-venv python3.11-distutils
    curl -sS https://bootstrap.pypa.io/get-pip.py | sudo python3.11

    # Instala Docker
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt update
    sudo apt install -y docker-ce docker-ce-cli containerd.io

    # Adiciona o usuário vagrant ao grupo docker
    sudo usermod -aG docker $USER

    # Baixa e instala o Rotki
    # Como o Rotki pode ter instruções específicas de instalação, revise https://rotki.readthedocs.io para as últimas instruções
    # Exemplo: instalação via pip (ajuste conforme necessário)
    python3.11 -m pip install rotki
  SHELL
end