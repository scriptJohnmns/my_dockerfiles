# Usa a imagem base do Ubuntu na versão 'rolling'
FROM ubuntu

# Define variáveis de ambiente para o usuário
# Você pode mudar 'meuusuario' para o nome que preferir
ARG USER_NAME=scripjohn
ARG USER_UID=1001
ARG USER_GID=1001

# Evita que o apt faça perguntas interativas durante a instalação
ENV DEBIAN_FRONTEND=noninteractive

# Atualiza a lista de pacotes e instala as ferramentas CLI que você precisa
# Inclui 'sudo' para o usuário não-root poder executar comandos como root
# O '&& \' permite quebrar a linha, e 'apt-get clean' junto com 'rm -rf /var/lib/apt/lists/*'
# ajuda a manter o tamanho final da imagem menor.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    htop \
    curl \
    git \
    sudo \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Cria o grupo e o usuário não-root com os IDs especificados
# Adiciona o usuário ao grupo 'sudo' e configura para não pedir senha ao usar sudo
RUN groupadd -g ${USER_GID} ${USER_NAME} && \
    useradd -u ${USER_UID} -g ${USER_GID} -m -s /bin/bash ${USER_NAME} && \
    echo "${USER_NAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USER_NAME} && \
    chmod 0440 /etc/sudoers.d/${USER_NAME}

# Define o diretório de trabalho padrão para o usuário não-root
WORKDIR /home/${USER_NAME}

# Troca para o usuário não-root.
# Todos os comandos subsequentes (RUN, CMD, ENTRYPOINT) serão executados como 'meuusuario'.
USER ${USER_NAME}

# Comando padrão que será executado quando o contêiner iniciar sem um comando específico.
# Isso te dá acesso direto ao shell Bash do 'meuusuario'.
CMD ["bash"]
