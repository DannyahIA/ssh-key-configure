## Passo a Passo

### 1. Gerar Chaves SSH para Cada Conta

1. Abra o **Git Bash**.
2. Navegue até o diretório inicial (`c:/Users/[seu_usuário]`):
   ```bash
   cd
   ```
3. Gere uma chave SSH para a **primeira conta do Bitbucket**:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "email_primeira_conta_bitbucket"
   ```
   - Salve a chave como `~/.ssh/id_rsa_bb1` (ou outro nome relevante).
4. Gere uma chave SSH separada para a **segunda conta do Bitbucket**:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "email_segunda_conta_bitbucket"
   ```
   - Salve a chave como `~/.ssh/id_rsa_bb2`.

### 2. Configurar o SSH-Agent

1. Adicione as chaves SSH ao `ssh-agent` para que ele possa gerenciar as conexões:
   ```bash
   ssh-add ~/.ssh/id_rsa_bb1
   ssh-add ~/.ssh/id_rsa_bb2
   ```
2. Verifique se as chaves foram adicionadas corretamente:
   ```bash
   ssh-add -l
   ```

### 3. Adicionar as Chaves SSH ao Bitbucket

1. Copie a chave pública da **primeira conta do Bitbucket**:
   ```bash
   clip < ~/.ssh/id_rsa_bb1.pub
   ```
2. Faça login na **primeira conta do Bitbucket** e adicione a chave copiada em [Configurações de SSH](https://bitbucket.org/account/settings/ssh-keys/).
3. Copie a chave pública da **segunda conta do Bitbucket**:
   ```bash
   clip < ~/.ssh/id_rsa_bb2.pub
   ```
4. Faça login na **segunda conta do Bitbucket** e adicione a chave copiada da mesma forma.

### 4. Configurar o Arquivo `~/.ssh/config`

Agora, precisamos criar um arquivo de configuração para associar as chaves SSH corretas a cada conta.

1. Abra o arquivo de configuração SSH (ou crie se não existir):
   ```bash
   nano ~/.ssh/config
   ```
2. Adicione as seguintes configurações para cada conta:

   ```bash
   # Primeira conta do Bitbucket
   Host bb1
       HostName bitbucket.org
       User git
       IdentityFile ~/.ssh/id_rsa_bb1

   # Segunda conta do Bitbucket
   Host bb2
       HostName bitbucket.org
       User git
       IdentityFile ~/.ssh/id_rsa_bb2
   ```
3. Salve e feche o arquivo.

### 5. Clonar Repositórios com Múltiplas Contas

Agora que você configurou duas identidades, use os aliases (`bb1` e `bb2`) no comando `git clone` para clonar repositórios de cada conta.

1. Para clonar um repositório da **primeira conta**:
   ```bash
   git clone git@bb1:usuario/repo.git
   ```
2. Para clonar um repositório da **segunda conta**:
   ```bash
   git clone git@bb2:usuario/repo.git
   ```

### 6. Gerenciar Configurações de Usuário no Git

Certifique-se de definir as configurações de usuário corretas para cada repositório.

1. Dentro de um repositório da **primeira conta**, defina as informações de usuário localmente:
   ```bash
   git config user.name "Nome da Primeira Conta"
   git config user.email "email_primeira_conta_bitbucket"
   ```
2. Dentro de um repositório da **segunda conta**, faça o mesmo para a segunda conta:
   ```bash
   git config user.name "Nome da Segunda Conta"
   git config user.email "email_segunda_conta_bitbucket"
   ```

### 7. Testar Conexão SSH com Cada Conta

1. Teste a conexão SSH para a **primeira conta**:
   ```bash
   ssh -T bb1
   ```
2. Teste a conexão SSH para a **segunda conta**:
   ```bash
   ssh -T bb2
   ```

Se tudo estiver configurado corretamente, você verá uma mensagem confirmando a autenticação em ambas as contas do Bitbucket.

---