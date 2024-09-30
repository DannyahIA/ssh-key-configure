#### 1. **Remover Chaves SSH do `ssh-agent`**

Se você tiver chaves SSH carregadas no **`ssh-agent`**, pode removê-las primeiro:

- Liste as chaves carregadas no **`ssh-agent`**:

  ```bash
  ssh-add -l
  ```

- Para remover todas as chaves carregadas no **`ssh-agent`**, execute:

  ```bash
  ssh-add -D
  ```

Isso removerá todas as chaves ativas no **`ssh-agent`**.

#### 2. **Remover Chaves SSH Salvas no Sistema**

As chaves SSH geralmente ficam armazenadas no diretório **`~/.ssh/`**. Para limpar essas chaves:

- Abra um terminal e navegue até o diretório **`~/.ssh/`**:

  ```bash
  cd ~/.ssh
  ```

- Liste os arquivos no diretório para ver as chaves SSH:

  ```bash
  ls
  ```

  Os arquivos típicos que você verá são:
  
  - **`id_rsa`** (chave privada)
  - **`id_rsa.pub`** (chave pública)
  - **`id_rsa_bb`** (se você gerou outra chave para Bitbucket, por exemplo)
  - **`known_hosts`** (armazena os servidores conhecidos onde você já conectou via SSH)

- Para remover todas as chaves, execute:

  ```bash
  rm id_rsa id_rsa.pub
  rm id_rsa_bb id_rsa_bb.pub  # ou outras chaves que você tiver criado
  ```

  Isso removerá suas chaves SSH privadas e públicas.

#### 3. **Remover o Arquivo de Configuração SSH**

Se você tiver um arquivo de configuração **SSH** que define aliases ou ajustes de chaves específicas, como **`~/.ssh/config`**, também deve removê-lo:

- Verifique se o arquivo **`config`** existe:

  ```bash
  ls ~/.ssh/config
  ```

- Se o arquivo existir e você quiser apagá-lo, execute:

  ```bash
  rm ~/.ssh/config
  ```

  Isso removerá todas as configurações específicas de SSH (como hosts personalizados ou rotas para múltiplas contas SSH).

#### 4. **Remover Hosts Armazenados no `known_hosts`**

O arquivo **`known_hosts`** armazena os registros dos servidores aos quais você se conectou via SSH. Se você quiser limpar esse histórico:

- Remova o arquivo **`known_hosts`**:

  ```bash
  rm ~/.ssh/known_hosts
  ```

  Isso excluirá todas as entradas de servidores conhecidos, e você será solicitado a aceitar as novas impressões digitais quando se reconectar a um servidor via SSH.

#### 5. **Remover Credenciais HTTPS do Git (Opcional)**

Se você estiver usando HTTPS para acessar seus repositórios (em vez de SSH), o Git pode ter salvo suas credenciais no sistema. Para limpar isso:

- No **Windows**, vá para **Painel de Controle > Gerenciador de Credenciais** e exclua as credenciais do **Git** ou **Bitbucket**.
  
- No **Linux/Mac**, se você usou o **Git Credential Manager**, pode listar as credenciais com:

  ```bash
  git credential-cache exit
  ```

  Ou, para remover credenciais individuais, use:

  ```bash
  git credential reject
  ```

#### 6. **Verificar e Remover Configurações Globais e Locais do Git**

Além de limpar as chaves SSH, você também pode querer remover as configurações globais e locais do Git que definem o uso de chaves SSH ou HTTPS.

- Para visualizar as configurações globais do Git:

  ```bash
  git config --global --list
  ```

- Para remover uma configuração global (como credenciais ou URLs):

  ```bash
  git config --global --unset credential.helper
  git config --global --unset url.ssh://git@bitbucket.org
  ```

- Para remover as configurações locais dentro de um repositório específico, navegue até o repositório e use:

  ```bash
  git config --unset credential.helper
  git config --unset url.ssh://git@bitbucket.org
  ```

#### 7. **Reiniciar o SSH-Agent (Opcional)**

Após fazer as limpezas, você pode querer reiniciar o **`ssh-agent`** para garantir que ele não tenha nada em memória. No terminal, execute:

```bash
eval $(ssh-agent -k)
```

Isso encerrará o processo do **`ssh-agent`**. Se você precisar reiniciá-lo mais tarde, basta executar:

```bash
eval $(ssh-agent)
```

---