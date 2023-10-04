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