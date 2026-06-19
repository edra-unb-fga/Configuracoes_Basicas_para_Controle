# Configuração da Missão Teste
Iremos configurar e rodar uma missão teste para checar se está tudo correto com o sistema.

Primeiramente, escolha uma pasta para clonar os repositórios, assim, manteremos tudo bem organizado!

Dentro da pasta escolhida, vamos clonar o repositório da [Missão Template](https://github.com/edra-unb-fga/Workspace_Template#).
```bash
git clone git@github.com:edra-unb-fga/Workspace_Template.git
```
Com tudo clonado, vamos abrir a pasta pelo terminal.
```bash
cd [SUA PASTA PARA OS PROJETOS]/Workspace_Template/
```
Agora, iniciaremos o Visual Studio Code (configure previamente sua conta).
```bash
code .
```
Com o código da missão aberto no Visual Studio Code, inicie **três** terminais, com o primeiro aberto, basta apertar **Ctrl+Shift+5** para iniciar os demais.

Antes, faremos o build do código para podermos iniciá-lo.
Lembrando, após qualquer modificação, é preciso **refazer o build!**
```bash
rm -rf build/ install/ log/
colcon build
```
Espere finalizar para avançar.

## Iniciar a Missão Teste
Em um dos terminais iniciaremos o tradutor entre o ROS2 e o PX4, ele ficará rodando durante toda a simulação.
```bash
MicroXRCEAgent udp4 -p 8888
```
No segundo, iniciaremos o Gazebo.
```bash
cd ~/PX4-Autopilot
make clean
make px4_sitl gz_x500_mono_cam
```
Estará tudo correto se aparecer a mensagem: **Ready for takeoff!**
**Detalhe:** Não é preciso limpar toda vez que for realizar uma nova inicialização do Gazebo, somente se for a primeira vez após uma grande modificação, por isso, caso prefira, fique à vontade para executar somente a última linha.
```bash
cd ~/PX4-Autopilot
make px4_sitl gz_x500_mono_cam
```

Já no último, iniciaremos o código, **dentro da pasta da missão**.
```bash
source install/setup.bash
ros2 run missao_template main
```
Nesse passo, com o tradutor em funcionamento, o Gazebo já aberto corretamente e o código iniciado, a Missão Template será executada.
A câmera do drone deve abrir automaticamente e em sequência a missão ser realizada!
