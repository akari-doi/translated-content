---
title: Introdução Express/Node
slug: Learn/Server-side/Express_Nodejs/Introduction
tags:
  - Aprender
  - Express
  - Iniciante
  - JavaScript
  - Node
  - Servidor
  - Tutorial
  - nodejs
translation_of: Learn/Server-side/Express_Nodejs/Introduction
original_slug: Learn/Server-side/Express_Nodejs/Introdução
---
<div>{{LearnSidebar}}</div>

<div>{{NextMenu("Learn/Server-side/Express_Nodejs/development_environment", "Learn/Server-side/Express_Nodejs")}}</div>

<p class="summary">Neste primeiro artigo sobre Express responderemos as questões " O que é Node?" e "O que é Express?", além de dar a você uma visão geral sobre o que torna o Express um framework web tão especial. Vamos descrever as principais características e mostrar alguns dos principais blocos de códigos de construção de um aplicativo Express (embora neste momento você ainda não tenha um ambiente de desenvolvimento para testá-lo).</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">Pré-requisitos:</th>
   <td>Conhecimentos básicos em informática. Uma compreensão geral de <a href="https://developer.mozilla.org/pt-BR/docs/Learn/Server-side/First_steps">programação web no lado servidor (backend)</a>, em particular, nos mecanismos de <a href="/pt-BR/docs/Learn/Server-side/First_steps/Client-Server_overview">interação cliente-servidor de websites</a>.</td>
  </tr>
  <tr>
   <th scope="row">Objetivos:</th>
   <td>Familiarizar-se com Express, como este framework trabalha junto ao Node, quais as funcionalidades que fornece e quais são os principais blocos de construção de um aplicativo Express.</td>
  </tr>
 </tbody>
</table>

<h2 id="O_que_é_Express_e_Node">O que é Express e Node ?</h2>

<p><a href="https://nodejs.org/">Node</a> (ou formalmente <em>Node.js</em>) é um ambiente em tempo de execução open-source (código aberto) e multiplataforma que permite aos desenvolvedores criarem todo tipo de aplicativos e ferramentas do lado servidor (backend) em <a href="/pt-BR/docs/Glossary/JavaScript">JavaScript</a>.  Node é usado fora do contexto de um navegador (ou seja executado diretamente no computador ou no servidor). Como tal, o ambiente omite APIs JavaScript específicas do navegador e adiciona suporte para APIs de sistema operacional mais tradicionais, incluindo bibliotecas de sistemas HTTP e arquivos.</p>

<p>Do ponto de vista do desenvolvimento de um servidor web, o Node possui vários benefícios:</p>

<ul>
 <li>Performance excelente. Node foi projetado para otimizar a taxa de transferência e a escalabilidade em aplicações web. É uma ótima combinação para resolver muitos problemas comuns no desenvolvimento da web (por exemplo, aplicações em tempo real).</li>
 <li>O código é escrito em "JavaScript simples e antigo". Isso significa menos tempo gasto para lidar com mudanças de código entre navegador e servidor web, não sendo necessária uma mudança na linguagem.</li>
 <li>JavaScript é uma linguagem de programação relativamente nova e apresenta algumas vantagens quando comparadas a outras linguagens tradicionais de servidor (por exemplo Python, PHP, etc.). Muitas outras linguagens novas e populares compilam/convertem em JavaScript, permitindo que você também use essas linguagens, como TypeScript, CoffeeScript, ClosureScript, Scala, LiveScript, etc.</li>
 <li>O Gerenciador de Pacotes do Node (NPM, na sigla em inglês) provê acesso a centenas de milhares de pacotes reutiliváveis. NPM possui a melhor coleção de dependências e também pode ser usado para automatizar a maior parte da cadeia de ferramentas de compilação.</li>
 <li>É portátil, com versões para diferentes sistemas operacionais, como Microsoft Windows, OS X, Linux, Solaris, FreeBSD, OpenBSD, WebOS e NonStop. Além disso, tem excelente suporte de muitos provedores de hospedagem na web, que muitas vezes fornecem documentação e infraestrutura específica para hospedar sites desenvolvidos em Node.</li>
 <li>Possui uma comunidade de desenvolvedores e um ecossistema muito ativo, com muitas pessoas dispostas a ajudar.</li>
</ul>

<p>Você pode utilizar o Node.js para criar um simples servidor web, utilizando o pacote Node HTTP.</p>

<h3 id="Olá_Node.js">Olá, Node.js</h3>

<p>O exemplo a seguir cria um servidor web que escuta qualquer tipo de requisição HTTP na URL  <code>http://127.0.0.1:8000/</code>  --  quando uma requisição é recebida, o script vai responder com a string (texto) "Olá Mundo". Se você já instalou o Node, você pode seguir os passos seguintes deste exemplo.</p>

<ol>
 <li>Abre o Terminal (no Windows, abra o prompt da linha de comando)</li>
 <li>Crie uma pasta onde você quer salvar o programa, por exemplo, <code>test-node</code>. Então, entre na pasta com o seguinte comando no terminal:</li>
</ol>

<pre class="notranslate">cd test-node
</pre>

<p>Use o seu editor de texto preferido, crie um arquivo chamado <code>hello.js</code> e cole o código a seguir:</p>

<pre class="brush: js notranslate">// Carrega o modulo HTTP do Node
var http = require("http");

// Cria um servidor HTTP e uma escuta de requisições para a porta 8000
http.createServer(function(request, response) {

  // Configura o cabeçalho da resposta com um status HTTP e um Tipo de Conteúdo
   response.writeHead(200, {'Content-Type': 'text/plain'});

   // Manda o corpo da resposta "Olá Mundo"
   response.end('Olá Mundo\n');
}).listen(8000, '127.0.0.1');

// Imprime no console a URL de acesso ao servidor
console.log('Servidor executando em http://127.0.0.1:8000/');</pre>

<p>Salve o arquivo na pasta que você criou acima.</p>

<p>Por último, vá para o terminal e digite o comando a seguir:</p>

<p><code>node hello.js</code></p>

<p>Enfim, abra o browser e digite <code>http://localhost:8000.</code> Você verá o texto "<strong>Olá, Mundo</strong>", no canto superior esquerdo.</p>

<h2 id="Web_Frameworks">Web Frameworks</h2>

<p>Algumas tarefas comuns no desenvolvimento web não são suportadas diretamente pelo Node. Se você quiser que a sua aplicação possua diferentes verbos HTTP (por exemplo <code>GET</code>, <code>POST</code>, <code>DELETE</code>, etc), que gerencie requisições de diferentes URLs ("rotas"), apresente arquivos estáticos ou utilize templates para mostrar as respostas (response) de maneira dinâmica, você não terá muita praticidade usando apenas o Node. Você terá duas opções. Escrever o código por conta própria ou então evitar todo esse trabalho de reinventar a roda ao utilizar um framework.</p>

<h2 id="Introduzindo_o_Express">Introduzindo o Express</h2>

<p><a href="https://expressjs.com/">Express</a> é o framework Node mais popular e a biblioteca subjacente para uma série de outros frameworks do Node. O Express oferece soluções para:</p>

<ul>
 <li>Gerenciar requisições de diferentes verbos HTTP em diferentes URLs.</li>
 <li>Integrar "view engines" para inserir dados nos templates.</li>
 <li>Definir as configurações comuns da aplicação web, como a porta a ser usada para conexão e a localização dos modelos que são usados para renderizar a resposta.</li>
 <li>Adicionar novos processos de requisição por meio de "middleware" em qualquer ponto da "fila" de requisições.</li>
</ul>

<p>O <em>Express </em>é bastante minimalista, no entanto, os desenvolvedores têm liberdade para criar pacotes de middleware específicos com o objetivo de resolver problemas específicos que surgem no desenvolvimento de uma aplicação. Há bibliotecas para trabalhar com cookies, sessões, login de usuários, parâmetros de URL, dados em requisições POST, cabeçalho de segurança e tantos outros. Você pode achar uma lista de pacotes de middleware mantidos pela equipe Express em <a href="http://expressjs.com/en/resources/middleware.html">Express Middleware</a> (juntamente com uma lista de pacotes populares desenvolvidos por terceiros).</p>

<div class="note">
<p><strong>Nota:</strong> Essa flexibilidade do Express é uma espada de dois gumes. Há pacotes de middleware para resolver quase qualquer problema ou requisito ao longo do desenvolvimento, mas utilizar os pacotes corretos para cada situação às vezes se torna um grande desafio. Não há "caminho certo" para estruturar um aplicativo. Muitos exemplos que você encontra na Internet não são bons ou mostram apenas uma pequena parte do que você precisa fazer para desenvolver uma aplicação web.</p>
</div>

<h2 id="De_onde_o_Node_e_o_Express_vieram">De onde o Node e o Express vieram?</h2>

<p>Node foi inicialmente lançado em 2009, mas naquela época apenas para Linux. O gerenciador de pacotes NPM veio no ano seguinte, 2010, e o suporte nativo do Windows chegou em 2012. A versão atual do Long Term Support (LTS) é o Node v8.9.3, enquanto a versão mais recente é o Node 9. Esse é um resumo da rica histórica do Node, mas é possível conhecer mais na <a href="https://pt.wikipedia.org/wiki/Node.js#History">Wikipédia</a>.</p>

<p>O Express foi lançado em novembro de 2010 e atualmente está na versão 4.16 da API. Você pode verificar o  <a href="https://expressjs.com/en/changelog/4x.html">changelog</a>  para obter informações sobre as mudanças na versão atual e o <a href="https://github.com/expressjs/express/blob/master/History.md">GitHub</a> para obter notas detalhadas das versões históricas.</p>

<h2 id="O_quão_popular_é_NodeExpress">O quão popular é Node/Express ?</h2>

<p>É importante considerar a popularidade de um framework web porque indica se a ferramenta continuará sendo mantida e atualizada, além de apontar quais recursos provavelmente estarão disponíveis na documentação, nas bibliotecas de complemento e no suporte técnico.</p>

<p>Não há nenhum número capaz de medir precisamente a popularidade de um framework (apesar de que alguns sites como o <a href="http://hotframeworks.com/">Hot Frameworks</a> avaliarem a popularidade a partir do número de projetos do GitHub e do número de perguntas do StackOverflow relativas a cada tecnologia). Diante dessa limitação, o mais importante é fazermos algumas outras perguntas para saber se o Node e o Express são "suficientemente populares" para não caírem nos problemas clássicos das tecnologias com pouca adesão da comunidade.</p>

<p>O Node e o Express continuam a evoluir ? Você pode obter ajuda na comunidade caso precise? Existe uma oportunidade para você receber trabalho remunerado ao dominar o Node e o Express ?</p>

<p>Baseado no <a href="https://expressjs.com/en/resources/companies-using-express.html">número de empresas de alto perfil</a> que usam Express, no número de pessoas contribuindo para o código base, e no número de pessoas que oferecem suporte (gratuito ou pago), a reposta é sim. O Node e o Express são tecnologias populares!</p>

<h2 id="Express_é_opinativo">Express é opinativo ?</h2>

<p>Os frameworks web costumam se autodeclararem "opinativos" ou "não opinativos".</p>

<p>Os frameworks opinativos são aqueles com "opiniões" sobre o "caminho certo" para lidar com qualquer tarefa específica. Muitas vezes, apoiam o desenvolvimento rápido em um domínio particular (resolvendo problemas de um tipo específico) porque a maneira correta de fazer qualquer coisa geralmente é bem compreendida e bem documentada. No entanto, são menos flexíveis na resolução de problemas fora de seu domínio principal e tendem a oferecer menos opções para quais componentes e abordagens podem usar nesses casos.</p>

<p>Frameworks não opinativos, por outro lado, têm muito menos restrições sobre a melhor maneira de utilizar componentes para atingir um objetivo, ou mesmo quais componentes devem ser usados. Eles tornam mais fácil para os desenvolvedores usar as ferramentas mais adequadas para completar uma tarefa específica, embora você precise encontrar esses componentes por si próprio.</p>

<p>Express é um framework não opinativo. Você pode inserir qualquer middleware que você goste no manuseio das solicitações em quase qualquer ordem que desejar. Pode estruturar o aplicativo em um arquivo ou em vários, usar qualquer estrutura de pastas dentro do diretório principal, etc.</p>

<h2 id="Como_se_parece_o_código_do_Express">Como se parece o código do Express ?</h2>

<p>Em um site tradicional baseado em dados, um aplicativo da Web aguarda pedidos HTTP do navegador da web (ou outro cliente). Quando um pedido é recebido, o aplicativo descreve quais ações são necessárias com base no padrão de URL e possivelmente informações associadas contidas em dados POST ou GET. Dependendo do que é necessário, pode-se ler ou escrever informações em um banco de dados ou executar outras tarefas necessárias para satisfazer a solicitação. O aplicativo retornará uma resposta ao navegador da Web, criando, de forma dinâmica, uma página HTML para o navegador, exibindo e inserindo os dados recuperados em espaços reservados em um modelo HTML.</p>

<p>Express fornece métodos para especificar qual função é chamada quando chega requisição HTTP (GET, POST, SET, etc.) e de rotas e métodos para especificar o mecanismo de modelo ("view") usado, onde o modelo arquivos estão localizados e qual modelo usar para renderizar uma resposta. Você pode usar o middleware Express para adicionar suporte para cookies, sessões e usuários, obtendo parâmetros POST / GET, etc. Você pode usar qualquer mecanismo de banco de dados suportado por Node (o Express não define nenhum comportamento relacionado a banco de dados).</p>

<p>As seções a seguir explicam algumas das coisas comuns que você verá ao trabalhar com o código Express e Node.</p>

<h3 id="Olá_Mundo_Express">Olá Mundo Express</h3>

<p>Primeiro, considere o padrão do exemplo do Express <a href="http://expressjs.com/pt-br/starter/hello-world.html">Olá Mundo</a> (discutiremos cada trecho do código nas seções abaixo e nas seções a seguir).</p>

<div class="note">
<p><strong>Dica:</strong> Se você tiver o Node e o Express já instalados (ou se você os instalar como mostrado no <a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/development_environment">próximo artigo</a>, você pode salvar este código em um arquivo chamado <strong>app.js</strong> e executá-lo em um prompt, ao digitar o comando <code>node app.js</code>.</p>
</div>

<pre class="brush: js notranslate">var express = require('express');
var app = express();

<strong>app.get('/', function(req, res) {
  res.send('Olá Mundo!');
});</strong>

app.listen(3000, function() {
  console.log('App de Exemplo escutando na porta 3000!');
});
</pre>

<p>As duas primeiras linhas <code>require()</code> importam o módulo Express e criam uma aplicação <a href="https://expressjs.com/en/4x/api.html#app">Express</a>. Esse objeto (tradicionalmente nomeado de <code>app</code>), tem métodos de roteamento de requisições HTTP, configurações de middleware, renderização de views HTML, registro de engines de templates e modificação das <a href="https://expressjs.com/en/4x/api.html#app.settings.table">configurações</a> que controlam como o aplicativo se comporta (por exemplo, o modo de ambiente, se as definições de rota são sensíveis a maiúsculas e minúsculas, etc).</p>

<p>A parte do meio do código (as três linhas que começam com <code>app.get</code>) mostra uma definição de rota. O método <code>app.get()</code> especifica uma função de retorno de chamada que será invocada sempre que exista uma solicitação HTTP <code>GET</code> com um caminho (<code>'/'</code>) relativo à raiz do site. A função de retorno de chamada requer uma solicitação e um objeto de resposta como argumentos, e simplesmente chama <code><a href="https://expressjs.com/en/4x/api.html#res.send">send()</a></code> na resposta para retornar a string "Olá Mundo!"</p>

<p>O bloco final inicia o servidor na porta '3000' e imprime um comentário de log no console. Com o servidor em execução, você pode acessar o <code>localhost:3000</code> em seu navegador para ver o exemplo de resposta retornado.</p>

<h3 id="Importando_e_criando_módulos">Importando e criando módulos</h3>

<p>Um módulo é uma biblioteca/arquivo de JavaScript que você pode importar para outro código usando a função <code>require()</code> do Node. Express por si é um módulo, assim como as bibliotecas de middleware e banco de dados que usamos em nossos aplicativos Express.</p>

<p>O código abaixo mostra como importamos um módulo por nome, usando o quadro Express como um exemplo. Primeiro invocamos a função <code style="font-style: normal; font-weight: normal;">require()</code>, especificando o nome do módulo como uma string (<code>'express'</code>), e chamando o objeto retornado para criar um <a href="https://expressjs.com/en/4x/api.html#app">aplicativo Express</a>. Podemos então acessar as propriedades e funções do objeto da aplicação.</p>

<pre class="brush: js notranslate">var express = require('express');
var app = express();
</pre>

<p>Você também pode criar seus próprios módulos para serem importados da mesma maneira.</p>

<div class="note">
<p><strong>Dica:</strong> Você vai <em><strong>querer</strong></em> criar seus próprios módulos porque isso permite que você organize seu código em peças gerenciáveis - um aplicativo monolítico (de arquivo único) é difícil de entender e manter. O uso de módulos também ajuda você a gerenciar o namespace, pois somente as variáveis que você exporta explicitamente são importadas quando você usa um módulo.</p>
</div>

<p>Para tornar os objetos disponíveis fora do módulo, você precisa apenas atribuí-los ao objeto <code>exports</code>. Por Exemplo, o módulo <strong>square.js</strong> abaixo é um arquivo que exporta os métodos <code>area()</code> e <code>perimeter()</code>:</p>

<pre class="brush: js notranslate">exports.area = function(width) { return width * width; };
exports.perimeter = function(width) { return 4 * width; };
</pre>

<p>Nós podemos importar este módulo usando <code>require()</code>. Depois, conecte ao(s) método(s) exportado(s) como mostrado a seguir:</p>

<pre class="brush: js notranslate">var square = require('./square'); // Chamamos o arquivo utilizando o require()
console.log('The area of a square with a width of 4 is ' + square.area(4));</pre>

<div class="note">
<p><strong>Nota:</strong> Você também pode especificar um caminho absoluto para o módulo (ou um nome, como fizemos inicialmente).</p>
</div>

<p>Se você deseja exportar um objeto completo em uma atribuição, em vez de criar uma propriedade de cada vez, atribua ao module.exports como mostrado abaixo (você também pode fazer isso para tornar a raiz do objeto exporter um construtor ou outra função):</p>

<pre class="brush: js notranslate">module.exports = {
  area: function(width) {
    return width * width;
  },

  perimeter: function(width) {
    return 4 * width;
  }
};
</pre>

<p>Para muitas outras informações sobre módulos veja <a href="https://nodejs.org/api/modules.html#modules_modules">Módulos</a> (Node API docs).</p>

<h3 id="Usando_APIs_assíncronas">Usando APIs assíncronas</h3>

<p>O código JavaScript frequentemente usa APIs assíncronas em vez de síncronas para operações que podem levar algum tempo para serem concluídas. Uma API síncrona é aquela em que cada operação deve ser concluída antes que a próxima operação seja iniciada. Por exemplo, as seguintes funções de log são síncronas e imprimirão o texto no console em ordem (Primeiro, Segundo).</p>

<pre class="brush: js notranslate">console.log('Primeiro');
console.log('Segundo');</pre>


<div>Em contrapartida, uma API assíncrona é aquela em que a API iniciará uma operação e retornará imediatamente (antes da conclusão da operação). Assim que a operação terminar, a API usará algum mecanismo para executar operações adicionais. Por exemplo, o código abaixo imprimirá "Segundo, Primeiro". Isso porque, mesmo que o método <code>setTimeout()</code> seja chamado primeiro e retornae imediatamente, a operação precisa de três segundos para finalizar.</div>


<pre class="brush: js notranslate">setTimeout(function() {
   console.log('Primeiro');
   }, 3000);
console.log('Segundo');
</pre>

<p>O uso de APIs assíncronas não bloqueadoras é ainda mais importante no Node do que no navegador, pois o Node é um ambiente de execução orientado por evento único (single threaded). "Single threaded" significa que todos os pedidos para o servidor são executados no mesmo tópico (em vez de serem gerados em processos separados). Esse modelo é extremamente eficiente em termos de velocidade e recursos do servidor, mas isso significa que, se qualquer uma das suas funções chamar métodos síncronos que demoram muito para completar, eles bloquearão não apenas a solicitação atual, mas todas as outras solicitações serão tratadas por sua aplicação web.</p>

<p>Há várias maneiras de uma API assíncrona notificar para a aplicação que alguma função chegou ao fim. A maneira mais comum é registrar uma função de retorno de chamada quando você invoca a API assíncrona, que será chamada de volta quando a operação for concluída. Usamos essa abordagem acima.</p>

<div class="note">
<p><strong>Dica:</strong> O uso de callbacks pode ser bastante "bagunçado" se você tiver uma sequência de operações assíncronas dependentes que devem ser executadas em ordem, porque isto resulta em multiplo níveis de callbacks aninhados. Este problema é comumente conhecido como "inferno de callback" ou "código hadouken". Pode-se reduzir o problema ao adotar boas práticas de programação (veja <a href="http://callbackhell.com/">http://callbackhell.com/</a>), utilizar um módulo como <a href="https://www.npmjs.com/package/async">async</a>, ou mesmo adotar recursos do ES6, como <a href="/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise">Promises</a>.</p>
</div>

<div class="note">
<p><strong>Dica:</strong> Uma convenção comum para Node e Express é usar as devoluções de retorno de erro. Nesta convenção, o primeiro valor em suas funções de retorno de chamada é um valor de erro, enquanto os argumentos subseqüentes contêm dados de sucesso. Há uma boa explicação de por que essa abordagem é útil neste blog: <a href="http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js">The Node.js Way - Understanding Error-First Callbacks</a> (fredkschott.com).</p>
</div>

<h3 id="Criando_manipuladores_de_rotas">Criando manipuladores de rotas</h3>

<p>No nosso <em>Olá Mundo</em> em Express (veja acima), nós definimos uma (callback) função manipuladora de rota para requisição <code>GET</code> HTTP para a raiz do site (<code>'/'</code>).</p>

<pre class="brush: js notranslate">app.<strong>get</strong>('/', function(req, res) {
  res.send('Olá Mundo');
});
</pre>

<p>A função de retorno de chamada requer uma solicitação e um objeto de resposta como argumento. Neste caso, o método simplesmente chama <code><a href="https://expressjs.com/en/4x/api.html#res.send">send()</a></code> na resposta para retornar a string "Olá Mundo!" <a href="https://expressjs.com/en/guide/routing.html#response-methods">Há uma série de outros métodos de resposta</a> para encerrar o ciclo de solicitação / resposta, por exemplo, você poderia chamar <code><a href="https://expressjs.com/en/4x/api.html#res.json">res.json()</a></code> para enviar uma resposta JSON ou <code><a href="https://expressjs.com/en/4x/api.html#res.sendFile">res.sendFile()</a></code>  para enviar um arquivo.</p>

<div class="note">
<p><strong>Dica JavaScript:</strong> Você pode usar qualquer argumento que você gosta nas funções de retorno de chamada. Quando o retorno de chamada é invocado, o primeiro argumento sempre será o pedido e o segundo sempre será a resposta. Faz sentido nomeá-los de tal forma que você possa identificar o objeto que você está trabalhando no corpo do retorno de chamada.</p>
</div>

<p>O Express também fornece métodos para definir manipuladores de rotas para todas as outras requisições HTTP, que são usadas exatamente da mesma maneira:  <code>post()</code>, <code>put()</code>, <code>delete()</code>, <code>options()</code>, <code>trace()</code>, <code>copy()</code>, <code>lock()</code>, <code>mkcol()</code>, <code>move()</code>, <code>purge()</code>, <code>propfind()</code>, <code>proppatch()</code>, <code>unlock()</code>, <code>report()</code>, <code>mkactivity()</code>, <code>checkout()</code>, <code>merge()</code>, <code>m-</code><code>search()</code>, <code>notify()</code>, <code>subscribe()</code>, <code>unsubscribe()</code>, <code>patch()</code>, <code>search()</code>, e <code>connect()</code>.</p>

<p>Há um método de roteamento especial, <code>app.all()</code>, que será chamado em resposta a qualquer método HTTP. É usado para carregar funções de middleware em um caminho específico para todos os métodos de solicitação. O exemplo a seguir (da documentação Express) mostra um manipulador que será executado para solicitações <code>/secret</code>, independentemente do verbo HTTP usado (desde que seja suportado pelo módulo http).</p>

<pre class="brush: js notranslate">app.all('/secret', function(req, res, next) {
  console.log('Acessando a sessão secreta...');
  next(); // passa o controle para o próximo manipulador
});</pre>

<p>As rotas permitem combinar padrões de caracteres específicos em um URL e extrair alguns valores do URL e passá-los como parâmetros para o manipulador de rotas (como atributos do objeto de solicitação passado como parâmetro).</p>

<p>Muitas vezes, é útil agrupar manipuladores de rotas para uma determinada parte de um site e acessá-los usando um prefixo de rota comum (por exemplo, um site com um Wiki pode ter todas as rotas relacionadas ao wiki em um arquivo e tê-los acessado com um prefixo de rota de / wiki /). Em Express, isso é alcançado usando o objeto <code><a href="http://expressjs.com/en/guide/routing.html#express-router">express.Router</a></code>. Por exemplo, podemos criar nossa rota wiki em um módulo chamado wiki.js e, em seguida, exportar o objeto <code>Router</code>, conforme mostrado abaixo:</p>

<pre class="brush: js notranslate">// wiki.js - Rotas de Wiki

var express = require('express');
var router = express.Router();

// Home page route
router.get('/', function(req, res) {
  res.send('Wiki home page');
});

// About page route
router.get('/about', function(req, res) {
  res.send('About this wiki');
});

module.exports = router;
</pre>

<div class="note">
<p><strong>Nota:</strong> Adicionar rotas ao objeto <code>Router</code> é como adicionar rotas ao objeto <code>app</code> (como mostrado anteriormente).</p>
</div>

<p>Para usar o roteador em nosso arquivo de aplicativo principal, então, <code>require()</code> o módulo de rota (<strong>wiki.js</strong>) e depois <code>use()</code> no aplicativo Express para adicionar o Router ao caminho de gerenciamento de middleware. As duas rotas serão acessíveis a partir de <code style="font-style: normal; font-weight: normal;">/wiki/</code> e <code style="font-style: normal; font-weight: normal;">/wiki/about/</code>.</p>

<pre class="brush: js notranslate">var wiki = require('./wiki.js');
// ...
app.use('/wiki', wiki);</pre>

<p>Vamos mostrar-lhe muito mais sobre trabalhar com rotas e, em particular, sobre o uso do <code>Router</code>, mais tarde, na seção vinculada <a href="https://developer.mozilla.org/pt-BR/docs/Learn/Server-side/Express_Nodejs/routes">Rotas e controladores</a>.</p>

<h3 id="Usando_middleware">Usando middleware</h3>

<p>O Middleware é usado extensivamente em aplicativos Express para que as tarefas ofereçam arquivos estáticos ao tratamento de erros, a comprensão de respostas HTTP. Enquanto as funções de rota terminam o ciclo de solicitação-resposta HTTP, retornando alguma resposta ao cliente HTTP, as funções de middleware normalmente executam alguma operação na solicitação ou resposta e, em seguida, ligue para a próxima função na "pilha", que pode ser mais um middleware ou uma rota manipuladora. A ordem em que o middleware é chamado depende do desenvolvedor do aplicativo.</p>

<div class="note">
<p><strong>Nota:</strong> O middleware pode executar qualquer operação, executar qualquer código, fazer alterações no objeto de solicitação e resposta, e também pode encerrar o ciclo de solicitação-resposta. Se não terminar o ciclo, ele deve chamar o <code>next()</code> para passar o controle para a próxima função de middleware (ou a solicitação ficará pendurada).</p>
</div>

<p>A maioria dos aplicativos usará middleware de terceiros para simplificar tarefas comuns de desenvolvimento web, como trabalhar com cookies, sessões, autenticação de usuários, acessar dados <code>POST</code> e JSON, log, etc. Você pode encontrar uma<a href="http://expressjs.com/en/resources/middleware.html"> lista de pacotes de middleware</a> mantidos pela equipe Express (que também inclui outros pacotes populares de terceiros). Outros pacotes Express estão disponíveis no gerenciador de pacotes do NPM.</p>

<p>Para usar middleware de terceiros, primeiro você precisa instalá-lo em seu aplicativo usando NPM. Por exemplo, para instalar o logger <a href="http://expressjs.com/en/resources/middleware/morgan.html">morgan</a> HTTP, você faria isso:</p>

<pre class="brush: bash notranslate"><code>$ npm install morgan
</code></pre>

<p>Você pode então chamar <code>use()</code> no objeto do aplicativo Express para adicionar o middleware à pilha:</p>

<pre class="brush: js notranslate">var express = require('express');
<strong>var logger = require('morgan');</strong>
var app = express();
<strong>app.use(logger('dev'));</strong>
...</pre>

<div class="note">
<p><strong>Nota:</strong> O middleware e as funções de roteamento são chamadas na ordem em que são declaradas. Para alguns middleware, a ordem é importante (por exemplo, se o middleware de sessão depende do middleware de cookies, então o manipulador de cookies deve ser adicionado primeiro). É quase sempre o caso em que o middleware é chamado antes de definir rotas, ou seus manipuladores de rotas não terão acesso à funcionalidade adicionada pelo seu middleware.</p>
</div>

<p>Você pode escrever suas próprias funções de middleware. É provável que você tenha que fazê-lo (somente para criar código de manipulação de erro). A única diferença entre uma função de middleware e um retorno de chamada de manipulador de rotas é que as funções de middleware têm um terceiro argumento <code>next</code>, que as funções de middleware devem chamar se não completam o ciclo de solicitação (quando a função de middleware é chamada, isso contém a próxima função que deve ser chamado).</p>

<p>Você pode adicionar uma função de middleware à cadeia de processamento com <code>app.use()</code> ou <code>app.add()</code>, dependendo se você deseja aplicar o middleware a todas as respostas ou a respostas com um verbo HTTP específico (<code>GET</code>, <code>POST</code>, etc. ). Você especifica rotas o mesmo em ambos os casos, embora a rota seja opcional ao chamar <strong>app.use()</strong>.</p>

<p>O exemplo abaixo mostra como você pode adicionar a função middleware usando ambos os métodos e com/sem rota.</p>

<pre class="brush: js notranslate">var express = require('express');
var app = express();

// Um exemplo de função middleware
var a_middleware_function = function(req, res, <em>next</em>) {
  // ... Executa alguma operação
  next(); // next() Chama o próximo middleware ou função de rotas
}

// Função adicionada com use() para todas rotas e requisições
app.use(a_middleware_function);

// Função adicionada com use() para uma rota específica
app.use('/someroute', a_middleware_function);

// função middleware adicionado para uma rota e requisição específica
app.get('/', a_middleware_function);

app.listen(3000);</pre>

<div class="note">
<p><strong>Dica JavaScript:</strong> Acima, declaramos a função de middleware separadamente e, em seguida, configuramos como retorno de chamada. Na nossa função anterior do operador de rotas, declaramos a função de retorno de chamada quando foi utilizada. Em JavaScript, ambas abordagens são válidas.</p>
</div>

<p>A documentação Express possui uma documentação excelente sobre como usar e escrever o middleware Express.</p>

<h3 id="Servindo_arquivos_estáticos">Servindo arquivos estáticos</h3>

<p>Você pode usar o middleware <a href="http://expressjs.com/en/4x/api.html#express.static">express.static</a> para servir arquivos estáticos, incluindo suas imagens, CSS e JavaScript (<code>static()</code> é a única função de middleware que é realmente parte do Express). Por exemplo, você usaria a linha abaixo para exibir imagens, arquivos CSS e arquivos JavaScript de um diretório chamado 'public' no mesmo nível onde você chama o nó:</p>

<pre class="brush: js notranslate">app.use(express.static('public'));
</pre>

<p>Todos os arquivos no diretório público são atendidos adicionando o nome do arquivo (relativo ao diretório "público" base) ao URL base. Então, por exemplo:</p>

<pre class="notranslate"><code>http://localhost:3000/images/dog.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/about.html
</code></pre>

<p>Você pode chamar <code>static()</code> várias vezes para atender vários diretórios. Se um arquivo não puder ser encontrado por uma função de middleware, ele simplesmente será transmitido ao middleware subsequente (a ordem em que o middleware é chamado é baseada em sua ordem de declaração).</p>

<pre class="brush: js notranslate">app.use(express.static('public'));
app.use(express.static('media'));
</pre>

<p>Você também pode criar um prefixo virtual para seus URL estáticos, em vez de ter os arquivos adicionados ao URL base. Por exemplo, aqui <a href="http://expressjs.com/en/4x/api.html#app.use">especificamos um caminho de montagem</a> para que os arquivos sejam carregados com o prefixo "/media":</p>

<pre class="brush: js notranslate">app.use('/media', express.static('public'));
</pre>

<p>Agora, você pode carregar os arquivos que estão no diretório <code>public</code> a partir do prefixo de caminho <code>/media</code>.</p>

<pre class="notranslate"><code>http://localhost:3000/media/images/dog.jpg
http://localhost:3000/media/video/cat.mp4
http://localhost:3000/media/cry.mp3</code>
</pre>

<p>Para obter mais informações, consulte <a href="Serving static files in Express">Servindo arquivos estáticos no Express</a>.</p>

<h3 id="Erros_de_manipulação">Erros de manipulação</h3>

<p>Os erros são tratados por uma ou mais funções de middleware especiais que possuem quatro argumentos, em vez dos três habituais: <code>(err, req, res, next)</code>. Por exemplo:</p>

<pre class="brush: js notranslate">app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
</pre>

<p>Isso pode retornar qualquer conteúdo exigido, mas deve ser chamado depois de todas as outras chamadas <code>app.use()</code> e rotas para que sejam o último middleware no processo de solicitação de pedidos!</p>

<p>Express vem com um manipulador de erros embutido, que cuida de todos os erros que podem ser encontrados no aplicativo. Essa função de middleware de gerenciamento de erros padrão é adicionada no final da pilha de funções do middleware. Se você passar um erro para <code>next()</code> e você não lidar com isso em um manipulador de erro, ele será tratado pelo manipulador de erros incorporado; o erro será gravado no cliente com o rastreamento da pilha.</p>

<div class="note">
<p><strong>Nota:</strong> O rastreamento da pilha não está incluído no ambiente de produção. Para executá-lo no modo de produção, você precisa configurar a variável de ambiente <code>NODE_ENV</code> para <code>'production'</code>.</p>
</div>

<div class="note">
<p><strong>Nota:</strong> HTTP404 e outros códigos de status de "erro" não são tratados como erros. Se você quiser lidar com isso, você pode adicionar uma função de middleware para fazê-lo. Para mais informações, consulte as <a href="http://expressjs.com/en/starter/faq.html#how-do-i-handle-404-responses">FAQ</a>.</p>
</div>

<p>Para obter mais informações, consulte <a href="http://expressjs.com/en/guide/error-handling.html">Gerenciamento de erros</a> (Express docs).</p>

<h3 id="Usando_Banco_de_Dados">Usando Banco de Dados</h3>

<p>Aplicativos Express podem usar qualquer mecanismo de banco de dados suportado pelo Node (o Express em si não define nenhum comportamento/requisitos adicionais específicos para gerenciamento de banco de dados). Existem muitas opções, incluindo PostgreSQL, MySQL, Redis, SQLite, MongoDB, etc.</p>

<p>Para usá-los, você deve primeiro instalar o driver do banco de dados usando NPM. Por exemplo, para instalar o driver para o popular NoSQL MongoDB você usaria o comando:</p>

<pre class="brush: bash notranslate"><code>$ npm install mongodb
</code></pre>

<p>O próprio banco de dados pode ser instalado localmente ou em um servidor em nuvem. No seu código Express, você precisa do driver, conecte-se ao banco de dados e execute as operações criar, ler, atualizar e excluir (CRUD). O exemplo abaixo (da documentação Express) mostra como você pode encontrar registros de "mamíferos" usando MongoDB.</p>

<pre class="brush: js notranslate">var MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/animals', function(err, db) {
  if (err) throw err;

  db.collection('mammals').find().toArray(function (err, result) {
    if (err) throw err;

    console.log(result);
  });
});</pre>

<p>Outra abordagem popular é acessar seu banco de dados indiretamente, através de um Object Relational Mapper ("ORM"). Nesta abordagem, você define seus dados como "objetos" ou "modelos" e o ORM mapeia estes para o formato de banco de dados subjacente. Esta abordagem tem o benefício de que, como desenvolvedor, você pode continuar a pensar em termos de objetos JavaScript, em vez de semântica de banco de dados, e que existe um local óbvio para realizar a validação e verificação de dados recebidos. Falaremos mais sobre bancos de dados em um artigo posterior.</p>

<p>Para obter mais informações, consulte <a href="https://expressjs.com/en/guide/database-integration.html">integração com banco de dados </a>(documentos express).</p>

<h3 id="Renderizando_dados_views">Renderizando dados (views)</h3>

<p>Os mecanismos de modelo (referidos como "view engines" por Express) permitem que você especifique a estrutura de um documento de saída em um modelo, usando marcadores de posição para os dados que serão preenchidos quando uma página for gerada. Os modelos geralmente são usados para criar HTML, mas também podem criar outros tipos de documentos. Express tem suporte para uma série de <a href="https://github.com/expressjs/express/wiki#template-engines">mecanismos de modelos</a>, e há uma comparação útil dos motores mais populares aqui: <a href="https://strongloop.com/strongblog/compare-javascript-templates-jade-mustache-dust/">Comparing JavaScript Templating Engines: Jade, Mustache, Dust and More</a>.</p>

<p>No seu código de configurações do aplicativo você configurou o mecanismo do modelo para usar e o local onde Express deve procurar modelos usando as configurações 'visualizações' e 'visualizar mecanismos', conforme mostrado abaixo (você também terá que instalar o pacote que contém a biblioteca do modelo também !)</p>

<pre class="brush: js notranslate">var express = require('express');
var app = express();

//  Definir o diretório para conter os modelos ('views')
app.set('views', path.join(__dirname, 'views'));

// Definir o motor de visualização para usar, neste caso 'some_template_engine_name'
app.set('view engine', 'some_template_engine_name');
</pre>

<p>A aparência do modelo dependerá do mecanismo que você usa. Supondo que você tenha um arquivo de modelo chamado "índice. &lt;Template_extension&gt;" que contenha espaços reservados para variáveis de dados denominadas 'título' e 'mensagem', você chamaria <code><a href="http://expressjs.com/en/4x/api.html#res.render">Response.render()</a></code> em uma função de roteador de rotas para criar e enviar a resposta HTML :</p>

<pre class="brush: js notranslate">app.get('/', function(req, res) {
  res.render('index', { title: 'About dogs', message: 'Dogs rock!' });
});</pre>

<p>Para obter mais informações, consulte <a href="http://expressjs.com/en/guide/using-template-engines.html">usando motores de modelo com Express</a> (Express docs).</p>

<h3 id="Estrutura_de_Arquivos">Estrutura de Arquivos</h3>

<p>Express não faz suposições em termos de estrutura ou quais os componentes que você usa. Rotas, visualizações, arquivos estáticos e outra lógica específica da aplicação podem viver em qualquer número de arquivos com qualquer estrutura de diretório. Embora seja perfeitamente possível ter todo o aplicativo Express em um único arquivo, geralmente faz sentido dividir seu aplicativo em arquivos com base em função (por exemplo, gerenciamento de contas, blogs, fóruns de discussão) e domínio de problema arquitetônico (por exemplo, modelo, exibição ou controlador se você está usando uma <a href="/en-US/docs/Web/Apps/Fundamentals/Modern_web_app_architecture/MVC_architecture">arquitetura MVC</a>).</p>

<p>Em um tópico posterior, usaremos o Express Application Generator, que cria um esqueleto de aplicativo modular que podemos estender facilmente para criar aplicativos da web.</p>

<ul>
</ul>

<h2 id="Sumário">Sumário</h2>

<p>Parabéns, você completou o primeiro passo em sua viagem Express/Node! Agora você deve entender os principais benefícios do Express e Node, e aproximadamente o que as principais partes de um aplicativo Express podem ser (rotas, middleware, tratamento de erros e código de modelo). Por ser um framework não opinativo, o Express permite que você defina a maneira como essas partes e essas bibliotecas são integradas.</p>

<p>Claro que Express é deliberadamente uma estrutura de aplicativos web muito leve, tanto seu benefício e potencial vem de bibliotecas e recursos de terceiros. Examinaremos essa questão com mais detalhes nos próximos artigos. No artigo a seguir, vamos analisar a criação de um ambiente de desenvolvimento de Node, para que você possa começar a ver algum código Express em ação.</p>

<h2 id="Veja_também">Veja também</h2>

<ul>
 <li><a href="https://medium.com/@ramsunvtech/manage-multiple-node-versions-e3245d5ede44">Venkat.R - Manage Multiple Node versions</a></li>
 <li><a href="https://nodejs.org/api/modules.html#modules_modules">Modules</a> (Node API docs)</li>
 <li><a href="https://expressjs.com/">Express</a> (home page)</li>
 <li><a href="http://expressjs.com/en/starter/basic-routing.html">Basic routing</a> (Express docs)</li>
 <li><a href="http://expressjs.com/en/guide/routing.html">Routing guide</a> (Express docs)</li>
 <li><a href="http://expressjs.com/en/guide/using-template-engines.html">Using template engines with Express</a> (Express docs)</li>
 <li><a href="https://expressjs.com/en/guide/using-middleware.html">Using middleware</a> (Express docs)</li>
 <li><a href="http://expressjs.com/en/guide/writing-middleware.html">Writing middleware for use in Express apps</a> (Express docs)</li>
 <li><a href="https://expressjs.com/en/guide/database-integration.html">Database integration</a> (Express docs)</li>
 <li><a href="Serving static files in Express">Serving static files in Express</a> (Express docs)</li>
 <li><a href="http://expressjs.com/en/guide/error-handling.html">Error handling</a> (Express docs)</li>
</ul>

<div>{{NextMenu("Learn/Server-side/Express_Nodejs/development_environment", "Learn/Server-side/Express_Nodejs")}}</div>

<h2 id="Próximos_módulos">Próximos módulos</h2>

<ul>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/Introduction">Introdução Express/Node </a>- Módulo Atual</li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/development_environment">Configurando um ambiente de desenvolvimento Node (Express)</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/Tutorial_local_library_website">Express Tutorial: The Local Library website</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/skeleton_website">Express Tutorial Part 2: Criando um esqueleto de website</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/mongoose">Express Tutorial Part 3: Utilizando Banco de Dados (com Mongoose)</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/routes">Express Tutorial Part 4: Rotas e Controladores</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/Displaying_data">Express Tutorial Part 5: Displaying library data</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/forms">Express Tutorial Part 6: Trabalhando com formulários</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Express_Nodejs/deployment">Express Tutorial Part 7: Deploying to production</a></li>
</ul>