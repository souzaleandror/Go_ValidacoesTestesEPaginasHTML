#### 04/10/2023

Curso de Go: validações, testes e páginas HTML

```
export GO111MODULE="on" 

Error = package github.com/lib/pq is not a main package
go env -w GO111MODULE=off
go get github.com/lib/pq
go mod init

lsof -i:8080
Kill -9

react-scripts --openssl-legacy-provider start
go get github.com/stretchr/testify
```

@01-Instalando e criando a primeira rota com Gin

@@01
Apresentação

[00:00] Olá, meu nome é Guilherme Lima. Boas-vindas a esse treinamento de Gin. Neste curso vamos aprender a fazer coisas muito legais e úteis para o nosso dia a dia como desenvolvedores. O que vamos fazer?
[00:13] Vamos aprender a criar uma forma de validar os nossos campos, os nossos alunos. Você tem uma struct, você precisará validar determinados campos, precisa ter X caracteres, não podem ser caracteres letras, só números, ou pode ser só números, não podem ser letras... Vamos aprender a fazer tudo isso neste curso e vai ser muito legal.

[00:31] Além disso, vamos focar grande parte deste treinamento escrevendo testes para configurar as nossas rotas, garantindo o comportamento que esperamos. Teremos rotas de teste, vamos criar alunos mocks, vamos testar os principais endpoints dessa aplicação, que vamos utilizar como base. Para finalizar com chave de ouro, o que vamos aprender?

[00:54] Renderizar páginas com o Gin. O Gin também é capaz de renderizar arquivos HTML utilizando estilização de CSS. Nós não vamos criar HTML e CSS na mão, vamos ver como estilizar isso e como fazer coisas incríveis, como alterar a nossa página 404 para que ela seja toda configurada e exiba a mensagem que esperamos.

[01:16] Este treinamento está incrível, realmente está muito legal. Espero que você goste do curso, que você aprenda bastante e te convido para começar esse desafio já. Vamos lá?

@@02
Preparando o ambiente

Olá!
É muito bom receber você neste curso de Gin.

Espero que seja uma experiência de aprendizado incrível e que possamos vencer todos os desafios juntos. Neste curso, vamos aprofundar nosso conhecimento utilizando Gin, criando validações dos dados, testes e como podemos renderizar páginas com este incrível framework.

Preparando ambiente
Para conseguir acompanhar este curso, é recomendado que você tenha o Go instalado.
Neste treinamento, teremos um projeto base que vamos aprofundar nossos estudos nele. Você pode realizar o download do projeto base neste link.

Usarei o VSCode para editar o código e para tornar o desenvolvimento ainda mais fácil, recomendo a instalação da extensão do Go desenvolvida pelo time da Google.

Para acompanhar esta aula, é recomendado que você tenha o Docker instalado pois o banco de dados de produção será executado no Docker.

Caso não tenha o Docker instalado e precise de ajuda:

Instalando o Docker no Windows
Instalando o Docker no Mac
Não se preocupe em executar ou subir o projeto base, isso será mostrado na!

A Alura é formada por pessoas que gostam de tecnologia e acreditam no poder da educação através dela. Somos uma comunidade que ama compartilhar conhecimento. Em caso de dúvida na instalação ou durante o curso, conte sempre com o fórum. Caso não tenha dúvidas, não deixe de participar do fórum para ajudar outras pessoas e fazer da comunidade um lugar ainda melhor! :)

https://golang.org/

https://github.com/alura-cursos/api_rest_gin_go/archive/refs/heads/aula_5.zip

https://code.visualstudio.com/

https://marketplace.visualstudio.com/items?itemName=golang.go

https://www.docker.com/products/docker-desktop

https://cursos.alura.com.br/course/docker-e-docker-compose/task/29235

https://cursos.alura.com.br/course/docker-e-docker-compose/task/29237

@@03
Carregando o projeto inicial

[00:00] Vamos iniciar os nossos estudos com o Gin? Neste curso nós não vamos criar um projeto do zero, nós vamos utilizar um projeto base e vamos desenvolver essa aplicação. Eu estou com esse projeto, já fiz o download dele aqui, já abri no Visual Studio Code. Toda a descrição do passo a passo de como você faz para baixar o projeto, subir o projeto no Docker, está na atividade “Preparando o Ambiente”.
[00:21] Então o que temos neste curso? Temos um banco de dados rodando no Docker e uma API Rest feita com Gin. Vou mostrar aqui alguns endpoints e vou subir essa aplicação para vermos. É um CRUD de alunos e temos algumas funcionalidades como: buscar um ID por aluno, buscar um aluno por CPF, isso ficou bem legal.

[00:43] O que eu quero fazer agora é desenvolver essa aplicação e realizar alguns testes junto com vocês, algumas validações. Para subirmos essa aplicação, eu vou rodar o comando do Docker, docker-compose build. Quando der um "Enter", se você estiver rodando a primeira vez, ele vai começar a criar toda a imagem do Docker.

[01:00] Lembrando que o meu Docker já está rodando aqui, então deixe o Docker rodar e faça esse comando docker-compose build. Depois que o build da imagem for finalizado, é só você rodar o comando docker-compose up e ele vai carregar a base de dados que temos para esse treinamento.

[01:19] Ele está carregando aqui a base de dados. Feito isso, nós precisamos deixar o Docker habilitado durante todo o nosso treinamento, mesmo utilizando o banco de dados no Docker, esse banco de dados é o banco de dados de desenvolvimento, não é o banco de dados de produção. Ele está carregando aqui, já subiu. Eu vou clicar no símbolo de mais (+) do menu do terminal para abrir mais um terminal.

[01:40] Eu vou carregar agora, vou subir o meu servidor do Go, go run main.go, quando pressionar "Enter" ele carrega aqui o servidor. Eu posso já dar uma requisição - lembrando que eu estou na porta 8080.

[01:54] Então no Postman, por exemplo, se eu passo um nome, vou passar aqui http://localhost:8080/gui, quando eu der um "Send", ele tem uma mensagem da API, que estávamos aprendendo como colocar, a pegar essa informação da requisição e colocar em uma mensagem.

[02:08] Então a API dá uma mensagem: "E aí, Gui, Tudo beleza?". Se eu coloco http://localhost:8080/alunos e dou um "Get", ele vai trazer a lista de todos os alunos para mim.

[02:16] O resto já temos normal, o get, o post para eu criar um novo aluno, delete para eu deletar, patch para eu editar, e assim por diante. Mas uma coisa interessante que eu quero mostrar para vocês é isso aqui, olha só, se eu faço um "Post" para um aluno - o post, eu quero criar um novo aluno na minha base de dados.

[02:33] Só que o nome eu vou deixar em branco, o CPF eu vou deixar em branco, o RG eu vou deixar em branco e o CPF eu vou deixar em branco. Eu dou um "Send", olha que interessante, ele criou um novo aluno com todos esses campos em branco.

[02:43] Não é o comportamento que eu quero. Eu quero que o nome nunca fique em branco, eu quero que o RG tenha no mínimo a quantidade de caracteres que tem um RG - no Brasil são 9 - e um CPF que tenha 11 dígitos. É isso o que vamos atacar no próximo vídeo.

@@04
Criando as validações

[00:00] Vamos descobrir, neste vídeo, como fazemos para conseguir validar alguns campos na nossa aplicação, porque nome, RG e CPF em branco não fazem sentido. Vou pesquisar no Google um pacote específico para realizar validações: "validator V2 golang".
[00:20] Nesse primeiro link, "Package validator", eu vou clicar nele. Aqui embaixo eu tenho para instalarmos, para trazermos esse pacote, colocarmos ele no nosso projeto, é só rodar esse comando go get gopkg.in/validator.v2.

[00:32] Eu já vou fazer isso agora. Parei o meu servidor do Go, vou rodar aqui, "Ctrl + V", go get gopkg.in/validator.v2. Ele já foi adicionado na nossa aplicação. O que vamos fazer? Precisamos importar esse pacote onde vamos utilizar. Onde queremos utilizar esse pacote? Queremos utilizar ele no nosso modelo. Por quê?

[00:52] É no nosso modelo que descrevemos: teremos o nome, RG e o CPF e eu quero que esses campos sejam validados. O que eu vou fazer? Eu vou copiar essa linha do import da validação "gopkg.in/validator.v2" e vou colocar ele aqui no import de "alunos.go".

[01:05] Ele vai reclamar porque não estamos usando, mas já vamos utilizá-lo. Então fiz o import do modelo e instalei já o pacote. Agora vamos ver como fazemos para conseguirmos de fato validar.

[01:20] Ele tem uma estrutura, nós conseguimos validar de algumas formas. Nós podemos falar: o name não pode ser em branco, esse nonzero. Quando vamos na descrição dele, para falar o que o acontece com esse nonzero, ele fala alguns pontos interessantes, olha só.

[01:36] Para inteiro, o 0 será considerado nonzero, então ele vai mostrar um erro. Se a string estiver em branco também vai ativar um erro e, se o ponteiro for nil, ele vai falar que esse campo aqui não pode ser em branco. Se for um ponteiro não pode ser nil, se for string não pode ser vazia e se for um inteiro não pode ser 0.

[01:57] Então essa é a ideia. Vou até passar o mouse só para lermos o que ele fala. Para int é 0, para string é isso - isso ele considera um valor vazio, um nonzero. Então o que eu vou fazer? Eu vou copiar essa linha `"validate:"nonzero"` .

[02:21] Vamos no nosso código. No nome aqui, tem o nome, vou dar um espaço, porque esse acento agudo está fechando o nome do JSON, e vou dar um "Ctrl + V": `json:"nome" "validate:"nonzero"`. Vamos ver outras coisas que podemos fazer para validar também os nossos campos.

[02:35] O RG e o CPF, o que eu tinha falado no vídeo anterior? Eu não vou fazer a validação a fundo para saber se o RG é um valor válido. Não, eu quero saber se todos os campos que são inseridos, eles possuem a quantidade mínima de valores. Eu não posso registrar um RG com 123, por exemplo, igual eu fazia nos meus testes, nos primeiros testes que estávamos realizando.

[02:57] Então o que eu vou fazer? Eu vou ver aqui, na documentação, como podemos validar. Nós podemos utilizar expressão regular, o regexp.

[03:06] Tem uma outra forma também, que é esse primeiro, o builtin validators. Ou seja, esse pacote já vem com algumas validações para ele, que é o len. Ele fala: para tipos numéricos, vamos verificar se é igual ao valor que foi passado como parâmetro.

[03:19] Aqui eu falo, por exemplo, len é igual a um determinado valor e ele vai falar para mim: esse campo não está válido, porque ele precisa deste valor aqui. Então o que eu vou fazer? Eu vou colocar um len para RG igual a 9 e um len para o CPF igual a 11. Vamos para o nosso código para fazermos isso também.

[03:40] Vamos utilizar a mesma propriedade, o validate, validate:, eu vou passar agora, entre aspas duplas, validate:"len=9" para o RG. E vou fazer a mesma coisa para o CPF, só que, no lugar de 9, eu vou passar 11, validate:"len=11".

[03:57] Só isso já vai validar? Não, nós colocamos nos nossos campos, mas precisamos de uma função que vai, de fato, verificar se esse conteúdo que nós passamos é válido ou não. Então, o que eu vou fazer? Eu vou criar uma função, um método que eu vou chamar de func ValidaDadosDeAluno(), no singular. Vou passar aqui o nosso aluno, (aluno *Aluno), apontando para o nosso aluno.

[04:28] O A maiúsculo aqui, porque estamos apontando para essa nossa estrutura. E vou retornar uma mensagem de erro, vou retornar um erro. Vamos supor: se o nosso aluno tiver algum campo que está incorreto - o nome é inválido, o RG e CPF não contém a quantidade exata de caracteres - o que eu quero fazer? Eu quero retornar essa mensagem de erro, error.

[04:48] Eu vou falar assim: error { if err :=, se o erro do validator - aqui um ponto muito importante, pessoal. Reparem que aparece aqui, validator está o meu "gopkg.in/validator.v2".

[05:03] Não é o validator do playground, é o validator do V2. Deixa eu até tirar aqui para ver se ele mostra o validator do playground. Não, não mostra. Sempre é esse aqui, o "gopkg.in/validator.v2". Vou clicar nesse validator, que é ele que nós importamos, ponto, eu quero que ele valide, validate, o meu aluno, o aluno, a instância que estamos utilizando naquele momento.

[05:25] Eu vou verificar, se o erro for diferente de nil, if err := validator.Validate(aluno); err != nill {}, eu quero retornar um erro, return err, temos de fato um erro em algum desses campos, retorne um erro que nós temos. Agora, se não tivermos nenhum erro, não vamos retornar nada, vamos retornar um return nil.

[05:52] Vou salvar esse código, vamos ver se está tudo certo aqui. Return, faltou o "R" aqui. Agora sim, return, agora está tudo certo. Nós já temos esses campos aqui, aqui depois de (aluno) é um ponto e vírgula, if err := validator.Validate(aluno); err != nill. Agora sim.

[06:06] Agora, aqui, nós temos o nosso modelo com essa validação. O que vamos fazer na sequência? Vamos aplicar essas validações, tanto quando vamos criar um aluno como quando vamos editar um aluno.

@@05
Aplicando as validações

[00:00] Agora que já criamos as nossas validações no nosso modelo, o que eu quero fazer é aplicar essas validações no nosso controle. Vamos fazer isso então? A primeira coisa que precisaremos fazer é ir no nosso controller. Vou no Visual Studio Code, vou abrir o nosso "controller.go". Quando criamos um novo aluno, olha o que fazemos.
[00:19] Temos uma instância de um aluno com base no nosso modelo. Pegamos todo o corpo da requisição e empacotamos dentro desse aluno, depois já salvamos no banco de dados e damos uma mensagem de status ok. Não é o que eu quero fazer, eu quero de fato validar esse aluno antes de mandar esses dados.

[00:36] Então a primeira coisa que eu vou fazer, eu vou verificar se o erro for igual ao que temos no nosso modelo, if err := models.ValidaDadosDeAluno(&aluno);, passando quem? O endereço de memória desse aluno, para o nosso corpo da requisição, e vamos fazer essa verificação.

[00:56] Se tivermos algum dado, vou colocar aqui se o erro não for igual a nil, err != nil, o que eu quero fazer é retornar esse erro. E tem algo interessante, esse erro que vamos retornar, ele vai acontecer da mesma forma dessa mensagem aqui de cima.

[01:15] Vamos indicar que foi uma requisição ruim, bad request e vamos retornar essa mensagem de erro. Vou copiar e colar exatamente esse código aqui. Caso não tenhamos nenhum erro, o que acontece? Salvamos no banco de dados e damos o JSON ok.

[01:30] Então essas linhas do erro eu vou copiar, deixa eu ver, 1, 2, 3, 4, 5 linhas, fechando nas chaves, e vou colocar também no momento em que formos editar um aluno. Aqui nós buscamos o aluno, depois o que eu quero fazer é verificar: valide esse aluno também.

[01:50] Então tanto na nossa função que cria como na nossa função que edita, eu quero aplicar essas validações. O que eu vou fazer agora é: abrir o meu terminal, vou limpar a tela e vou dar um go run main.go, para vermos se de fato essas validações estão acontecendo. Ele carregou. Vou dar um "Get", só para visualizarmos.

[02:13] Temos o aluno 1, que está com o ID, o RG e o CPF inválidos, tem o aluno 2 e o aluno 12, o ID 12. Não se preocupe com o valor do ID, aqui foi realizando alguns testes, antes de gravarmos. Então aqui eu tenho o aluno 12, que está tudo errado. Os três alunos estão com os valores errados de RG e CPF, mas vamos começar com o momento em que vamos criar um novo aluno.

[02:36] Quando eu realizar uma requisição post, observe, ele falou: nome zero value, RG tamanho inválido e CPF tamanho inválido. Não criamos esse aluno. Se eu voltar no nosso banco e fizer um get, repare que temos até o aluno 12.

[02:52] O que eu vou fazer? Eu vou criar um novo aluno, só que agora eu vou colocar só o nome. Eu vou chamar aqui de "Murilo" e vou dar um "Send" para uma requisição post. Repare que a mensagem do nome já sumiu.

[03:06] Ela estava sendo exibida assim: nome zero value. Agora, quando eu coloco o Murilo, essa mensagem já sumiu. Está ficando bem mais legal, está ficando um pouco mais realista. O que eu vou fazer? Vou colocar "123" só, no RG. Quando eu dou um "Send", repare que ele continua com a mensagem de RG inválido.

[03:21] "12345678", continua com a mensagem inválida. Quando eu passar o 9, olha o que vai acontecer, que legal: vai ficar só uma mensagem de CPF inválido.

[03:30] Vou colocar aqui o CPF "12345678901", 11 dígitos. Quando eu dou um "Send" aqui, ele criou o aluno 13 com esses valores.

[03:41] Vamos testar agora o nosso método de editar? Editar é o nosso método "Patch", eu quero editar - deixa eu só lembrar, que eu nem lembro. Acabei de ver e eu não lembro. O ID 1 é o aluno Gui, o RG está errado e o CPF está errado.

[03:53] Então vamos lá. Vou dar uma requisição patch, para eu conseguir editar o aluno com o ID 1, http://localhost:8080/alunos/1. O nome é o Gui - deixa eu tirar aqui, eu quero editar só o RG e o CPF. Quando eu dou um "Send" aqui, ele editou. O nome do aluno 1 continua o mesmo, o RG e o CPF.

[04:11] Se eu quiser editar agora para um RG inválido, vamos ver? Ele fala que o RG não tem o tamanho válido. Mas errei, o último dígito não era o 9, era o 1, por exemplo, só para ter os 9 dígitos. Agora sim.

[04:23] Isso acontece também para o nosso aluno - eu não lembro qual é o ID da Ana, deixa eu lembrar aqui, só de teste, para praticarmos. ID 1 não, quero todos os alunos, ID 1, ID 3, que é o ID da Ana. Eu vou deletar já esse ID 12, para não ficarmos com esse alunos todo em branco. Deletado com sucesso. Eu quero o ID 3, só para visualizarmos os dados da Ana.

[04:50] Ela está com o RG e o CPF inválidos. Vou dar um patch com esse valor mesmo, depois testamos as outras coisas. Maravilha, deu certo.

[04:58] Só que aqui eu quero mostrar algo muito interessante para vocês. Da forma que fizemos parece: nossa, ficou incrível, realmente conseguimos ter 9 dígitos para o RG, 11 dígitos para o CPF e temos agora que o nome desse aluno não pode ser mais em branco, não pode ser mais vazio. “Puxa, Gui, ficou incrível, eu já vou colocar o meu projeto em produção”.

[05:17] Ainda não. Tem um passo muito interessante que eu quero mostrar para você ainda neste vídeo. Olha que interessante: o RG, se formos no nosso modelo, o modelo de aluno, repare que o RG, ele é uma string e o CPF é uma string.

[05:29] O que isso significa? Que podemos colocar letras nesses campos. Mas será que isso vai dar certo? Vamos ver, eu vou tirar o número 1 e o número 8 do RG e vou colocar o "1234567AB". Não faz sentido o RG ter "AB". A mesma coisa para o CPF, vou colocar "123456789CD". Repare, quando eu der um "Send" no "Patch" para atualizar a Ana, que estava só com valores numéricos, olhe o que vai acontecer. Ele vai deixar passar.

[05:55] Por quê? A nossa validação, ela fala assim: precisa ter 9 caracteres, não importa que caracteres são esses. Isso nós vamos corrigir no próximo vídeo. O que eu quero fazer é com que o RG e o CPF tenham, de fato, um tamanho de 9 e 11, respectivamente, mas que todos esses valores sejam números.

@@06
Regex

[00:00] Nós queremos que o campo CPF e o campo RG tenham apenas valores numéricos e não letras. Para isso, o que vamos fazer? Se olharmos na documentação mesmo do validator, repare que ele tem uma forma que é o regexp.
[00:12] Nós aplicamos uma validação do tamanho, o que eu posso fazer agora é colocar, aplicar uma validação do tipo regexp, para falar: todos esses campos precisam ser numéricos. Então o que eu vou fazer? Vou no nosso código, no nosso modelo, no campo do RG eu vou colocar que tem o validate:"len=9", vou colocar uma vírgula e vou dar um "Ctrl + V" daquele código que nós temos na nossa aplicação: regexp=^[a-zA-Z]*$ .

[00:37] Só que aqui será um pouco diferente. Por quê? Repare, essa validação do regexp que ele está fazendo, ele está utilizando letras, ele está falando: podem ser letras minúsculas ou letras maiúsculas. Não é o que eu quero, eu quero que esses campos tenham apenas valores numéricos. Para isso eu vou colocar [0-9]: regexp=^[0-9]*$.

[00:56] Ou seja, eu preciso ter, no RG, uma string que tenha o tamanho de 9 e que tenha apenas valores numéricos. Repare que ele está dando essa mensagem aqui por quê? Porque precisamos fechar a nossa parte da validação. Nós começamos o validate, vou tirar do "len=9" essas aspas duplas e vou colocar aqui no final, aspas duplas.

[01:21] Repare que agora não temos o erro. Ele fala: validate, abriu aspas, toda a nossa linha, a string de validação, tudo o que precisamos colocar de campo, nós colocamos separando com vírgulas cada validação, e colocamos o regex.

[01:35] Vou fazer o mesmo para o nosso CPF. Ele tem o tamanho de 11 e terá também - deixa eu só tirar essas aspas duplicadas. E terá também só valores numéricos, não queremos strings e letras neste campo.

[01:50] Limpando o meu terminal, fechei o meu servidor, vou rodar mais uma vez, go run main.go. Ele aplicou. O que eu vou fazer agora? Eu vou tentar realizar o mesmo patch e vou mudar a letra. No lugar de CD, eu vou chamar de CE: "123456789CE". Quando eu der um "Send", repare que ele vai dar uma mensagem de erro de expressão regular tanto no CPF como no RG.

[02:13] Então quer dizer que eu não posso colocar letras no RG e no CPF? Não, não pode. Se eu passar aqui o "123456789" no RG, dou um "Send" aqui, ele vai falar que tem um erro no CPF que está "123456789CE".

[02:27] Olha que legal isso, quando deixamos nos dois ele vai concatenando essas mensagens de erro, então não pode. Mas e se quiser deixar só uma letra mesmo, "890A" e deixo o "A" ali, não vai passar? Não vai passar, ele não vai deixar.

[02:43] Se eu passo apenas valores numéricos e dou um "Send", ele conseguiu atualizar aqui o valor da Ana. Mudou o CPF, não é 12, começa com "47", por exemplo, "473". Dou um "Send" aqui e a Ana foi atualizada.

[02:58] Dessa forma nós conseguimos validar os campos da nossa struct. Olha que interessante, nós aplicamos a validação que nós queremos, depois criamos uma função que aponta para o aluno que vamos receber. No nosso controle, falamos: “valide esse aluno que estamos tentando criar ou editar nesse momento”. Isso ficou bem legal.

@@07
Importância das validações

Uma aluna importou um pacote de validação e implementou diferentes formas para validar cada campo, como ilustra o código abaixo:
Nome string `json:"nome" validate:"nonzero"`
RG   string `json:"rg" validate:"len=9, regexp=^[0-9]*$"`COPIAR CÓDIGO
Analisando os trechos de código acima, podemos afirmar que:

O código validate:"nonzero" pode ser aplicado em tipos inteiros.
 
Alternativa correta! Podemos aplicar essa validação em tipos inteiros, onde é verificado se o valor não é zero.
Alternativa correta
O código validate:"nonzero" não pode ser aplicado em tipos string.
 
Alternativa correta
O código validate:"nonzero" não pode ser aplicado em tipos ponteiros.
 
Alternativa incorreta. Podemos, sim, aplicar a validação em ponteiros. Para ponteiros, o valor do ponteiro é usado para teste para diferente de zero, além do próprio ponteiro não ser nil.

@@08
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito isso, excelente. Se ainda não fez, é importante que você implemente o que foi visto no vídeo para poder continuar com a próxima aula, que tem como pré-requisito todo o código escrito até o momento.
Caso não encontre uma solução nas perguntas feitas por alunos e alunas deste curso, para comunicar erros e tirar dúvidas de forma eficaz, clique neste link e saiba como utilizar o fórum da Alura.

https://cursos.alura.com.br/comunicando-erros-e-tirando-duvidas-em-foruns-c19

@@09
O que aprendemos?

Nesta aula:
Carregamos o projeto base e criamos a imagem do banco de dados no Docker;
Criamos nossas validações na struct de Aluno, garantindo que um campo não fique em branco e tenha uma quantidade específica de caracteres;
Aplicamos essa validação no controller no momento que criamos ou editamos um aluno.
Na próxima aula:
Vamos mergulhar no mundo de testes com Go!

#### 05/10/2023

@02-Testes

@@01
Projeto da aula anterior

Aqui você pode baixar o zip da aula 01 ou acessar os arquivos no Github!

https://github.com/alura-cursos/api_rest_gin_go_2-validacoes-e-testes/archive/refs/heads/aula_1.zip

https://github.com/alura-cursos/api_rest_gin_go_2-validacoes-e-testes

@@02
Teste no Postman

[00:00] Agora vamos iniciar uma nova fase nos nossos treinamentos de Gin, que são os testes. O que acontece? Todo sistema de software, todos, sem exceção, ele está destinado a crescer e evoluir, seja em funcionalidade ou alterando funcionalidades que já existem.
[00:15] Pensando nisso, em um time em que trabalham várias pessoas no mesmo projeto, uma das formas que temos de garantir que a alteração em uma parte do código não vai interferir em outras partes do código é através de testes. Eu quero mostrar para vocês testes, inicialmente um teste muito simples, utilizando o Postman.

[00:32] Olha que interessante: na nossa aplicação, quando eu faço uma requisição "Get" para http://localhost:8080/gui e dou um "Send", a API retorna uma mensagem - ela já está configurada e retorna uma mensagem que diz: "API diz: E aí, gui, tudo beleza?"

[00:49] Temos várias informações que acontecem aqui. Nós verificamos o status code, podemos verificar o corpo do conteúdo, vários tipos de teste que podemos realizar. Eu quero mostrar para vocês alguns testes que realizamos utilizando o próprio Postman também. Se viermos no Postman, aqui embaixo do comando, "Params", "Authorization", "Headers", "Body", nós temos aqui uma aba de "Tests", antes de "Settings".

[01:11] Podemos verificar, por exemplo, se o status code da resposta é 200. Ele começa aqui, do lado direito, vou scrollar um pouco para baixo, "Status code: Code is 200".

[01:22] Quando eu dou um "Send", olha que interessante, ele me mostrou a mesma resposta, só que aqui, no "Test Results", na parte de baixo, ele passou 1/1, é 200. Porém, se eu faço assim, vou passar nada, http://localhost:8080 e dou um "Send", ele deu uma mensagem de erro. No corpo da requisição ele devolveu um 404. Mas, na mensagem de erro, ele falou: status code devia ser 200 e tem um erro de assertação.

[01:49] Para deixar esse código do erro no Postman, esse teste, ainda melhor, podemos falar Status code da requisição deve ser 200. Quando eu dou um "Send", ele fala exatamente dessa mesma forma.

[02:03] Esse teste, no Postman, é feito utilizando JavaScript, então pm.test, que é a instância do Postman, e ele faz aqui, cria uma função assíncrona e tal, e faz toda essa chamada. Nós podemos criar outros tipos de testes aqui também. Vou colocar o meu nome, http://localhost:8080/gui, só para recebermos um 200 e ficar verde, mostrando que o teste passou.

[02:24] Eu quero verificar, por exemplo, se o conteúdo, se esse conteúdo que aparece aqui - vou colocar aqui embaixo no "Body", que a API fala: "A API diz: E ai, gui, tudo beleza?" - é exatamente igual a esse valor.

[02:37] Então fiz a requisição, ele falou: olha, o que eu espero de body desta requisição é exatamente esse conteúdo. Tem aqui do lado direito, "Response body is equal to a string". Vou clicar nessa opção "Response body is equal to a string", vou tirar essa mensagem, vou colocar aspas simples e vou dar um "Ctrl + V" da mensagem que temos ali embaixo.

[03:01] Mas repare que essa mensagem aparece em um formato JSON, então vou cancelar, vou colocar abrindo e fechando chaves: aspas simples, abre e fecha chaves, aspas duplas e começa a nossa string.

[03:13] Vou dar um "Send" e vamos receber uma mensagem de erro, porque ele fala: olha, o que eu esperava era: "A API diz: E aí, gui, tudo beleza?", mas olhe o que eu recebi. Olhando, visualmente, parece que está tudo igual, a única diferença é esse espaço que temos entre as frases.

[03:29] Vou tirar esse espaço. Quando eu dou o "Send" mais uma vez, ele dá que ambos os testes passaram. Eu posso falar aqui, por exemplo, "Verificando o conteúdo da resposta". Vou dar um "Send" aqui e está tudo certo, esse teste passou.

[03:50] Podemos realizar alguns testes no Postman também, mas, se pararmos para pensar, esses testes que eu estou realizando no Postman, eles estão funcionando no meu Postman. Seria legal se pudéssemos criar, de fato, testes para serem realizados dentro do Go. É isso o que vamos fazer na sequência. Esses dois testes que eu fiz, eu quero realizá-los utilizando o Go.

@@03
Meu primeiro teste

[00:00] Criamos dois testes no Postman, que verificam o status code e o corpo da resposta de uma requisição. O que queremos fazer agora é criar esses mesmos testes no Go. Para isso o Go já tem uma forma de executarmos os nossos testes. Se eu escrevo, no terminal, go test sem o servidor estar rodando, olha só que interessante.
[00:19] Ele fala que não tem nenhum arquivo de teste para você testar, para o Go, para testarmos. Então vamos criar um arquivo de teste, que eu vou chamar de "main_test.go". Repare que esse nome é bem interessante, "main_test.go". O que eu vou fazer? Vou falar que esse pacote de teste, ele faz parte do pacote package main - vou minimizar o menu lateral só para vermos melhor.

[00:42] Ele faz parte do pacote main. Vamos entender que tipo de teste vamos realizar e em qual momento nós estamos aqui, dentro dessa aplicação. Essa aplicação, ela está funcionando em ambiente de desenvolvimento. “Puxa, Gui, então quer dizer que o nosso banco de dados do Postgres não é o banco de dados de produção?” Não, é o banco de dados de desenvolvimento.

[01:02] Algumas aplicações e alguns frameworks utilizam outros tipos de bancos de dados para conseguir realizar os seus testes. Vou dar um exemplo: no Django, o banco de dados inicial, que vem na aplicação, é o SQLite. Quando colocamos um projeto Django em produção, geralmente, na maioria esmagadora dos casos, colocamos esse projeto em um Postgres, em um MySQL ou em um outro banco de dados.

[01:25] Então teríamos um outro banco de dados de produção e um banco de dados de desenvolvimento. No nosso caso, nós só temos o ambiente de desenvolvimento, então não estamos trabalhando com o banco de dados em produção. Todos os testes que eu fizer eu vou vincular esse banco de dados de desenvolvimento, que nós temos aqui.

[01:40] O que eu quero fazer? Eu quero testar o nosso arquivo main. Nós nos conectamos com o banco de dados e subimos as nossas rotas da aplicação. Eu quero testar essas rotas, esses endpoints, quero verificar se o status code que eu estou recebendo é o correto, se o comportamento que estamos tendo é o correto também.

[01:57] O que eu vou fazer? Primeira coisa, vamos testar esse nosso primeiro endpoint, que faz o controle da saudação. Só que eu não vou pegar esse meu arquivo de rotas do HandleRequest e executar ele aqui. Eu vou criar um registro de rota com a ação que teremos, com o get, para cada um desses, chamando o nosso controller.

[02:17] Então eu vou criar, no "main_test.go", uma função que eu vou chamar de func SetupDasRotasDeTeste(). O que eu quero fazer? Essa minha função, ela vai retornar uma instância do Gin, vai retornar uma instância () *gin.Engine{}. Aqui, dentro das chaves, eu vou criar rotas := gin.Default(), que é o que estamos utilizando na nossa aplicação de rotas mesmo, gin.Default.

[02:55] Não vou registrar nenhuma rota, vou dar um return dessas rotas, return rotas. Agora sim. Já temos aqui o nosso setup das rotas. A primeira rota que eu quero testar é a seguinte - antes de testar a rota de fato, vamos entender como funciona uma função de teste no Go, que isso é muito importante.

[03:12] A função de teste é uma função normal, que já temos como parâmetro, só que ela possui uma assinatura específica. Ou seja, a primeira palavra da minha função de teste precisa ser Teste com "T" maiúsculo. Isso significa que todas as funções de teste devem começar com essa palavra teste com "T" maiúsculo, seguido do teste que vamos realizar.

[03:34] Então o primeiro teste que eu quero realizar aqui é um teste que eu sei que vai falhar. Será o func TestFalhador(), só para visualizarmos como é que funciona. Só isso é suficiente? Não, toda função de teste, ela precisará receber um parâmetro, que é o ponteiro apontando para o teste que vamos utilizar.

[03:52] Então (t, por convenção usamos o "T", apontando para "testing.T": (t *testing.T), que é o teste que vamos utilizar. Dentro desse "T", quando eu coloco um ponto, tem vários métodos que eu posso utilizar, várias funções que eu posso utilizar para realizar os meus testes e deixar os meus testes ainda melhores.

[04:12] Então o primeiro teste que eu quero realizar é um teste que vai falhar. Eu vou colocar esse t.Fatalf(), porque eu quero colocar uma mensagem, uma string, e esse teste eu vou chamar ("Teste falhou de propósito, não se preocupe"). Esse é o meu primeiro teste.

[04:34] Teste falhou de propósito, o que eu vou fazer agora? Vou realizar o mesmo código no terminal, go test - Go, realize os testes e veja o que acontece. Só para vermos a cara de um teste que falha. Sempre no Go, a cara de um teste que vai falhar é assim - vou minimizar para podermos visualizar melhor. Está a mesma coisa, vou deixar grande mesmo.

[04:52] Então aqui, o "TestFalhador", ele dá o tempo que demorou para esse teste falhar e dá a mensagem: “Teste falhou de propósito, não se preocupe”. O que eu quero fazer na sequência? Toda função de teste vai ter a palavra Test, com "T" maiúsculo, seguido do nome do teste que vamos realizar, e vai receber como parâmetro o "T" apontando para o testing.T.

[05:13] E temos, dentro de "T", várias funções que podemos utilizar para realizar os nossos testes, mandar as mensagens de uma forma melhor. O que eu quero fazer, na sequência, é de fato criar um teste que verifique o status code e que verifique o corpo da requisição. Isso nós vamos fazer na sequência.

@@04
StatusCode

[00:00] Nesse vídeo vamos verificar o status code da nossa requisição de saudação. Para começar, eu quero verificar esse endpoint aqui.
[00:08] Quero verificar se uma requisição Get passando o nome, passando o parâmetro, que chama esse controller, nós temos o status code esperado, que é o 200, que é o de sucesso. Esse TestFalhador não serve para nada, a não ser falhar, então vamos tirar ele e vou dar um nome para esse nosso teste.

[00:25] Será o TestVerificaStatusCodeDaSaudacaoComParametro(t *testing.T). Fica bem bonito. Vou apagar esse t.Fatalf, que não vamos utilizar agora. Vamos começar. Eu tenho aqui em cima um setup de rotas que vamos registrar novas rotas para realizar os nossos testes.

[00:59] A primeira coisa que eu vou fazer será criar, pegar uma instância do Gin, com esse setup de rotas, r := SetupDeRotasDeTeste(). O que acontece? Essa linha significa que eu estou criando uma nova instância do Gin que não tem nenhuma rota cadastrada. Eu preciso cadastrar uma rota. Eu vou copiar essa linha 10 do meu HandleRequest.

[01:24] Eu vou copiar ela toda: r.GET("/:nome", controllers.Saudacoes), e vou fazer tudo isso. Repare que o controller está dando uma mensagem de erro, que ele não está aqui. Quando eu salvar ele vai trazer o controller no import para mim. Até agora eu tenho, dentro dessa rota de teste, o Gin com apenas um endpoint registrado.

[01:44] O que eu preciso fazer agora é realizar, de fato, uma requisição. Para isso, eu vou colocar aqui req, e essa função que realiza a requisição devolve mais uma mensagem de erro, que não vamos fazer a verificação agora, então eu vou ocultar. Fica req, _:= http.NewRequest().

[02:07] Eu vou falar que essa requisição que eu quero realizar, ela vai ser uma requisição get e eu preciso passar qual é a URL, o string na URL. A minha string será por barra localhost e vou passar a mensagem gui. Vou dar uma vírgula e qual é o corpo dessa requisição? Tem algum JSON, algum dado que eu quero passar para essa requisição? Não, não tem nada. Então eu passo ele com nil, req, _:= http.NewRequest ("GET", "/gui", nil).

[02:33] Um outro ponto muito importante, além dessa requisição, eu preciso armazenar essa minha resposta. Então eu vou criar uma variável chamada resposta := http e aqui é algo muito interessante, httptest.NewRecoder(). Vamos entender o que é esse NewRecorder. NewRecorder é uma função muito interessante.

[02:59] O que ela faz? Ela vai implementar a interface de quem vai realizar, de quem vai armazenar essa resposta. Ela nos fornece essa funcionalidade de que temos uma requisição, temos a resposta e precisamos armazenar todos os dados dessa resposta, o corpo, o status code, várias informações, e essa função implementa uma interface de response writer que faz essa funcionalidade para nós.

[03:23] Então temos um cenário bem legal para conseguirmos testar. “Gui, eu tenho uma pergunta: a requisição já foi realizada?” Não, a requisição ainda não foi realizada. Nós temos uma variável que já fala: “a requisição vai ser assim”. E temos uma variável de resposta que fala: “guarde essa resposta da requisição”. Mas, de fato, ela não foi realizada. Como fazemos para realizar essa requisição?

[03:45] Nós fazemos através desse código r.ServeHTTP(). É esse cara que vai, de fato, realizar a requisição. Ele vai precisar de dois argumentos, o primeiro é: onde eu guardo a resposta dessa requisição? Guarde dentro de (resposta, ). E qual é o tipo de requisição que eu vou fazer? Eu vou mandar a requisição, a requisição é essa aqui (resposta, req).

[04:06] Nesse momento, se realizarmos o teste, ele vai realizar a requisição e vai guardar a resposta. O que falta verificarmos? Apenas se o status code é igual ao da resposta. Eu vou colocar aqui if resposta.Code. Se a resposta.Code != http.StatusOK {} - se ela não for igual. Qual é o StatusOK?

[04:36] O StatusOK é o 200. Se ela não for igual, vamos exibir uma mensagem de erro que nós já aprendemos, que é o t.Failf(), porque vamos criar uma string para informar essa mensagem. Vamos informar aqui ("Status error: "), e aqui eu posso juntar, por exemplo, ("Status error: valor recebido foi ").

[05:00] Vou concatenar a nossa mensagem com o %d, ("Status error: valor recebido foi %d, e o esperado era %d"). Coloquei os dois ali, o que eu vou fazer agora será juntar esses dois valores. O primeiro valor que queremos, o valor recebido foi a , resposta.Code,. O valor esperado era o , http.StatusOK, esse era o valor esperado.

[05:33] Aqui eu estou em uma linha só, deixa eu tirar só para conseguirmos visualizar. Todo o meu código está feito assim, porque eu cortei para conseguirmos visualizar melhor, mas tudo isso está em uma linha. Então vamos só relembrar o que fizemos. Pegamos uma instância do Gin, registramos uma nova rota, falamos que a requisição será get com esse parâmetro gui e não temos nenhum corpo da requisição.

[05:54] A resposta nós vamos armazenar, queremos salvar todo o conteúdo da resposta, status code, body, a resposta ele vai criar aqui para nós. E o r.ServeHTTP, ele vai de fato realizar essa requisição. Depois nós verificamos: o status code da resposta, que vai vir, é igual ao 200, que é o sucesso?

[06:14] Se for igual, maravilha, o nosso teste vai passar. Se não for, o nosso teste falhou e será muito triste. Então go test, quando damos um "Enter" aqui, olha que interessante, ele vai falar que passou. "Pass", tem um ok e o nosso teste passou.

[06:28] Então quando passamos um parâmetro para essa requisição, ela devolve um status code ok. E se eu não passar? Tirei o gui. Vamos realizar mais uma vez esse teste, só para visualizarmos? Quando eu tiro, nós temos uma mensagem de erro muito importante, bem legal. Olha só: "Status error: o valor recebido foi 404 e o esperado era 200".

[06:51] Eu vou voltar o teste com o gui, que é o que queremos fazer, o teste para passar. Ou você pode colocar o seu nome também, não tem problema. E realizamos aqui o nosso primeiro teste. O nosso primeiro não, o primeiro foi o falhador, esse é o nosso segundo teste. O que eu quero fazer, na sequência, é verificar, de fato, o conteúdo. Será que o corpo dessa requisição tem o valor esperado? Isso vamos fazer na sequência.

@@05
Assert

[00:00] Existe um pacote no Go que fornece muitas ferramentas para conseguirmos testar o nosso código e garantir o comportamento esperado. Se observarmos a nossa aplicação, nós colocamos um if que verifica a resposta que nós recebemos com o Http.StatusOK, com o status 200.
[00:17] Só que existe uma forma de não precisarmos escrever esse if na mão. Eu vou pesquisar no Google por "golang testify". Nesse primeiro link, repare que ele vai dar uma explicação, a documentação do testify. Eu quero saber se uma coisa é igual, eu passo o meu teste, o "T", como primeiro parâmetro, verifico os dois valores e posso passar também uma mensagem.

[00:46] Vamos instalar então esse pacote na nossa aplicação, para conseguirmos utilizar? Vou procurar a seção sobre instalação.

[00:55] Tem aqui, embaixo, go get github.com/stretchr/testify. Vou copiar essa linha. No nosso código, no nosso terminal, vou dar o go get github.com/stretchr/testify, já temos aqui o go get para instalar toda essa dependência. Está instalado, agora, o que precisamos fazer? Lembra que no nosso código nós temos todas essas três linhas aqui do if?

[01:19] Eu vou tirar essas linhas e vou colocar o assert. Repare que aqui tem algo muito importante: nós temos o assert do "go-playground" e o assert do "stretchr/testify".

[01:34] É o segundo, é esse do "stretchr/testify", não é o assert do "go-playground", é o segundo. Assim que eu coloco o segundo ele já faz um import para mim do testify. Então eu quero saber se uma coisa é igual? assert.Equal(). Aqui eu vou passar, em primeiro lugar, vamos passar a nossa instância de teste, o nosso valor de teste, o nosso "T". Eu vou falar: meu teste é esse aí.

[02:00] Segundo: qual é o valor esperado. O valor esperado é o (t, http.StatusOK, ). E qual é o valor que eu quero testar, o valor atual, (t, http.StatusOK, resposta.Code). Ficou muito mais bonito de ler o nosso teste.

[02:21] Limpando o meu terminal, se eu rodar o go test, olha que interessante, teremos o mesmo resultado: Ok e passou. Se eu tirar o gui, para recebermos uma notícia aqui, vamos ver? go test, temos aqui o nosso erro.

[02:37] Podemos até fazer a mensagem de erro padronizada, igual estávamos fazendo anteriormente, eu deixo de desafio para você colocar essas mensagens de erro. Eu vou colocar só no primeiro, só para visualizarmos como vai ficar no nosso código.

[03:01] Aqui embaixo, na resposta, o terceiro parâmetro eu posso colocar (t, http.StatusOK, resposta.Code, "Deveriam ser iguais"). Abrindo o meu terminal - deixa eu só abaixar o terminal para conseguirmos ver todo o código ali em cima.

[03:16] go test mais uma vez e "Deveriam ser iguais" na mensagem de erro que aparece aqui. Muito legal. “Poxa, Gui, mas o que eu quero ver é a resposta da requisição, o corpo da requisição”. Para isso existe um pacote Go que já vem instalado, build-in, que ele conseguirá ler todo o corpo da requisição e pegar o JSON que nós precisamos. Então vamos lá, primeira coisa, para eu verificar o corpo da requisição, eu preciso fazer um mock da minha resposta. O que é um mock?

[03:50] O que eu espero. Eu vou chamar aqui mockDaResposta := será o quê? Será aquele meu código que aparece na minha API: "API diz", esse código aqui. Eu vou copiar todo ele, só que a partir das chaves. Deixa eu colocar aqui na resposta, no corpo, vou pegar todo esse conteúdo aqui debaixo.

[04:20] Só que eu vou pegar das chaves, eu vou pegar o mesmo do teste, esse aqui todo, a partir das chaves, eu não vou copiar as aspas simples.

[04:28] Como eu faço? Se eu colocar no código, vai ter um erro. Ele vai falar: “olha, estou esperando um operador, não é isso o que eu quero, eu quero que seja todo esse conteúdo”. Para isso eu vou encapsular esse conteúdo através de uma crase invertida. Tenho aqui o mock da resposta, ele vai falar que você tem o mock da resposta, mas você não está usando, você declarou, mas não está usando.

[04:51] Então vamos usar. Só que antes de usarmos, vamos pegar só o corpo da requisição. Eu vou chamar aqui de respostaBody :=, que é o corpo da requisição. Essa respostaBory :=, uma forma que temos para utilizar é utilizando o pacote := ioutil.ReadAll(). Ou seja, para ele ler todo. Ler todo o quê?

[05:19] A resposta, o nosso resposta ali em cima, (resposta.Body), ler todo o corpo da requisição. Assim que eu salvo, repare que temos essas duas variáveis, elas estão aqui, mas ainda não estamos utilizando. Vamos utilizar elas onde? No assert. Eu vou falar assert.Equal(). Eu quero, passando o nosso teste T, eu quero o valor esperado. É o (t, mockDaResposta,.

[05:47] E o valor que eu vou receber é a nossa (t, mockDaResposta, respostaBody). Salvando esse cara. Repare que esse ioutil.ReadAll, ele está dando uma mensagem de erro, porque ele retorna mais de um parâmetro. Eu vou deixar os dois aqui.

[06:03] Nós vamos rodar esse nosso teste para ver o que vai acontecer. Vou jogar o terminal para cima - dê um pause se você quiser ler. Vou jogar para cima, go test para ver o que acontece. Nós temos uma mensagem de erro muito doida.

[06:17] Repare que ele falou que o valor esperado era uma string "API diz", e o valor que ele recebeu é um monte de número, um monte de caractere muito louco. Por quê? Porque essa nossa função do ioutil, ela oferece uma abstração de super alto nível, que lê os dados e retorna em um conjunto de bytes, em vários bytes.

[06:40] E não são os bytes que nós queremos, nós queremos que essa respostaBody seja do tipo string. Então eu posso colocar aqui assert.Equal(t, mockDaResposta, string(respostaBody)) para ela. Vamos fazer o teste mais uma vez, go test. Olha que interessante: agora sim, temos uma mensagem.

[06:54] O nosso teste não passou, vamos verificar o que aconteceu. Ele falou que recebemos uma diferença do que nós precisamos.

[07:03] Nós recebemos um 404 na página que estávamos esperando. Por quê? Porque eu tirei o gui ali de cima. Eu vou colocar o gui, agora sim. Subindo o terminal um pouco, limpando, go test. Maravilha.

[07:21] Passamos neste primeiro teste e passamos nos dois testes da nossa API. Então ele deu certo. “Gui, mas está exibindo mesmo esses valores?” Vamos colocar aqui para imprimirmos? fmt.println(), vou colocar (string(respostaBopdy)), do tipo string, só para vermos o que está vindo. Vou colocar também o nosso mock, fmt.Println(mockDaResposta). Não precisa ser do tipo string, porque ele já é do tipo string.

[07:57] Limpando esses dois testes, rodando mais uma vez. Olha só, teremos os dois, a mesma coisa, "API diz":"E aí, gui, tudo beleza?". O primeiro é o da respostaBody e o segundo o meu mock, o que eu esperava.

[08:08] Realmente temos o resultado que esperamos. Nós pegamos o corpo da requisição, com o ioutil.ReadAll() e depois, para conseguir fazer essa comparação, só convertemos ele, só mudamos aquele conjunto de bytes para string. E nós fizemos o mesmo teste que realizávamos no Postman, nós fizemos no VS Code também.

https://github.com/stretchr/testify

@@06
Escrevendo um bom teste

Nesta aula, vimos que existe um comando específico do Go para testar nossa aplicação, como a convenção da escrita dos testes e isso foi incrível.
Sabendo disso, analise as afirmações abaixo e marque as verdadeiras:

Alternativa correta
É possível realizar vários tipos de testes em uma aplicação.
 
Alternativa correta! Existem outros tipos de testes, cada um com um objetivo diferente.
Alternativa correta
Conhecer o sistema que será desenvolvido e aplicar diferentes testes, entendendo os fluxos e regras, fará grande diferença na criação dos testes.
 
Alternativa correta! Entender o objetivo do sistema é essencial para o sucesso dos testes.
Alternativa correta
Devemos apenas saber como escrever testes no Go.

https://pt.wikipedia.org/wiki/Teste_de_software

@@07
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito isso, excelente. Se ainda não fez, é importante que você implemente o que foi visto no vídeo para poder continuar com a próxima aula, que tem como pré-requisito todo o código escrito até o momento.
Caso não encontre uma solução nas perguntas feitas por alunos e alunas deste curso, para comunicar erros e tirar dúvidas de forma eficaz, clique neste link e saiba como utilizar o fórum da Alura.

@@08
O que aprendemos?

Nesta aula:
Realizamos um teste no Postman que verifica o statusCode de uma resposta;
Criamos nosso primeiro teste em Go, o TestFalhador;
Escrevemos um teste que verifica o endpoint de Saudação da API;
Instalando o assert e alteramos o código verificando o corpo da resposta.
Na próxima aula:
Vamos testar a busca por ID, e os métodos DELETE e PATCH da nossa API!

#### 06/10/2023

@03-Testando os endpoints

@@01
Projeto da aula anterior

Aqui você pode baixar o zip da aula 02 ou acessar os arquivos no Github!

https://github.com/alura-cursos/api_rest_gin_go_2-validacoes-e-testes/archive/refs/heads/aula_2.zip

https://github.com/alura-cursos/api_rest_gin_go_2-validacoes-e-testes/tree/aula_2

@@02
Testando a listagem de um recurso

[00:00] Agora que já realizamos o nosso primeiro teste de um endpoint, vamos testar os nossos outros endpoints? Olha só, temos aqui no HandleRequest um recurso, um endpoint que lista todos os alunos.
[00:10] Eu quero testar isso, para ver se, de fato, chega uma requisição, se conseguimos exibir esses alunos e mostrar a resposta certa. Então vamos lá, para começar, vou criar uma nova função de teste. Eu vou escrever aqui func Test(), com a assinatura que nós já temos,(t *testing.T) e vou dar um nome para esse teste.

[00:34] Esse teste será o "TestListandoTodosOsAlunosHandler", por exemplo, de lidar com a requisição. func TestListandoTodosOsAlunosHandler(t *testing.T). O que eu preciso fazer para esse teste? Em primeiro lugar, se eu quero listar todos os alunos que eu tenho registrados, eu preciso acessar o banco de dados.

[00:58] Lembrando que o banco de dados que estamos utilizando é o banco de dados de desenvolvimento. Então o que eu vou fazer? Vou chamar do database, database.ConectaComBancoDeDados(), a nossa função. Assim que eu salvo, repare que ele já trouxe nos imports o database para conseguirmos conectar.

[01:15] A nossa função de teste já está conectada com o banco de dados de desenvolvimento. O que eu preciso agora é de uma rota do Gin, r := SetupDasRotasDeTeste() e vou registar a nossa primeira rota de teste, será exatamente essa linha r.GET("/alunos", controller.TodosAlunos), e vai chamar o controller que exibe todos os alunos.

[01:41] Já temos a conexão com o banco de dados, já temos o setup das rotas de teste, uma instância do Gin, e já registramos uma rota para esse recurso. Vamos ter aqui o req, _ :=, não vamos lidar com as validações desse método ainda. Vamos realizar aqui a nossa requisição com :=http- vamos registrar a nossa requisição, melhor - req, _:= http.NewRequest().

[02:08] Vamos falar: qual será o método que vamos utilizar? Será o método get. Qual será o path que vamos usar? Será o ("GET", "/alunos", nil). E vamos passar algum conteúdo no corpo dessa requisição? Não. Então vou colocar aqui como nil. Temos a nossa requisição, eu preciso de uma variável para a resposta.

[02:27] resposta := httptest.NewRecorder(), para ele armazenar todas as informações que temos do corpo desta resposta específica, dentro dessa nossa variável. Vamos realizar, de fato aqui, essa requisição, r.ServeHTTP(), eu vou passar aqui a resposta e vou passar aqui a nossa requisição, (resposta, req).

[02:55] A requisição está acontecendo. O que eu preciso verificar agora é o seguinte, vamos fazer a nossa assertação para vermos se, de fato, estamos recebendo o status code que esperamos. assert.Equal(t), devolvendo o nosso teste primeiro. Depois eu preciso fazer assim: o que eu espero, o valor esperado, é o (t, http.StatusOK,), isso é o que eu espero.

[03:23] O que eu vou receber é o status code da resposta, (t, http.StatusOK, resposta.Code).

[03:30] Vou limpar o meu terminal, vou rodar mais uma vez com o go test, vamos ver o que vai acontecer. Deu aqui que ele passou, ficou ok, ficou verde, ele passou nesse nosso teste. Então olha só, aqui em cima é o nosso primeiro teste. É o teste do controller Saudação, e aqui o segundo teste, que lista todos os alunos.

[03:51] Aqui vai um ponto interessante: será mesmo que ele acessou o nosso banco de dados e listou todos os alunos? Nós podemos verificar isso. O que eu vou fazer? Depois que tudo aconteceu, o teste foi aprovado e estamos super felizes, comemorando, eu vou verificar se temos algum aluno aqui. fmt.Println(), vou colocar (resposta.Body), para vermos o corpo dessa requisição, para descobrirmos se tem algum aluno sendo exibido.

[04:17] Vou limpar o meu terminal, go test mais uma vez. Ele listou aqui todos os nossos alunos. Olha que interessante, temos o ID 1, que é o Gui, tem a Ana - deixa eu ver se eu acho a Ana aqui nessa bagunça. Tem o ID 1 Guilherme, ID 2, nem lembro qual ID do Murilo. Achei a Ana. Listou todo mundo. Então, de fato, conseguimos conectar com o banco de dados e exibir essa resposta. Lembrando que o ambiente que nós estamos é um ambiente de desenvolvimento.

[04:50] Agora que sabemos que, de fato, esses alunos estão aparecendo, eu posso tirar essa nossa mensagem, o fmt.Println(resposta.Body), porque conseguimos exibir, listar só os alunos que temos no banco de dados.

@@03
Aluno Mock

[00:00] A nossa API tem alguns alunos já cadastrados. No meu caso eu quero mostrar isso para vocês, então eu vou rodar aqui a nossa aplicação, com go run main.go. Quando fazemos uma requisição para o https://localhost:8080/alunos, temos o ID 1 - deixa eu jogar aqui para cima para visualizarmos.
[00:15] O aluno Gui, a aluna Ana e o aluno Murilo. Se eu deleto esses três alunos - vou deletar aqui, https://localhost:8080/alunos/1. Não lembro o ID da Ana, acho que é o ID 3. Deixa eu ver, só para ter certeza, qual é o ID do Murilo mesmo, que eu não lembro. O do Murilo, o ID 13. Vou deletar aqui, "Delete", https://localhost:8080/alunos/13. Deletei, dei um "Send". Deixa eu colocar no "Body", o alunos foi deletado.

[00:45] Se eu dou um "Get" para https://localhost:8080/alunos, sem a barra, temos aqui que está vazio, não temos ninguém.

[00:53] Mas, se observarmos, o status continua 200. O que isso significa? Significa que se eu rodo a minha aplicação agora, se eu rodo os meus teste, olha que interessante: rodei meu teste e ambos os meus testes vão passar, tanto o teste da saudação, que está certo, não tem relação com o banco de dados, como o teste que verifica os alunos também, ele passou.

[01:14] Não importa se eu tenho alunos ou não. Isso é um pouco ruim. Por quê? Seria interessante se conseguíssemos criar um aluno de teste para esse nosso teste, para essa nossa verificação. Pensando nisso, vamos criar duas funções, uma que cria um aluno e insere no banco de dados essa função, eu posso chamar até de “cria aluno mock”, e uma outra que “deleta aluno mock”, que vai no banco de dados e deleta.

[01:36] Nós falamos assim: sempre que for criar um aluno novo, será com essas características, depois que realizamos os testes, nós deletamos esse aluno. Então o que eu vou fazer? Embaixo da nossa função SetupRotasDeTeste, eu vou criar uma função que eu vou chamar de func CriaAlunoMock(). Essa função, ela não tem nada, não temos nenhum retorno, só inserimos esse aluno no banco de dados.

[02:01] E terei uma outra função, que vou chamar de func DeletaAlunoMock(), para garantirmos que teremos pelo menos um aluno nesse banco de dados. Eu vou colocar aqui o nosso primeiro aluno, na nossa função que cria. Nós já sabemos que um aluno é aluno := models.Aluno{}, e vou criar aqui um aluno.

[02:29] O aluno, nós sabemos que ele tem um nome, o nome desse aluno será {Nome: "Nome do Aluno Teste", }. Ele terá um CPF, , CPF: "", nós precisamos passar todos os dígitos, então , CPF: "12345678901",, que é um CPF. Também temos o RG, que são 9 dígitos, então , RG: "123456789"{.

[03:01] O que eu vou fazer depois que eu tenho esse aluno, essa instância desse aluno? Vou gravar ele no banco de dados, database.DB.Create(&aluno), para ele criar de fato esse aluno, passando todo o endereço desse aluno, o endereço de memória do aluno. No final, o que precisamos? Precisamos armazenar o ID desse aluno. Por quê?

[03:24] Como eu vou saber qual aluno que eu quero deletar? Então teremos aqui ID, que eu preciso passar, só que esse ID, ele precisa ser visto por outras classes também. Então aqui em cima, antes do SetupRotasDeTeste, eu vou criar uma variável, eu vou chamar de var ID int, vou chamar que o ID é um valor inteiro.

[03:43] E na CriaAlunoMock eu vou armazenar esse valor do ID. Eu vou falar que o ID do aluno será esse: ID := int(), um valor inteiro do (aluno.ID). Eu tenho aqui esse valor agora. Salvei essas alterações, está tudo certo. O ID, como eu já declarei aqui em cima, eu não preciso dos dois pontos, ID = int(aluno.ID), só o igual.

[04:07] E para eu deletar um aluno, o que eu vou fazer? Eu vou pegar um aluno, vou criar uma instância do aluno. Deixa eu criar aqui, var aluno, que é do tipo var alunos model.Aluno, e vou pedir agora para o meu banco de dados deletar esse aluno, database.DB.Delete(). Eu passo quem? O endereço de memória do aluno, vírgula e o ID que eu tenho de referência, (&aluno, ID).

[04:37] “Gui, você criou as duas funções. Você tem as duas funções, você tem a que cria o aluno mock e a que deleta o aluno mock. Mas nós precisamos exibir esse aluno, testar esse aluno. Então o que eu vou fazer?” Quando temos a nossa função que testa os alunos, olha só.

[04:55] Fazemos a conexão das nossas variáveis setup - aqui embaixo. Nós conectamos com o banco de dados. Depois que nós conectamos, o que eu vou fazer? Eu vou criar um aluno, CriaAlunoMock(). Ele vai criar um determinado aluno no banco de dados.

[05:09] Ele vai fazer toda a função e, no final, o que eu quero? Eu quero que ele delete esse aluno. Então depois que essa função for toda executada, eu vou colocar aqui no defer, não esqueça de deletar o aluno mock, defer DeletaAlunoMock(). E ele vai deletar aqui o aluno.

[05:23] O que eu vou fazer, só para conseguirmos visualizar esse aluno? Eu vou tirar esse //defer DeletaAlunoMock(), para vermos esse aluno criado. fmt.println(), eu vou passar aqui a (resposta.Body), para visualizarmos esse aluno, que de fato ele foi criado. Vou rodar o meu teste - estou só limpando aqui - go test.

[05:48] Ele rodou e ele falou: nós temos um aluno é o ID 14, foi criado nesta data e tem aqui o aluno - cadê o CPF dele? É esse, e o nome do aluno teste está aqui em cima, "Nome do Aluno Teste". Só que esse aluno está no banco de dados. Se eu der uma requisição, eu subo o meu servidor e dou uma requisição "Get" para alunos, temos aquele aluno, "Nome do Aluno Teste" criado.

[06:11] Não é o que eu quero fazer. Eu quero usar esse aluno teste, depois que eu faço o teste, eu quero deletar todo esse aluno. Então colocamos o defer DeletaAlunoMock() e maravilha, não temos mais a preocupação de executar um teste que não tenha ninguém cadastrado. Nós garantimos que, neste teste, terá pelo menos 1 registro aqui dentro.

@@04
Testando a busca por CPF

[00:00] Vamos realizar mais um teste? No endpoint que eu queria testar, nós temos alguns aqui, é esse da busca por CPF.
[00:06] O que acontece? Na nossa aplicação - deixa eu colocar ela para rodar aqui, go run main.go, eu vou criar uma pessoa com uma requisição "Post" - deixa eu só ver se eu não tenho ninguém, que se eu tiver alguém eu já deixo aqui. Maravilha, temos o aluno 14, que é o aluno teste.

[00:25] O CPF dele é esse aqui: "12345678901". Se eu tenho aqui https://localhost:8080/alunos/cpf/12345678901, o valor do CPF, ele me devolve exatamente esse aluno que nós temos.

[00:35] Então conseguimos buscar por CPF, isso ficou bem legal. Deixa eu minimizar o terminal para nós vermos. Nós temos essa busca. Eu quero justamente testar esse cenário aqui. O que precisaremos fazer? Primeiro vamos criar uma função de teste, colocar o prefixo que é importante, que é o Test, eu vou colocar func TestBuscaAlunoPorCPFHandler(), que lida com essa requisição.

[01:05] Temos (t *testing.T). Agora, o que eu vou fazer? Vamos criar aqui a nossa função. Primeiro, essa nossa função, que busca o aluno por CPF, precisa de conexão com o banco de dados? Precisa, então database.ConectaComBancoDeDados(). Essa nossa função, precisamos de um recurso prévio criado?

[01:30] Sim, precisamos criar um determinado aluno mock, CriaAlunoMock(). Então eu já vou criar esse aluno e eu já vou falar: quando acabar toda essa função, você delete esse aluno mock. defer DeletaAlunoMock(). O que eu preciso fazer também?

[01:42] Eu vou precisar criar um cenário, uma rota, uma instância do Gin, então r := SetupDasRotasDeTeste(). Agora sim podemos criar, registrar essa rota que nós temos, r.GET("/alunos/cpf/:cpf", controllers.BsucaAlunoPorCPF). Vou colar aqui essa rota. Temos essa rota registrada, o que eu quero fazer agora é armazenar uma requisição.

[02:07] Uma requisição para esse endpoint, ela será assim, eu quero pegar o req, _ := http.NewRequest(), que já sabemos. A ação, que vamos fazer, será uma ação get, tudo maiúsculo, agora sim. Depois precisamos passar qual é o path que vamos utilizar, será ("GET", "/alunos/cpf/12345678901, nil"), e o valor do CPF deste aluno, que é esse "1234578901".

[02:38] Vou copiar aqui. Temos algum campo, que vamos passar extra? Não, não temos nada que queremos passar no corpo dessa requisição. Teremos também uma resposta, que vamos armazenar com resposta := httptest.NewRecorder().

[02:57] O que eu vou fazer? Efetivar: realize essa requisição e armazene esta resposta com r.ServeHTTP(), armazenando a resposta e a requisição que queremos fazer, que já temos registrada, que é essa (resposta, req). Ele vai fazer essa requisição. No final, queremos verificar o quê?

[03:17] Se, quando buscamos um aluno, se o status code é igual, assert.Equal(). Eu vou passar aqui o nosso teste, o nosso ponteiro de teste, vou passar o nosso Http, (t, http.StatusOK, ), e vou passar a resposta, resposta.Code). Temos aqui o nosso teste criado.

[03:55] Vamos realizar esse teste - antes deixa eu ver o meu servidor, está rolando. Eu vou deletar esse aluno, para garantirmos que não tem erro. Ou melhor, não vamos deletar, vamos deixar com esse erro? Vamos ver esse erro explodindo? Nós teremos dois alunos. Vou rodar o meu teste, vamos visualizar. Não estourou nenhum erro.

[04:12] Nós temos dois alunos criados com o mesmo ID. Só que tem uma coisa que está me preocupando, olha só - deixa eu rodar o nosso servidor só para eu tirar esse aluno. Não explodiu nenhum erro na cara, o computador está funcionando normal. Eu quero deletar o aluno com o ID 14. https://localhost:8080/alunos/14, deleto o aluno 14. Maravilha, não temos ninguém aqui, justamente o que queremos.

[04:40] Está vazio, nós só queremos os alunos nossos de teste. Só que tem algo que me preocupa, olha só, essa é a mensagem de teste, a resposta de teste.

[04:48] É uma bagunça, é muita informação para ler, é difícil conseguirmos entender quais testes estão rodando, quais testes estão falhando. Pensando nisso, existe uma forma de conseguirmos falar: simplifique as mensagens de teste, eu quero só o modo de release dos testes, não precisa detalhar tudo.

[05:05] Onde fazemos isso? Na nossa função de SetupDasRotasDeTeste(), eu posso falar gin.Mode(). Aqui dentro eu vou passar que eu quero usar o Gin no modo de release, (gin.ReleaseMode). Salvo essa linha. Temos uma mensagem, porque não é o mode, é o gin.SetMode(gin.ReleaseMode), escrevi errado.

[05:28] Então eu setei o modo de release para conseguirmos ver as instâncias do Gin. Eu vou rodar aqui, go test, olha que interessante como as nossas mensagens serão exibidas de forma mais organizada.

[05:39] Muito melhor. Se eu minimizar a tela, que ela está grande, dá para vermos exatamente qual endpoint estamos testando, qual o método que estamos utilizando, e ele fala aqui que nós passamos nos testes e tal, e deu um sucesso, foi uma maravilha.

[05:53] Então você precisa de mais informações para conseguir fazer um teste - puxa, eu quero ver, de fato, as mensagens, tudo o que aconteceu - podemos usar o modo normal. Ou comentar essa linha do Gin mode, ele já vai exibir sem ser no modo release. Dessa forma conseguimos visualizar os nossos testes de uma forma mais simples.

[06:14] “Puxa, Gui, eu quero fazer outra coisa, eu quero testar apenas uma requisição, não quero testar tudo, como eu faço?” Vamos lá, nós temos na nossa aplicação - deixa eu rodar de novo, só para visualizarmos. Nós temos três testes, o teste da saudação, o teste da lista de alunos e agora o teste que busca o aluno por CPF.

[06:36] O que eu quero fazer agora é executar só um desses testes, eu quero executar só o primeiro. Deixa eu pegar só esse primeiro teste, o teste TestVerificaStatusCodeDaSaudacaoComParametro. Vou clicar, "Ctrl + C", vou copiar o nome desse teste, vou no terminal e digito: go test -run e vou passar quem? O nome do teste que eu quero executar.

[06:58] Então go test -run TestVerificaStatusCodeDaSaudacaoComParametro. Ele executa só esse teste. Dessa forma conseguimos otimizar para não executar todos os testes que nós temos.

[07:08] O go test, ele vai executar todo mundo. O go test -run, seguido do nome do teste, ele vai executar o teste que estamos listando ali.

@@05
A API diz...

Um aluno estava escrevendo um teste que verifica o endpoint de saudação, como vimos em aula, porém não obteve sucesso. Então, resolveu compartilhar seu código e o resultado de seus testes, como ilustra os códigos abaixo:
Código do teste:
1. func TestVerificaStatusCodeDaSaudacaoComParametro(t *testing.T) {
2.         r := SetupDasRotasDeTeste()
3.         r.GET("/:nome", controllers.Saudacoes)
4.         req, _ := http.NewRequest("GET", "/", nil)
5.         resposta := httptest.NewRecorder()
6.         r.ServeHTTP(resposta, req)
7.         assert.Equal(t, http.StatusOK, resposta.Code, "Deveriam ser iguais")
8.         mockDaResposta := `{"API diz":"E ai gui, Tudo beleza?"}`
9.         respostaBody, _ := ioutil.ReadAll(resposta.Body)
10.         assert.Equal(t, mockDaResposta, string(respostaBody))
11. }COPIAR CÓDIGO
Resultado do teste:
Messages:       Deveriam ser iguais
    main_test.go:47: 
                Error Trace:    main_test.go:47
                Error:          Not equal: 
                                expected: "{\"API diz\":\"E ai gui, Tudo beleza?\"}"
                                actual  : "404 page not found"

                                Diff:
                                --- Expected
                                +++ Actual
                                @@ -1 +1 @@
                                -{"API diz":"E ai gui, Tudo beleza?"}
                                +404 page not foundCOPIAR CÓDIGO
Analisando o código acima, podemos afirmar que:

O erro se encontra na linha 4 do código de teste onde registramos a requisição.
 
Alternativa correta! O erro neste caso foi não ter passado nenhum valor como parâmetro. A forma correta seria alterar a chamada da função para http.NewRequest("GET", "/gui", nil).
Alternativa correta
O resultado do teste indica que o valor esperado não é igual ao valor atual ou recebido e deveriam ser iguais.
 
Alternativa correta! Ler o resultado dos testes parece ser difícil no começo mas com tempo e prática, vamos ganhando experiência.
Alternativa correta
O erro se encontra na linha 4 do código de teste onde registramos a requisição e passamos nil como terceiro parâmetro da função http.NewRequest.

@@06
Faça como eu fiz

Chegou a hora de você seguir todos os passos realizados por mim durante esta aula. Caso já tenha feito isso, excelente. Se ainda não fez, é importante que você implemente o que foi visto no vídeo para poder continuar com a próxima aula, que tem como pré-requisito todo o código escrito até o momento.
Caso não encontre uma solução nas perguntas feitas por alunos e alunas deste curso, para comunicar erros e tirar dúvidas de forma eficaz, clique neste link e saiba como utilizar o fórum da Alura.

https://cursos.alura.com.br/comunicando-erros-e-tirando-duvidas-em-foruns-c19

@@07
O que aprendemos?

Nesta aula:
Criamos um teste que garanta o comportamento da listagem de alunos;
Geramos um aluno mock para ser usado em nossos testes;
Realizamos o teste do enpoint que busca um aluno por CPF.
Na próxima aula:
Vamos testar a busca por ID, e os métodos DELETE e PATCH da nossa API!
