# Configurações Básicas para Controle
Tutorial de instalação e configuração básica para controle.

# Ubuntu
Sistema Operacional: Ubuntu 22.04.5 LTS (Jammy Jellyfish).

Os recursos que utilizamos estão disponíveis mais facilmente no Linux, por isso, precisaremos fazer a instalação especificamente do Ubuntu 22.04.5 LTS.
Caso já tenha instalado, fique à vontade para pular essa etapa!

## Preparação do Sistema Operacional:
* Baixe a [imagem iso](https://releases.ubuntu.com/jammy/) do Ubuntu;
* Separe um pen drive de no **mínimo 8gb**;
* Baixe o executável do [Rufus](https://rufus.ie/pt_BR/) para preparar o instalador do sistema;
* Abra o executável do Rufus e **selecione a imagem ISO** do Ubuntu;
* Escolha o pen drive que será configurado e clique em **START**;
* Quando finalizar, seu instalador do Ubuntu estará pronto!

## Instalação do Ubuntu
* Reserve pelo menos **80gb** no seu disco rígido para o novo sistema;
* Desligue o computador e com o pen drive conectado acesse a bios do seu dispositivo;
* Na bios, mude a prioridade de boot de forma que o instalador seja o primeiro da lista;
* Salve as alterações e reinicie o computador;
* Quando ligar, o instalador do Ubuntu deve ser iniciado;
* Siga as etapas caso queira fazer um dual-boot ou uma instalação limpa.

# Instalações
Agora, faremos uma série de instalações e configurações para conseguirmos realizar as simulações.

## PX4 Autopilot
Para baixar e instalar o [PX4](https://docs.px4.io/main/en/dev_setup/dev_env_linux_ubuntu#simulation-and-nuttx-pixhawk-targets) rode esses códigos no seu terminal.
```bash
git clone https://github.com/PX4/PX4-Autopilot.git --recursive -b release/1.15
```
```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```
**Reinicie** seu dispositivo antes de continuar.

## ROS2 Humble
Iremos configurar o [ROS2 Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html).

Definir o locale.
```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

Agora precisaremos adicionar o repositório ROS2 apt no nosso sistema.
```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Os pacotes ros-apt-source fornecem chaves e configurações de fontes do APT para os vários repositórios do ROS.

Instalar o pacote ros2-apt-source configurará os repositórios do ROS 2 para o seu sistema. As atualizações da configuração dos repositórios ocorrerão automaticamente quando novas versões desse pacote forem lançadas nos repositórios do ROS.
```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

Instalar os pacotes do ROS2.

Atualize seus caches de repositório apt após configurar os repositórios.
```bash
sudo apt update
```

Os pacotes do ROS 2 são compilados em sistemas Ubuntu atualizados com frequência. É sempre recomendado garantir que seu sistema esteja atualizado antes de instalar novos pacotes.
```bash
sudo apt upgrade
```

Instalação do ROS.
```bash
sudo apt install ros-humble-desktop
```

Configuração do ambiente.
```bash
source /opt/ros/humble/setup.bash
```

**Teste de Funcionamento:**
Em um primeiro terminal, execute este código.
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```
Já no segundo, execute este outro.
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```
Você deve ver o talker dizendo que está publicando mensagens e o listener dizendo “eu ouvi essas mensagens”. Isso verifica que as APIs em C++ e Python estão funcionando corretamente.

## Instalando Dependências
Faremos a instalação de algumas dependências do Python.
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip python3-venv python3-dev -y
```
```bash
pip3 install --user -U empy pyros-genmsg setuptools
```
Agora corrigiremos outras.
```bash
pip3 install numpy pymap3d simple-pid --upgrade
```
Instalação do Yolo
```bash
pip install --upgrade pip
pip install ultralytics 
```
Dependências necessárias para o funcionamento do Gazebo.
```bash
pip3 install kconfiglib
pip install --user jsonschema
pip install --user jinja2
```

## Micro DDS
É o responsável pela tradução entre o ROS2 e o PX4.
```bash
git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
cd Micro-XRCE-DDS-Agent
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig /usr/local/lib/
```

# Gazebo
Versão: Gazebo Harmonic v8.11.0

Durante a instalação do ROS2 Humble, o Gazebo Classic 7 (versão 7.9.0) é automaticamente instalado, porém, não funciona para as simulações que faremos. Por isso, iremos removê-lo completamente e substituí-lo pelo correto.

Desinstalação completa do Gazebo Classic 7.
```bash
sudo apt remove gz-garden gz-sim7 ros-humble-ros-gzgarden
sudo apt autoremove
```
Baixar e instalar o Gazebo Harmonic.
```bash
sudo apt update
sudo apt install gz-harmonic
```
```bash
sudo apt install ros-humble-ros-gzharmonic
```
