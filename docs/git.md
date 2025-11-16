---
icon: simple/git
tags:
    - ctt
---

# Git

## Configurações iniciais

```bash
git version
git config --global user.name "Giovani"
git config --global user.email "giovani.bontempo@faculdadeimpacta.com.br"
git config --global --list # (1)!
```

1. O comando é usado para exibir as configurações globais do Git que foram definidas no sistema. Essas configurações são armazenadas em um arquivo chamado `.gitconfig`, que geralmente fica localizado no diretório home do usuário.

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

Marcador de Conflitos:

```
<<<<<<< HEAD
{Conteúdo do branch de destino}
=======
{Conteúdo do branch de origem}
>>>>>>>refs/remote/origin/main
```

## GitFlow

