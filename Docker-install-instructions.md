# Instalación de Docker sobre Ubuntu 18

# 1. Instalar desde los repositorios

# Actualizar repositorios
`sudo apt-get update`
`code`

# Instalar dependencias

´sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release´

# Añadir el GPG Key de docker

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Agregar un repositorio

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


# Actualizar nuevamente repositorios
sudo apt-get update

# Instalar Docker engine

sudo apt-get install docker-ce docker-ce-cli containerd.io

# Habilitar docker 
sudo systemctl enable docker

# Agregamos el usuario al grupo de docker
whoami
usermod -aG docker <usuario>

# probar docker
docker run hello-worl
