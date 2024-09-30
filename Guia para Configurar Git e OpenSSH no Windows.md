### Requisitos:
- **Git para Windows**
- **OpenSSH**

## Passo a Passo

### 1. Configurar o Git
1. Abra o Git Bash.
2. Configure seu nome de usuário e e-mail globalmente:
   ```bash
   git config --global user.name "Seu Nome"
   git config --global user.email "seu_email@email.com"
   ```

### 2. Criar Chaves SSH

1. Clique com o botão direito na área de trabalho e selecione "Git Bash Here".
2. Navegue até o diretório inicial (c:/Users/[seu_usuário]):
   ```bash
   cd
   ```
3. Gere uma chave SSH para o GitHub:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "seu_email_github"
   ```
   - **Nota**: Pressione **Enter** para usar o local padrão `~/.ssh/id_rsa`.
   - Digite uma senha quando solicitado.
4. Gere uma chave SSH separada para o Bitbucket:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "seu_email_bitbucket"
   ```
   - Salve a chave como `~/.ssh/id_rsa_bb`.

### 3. Configurar o SSH-Agent para Carregar Automaticamente

1. Verifique a existência dos arquivos `.profile` ou `.bashrc` no seu diretório inicial:
   - Se não existirem, crie um dos arquivos.
2. Adicione o seguinte script ao arquivo `.bashrc` (ou `.profile`):
   ```bash
   env=~/.ssh/agent.env

   agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

   agent_start () {
       (umask 077; ssh-agent >| "$env")
       . "$env" >| /dev/null ; }

   agent_load_env

   # agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
   agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

   if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
       agent_start
       ssh-add
   elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
       ssh-add
   fi

   unset env
   ```
3. Salve e feche o arquivo.

### 4. Adicionar Chaves SSH ao GitHub e Bitbucket

1. Adicione as chaves SSH ao GitHub e Bitbucket:
   1. Copie a chave pública do GitHub:
      ```bash
      clip < ~/.ssh/id_rsa.pub
      ```
   2. Faça login no GitHub e adicione a chave copiada à sua conta [nesta página](https://github.com/settings/keys).
   3. Copie a chave pública do Bitbucket:
      ```bash
      clip < ~/.ssh/id_rsa_bb.pub
      ```
   4. Faça login no Bitbucket e adicione a chave copiada [nesta página](https://bitbucket.org/account/settings/ssh-keys/).
   
   **Nota**: Execute os comandos `clip` individualmente para copiar as chaves corretamente.

### 5. Criar Arquivo de Configuração do SSH

1. No Git Bash, crie ou edite o arquivo `~/.ssh/config`:
   ```bash
   nano ~/.ssh/config
   ```
2. Adicione as seguintes configurações para configurar os aliases:
   ```bash
   # GitHub (padrão)
   Host gh
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_rsa

   # Bitbucket (secundário)
   Host bb
       HostName bitbucket.org
       User git
       IdentityFile ~/.ssh/id_rsa_bb
   ```
3. Salve o arquivo e saia.

### 6. Adicionar Chaves SSH ao SSH-Agent

1. Adicione as chaves ao SSH-Agent:
   ```bash
   ssh-add ~/.ssh/id_rsa
   ssh-add ~/.ssh/id_rsa_bb
   ```
2. Verifique se as chaves foram adicionadas corretamente:
   ```bash
   ssh-add -l
   ```

### 7. Verificar Conexão com GitHub e Bitbucket

1. Teste a conexão com o GitHub:
   ```bash
   ssh -T gh
   ```
2. Teste a conexão com o Bitbucket:
   ```bash
   ssh -T bb
   ```

Se as chaves foram configuradas corretamente, você verá uma mensagem confirmando a autenticação.

### 8. Alterar Conta do Git para Projetos Diferentes

- Para usar uma conta secundária, defina o nome e o e-mail locais para um repositório específico:
  ```bash
  git config user.name "Nome da Conta Secundária"
  git config user.email "email_da_conta_secundaria@email.com"
  ```
- Para a conta principal, o Git usará a configuração global definida no passo 1.

### 9. Extra: Instalar o Git com OpenSSH Usando Winget

- Se você ainda não tiver o Git ou OpenSSH, instale usando o **winget**:
  ```bash
  winget install --id Git.Git -c --source winget
  ```

---