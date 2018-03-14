GIT - Resumo
=============

*... EM DESENVOLVIMENTO*

Configuração
--------------

Para configurar os dados dos usuários::

    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com

Para configurar o editor padrão para mensagens de commit::

    $ git config --global core.editor emacs

Caso queira verificar suas configurações atuais, utilize o parâmetro ``--list``::

    $ git config --list
    user.name=John Doe
    user.email=johndoe@example.com
    color.status=auto
    color.branch=auto
    color.interactive=auto
    color.diff=auto
    ...


Alias para um log mais "pretty"::

    $ git config --global alias.logp "log --pretty --oneline --graph --all --decorate"
    $ git logp  # Para usar


Utilizando a ajuda
---------------------------

Utilize o parâmetro --help para consultar a documentação de uso de um determinado comando. Exemplo::

    $ git diff --help

O comando acima mostra a ajuda para o comando ``git diff``.

Iniciando repositórios
-----------------------

Para iniciar um repositório usamos o comando ``git init``. Ele ciará o subdiretório *.git* e deixará tudo pronto para adição dos arquivos.

Em seguida, precisamos incluir os arquivos que queremos que seja controlado pelo Git. Para isso, utilizamos o comando ``git add``. Exemplo::

    $ git add *.php
    $ git add LICENSE

O comando acima adiciona todos os arquivos *.php* presentes no diretório atual e o arquivo LICENSE ao Git. Feito isso eles estão em *staging* (veremos mais sobre em **Adicionando alterações para commit**) e prontos para commit::

    $ git commit -m "Primeiro commit"

O comando acima irá consolidar os arquivos adicionados em *staging* para o controle de versão do Git com a mensagem "Primeiro commit".

Ate o momento, seu repositório, commits e arquivos sob controle de versão estão apenas locais, nenhuma comunicação de rede foi feita. Para enviar para um repositório "cental", utilizamos o comando ``git push``, porém antes é necessário configurar um *remote* para isso. Exemplo::

    $ git remote add origin git@github.com:alissonperez/git-resumo.git

Agora temos um repositório remoto apontando para o Github. Podemos efetuar o *push* com nossas alterações::

    $ git push -u origin master

Isso fará com que o nosso commit feito anteriormente sejá enviado para o Github. É possível trabalhar com vários repositórios remotos devido ao Git trabalhar de forma distribuida, veja mais sobre em **Repositórios remotos (remotes)**.

Baixando repositórios
----------------------

Utilize o comando ``git clone [URL/Caminho do repositório]`` para baixar localmente todo o repositório remoto. Exemplo::

    $ git clone git@github.com:alissonperez/git-resumo.git

O comando acima irá baixar o repositório todo desta documentação dentro do diretório *git-resumo*. Você estará pronto para alterar arquivos, adicionar para commit (em *staging*) e efetuar seus *commits* e *pushes*.

Verificando diferenças
--------------------------

Após efetuar alterações, queremos ver o que exatamente foi alterado. Com o ``git diff`` é possível ver o que está diferente no *working directory* mas não em *staging* para commit (veremos sobre *staging* no próximo tópico)::

    $ git diff

Para verificar as diferenças do que está em *staging* para ser comitado, use::

    $ git diff --staged

Ou::

    $ git diff --cached

OBS.: *staged* e *cached* são sinonimos.

Adicionando alterações para commit
-------------------------------------

No Git há um local intermediário entre o diretório local e o que está "commitado". Isso é chamado de *stage* e apenas arquivos neste local podem ir em um commit (pode-se usar o parâmetro ``-a`` ao commitar para pular este local e enviar no commit todos os arquivos alterados, veremos adiante).

Após efetuar as alterações, execute o comando ``git status`` para verificar o que está em *staging* (para ir no próximo commit) e o que esta no *working directory*.

Para adicionar arquivos alterados no próximo commit, use o comando ``git add``. Exemplo::

    $ git status
    On branch master
    Untracked files:
      (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)

    $ git add README
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        new file:   README

No comando acima, após executar o comando ``git status`` verificamos que o arquivo README estava foram do *staging* (em *untracked files*). Executamos em seguida o comando ``git add README`` para adicionar o arquivo em *staging*. Podemos ver que o arquivo está pronto para commit com o último commando ``git status``, onde o README aparece em *staging*.

Isso é muito útil quando queremos determinar exatamente o que subir separando os commits por assunto ou funcionalidade, por exemplo.

Comitando alterações
---------------------

Use o comando ``git commit`` para comitar as alterações em *staging*. Será aberto o editor configurado para informar a mensagem do commit.

Pode-se passar o argumento ``-v`` para que o diff das alterações seja adicionado ao editor no momento de informar a mensagem::

    $ git commit -v

Removendo arquivos do Git
----------------------------

Use o comando ``git rm`` para remover o arquivo do controle de versão.

Para remover o arquivo apenas do controle de versão e mante-lo em seu HD, use o parametro ``--cached``::

    $ git rm --cached README

Dessa forma, o arquivo README será removido do git mas ainda continuará em seu HD. Isso é útil quando um arquivo deveria ter sido adicionado ao ``.gitignore`` mas foi acidentalmente adicionado ao git.

É possível utilizar *coringas* para remover arquivos e diretórios, utilize ``\`` para isso::

    $ git rm log/\*.log


Movendo arquivos
--------------------

O git possui o comando ``git mv`` para renomear/mover arquivos::

    $ git mv file_from file_to

Porém, isso não é nada mais que::

    $ mv README.md README
    $ git rm README.md
    $ git add README

O git irá interpretar isso como uma renomeação de arquivo. Ambos os comando são equivalentes.

Visualizando histórico de commits
-----------------------------------

Use o comando ``git log`` para ver o histórico de commits com mensagens, autor, SHA-1 hash e data do commit.

Para ver as alterações no log do commit, utilize o parâmetro ``-p``, exemplo::

    $ git log -p

Pode-se também informar um número negativo para limitar o número de commits de retorno do comando, exemplo::

    $ git log -p -1

Para o comando acima, serão exibidas todas as informações do **último** commit, incluindo as alterações nos arquivos comitados.

Use o parâmetro ``--stat`` para ver uma informação abreviada sobre os commits.

Use o parâmetro ``--pretty`` para alterar o formato de saída do log. Exemplos::

    # Mostra os commits em uma linha
    $ git log --pretty=oneline

Outros parâmetros para o parâmetro *pretty* são: *short*, *full* e *fuller*.

É possível também informar o formato de saída, isso é útil quando é necessário tratar a saída por outro programa, exemplo::

    $ git log --pretty=format:"%h - %an, %ar : %s"
    ca82a6d - Scott Chacon, 6 years ago : changed the version number
    085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
    a11bef0 - Scott Chacon, 6 years ago : first commit

Veja mais sobre em http://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History

Use o parâmetro ``--graph`` em conjunto com ``--branches`` para exibir em ASCII os branchs e merges feitos no projeto.

Desfazendo alterações
-----------------------

É possível fazer algumas alterações no commit anterior, como a última mensagem ou adicionar algum arquivo.

Para alterar a mensagem anterior, execute o seguinte commando logo após o commit com a mensagem errada::

    $ git commit --amend

Será aberto o editor para preenchimento da mensagem com a anterior preenchida. Faça a alteração e ao gravar a mensagem do commit anterior será substituida pela nova.

É possível também adicionar arquivos ao commit anterior por meio do parâmetro ``--amend``. Exemplo::

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend

Use ``git reset HEAD <file>...`` para remover um arquivo do *staging*.

use ``git checkout -- [file]`` para desfazer as alterações feitas em determinado arquivo. **Cuidado, com esse comando suas alterações serão desfeitas e não poderão ser recuperadas.**

**POR SEGURANÇA, evite fazer alterações no commit anterior caso já tenha feito push para um branch remoto.**

Branches
----------------------------

Há várias formas de uso para *branches*, pode-se dizer que utilizamos basicamente para separar caminhos de desenvolvimento, seja para um novo recurso ou feature, separar ambientes de desenvolvimento e produção (*stable*) ou simplesmente corrigir um bug. No Git branches são extremamente úteis e muito "baratos" de fazer e remover.

Antes de explicar como funcionam branches, precisamos mostrar como o Git trabalha com *commits*.

Tudo no Git gira em torno dos *commits*. Cada *commit* possui um ponteiro para um snapshot completo do repositório. Onde os arquivos alterados são armazenados por completo e os que não foram alterados são apontados para a versão anterior por meio de um link simbólico.

Com o *commit* também são guardadas algumas informações, como o nome e e-mail do autor, mensagem digitada e um ponteiro para o *commit* feito anteriormente (um ponteiro para *commits* normais, um ou mais ponteiros para *commits* resultantes de merge entre branches, e nenhum ponteiro para o primeiro *commit* do repositório).

A ilustraração que segue demonstra uma sequencia de commits (o mais recente à direita) com os ponteiros para os anteriores (cada letra representa um commit)::

    A---B---C---D

Um *branch* nada mais é que um ponteiro para um determinado *commit* dessa linha::

              master
                |
    D---E---F---G

Neste caso, o branch master está apontando para o commit **G**. Podemos agora criar um novo branch chamado **bug42**, com o intuito de resolver um bug encontrado no projeto::

    $ git branch bug42

Com o comando acima, será criado um novo branch (um novo ponteiro apontando para o mesmo commit do branch atual) resultando no seguinte::

              master
                |
    D---E---F---G
                |
              bug42

Vamos agora trocar para o novo *branch*, para isso utilizamos o comando ``git checkout``::

    $ git checkout bug42

Agora estamos no branch *bug42*, qualquer alteração que fizermos aqui não terá qualquer influência no branch *master*. Vamos então alterar alguns arquivos e fazer um commit::

    $ vim footer.inc.php
    $ git add footer.inc.php
    $ git commit -m "Corrigido e-mail no rodapé da aplicação"

Nosso histórico de *commits* agora será::

              master
                |
    D---E---F---G---H
                    |
                  bug42

Note que o branch *master* se manteve no commit **G** enquanto o branch *bug42* acompanhou o nosso novo *commit* **H**.

Após efetuar os testes necessários, desejamos juntar nossa alteração no branch *master*, para isso, devemos retornar ao branch *master* e utilizar o comando ``git merge``::

    $ git checkout master
    $ git merge bug42

Resultando em::

                  master
                    |
    D---E---F---G---H
                    |
                  bug42

Note que o *branch* master apenas avançou para o commit **H**, sendo apontado pelo branch *bug42*. Esse avanço recebe o nome de **fast-forward**, pois não houve nenhuma divergencia na "história" dos branches (isso ocorreria se alguém fizesse um commit no branch master enquanto já comitamos algo no branch *bug42*, veremos mais adiante o que fazemos nessa situação) e foi necessário apenas atualizar o ponteiro do branch master para o commit seguinte.

Agora podemos remover o branch *bug42*::

    $ git branch -d bug42

E o nosso histórico de commits será::

                  master
                    |
    D---E---F---G---H

Branches são extremamente baratos, internamente são apenas arquivos com 41 bytes (40 caracteres e uma quebra de linha). Dessa forma, podemos usar branches para qualquer desenvolvimento novo, por mais simples que seja, e trabalharmos nos branches principais apenas efetuando *merges*.

Merge entre branches "divergentes"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Em um ambientes com mais colaboradores, branches podem seguir caminhos distintos com muita facilidade e em algum momento futuro precisaremos juntá-los. Para demonstrar como isso é feito, vamos voltar a um exemplo mostrado anteriormente::

              master
                |
    D---E---F---G---H
                    |
                  bug42

Vamos supor agora que o branch master recebe um novo commit. A situação seria a seguinte::

                  master
                    |
    D---E---F---G---I
                 \
                  --H
                    |
                  bug42

Note que o branch master "andou" para o commit **I** enquanto nosso branch **bug42** está apontando para o commit **H** e não possui as alterações feitas no commit **I** do branch master. Neste momento os dois branches estão divergentes e será necessário um novo commit para juntar os dois (ao fazer o *merge*).

Após terminarmos nossas alterações no branch bug42 e efetuarmos os testes necessários queremos juntá-las ao branch master. Como vimos anteriormente, vamos utilizar o comando ``git merge``::

    $ git checkout master
    $ git merge bug42

Agora o histórico de commits será o seguinte::

                      master
                        |
    D---E---F---G---I---J
                 \     /
                  --H--
                    |
                  bug42

Veja que o comando merge criou o commit **J** para juntar os dois branches com as alterações que estavam presentes em cada um (commit **I** e commit **H**).

É importante notar que, caso haja algum conflito (edições na mesma linha de um arquivo em ambos os branches, por exemplo), pode ser que o Git informe do conflito e solicite que este seja corrigido manualmente caso ele não consiga resolve-los automaticamente.

Mais adiante veremos uma outra forma de tratar divergências entre branchs utilizando o comando ``rebase``.

Repositórios remotos (remotes)
---------------------------------------------

**Remotes** são repositórios remotos onde é feito **pull** (obtenção das alterações) e **push** (envio das alterações). Por exemplo, o *Github* é um repositório remoto onde fazemos pull e push.

Utilize o comando ``git remote`` para exibir os repositórios remotos conhecidos. Use o parâmetro ``-v`` para exibir a URL do repositório, exemplo::

    $ git remote -v
    origin  https://github.com/schacon/ticgit (fetch)
    origin  https://github.com/schacon/ticgit (push)

Por padrão, ao fazer *clone* de um repositorio, o git criará um remote com o nome *origin* apontando para esse *remote*.

Para adicionar novos *remotes*, use o comando ``git remote add [name] [URL]``.

Após adicionar o *remote*, é possível fazer ``fetch`` do conteúdo. Ao efetuar o ``fetch``, todo o conteúdo do repositório será baixado localmente e seus branchs estarão disponíveis para ``checkout`` (ou qualquer operação com *branchs*, com **exceção de pushes**).

O nome do branch baixado do *remote* segue a seguinte regra: Caso o *remote* tenha o nome de *pb*, o branch *master* de *pb* está disponivel com o nome *pb/master*.

É importante notar que o comando ``git fetch [nome]`` apenas baixa os arquivos localmente mas **não faz merge** com as suas alterações locais. Para que o merge aconteça automaticamente, utiliza ``git pull [nome]``.

Para enviar as alterações (fazer *push*), utilize o comando ``git push [remote-name] [branch-name]``.

Alguns comandos úteis para *remotes*:

- Mostrar informações: ``git remote show [nome]``
- Renomear (localmente): ``git remote rename [nome atual] [novo nome]``
- Remover: ``git remote remove [nome]``

Trabalhando com branches remotos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Conforme dito anteriormente, branchs remotos são copias locais para os branchs na última vez que foram conectados (feito *fetch*). É permitido alterar os arquivos e efetuar commits, mas é **não** é possível efetuar *push* de qualquer informação. Ĉaso tenha alterado algo em um branch remoto, quando um checkout para outro branch for feito, todas essas alterações serão perdidas.

O branch remoto segue a seguinte nomenclatura **nome-remoto/nome-do-branch**. Exemplo: Ao fazer checkout para o branch *origin/master* (comando ``git checkout origin/master``) você estará vendo o branch master como é no repositório remoto *origin* deste o último ``fetch`` com este remoto. Você poderá efetuar alterações neste branch e comita-las, mas **não** efetuar push.

Vejamos outro exemplo: Supondo que você estra trabalhando em um projeto hospedado no Github. Quando você efetuar o ``clone`` o git irá baixar todos os dados localmente, adicionará o *remote* **origin** como referência ao repositório original no Githug e você estará no branch *master* automaticamente. Após alguns commits você deseja verificar como estava o branch master no Github após sua última conexão (último ``fetch`` feito), você então faz o checkout para *origin/master* e consulta o que deseja, caso ninguém tenha feito commit algum neste meio tempo (entre seu ``fetch`` e o ``checkout``), este branch representará exatamente o que está no Github. Após algumas alterações e commits para teste, você deseja retornar ao branch **master** (onde estava trabalhando), ao efetuar o checkout para o *master* as alterações no branch *origin/master* serão todas perdidas.

Caso alguem faça um commit no branch **master** no *remote* **origin**, o seu branch **origin/master** estará desatualizado. Para obter as alterações para o remoto *origin* é necessário efetuar um *fetch* com o comando ``git fetch origin`` (onde *origin* é o nome do *remote*). Isso fará com que as alterações feitas no *remote* sejam baixadas para o seu branch *origin/master* localmente.

Efetuando push
^^^^^^^^^^^^^^^^^^^

Usamos o comando ``git push`` para enviar nossas alterações para os repositórios remotos. É possível efetuar push para o um branch específico, por exemplo: O seguinte comando ``git push origin iss53`` faz o push do branch local *iss53* para o repositório remoto *origin* no branch *iss53*. Caso este branch não exista no remoto, será criado.

É possível também informar qual o nome para o branch remoto, isso pode ser feito da seguinte forma: ``git push origin branch-local:branch-remoto``, esse comando significa: "Pegue o meu branch local chamado *branch-local* e envei para o branch *branch-remoto* no repositório remoto *origin*."


Tracking branchs
^^^^^^^^^^^^^^^^^^^^^^^

Para poder fazer push em branches remotos, é necessário criar um branch local com tracking para o branch remoto desejado, esse branch local é chamado *tracking branch* ou *upstream branch*. Isso já ocorre ao fazer *clone* de um repositório, o branch *master* é um *tracking branch* com relação direta para o branch remoto *origin/master*. Com isso, ao fazer *pull* e *push* do branch master o git já sabe qual remoto deve usar para baixar ou enviar as informações.

Vejamos um exemplo: Supondo que você tem um branch remoto chamado *remoto/iss53* e quer fazer alterações e efetuar o *push* em seguida. Para que o *push* seja possível, é preciso criar um branch local com base no branch remoto (*tracking branch*), isso é feito por meio do seguinte comando::

    $ git checkout -b iss53 remoto/iss52

Ou::

    $ git checkout --track remoto/iss52

Isso criará um branch local com *tracking* para o branch remoto *remoto/iss53* possibilitando efetuar pushes (caso você tenha permissão de escrita no *remote*).

Também é possível colocar um branch em tracking manualmente com o seguinte comando: ``git branch -f iss53 -t remoto/iss53``, aqui é usado o parâmetro ``-f`` para forçar a atualização do branch já existente.

Para ver os branchs e com quais remotos cada um faz *tracking*, use o comando `git branch -vv`. Exemplo::

    $ git branch -vv
      iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
      master    1ae2a45 [origin/master] deploying index fix
    * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
      testing   5ea463a trying something new

No exemplo acima, o *tracking branch* **iss53** está ligado ao branch remoto **origin/iss53** e está à frente 2 commits (há dois commits pendentes para *push*); O *tracking branch* **master** está atualizado em relação ao remoto; O *tracking branch* **serverfix** está ligado ao branch remoto **teamone/server-fix-good** (repare que *tracking branches* não necessáriamente precisam ter o mesmo nome do branch remoto) com 3 commits pendentes para *push* e desatualizado em 1 commit (necessário fazer *pull*); O último branch **testing** não está ligado a nenhum branch e é, portanto, um branch normal.

Para alterar o *tracking* de um branch local, utilize o comando `git branch -u origin/serverfix` (ou dependendo da versão ``git branch teste --set-upstream origin/master``)

Efetuando pull
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Utilizamos ``git fetch`` para atualizar os branches remotos, em seguida geralmente utilizamos ``git merge`` para juntar as alterações remotas com as locais (veremos adiante o uso também de ``rebase``). Porém, podemos utilizar o comando ``git pull`` para efetuar ambos os comandos de uma única vez (``fetch`` e ``merge``). Ao executá-lo, o Git identificará o repositório remoto, os commits serão baixados e o merge será feito em seguida. Caso o branch não seja um *tracking branch*, será solicitado o repositório e branch de origem para efetuar o pull.

Removendo branches remotos
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Caso seja necessário remover um branch remoto, utilize o seguinte comando::

    $ git push origin --delete serverfix

No comando acima, é removido o branch *serverfix* do remoto *origin*.

Dica: Caso esteja usando uma URL HTTPS com autenticação para efetuar os *pushes*, use o comando ``git config --global credential.helper cache`` para armazenar os dados de acesso (usuário e senha) do repositório.

Branches - Rebasing
---------------------

No Git, há duas formas de juntar alterações em branchs diferentes: **merge** e **rebase**.

Merge foi visto anteriormente, ele junta as alterações de dois branchs em um novo commit. É a maneira mais fácil para juntar as alterações em dois branches diferentes.

Porém, há um outro jeito. Imagine que você está trabalhando em um branch chamado *develop* e em outro chamado *master*, ambos já receberam commits desde a criação do branch *develop* (ou seja, ambos já seguiram dois caminhos distintos) e você deseja que as alterações do branch *master* sejam aplicadas ao branch *develop* **antes** das alterações feitas neste mesmo branch. Com o comando ``git rebase [branch]`` os commits do branch informado serão aplicados em ordem cronológica ao branch atual.

Exemplo de uso::

    $ git checkout develop
    $ git rebase master

O codigo acima faz com que o branch *develop* receba os commits do branch *master*, atualizando-o. Dessa forma, ao efetuar um merge do branch *master* com o branch *develop* será feito apenas um *avanço rápido* (fast-forward) do branch *master* para apontar para o commit do branch *develop*::

    $ git checkout master
    $ git merge experiment

Outra vantagem do *rebase* é o histórico de alterações mais limpo, mais linear, fazendo com que as alterações tenham ocorrido em série, mesmo que originalmente tendo ocorrido em paralelo. Evita-se assim commits apenas para *merge* entre branches

**IMPORTANTE: Use rebase apenas em commits dentro do seu repositório local (commits locais, que ainda não foram enviados para um local remoto via PUSH). Caso não siga esta regra, todo o histórico de outras pessoas ficará confuso e bagunçado. SEMPRE SIGA ESTA REGRA!**

No caso de rebase no repositório remoto, é recomendável que não se faça ``pull`` diretamente. Ao invés disso, faça ``fetch`` e em seguida um ``rebase``. Ex.::

    $ git fetch
    $ git rebase remoto/master

Isso evitará que sejam criados commits de merge localmente e que os commits que foram alterados remotamente não sejam reinseridos ao efetuar push.

Pode-se também efetuar ``git pull --rebase``, terá o mesmo efeito do código anterior.

Como dito anteriormente, é altamente recomendável usar rebase apenas para reorganizar seus commits localmente, devemos evitar ao máximo fazer rebase em commits que já foram enviados para remotos. Se necessário, certifique-se que todos farão pull usando o parâmetro ``--rebase``.

Stashing
===========

Há momentos onde é necessário mudar de branch sem ter terminado o trabalho atual e não queremos fazer um commit pela metade. É possível guardar o status atual dos arquivos no branch limpando todo *working directory*. Para isso, usamos o comando ``git stash``.

Exemplo:

Supondo que temos a seguinte situação::

    $ git status
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

        modified:   index.html

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   lib/simplegit.rb

Precisamos alterar de branch mas não queremos comitar nada. Neste caso, executamos o comando ``git stash`` ou ``git stash save`` para guardar o status atual na pilha::

    $ git stash
    Saved working directory and index state \
      "WIP on master: 049d078 added the index file"
    HEAD is now at 049d078 added the index file
    (To restore them type "git stash apply")


Agora nosso diretório de trabalho está com o status limpo e podemos mudar de diretório::

    $ git status
    # On branch master
    nothing to commit, working directory clean


Após o trabalho feito em outro branch, podemos voltar para este branch e recuperar o status anterior com o comando ``git stash apply``. Caso queira também recuperar os arquivos que estavam em *stage* (adicionados para commit), adicione o parâmetro ``--index``::

    $ git stash apply --index
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #      modified:   index.html
    #
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #
    #      modified:   lib/simplegit.rb
    #

É possível também limpar os arquivos do *working directory* mas manter o que está em stage (adicionado para commit). Para isso, utilize o parâmetro ``--keep-index``. Veja um exemplo::

    $ git status -s
    M  index.html
     M lib/simplegit.rb

    $ git stash --keep-index
    Saved working directory and index state WIP on master: 1b65b17 added the index file
    HEAD is now at 1b65b17 added the index file

    $ git status -s
    M  index.html


Caso queira listar o que está na pilha, use o comando ``git stash list``::

    $ git stash list
    stash@{0}: WIP on master: 049d078 added the index file
    stash@{1}: WIP on master: c264051 Revert "added file_size"
    stash@{2}: WIP on master: 21d80a5 added number to log


Referências
--------------

Feito com base no estudo da documentação do site oficial: http://git-scm.com/book/en/v2/Getting-Started-About-Version-Control
