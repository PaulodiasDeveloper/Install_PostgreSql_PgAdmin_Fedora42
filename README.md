# **Instalação do PostgreSQL e pgAdmin no Fedora**

Este guia fornece um passo a passo detalhado para instalar e configurar o servidor de banco de dados **PostgreSQL** e a ferramenta de administração **pgAdmin 4** em sistemas operacionais Fedora (testado e adaptado para versões recentes, como o Fedora 42).

## **📋 Pré-requisitos**

* Um sistema com Fedora instalado.  
* Acesso ao terminal com privilégios de superusuário (sudo).

## **🚀 Passo 1: Instalação do Servidor PostgreSQL**

Utilizaremos o repositório oficial do PostgreSQL (PGDG) para garantir a instalação da versão mais recente e evitar conflitos com os pacotes padrão do sistema.

### **1.1 Atualizar o Sistema**
Garanta que todos os pacotes do seu sistema estão atualizados.  
```
sudo dnf update \-y
```

### **1.2 Adicionar o Repositório do PostgreSQL**

Adicione o repositório oficial mantido pela equipe do PostgreSQL.  
```
sudo dnf install \-y https://download.postgresql.org/pub/repos/yum/reporpms/F-42-x86\_64/pgdg-fedora-repo-latest.noarch.rpm
```

**Nota:** Se o link acima estiver quebrado, visite o [site de downloads do PostgreSQL](https://www.postgresql.org/download/linux/redhat/) para obter o link correto para a sua versão do Fedora.

### **1.3 Desabilitar o Módulo Padrão**

Para evitar conflitos, desabilite o módulo nativo do PostgreSQL que vem com o Fedora.
```  
sudo dnf \-qy module disable postgresql
```

### **1.4 Instalar o Servidor**

Instale a versão mais recente do servidor PostgreSQL. Neste exemplo, usamos a versão 17\.  
```
sudo dnf install \-y postgresql17-server
```

### **1.5 Inicializar o Banco de Dados**

Antes do primeiro uso, o cluster de banco de dados precisa ser inicializado.

```
sudo /usr/pgsql-17/bin/postgresql-17-setup initdb
```

### **1.6 Iniciar e Habilitar o Serviço**

Configure o serviço do PostgreSQL para iniciar automaticamente com o sistema e inicie-o imediatamente.

```
\# Habilita o serviço para iniciar no boot  
sudo systemctl enable postgresql-17

\# Inicia o serviço agora  
sudo systemctl start postgresql-17
```

### **1.7 Definir Senha do Superusuário**

Por segurança, é fundamental definir uma senha para o usuário administrador postgres.

```
\# Mude para o usuário postgres  
sudo \-i \-u postgres

\# Abra o cliente psql  
psql

\# No prompt do psql, execute o comando para definir a senha  
\# Substitua 'sua\_senha\_forte' por uma senha de sua escolha  
\\password postgres
```

Após definir e confirmar a senha, saia do psql com \\q e retorne ao seu usuário com exit.

## **🐘 Passo 2: Instalação do pgAdmin 4**

O pgAdmin 4 é uma interface gráfica completa para gerenciar seus bancos de dados.

### **2.1 Adicionar o Repositório do pgAdmin**

```
sudo rpm \-i https://ftp.postgresql.org/pub/pgadmin/pgadmin4/yum/pgadmin4-fedora-repo-2-1.noarch.rpm
```

### **2.2 Instalar o pgAdmin**

```
sudo dnf install pgadmin4 \-y
```

### **2.3 Configurar o Acesso Web (Opcional)**

O pgAdmin pode ser executado como um serviço web. Este script configura o acesso via navegador.  
sudo /usr/pgadmin4/bin/setup-web.sh

Siga as instruções para definir um e-mail (usuário) и uma senha de acesso.

## **🔌 Passo 3: Conectando o pgAdmin ao Banco de Dados**

Com tudo instalado, o último passo é conectar a interface gráfica ao seu servidor local.

1. **Abra o pgAdmin 4** a partir do menu de aplicativos do seu sistema.  
2. Na primeira execução, ele pedirá uma **"Master Password"**. Esta senha protege as credenciais de todos os servidores que você salvar.  
3. No painel esquerdo, clique com o botão direito em **Servers \> Create \> Server...**.  
4. Na janela que se abre, preencha as abas:  
   * **Aba General**:  
     * **Name**: Dê um nome descritivo para a sua conexão (ex: PostgreSQL Local).  
   * **Aba Connection**:  
     * **Host name/address**: localhost  
     * **Port**: 5432  
     * **Maintenance database**: postgres  
     * **Username**: postgres  
     * **Password**: A senha que você definiu no **Passo 1.7**.  
5. Clique em **Save**.

Se tudo estiver correto, seu servidor local aparecerá no painel esquerdo, pronto para ser gerenciado.

## **📄 Licença**

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.