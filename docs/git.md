---
icon: simple/git
tags:
    - ctt
---

# Git

??? info "Recursos"
    - [Visualizing Git](https://git-school.github.io/visualizing-git/)
    - [Learn Git and GitHub](https://roadmap.sh/git-github)

## Configurações iniciais

```bash
git version
git config --global user.name "Giovani"
git config --global user.email "giovani.bontempo@faculdadeimpacta.com.br"
git config --global --list # (1)!
```

```bash
ssh-keygen -t ed25519 -C "seu-email-do-github@example.com"
# ou então
ssh-keygen -t rsa -b 4096 -C "seu-email-do-github@example.com"
cat ~/.ssh/id_ed25519.pub #copie o código
```

Após copiar o código, colar no github: settings > ssh and gpg keys > new ssh key


1. O comando é usado para exibir as configurações globais do Git que foram definidas no sistema. Essas configurações são armazenadas em um arquivo chamado `.gitconfig`, que geralmente fica localizado no diretório home do usuário.

## Áreas do Git

![](https://www.i2tutorials.com/wp-content/media/2023/02/work2.png)

- Working Directory: O arquivo que você está editando.
- Staging Area: O conjunto de alterações que será incluído no próximo commit.
- Repository (Repositório): O banco de dados de commits, pode ser local ou remoto.

## Principais comandos

```bash
# inicialização
git init # (2)!
git init -b main # (1)!

git clone https://github.com/NOME-USER/NOME-REPO

# commitar
git add . # (4)!
git commit -m "mensagem do commit" # (5)!

# informações
git log # (3)!
git status #(6)!

# BRANCH
git branch <nome-da-branch>	# (7)!

# Ambos trocam para a branch especificada:
git switch <nome_branch>
git checkout <nome-branch>

git checkout -b <nome-da-branch> # (8)!
git branch	# (9)!

#renomear
git branch -m novo-nome # (10)!
git branch -m nome-antigo novo-nome # (11)!

# deletar
git branch -d nome-da-branch # (12)!
git branch -D nome-da-branch # (13)!

# MERGE 
git checkout main
git merge featureA # (14)!

# REBASE
git checkout main
git rebase featureA # (15)!
```

1. Inicializa o controle de versão na branch `main`.
2. Um diretório oculto chamado .git será criado, contendo todos os dados de controle de versão.
3. Lista informações sobre os últimos commits, como hash do commit, data, autor, hora e mensagem do commit. É possível observar o histórico de commits e branchs.
4. Ou apenas para incluir arquivos: `git add arq1.txt` ou ainda pra remover dos arquivos rastreados com `git rm arq1.txt`
5. Com a opção `-a`: `git commit -a -m "mensagem do commit"` faz com que todos os arquivos modificados que já estão sob rastreamento sejam automaticamente incluídos no commit. Sem essa opção, a cada novo commit, você precisaria usar git add para adicionar manualmente todos os arquivos que pretende versionar.
6. Mostra o estado do diretório de trabalho e do index, como: 
    - Arquivos do diretório de trabalho que foram alterados pelo desenvolvedor, mas que ele ainda não adicionou no index.
    - Arquivos que encontram-se no index, aguardando um commit.
7. Cria uma nova branch chamada `<nome-da-branch>`
8. O `-b` cria nova branch e troca para ela.
9. Lista todas as branches no repositório. Mostra em qual você está com `*`.
10. Renomeia branch em que você está.
11. Renomeia branch estando em outra.
12. Deleta, mas só funciona se a branch já foi mesclada (merged) com a principal.
13. Deleta, mas força exclusão, descartando tudo que ainda não foi mesclado.
14. Maneira pela qual podemos passar as alterações de um branch (`featureA`) para outra branch (`main`).
15. Maneira de transferir um conjunto de commits de uma branch (`featureA`) para outra (`main`). O objetivo principal do rebasing é criar um histórico de commits mais linear e limpo.

### Reverter mudanças

```bash
git restore <nome-do-arquivo> # (1)!
git restore --staged <nome-do-arquivo> # (2)!
git restore --source=<hash-do-commit> <nome-do-arquivo> # (3)!

# REVERTENDO COMMITS
git revert <hash-do-commit> # (4)!
git revert HEAD~2 # (6)!
git revert --continue # (7)!

git reset --hard HEAD~2 # (5)!
```

1. Você fez alterações em um arquivo, percebeu que cometeu um erro e quer descartar tudo para começar de novo a partir do último commit. O arquivo em sua cópia de trabalho é sobrescrito com a versão do último commit (ou do staging, se estiver staged). As alterações que você fez no arquivo são permanentemente perdidas.
2. Você fez `git add` em um arquivo por engano ou decidiu que as alterações nele precisam de mais trabalho e você decidiu que ele não deveria fazer parte do próximo commit. O arquivo é removido da área de staging. A versão modificada do arquivo em sua cópia de trabalho é mantida; ele simplesmente se move de volta para a seção "Changes not staged for commit".
3. Você precisa ver ou reverter para a versão de um arquivo de um ponto específico no histórico do projeto. O arquivo na sua cópia de trabalho é revertido para a versão daquele commit específico.
4. O `git revert` desfaz o efeito de um commit específico adicionando um novo commit no topo da branch, sem alterar ou reescrever a história já existente (os commits subsequentes ao que será revertido). Sempre deve apontar para um commit específico.
5. Use-o quando você comitou algo errado localmente e quer fingir que o commit nunca existiu para "começar de novo". Use apenas para desfazer commits que ainda não foram enviados (*pushed*) para o repositório remoto.
6. Comando alternativo, sendo o número 1 o último commit, o 2 o penúltimo, o 3 o antepenúltimo e assim por diante...
7. Após resolver um conflito de `revert`, use esse comando para continuar o processo.

Os três modos do `git revert`:

| Opção | Frase Intuitiva |
| :--- | :--- |
| `--soft` | Reverte apenas o *commit*, mantendo todas as alterações prontas para um novo *commit*. O código não é tocado (você desfaz o *commit* e o código fica como se tivesse feito `git add` em tudo). |
| `--mixed` | Reverte o *commit* e o *staging*, deixando as alterações como arquivos modificados na sua área de trabalho. O código não é tocado, mas sai do *staging* (você desfaz o *commit* e o `git add`). |
| `--hard` | Volta o projeto no tempo, apagando o *commit* e descartando permanentemente todas as alterações no código. O código é apagado (você desfaz o *commit* e as alterações no código).|

## Workflows Colaborativos
### GitFlow

![](https://miro.medium.com/1*MsVN9FOK7Aaue2gFYsehIg.png)

- branches principais e permanentes
    - `main` (ou `master` ou `trunk`) é o branch usado para armazenar as versões de um sistema que estão em **produção**.
    - `develop` é usado para armazenar código com funcionalidades que já foram implementadas, mas que ainda não passaram por um teste final (QA).
- branches temporárias
    - Branches de funcionalidade (`feature`) 
    - Branches de `release` são usados para preparar uma nova release
    - Branches de `hotfix` são usados para corrigir um erro crítico que foi detectado em produção (`main`)

### Github Flow

Existem apenas o branch principal e branches de funcionalidade, com suporte a revisão de código antes de integração, por meio do Pull Requests (PR).

![](https://user-images.githubusercontent.com/6351798/48032310-63842400-e114-11e8-8db0-06dc0504dcb5.png)

### TBD

Trunk-based development usa apenas a branch principal (`main`), e todos os desenvolvedores realizam seus commits diretamente no branch principal. Os desenvolvedores podem criar branches de funcionalidade, mas tais branches devem ter uma duração limitada, sendo integrados na branch principal.

