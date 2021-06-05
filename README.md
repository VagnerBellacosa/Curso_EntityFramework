# Curso_EntityFramework

Apontamentos sobre Entity Framework

## O que é Entity Framework?

Primeiramente, o que é esse tal de **Entity Framework** que o pessoal tanto fala por aí? Bom, de maneira extremamente resumida, o Entity Framework é um *framework ORM* para **.NET** desenvolvido pela **Microsoft** (e atualmente open source), cuja primeira versão saiu em 2008 no .NET Framework 3.5 SP1. E o que viria a ser um **ORM**?

### ORM
ORM é uma abreviação de *“object-relational mapping“*, ou seja, mapeamento objeto-relacional. De maneira bem simplificada, frameworks ORM possibilitam o mapeamento das tabelas do banco com classes do seu projeto. Ou seja, se você tem uma tabela de “Clientes” no seu banco de dados, um framework ORM facilita o mapeamento dessa tabela com uma possível classe “Cliente” do seu projeto.

Quando implementamos a camada de acesso a dados da nossa aplicação, existem basicamente duas maneiras de nos comunicarmos com o banco de dados: ou utilizamos ADO.NET puro (com as classes DbConnection, DbCommand, etc., escrevendo manualmente os comandos SQL para recuperarmos e salvarmos os dados no banco) ou utilizamos um ORM (que por trás dos panos faz toda a mágica com o ADO.NET, criando os comandos SQL automaticamente). Se você quiser saber mais sobre as vantagens e desvantagens de cada um desses modelos, confira este vídeo do André Baltieri.

Além desses dois modelos, existe também um meio termo, que são os micro-ORMs. Caso você se interesse por esse meio termo, confira o meu artigo sobre Dapper, que é o micro-ORM para .NET mais conhecido atualmente.

No ecossistema .NET, o Entity Framework não é o único ORM disponível. O outro ORM mais conhecido para essa plataforma é o NHibernate, que é um porte para .NET do Hibernate (originalmente desenvolvido em Java). Como você pode perceber pelo título, essa série focará no Entity Framework. Talvez mais para a frente eu escreva uma outra série com o mesmo conteúdo em NHibernate (que eu não tenho absolutamente nenhuma experiência até agora).

Entity Framework Full? Entity Framework Core?
Até o começo de 2016, a Microsoft vinha lançando versões sequenciais do Entity Framework. Ela começou com a versão 1, pulou alguns números, aí tivemos a versão 4, a versão 5 e chegamos na versão 6 no final de 2013. Tudo estava caminhando normalmente, até que ela decidiu inventar um .NET Framework novo, do zero, que seria multi-plataforma. É o .NET Core que você provavelmente já deve ter ouvido alguma coisa sobre ele.

Junto com essa iniciativa do .NET Core, o time de acesso a dados do .NET resolveu re-escrever o Entity Framework também. Essa nova versão, desenvolvida do zero, também seria multi-plataforma, nos mesmos moldes do .NET Core. Depois de algumas discussões sobre o nome, a Microsoft nomeou essa nova versão do Entity Framework como “Entity Framework Core“.

Os conceitos básicos do Entity Framework Core são os mesmos do Entity Framework “full” (que veremos no artigo de hoje). O grande problema do Entity Framework Core é que ele não tem todas as funcionalidades disponíveis no Entity Framework “full“. Muita gente diz que ele ainda não está estável o suficiente para ser utilizado em produção. Como eu nunca utilizei nenhum deles em produção, eu não posso dar uma opinião sobre isso com propriedade, mas eu particularmente só utilizaria o Entity Framework Core hoje em dia caso eu realmente tivesse que desenvolver um projeto com o .NET Core (o que não foi o caso até agora).

Eu optei por utilizar o Entity Framework “full” nessa série. Quem sabe quando chegarmos no final dela, eu não faça um porte para o Entity Framework Core para vermos as surpresas que encontraremos na conversão.

Code First? Database First?
Uma vez entendido o conceito básico do Entity Framework, a pergunta que vem em seguida é: code first ou database first? Essas são as duas maneiras de trabalharmos com o Entity Framework. A grande diferença entre essas duas sistemáticas está no objeto que será a base para o mapeamento. No code first, as classes do projeto são a base para o mapeamento (elas são utilizadas para gerar as tabelas do banco de dados). Já no database first, temos o inverso (as tabelas do banco são utilizadas para gerarmos as classes do projeto).

Eu vejo o code first sendo muito mais utilizado do que o database first, por isso nós utilizaremos essa sistemática durante toda essa série. Assim que terminarmos toda a explicação do code first, utilizaremos o banco gerado pelo Entity Framework em um exemplo com database first.

**Exemplo base:**  *dados em memória*

Para interagirmos com o banco de dados, criaremos uma aplicação muito simples utilizando Windows Forms. Essa aplicação terá inicialmente duas classes de domínio: CategoriaProduto e Produto. Mais para a frente expandiremos esse domínio para abordarmos funcionalidades mais avançadas do Entity Framework.

**Exemplo:** *simples com Code First*

Agora que nós já temos a nossa lista de categorias e produtos armazenadas em memória, vamos ver como nós podemos fazer para gerarmos um banco de dados com essa estrutura utilizando o Entity Framework. A primeira coisa que temos que fazer é criarmos um contexto. O contexto terá a lista de entidades que serão gerenciadas (no nosso caso, CategoriaProduto e Produto), bem como configurações adicionais do Entity Framework.

A classe de contexto deve sempre herdar de DbContext (que é uma classe base do Entity Framework). Dentro do contexto, devemos criar uma propriedade “DbSet” para cada entidade. Esses “DbSets” serão, em linhas gerais, as tabelas do banco de dados. No nosso caso, nós teremos um DbSet para a classe CategoriaProduto e outro DbSet para a classe Produto. Além disso, no construtor do contexto, nós adicionaremos as categorias padrão caso a tabela de categorias ainda esteja vazia:

