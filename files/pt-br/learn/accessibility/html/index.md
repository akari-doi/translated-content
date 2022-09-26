---
title: 'HTML: Boas práticas em acessibilidade'
slug: Learn/Accessibility/HTML
tags:
  - Acessibilidade
  - Artigo
  - Código
  - Iniciante
  - aprendizado
  - botões
  - formulários
  - leitor de telas
  - semántica
  - teclado
  - tecnologia assistiva
translation_of: Learn/Accessibility/HTML
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/Accessibility/What_is_Accessibility","Learn/Accessibility/CSS_and_JavaScript", "Learn/Accessibility")}}</div>

<p class="summary">Uma grande parte do conteúdo presente na internet pode se tornar acessível apenas com a utilização correta dos elementos HTML. Esse artigo mostra em detalhes como o HTML pode ser utilizado para garantir o máximo de acessibilidade.</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">Pré-requsitos:</th>
   <td>
    <p>Conceitos básicos de computadores, entendimento básico de HTML (veja <a href="/pt-BR/docs/Aprender/HTML/Introducao_ao_HTML">Introdução ao HTML</a>), e entendimento do <a href="/en-US/docs/Learn/Accessibility/What_is_accessibility">que é acessibilidade</a>.</p>
   </td>
  </tr>
  <tr>
   <th scope="row">Objetivos:</th>
   <td>
    <p>Ganhar familiaridade com os elementos do HTML que trabalham a favor da acessibilidade e utilizá-los de forma apropriada nos seus documentos da web.</p>
   </td>
  </tr>
 </tbody>
</table>

<h2 id="HTML_e_acessibilidade">HTML e acessibilidade</h2>

<p>Na medida que se aprende mais sobre HTML — lendo sobre recursos, observando exemplos, etc. — é normal se deparar com um assunto: a importância de se utilizar a semântica HTML (as vezes chamada de POSH, ou <em>Plain Old Semantic HTML</em>). Esse assunto significa utilizar corretamente os elementos HTML, cada qual com seu propósito, o máximo que for possível.</p>

<p>Você deve estar se perguntando porque isso é tão importante. Afinal, é possível usar uma combinação de CSS e JavaScript que faz com que qualquer elemento HTML se comporte da forma que você quiser. Por exemplo, um botão para dar play em um vídeo no seu site pode ser criado dessa forma:</p>

<pre class="brush: html">&lt;div&gt;Play vídeo&lt;/div&gt;</pre>

<p>Mas como você com mais detalhes a seguir, faz muito mais sentido utilizar o elemento correto para essa finalidade:</p>

<pre class="brush: html">&lt;button&gt;Play vídeo&lt;/button&gt;</pre>

<p>O elemento  <code>&lt;button&gt;</code> possui vários estilos já aplicados nele mesmo por padrão (o que você provavelmente vai querer sobrescrever), ele já é embutido com padrões de acessibilidade pelo teclado — botões podem ser navegados através da tecla tab do teclado, e ativados utilizando a tecla Enter / Return.</p>

<p>A semântica do HTML não demora mais para escrever do que a versão não-semântica (ruim) se você escrevê-la consistentemente desde o começo do seu projeto. Existem também outros benefícios de utilizá-la, além da acessibilidade:</p>

<ol>
 <li><strong>Mais fácil de ser desenvolvida</strong> — como mencionado acima, você consegue algumas funcionalidades por padrão, também torna o código mais fácil de ser lido e entendido.</li>
 <li><strong>Melhor nos dispositivos móveis</strong> — HTML semântico é mais leve que o código não-semântico (aquele código espaguete) em comparação de tamanho de arquivos, também é mais fácil de ser adaptado ao responsivo.</li>
 <li><strong>Bom para o SEO</strong> — mecanismos de busca dão mais importância para palavras-chave que são incluidas em títulos, links, etc. do que para palavras-chave incluídas em <code>&lt;div&gt;</code>s não semânticas, etc. Então suas páginas serão mais fáceis de serem encontradas pelos seus clientes.</li>
</ol>

<p>Então vamos dar uma olhada em como fazer o HTML mais acessível.</p>

<div class="note">
<p><strong>Nota</strong>: É uma boa ideia ter um leitor de tela instalado no seu computador, dessa forma é possível testar os exemplos que serão mostrados abaixo. Veja o nosso <a href="/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Screenreaders">Guia de Leitores de Tela</a> para mais detalhes.</p>
</div>

<h2 id="Boa_semântica">Boa semântica</h2>

<p>Nós já falamos sobre a importância da boa semântica e porque precisamos utilizar os elementos HTML adequados para cada finalidade. Isso não pode ser ignorado, e é uma das grandes áreas onde a acessibilidade pode ser completamente quebrada se não feito de forma correta.</p>

<p>Em toda a web, é verdade que as pessoas fazem coisas bem estranhas utilizando o HTML. Alguns abusos do HTML são resultado de práticas antigas que ainda não foram completamente esquecidas, e outros são só simples ignorância das boas práticas. Em qualquer um desses casos, é necessário a substituição de códigos ruins por códigos bons, em qualquer local que forem encontrados, se possível.</p>

<p>As vezes você não terá em mãos o poder de jogar fora o código ruim — suas páginas web podem ter sido geradas por algum framework que você não possui controle total, ou você pode ter algum conteúdo de terceiros na sua página (como banners de publicidade) que você não tem controle sobre.</p>

<p>O objetivo aqui não é "tudo ou nada", contudo — qualquer melhoria que você for capaz de fazer irá ajudar a causa da acessibilidade.</p>

<h3 id="Conteúdo_textual">Conteúdo textual</h3>

<p>Uma das melhores formas de ajudar um leitor de tela a interpretar sua página é criar uma boa e consistente estrutura de títulos, parágrafos, listas, etc. Um exemplo de boa semântica vai ser parecido com o a seguir:</p>

<pre class="brush: html example-good line-numbers language-html"><code class="language-html"><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>h1</span><span class="punctuation token">&gt;</span></span>Meu título<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>h1</span><span class="punctuation token">&gt;</span></span>

<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>p</span><span class="punctuation token">&gt;</span></span>Essa é a primeira sessão do meu documento.<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>p</span><span class="punctuation token">&gt;</span></span>

<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>p</span><span class="punctuation token">&gt;</span></span>Eu vou adicionar outro parágrafo aqui também.<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>p</span><span class="punctuation token">&gt;

&lt;ol&gt;
  &lt;li&gt;Aqui é&lt;/li&gt;
  &lt;li&gt;uma lista para&lt;/li&gt;
  &lt;li&gt;você ler&lt;/li&gt;
&lt;/ol&gt;</span></span>

<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>h2</span><span class="punctuation token">&gt;</span></span>Meu sub-título<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>h2</span><span class="punctuation token">&gt;</span></span>

<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>p</span><span class="punctuation token">&gt;</span></span>Essa é a primeira sub sessão do meu documento. Eu adoro quando as pessoas conseguem encontrar meu conteúdo!<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>p</span><span class="punctuation token">&gt;</span></span>

<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>h2</span><span class="punctuation token">&gt;</span></span>Meu segundo sub-título<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>h2</span><span class="punctuation token">&gt;</span></span>

<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>p</span><span class="punctuation token">&gt;</span></span>Essa é a primeira sub sessão do meu documento. Eu acho que essa é mais interessante que a última.<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>p</span><span class="punctuation token">&gt;</span></span></code></pre>

<p>Nós preparamos uma versão com o texto mais longo para que você tente utilizar um leitor de tela (veja <a href="http://mdn.github.io/learning-area/accessibility/html/good-semantics.html">good-semantics.html</a>). Se você tentar navegar dentro do documento, vai perceber que é bem fácil:</p>

<ol>
 <li>O leitor de tela lê cada título a medida que você progride pelo conteúdo, notificando ao usuário o que é um título, o que é um parágrafo, etc.</li>
 <li>Ele para depois de cada elemento, deixando você ir na velocidade em que é mais confortável.</li>
 <li>Você pode pular para o título mais próximo/anterior em muitos leitores de tela.</li>
 <li>Você também pode fazer uma lista com todos os títulos em muitos leitores de tela, possibilitando a navegação em um sumário para encontrar conteúdos específicos.</li>
</ol>

<p>As pessoas ás vezes escrevem títulos, parágrafos, etc. utilizando HTML para vizualização e quebras de linha, ás vezes como o seguinte:</p>

<pre class="brush: html example-bad line-numbers  language-html"><code class="language-html"><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>font</span> <span class="attr-name token">size</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>7<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Meu título<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>font</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span>
Essa é a primeira sessão do meu documento.
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span>
Eu vou adicionar outro parágrafo aqui também.
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;
1. Aqui é
</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;
2. uma lista para
</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;
3. você ler.
</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>font</span> <span class="attr-name token">size</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>5<span class="punctuation token">"</span></span><span class="punctuation token">&gt;</span></span>Meu sub-título<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>font</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span>
Essa é a primeira sub sessão do meu documento. Eu adoro quando as pessoas conseguem encontrar meu conteúdo!
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>font</span> <span class="attr-name token">size</span><span class="attr-value token"><span class="punctuation token">=</span><span class="punctuation token">"</span>5<span class="punctuation token">"</span></span><span class="punctuation token">&gt;Meu segundo sub-título</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;/</span>font</span><span class="punctuation token">&gt;</span></span>
<span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span><span class="tag token"><span class="tag token"><span class="punctuation token">&lt;</span>br</span><span class="punctuation token">&gt;</span></span>
Essa é a primeira sub sessão do meu documento. Eu acho que essa é mais interessante que a última.</code></pre>

<p>Se você tentar utilizar um leitor de tela na nossa versão mais longa (ver <a href="http://mdn.github.io/learning-area/accessibility/html/bad-semantics.html">bad-semantics.html</a>), você não terá uma boa experiência — o leitor de tela não encontrará nenhuma sinalização, então você não terá acesso ao conteúdo. A página inteira vai parecer como um único bloco gigante, então será lida de uma vez só, ao mesmo tempo.</p>

<p>Existem também outros problemas além da acessibilidade — é mais difícil estilizar o seu conteúdo com CSS, ou manipulá-lo com JavaScript porque não há elementos para serem utilizados como seletores.</p>

<h4 id="Usando_linguagem_clara">Usando linguagem clara</h4>

<p>A linguagem que você usa também pode afetar a acessibilidade. No geral, você deve utilizar uma linguagem clara, que não é exageradamente complexa, e que não use jargões ou gírias desnecessárias. Isso não traz somente benefícios para pessoas com deficiência cognitiva, mas também beneficia pessoas que não estão lendo em sua primeira língua, jovens leitores... todo mundo, de fato! Tirando isso, você deve tentar evitar uma linguagem ou caracteres que não podem ser lidos ou entendidos bem por um leitor de tela. Por exemplo:</p>

<ul>
 <li>Não utilize traços se você pode evitá-los. Ao invés de escrever 5-7, escreva 5 a 7.</li>
 <li>Expanda as abreviações — ao invés de escrever Jan, escreva Janeiro.</li>
 <li>Expanda os acrônimos, pelo menos uma ou duaz vezes. Ao invés de escrever direto HTML, escreva <em>Hypertext Markup Language</em>, ou HTML.</li>
</ul>

<h3 id="Layouts_de_páginas">Layouts de páginas</h3>

<p>Antigamente, nos dias velhos e ruins, as pessoas costumavam criar layouts para páginas utilizando tabelas HTML — usando as células da tabela para se comportarem como cabeçalho, rodapé, barra lateral, coluna de conteúdo, etc. Essa não é uma boa ideia porque um leitor de tela provavelmente irá retornar umas leituras um pouco confusas, especialmente se o layout é complexo e possui várias tabelas aninhadas dentro das células.</p>

<p>Tente ler o nosso exemplo <a href="http://mdn.github.io/learning-area/accessibility/html/table-layout.html">table-layout.html</a>, que se parece com algo assim:</p>

<pre class="brush: html">&lt;table width="1200"&gt;
      &lt;!-- linha do título principal --&gt;
      &lt;tr id="titulo"&gt;
        &lt;td colspan="6"&gt;

          &lt;h1 align="center"&gt;Cabeçalho&lt;/h1&gt;

        &lt;/td&gt;
      &lt;/tr&gt;
      &lt;!-- linha do menu de navegação  --&gt;
      &lt;tr id="nav" bgcolor="#ffffff"&gt;
        &lt;td width="200"&gt;
          &lt;a href="#" align="center"&gt;Home&lt;/a&gt;
        &lt;/td&gt;
        &lt;td width="200"&gt;
          &lt;a href="#" align="center"&gt;Nossa equipe&lt;/a&gt;
        &lt;/td&gt;
        &lt;td width="200"&gt;
          &lt;a href="#" align="center"&gt;Projetos&lt;/a&gt;
        &lt;/td&gt;
        &lt;td width="200"&gt;
          &lt;a href="#" align="center"&gt;Contato&lt;/a&gt;
        &lt;/td&gt;
        &lt;td width="300"&gt;
          &lt;form width="300"&gt;
            &lt;input type="pesquisa" name="q" placeholder="Pesquisar..." width="300"&gt;
          &lt;/form&gt;
        &lt;/td&gt;
        &lt;td width="100"&gt;
          &lt;button width="100"&gt;Ir!&lt;/button&gt;
        &lt;/td&gt;
      &lt;/tr&gt;
      &lt;!-- linha de espaçamento --&gt;
      &lt;tr id="espacamento" height="10"&gt;
        &lt;td&gt;

        &lt;/td&gt;
      &lt;/tr&gt;
      &lt;!-- linha do conteúdo principal --&gt;
      &lt;tr id="main"&gt;
        &lt;td id="content" colspan="4" bgcolor="#ffffff"&gt;

          &lt;!-- conteudo vem aqui --&gt;
        &lt;/td&gt;
        &lt;td id="aside" colspan="2" bgcolor="#ff80ff" valign="top"&gt;
          &lt;h2&gt;Related&lt;/h2&gt;

          &lt;!-- Outro conteúdo vem aqui --&gt;

        &lt;/td&gt;
      &lt;/tr&gt;
      &lt;!-- linha de espaçamento --&gt;
      &lt;tr id="spacer" height="10"&gt;
        &lt;td&gt;

        &lt;/td&gt;
      &lt;/tr&gt;
      &lt;!-- linha do rodapé --&gt;
      &lt;tr id="footer" bgcolor="#ffffff"&gt;
        &lt;td colspan="6"&gt;
          &lt;p&gt;©Copyright 2050 por ninguém. Todos os direitos reservados.&lt;/p&gt;
        &lt;/td&gt;
      &lt;/tr&gt;
    &lt;/table&gt;</pre>

<p>Se você tentar navegar por esse código utilizando um leitor de texto, provavelmente ele vai te dizer que existe uma tabela que pode ser reconhecida (mesmo que alguns leitores de tela consigam diferenciar entre layouts de tabelas e o conteúdos das tabelas). Você provavelmente (dependendo de qual leitor de tela que está utilizando) terá que entrar na tabela como um objeto e ler as suas características separadamente, para depois precisar sair da tabela novamente e continuar navegando pelo conteúdo.</p>

<p>Layouts feitos com tabela são uma relíquia do passado — fazia sentido utilizá-las lá atrás quando o suporte do CSS não era difundido pelos navegadores, mas esses layouts de tabela criam confusão para os usuários de leitor de tela, além de serem ruins por outros motivos (abuso de tabelas <span class="short_text" id="result_box" lang="pt"><span>indiscutivelmente precisa de mais marcação e torna o design menos flexível). Não faça dessa forma!</span></span></p>

<p>Você pode verificar essas <span class="short_text" id="result_box" lang="pt"><span>reivindicações ao comparar a última experiência com um <a href="http://mdn.github.io/learning-area/html/introduction-to-html/document_and_website_structure/">exemplo de estrutura de website mais moderna</a>, o que pode se parecer com algo assim:</span></span></p>

<pre class="brush: html">&lt;header&gt;
  &lt;h1&gt;Cabeçalho&lt;/h1&gt;
&lt;/header&gt;

&lt;nav&gt;
  &lt;!-- Navegação principal aqui --&gt;
&lt;/nav&gt;

&lt;!-- Aqui é o conteúdo principal da página --&gt;
&lt;main&gt;

  &lt;!-- Que contém um artigo --&gt;
  &lt;article&gt;
    &lt;h2&gt;Título do artigo&lt;/h2&gt;

    &lt;!-- Conteúdo do artigo aqui --&gt;
  &lt;/article&gt;

  &lt;aside&gt;
    &lt;h2&gt;Relacionados&lt;/h2&gt;

    &lt;!-- Conteúdo a parte aqui --&gt;
  &lt;/aside&gt;

&lt;/main&gt;

&lt;!-- E aqui é um rodapé utilizado em todas as páginas do nosso site --&gt;

&lt;footer&gt;
  &lt;!-- Conteúdo do rodapé aqui --&gt;
&lt;/footer&gt;</pre>

<p>Se você experimentar ler esse exemplo de estrutura mais moderna com um leitor de tela, você vai perceber que o layout feito por marcação não atrapalha na hora de retornar o conteúdo do site. Também é muito mais limpo e pequeno em termos de tamanho de código, o que significa um código mais fácil de se dar manutenção e menos uso de banda para o usuário fazer o download (particularmente prevalente para as pessoas com conexão lenta).</p>

<p>Outra consideração que pode ser feita é criar layouts utilizando a semântica HTML5 nos elementos, como visto no exemplo (veja <a href="/en-US/docs/Web/HTML/Element#Content_sectioning">content sectioning</a>) — você pode criar um layout utilizando apenas elementos aninhados {{htmlelement("div")}}, mas é melhor e mais apropriado seccionar elementos de uma forma que eles envelopem a navegação principal ({{htmlelement("nav")}}), rodapé({{htmlelement("footer")}}), unidades de conteúdo repetidas({{htmlelement("article")}}), etc. Eles trazem semânticas extras para os leitores de tela(e outras ferramentas) para dar aos usuários mais dicas sobre o conteúdo no qual eles estão navegando (veja <a href="http://www.weba11y.com/blog/2016/04/22/screen-reader-support-for-new-html5-section-elements/">Screen Reader Support for new HTML5 Section Elements</a> para uma ideia do que é suporte de leitor de tela).</p>

<div class="note">
<p><strong>Nota</strong>: Ao mesmo tempo que seu conteúdo deve ter boa semântica e um layout bonito, deve-se fazer sentido que em sua ordem de fonte — você poderá sempre movimentá-la utilizando CSS depois, mas você deve colocar a ordem de fonte de forma correta desde o começo, para que os usuários que utilizam de leitores de tela possam receber uma leitura que faz sentido.</p>
</div>

<h3 id="Controles_de_UI">Controles de UI</h3>

<p>Por controles de UI, o que nós queremos dizer é as partes dos documentos web que os usuários interagem com — mais comumente botões, links, e formulários. Nessa seção nós daremos uma olhada em princípios da acessibilidade que deverão ser analisados com cuidado ao criar esses controles de UI. Os artigos mais recentes do WAI-ARIA e multimedia irão olhar para outros aspectos da acessibilidade de UIs.</p>

<p>Um aspecto chave da acessibilidade de controles Ui é que, por padrão, os navegadores premitem que esses controles sejam acessados pelo teclado. Você pode experimentar isso utilizando o nosso exemplo <a href="http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html">native-keyboard-accessibility.html</a> (ver o <a href="https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html">código-fonte</a>) — abra em uma nova aba e experimente apertar a tecla tab; depois de algumas tecladas, você irá ver o foco da aba se mover entre diferentes elementos que podem ser focados; os elementos focados são dados um estilo de destaque em todos os navegadores (muda levemente entre diferentes navegadores) dessa forma você pode dizer qual elemento está em foco.</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14215/button-focused-unfocused.png" style="border-style: solid; border-width: 1px; display: block; height: 39px; margin: 0px auto; width: 288px;"></p>

<p>Você pode apertar Enter/Return para seguir um link que está focado ou apertar um botão (nós incluimos um pouco de JavaScript para fazer os botões chamarem uma mensagem), ou começar a escrever para inserir um texto em um formulário de texto (outros elementos possuem controles diferentes, por exemplo o elemento {{htmlelement("select")}} pode ter suas opções visíveis e selecionáveis utilizando as teclas de flecha para cima e para baixo.</p>

<div class="note">
<p><strong>Nota</strong>: Navegadores diferentes podem ter mais opções de controle pelo teclado. Veja <a href="/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Using_native_keyboard_accessibility">Using native keyboard accessibility</a> para mais detalhes.</p>
</div>

<p>Você essencialmente consegue esse comportamento de graça, só ao utilizar os elementos apropriados, ex.</p>

<pre class="brush: html example-good">&lt;h1&gt;Links&lt;/h1&gt;

&lt;p&gt;Esse é um link para &lt;a href="https://www.mozilla.org"&gt;Mozilla&lt;/a&gt;.&lt;/p&gt;

&lt;p&gt;Outro link, para &lt;a href="https://developer.mozilla.org"&gt;Mozilla Developer Network&lt;/a&gt;.&lt;/p&gt;

&lt;h2&gt;Botões&lt;/h2&gt;

&lt;p&gt;
  &lt;button data-message="Esse é do primeiro botão"&gt;Clique em mim!&lt;/button&gt;
  &lt;button data-message="Esse é do segundo botão"&gt;Clique em mim também!&lt;/button&gt;
  &lt;button data-message="Esse é do terceiro botão"&gt;E em mim!&lt;/button&gt;
&lt;/p&gt;

&lt;h2&gt;Formulário&lt;/h2&gt;

&lt;form&gt;
  &lt;div&gt;
    &lt;label for="name"&gt;Preencha com seu nome:&lt;/label&gt;
    &lt;input type="text" id="name" name="name"&gt;
  &lt;/div&gt;
  &lt;div&gt;
    &lt;label for="age"&gt;Preencha com a sua idade:&lt;/label&gt;
    &lt;input type="text" id="age" name="age"&gt;
  &lt;/div&gt;
  &lt;div&gt;
    &lt;label for="mood"&gt;Escolha o seu humor:&lt;/label&gt;
    &lt;select id="mood" name="mood"&gt;
      &lt;option&gt;Feliz&lt;/option&gt;
      &lt;option&gt;Triste&lt;/option&gt;
      &lt;option&gt;Bravo&lt;/option&gt;
      &lt;option&gt;Preocupado&lt;/option&gt;
    &lt;/select&gt;
  &lt;/div&gt;
&lt;/form&gt;</pre>

<p>Isso significa que utilizar links, botões, elementos de formulário e rótulos adequadamente (incluindo o elemento {{htmlelement("label")}} para controles de formulário).</p>

<p>Contudo, novamente é o caso onde as pessoas ás vezes fazem coisas estranhas utilizando o HTML. Por exemplo, você pode se deparar com botões escritos utilizando o elemento {{htmlelement("div")}}, como a seguir:</p>

<pre class="brush: html example-bad">&lt;div data-message="Esse é do primeiro botão"&gt;Clique em mim!&lt;/div&gt;
&lt;div data-message="Esse é do segundo botão"&gt;Clique em mim também!&lt;/div&gt;
&lt;div data-message="Esse é do terceiro botão"&gt;E em mim!&lt;/div&gt;</pre>

<p>Mas usar esse código não é recomendado - você perde imediatamente a acessibilidade do teclado que você teria se tivesse usado apenas elementos {{htmlelement ("button")}}, além de não obter nenhum estilo padrão de CSS.</p>

<h4 id="Aplicando_de_volta_a_acessibilidade_do_teclado">Aplicando de volta a acessibilidade do teclado</h4>

<p>Adicionar tais vantagens de volta leva um pouco de trabalho (você pode ver um exemplo de código no nosso exemplo <a class="external external-icon" href="http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html">fake-div-buttons.html</a> - e também pode ver o <a class="external external-icon" href="https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html">source code</a>). Aqui nós acrescentamos aos nossos falsos botões <code>&lt;div&gt;</code> a capacidade de serem focados (inclusive via tab) dando a cada um o atributo <code>tabindex="0"</code>:</p>

<pre class="brush: html">&lt;div data-message="Esse é do primeiro botão" tabindex="0"&gt;Clique em mim!&lt;/div&gt;
&lt;div data-message="Esse é do segundo botão" tabindex="0"&gt;Clique em mim também!&lt;/div&gt;
&lt;div data-message="Esse é do terceiro botão" tabindex="0"&gt;E em mim!&lt;/div&gt;</pre>

<p>Basicamente, o atributo {{htmlattrxref ("tabindex")}} destina-se principalmente a permitir que elementos tabulares tenham uma ordem de tabulação personalizada (especificada em ordem numérica positiva), em vez de apenas serem tabulados em sua ordem de origem padrão. Isso é quase sempre uma má ideia, pois pode causar grandes confusões. Use-o somente se você realmente precisar, por exemplo, se o layout mostrar coisas em uma ordem visual muito diferente do código-fonte, e você quiser fazer as coisas funcionarem mais logicamente. Existem duas outras opções para <code>tabindex</code>:</p>

<ul>
 <li><code>tabindex="0"</code> — conforme indicado acima, esse valor permite que elementos que normalmente não podem ser tabulados se tornem tabeláveis. Este é o valor mais útil do <code>tabindex</code>.</li>
 <li><code>tabindex="-1"</code> — isso permite que elementos normalmente não tabuláveis recebam foco de maneira programática, por exemplo, via JavaScript, ou como alvo de links. </li>
</ul>

<p>Embora a adição acima nos permita acessar os botões, ela não nos permite ativá-los através da tecla Enter/Return. Para fazer isso, temos que adicionar o seguinte truque de JavaScript:</p>

<pre class="brush: js line-numbers  language-js"><code class="language-js">document<span class="punctuation token">.</span>onkeydown <span class="operator token">=</span> <span class="keyword token">function</span><span class="punctuation token">(</span>e<span class="punctuation token">)</span> <span class="punctuation token">{</span>
  <span class="keyword token">if</span><span class="punctuation token">(</span>e<span class="punctuation token">.</span>keyCode <span class="operator token">===</span> <span class="number token">13</span><span class="punctuation token">)</span> <span class="punctuation token">{</span> <span class="comment token">// The Enter/Return key</span>
    document<span class="punctuation token">.</span>activeElement<span class="punctuation token">.</span><span class="function token">click</span><span class="punctuation token">(</span><span class="punctuation token">)</span><span class="punctuation token">;</span>
  <span class="punctuation token">}</span>
<span class="punctuation token">}</span><span class="punctuation token">;</span></code></pre>

<p> </p>

<p>Aqui nós adicionamos um "ouvinte" (listener) ao objeto de documento (<code>document</code>) para detectar quando um botão foi pressionado no teclado. Verificamos qual botão foi pressionado por meio da propriedade <code><a href="https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode">keyCode</a></code> do objeto de evento; se for o código que corresponde a Enter/Return, executamos a função armazenada no manipulador <code>onclick</code> do botão usando <code>document.activeElement.click()</code>. <code><a href="https://developer.mozilla.org/en-US/docs/Web/API/Document/activeElement">activeElement</a></code> nos dá o elemento que está atualmente focado na página.</p>

<p>Isso acrescenta um monte de problemas extras para construir a funcionalidade de volta. E não deveríamos ter outros problemas com isso. <strong>É sempre melhor apenas usar o elemento certo.</strong></p>

<h4 id="Rótulos_de_texto_significativos">Rótulos de texto significativos</h4>

<p>Os rótulos de texto de controle da interface do usuário são muito úteis para todos os usuários, mas deixá-los claros e simples é particularmente importante para usuários com deficiências.</p>

<p>Você deve certificar-se de que seus rótulos de texto de botão e link sejam compreensíveis e distintos. Não use apenas "Clique aqui" para seus rótulos, pois os usuários de leitores de tela podem utilizar atalhos para exibir/ouvir listas de botões e controles de formulários e não identificá-los adequadamente. A captura de tela a seguir mostra nossos controles sendo listados pelo VoiceOver no Mac.</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14335/voiceover-formcontrols.png" style="display: block; height: 604px; margin: 0px auto; width: 802px;"></p>

<p>Certifique-se de que seus rótulos fazem sentido fora de contexto, lidos individualmente e no contexto do parágrafo em que estão. Por exemplo, este seria um bom exemplo para link:</p>

<pre class="brush: html example-good">&lt;p&gt;As baleias são criaturas realmente incríveis. &lt;a href="whales.html"&gt;Saiba mais sobre baleias&lt;/a&gt;.&lt;/p&gt;</pre>

<p>Porém este, é um exemplo ruim para link:</p>

<pre class="brush: html example-bad">&lt;p&gt;As baleias são criaturas realmente incríveis. Para saber mais sobre baleias, &lt;a href="whales.html"&gt;clique aqui&lt;/a&gt;.&lt;/p&gt;</pre>

<div class="note">
<p><strong>Nota</strong>: Você pode encontrar muito mais sobre implementação de link e melhores práticas no artigo <a href="/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks">Criando hyperlinks</a>. Você também pode ver alguns bons e maus exemplos em <a href="http://mdn.github.io/learning-area/accessibility/html/good-links.html">good-links.html</a> e <a href="http://mdn.github.io/learning-area/accessibility/html/bad-links.html">bad-links.html</a>.</p>
</div>

<p>Os rótulos de formulário (labels) também são importantes para dar a você uma ideia sobre o que precisa ser preenchido em cada entrada de formulário. O seguinte exemplo aparentemente é bem razoável:</p>

<pre class="brush: html example-bad">Preencha seu nome: &lt;input type="text" id="nome" name="nome"&gt;</pre>

<p>Entretanto, esse exemplo não é tão útil para usuários com deficiência. Não há nada para associar o rótulo de forma não ambígua à entrada do formulário e deixar claro como preenchê-lo, se você não puder vê-lo. Se você acessar esse item usando um leitor de tela, só irá ouvir uma descrição genérica "editar texto" sem associar corretamente qual o tipo de texto a ser editado.</p>

<p>Já o exemplo a seguir, é bem melhor:</p>

<pre class="brush: html example-good">&lt;div&gt;
  &lt;label for="nome"&gt;Preencha seu nome:&lt;/label&gt;
  &lt;input type="text" id="nome" name="nome"&gt;
&lt;/div&gt;</pre>

<p>Com o código assim, o rótulo será claramente associado à entrada; a descrição será algo como "Preencha seu nome: editar texto".</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14337/voiceover-good-form-label.png" style="display: block; margin: 0 auto;"></p>

<p>Como um bônus adicional, na maioria dos navegadores a associação de um rótulo a uma entrada de formulário significa que você pode clicar no rótulo para selecionar/ativar o elemento de formulário. Isso consequentemente aumenta o tamanho da área clicável dos elementos, facilitando assim a seleção.</p>

<div class="note">
<p><strong>Nota</strong>: Você pode ver alguns bons e maus exemplos de formulários em <a href="http://mdn.github.io/learning-area/accessibility/html/good-form.html">good-form.html</a> e <a href="http://mdn.github.io/learning-area/accessibility/html/bad-form.html">bad-form.html</a>.</p>
</div>

<h2 id="Tabelas_de_dados_acessíveis">Tabelas de dados acessíveis</h2>

<p>Uma tabela básica de dados pode ser escrita com uma marcação muito simples, como neste exemplo:</p>

<pre class="brush: html">&lt;table&gt;
  &lt;tr&gt;
    &lt;td&gt;Name&lt;/td&gt;
    &lt;td&gt;Age&lt;/td&gt;
    &lt;td&gt;Gender&lt;/td&gt;
  &lt;/tr&gt;
  &lt;tr&gt;
    &lt;td&gt;Gabriel&lt;/td&gt;
    &lt;td&gt;13&lt;/td&gt;
    &lt;td&gt;Male&lt;/td&gt;
  &lt;/tr&gt;
  &lt;tr&gt;
    &lt;td&gt;Elva&lt;/td&gt;
    &lt;td&gt;8&lt;/td&gt;
    &lt;td&gt;Female&lt;/td&gt;
  &lt;/tr&gt;
  &lt;tr&gt;
    &lt;td&gt;Freida&lt;/td&gt;
    &lt;td&gt;5&lt;/td&gt;
    &lt;td&gt;Female&lt;/td&gt;
  &lt;/tr&gt;
&lt;/table&gt;</pre>

<p>Mas essa tabela possui alguns problemas - não há como um usuário de leitor de telas associar linhas ou colunas como agrupamentos de dados. Para fazer isso, você precisa saber quais são as linhas de cabeçalho e se elas estão direcionando linhas, colunas etc. Isso só pode ser feito visualmente para a tabela acima (veja o exemplo <a href="http://mdn.github.io/learning-area/accessibility/html/bad-table.html">bad-table.html</a> e tente navegar pela tabela você mesmo).</p>

<p>Agora dê uma olhada no exemplo da nossa <a href="https://github.com/mdn/learning-area/blob/master/css/styling-boxes/styling-tables/punk-bands-complete.html">tabela de bandas punk</a> - você pode ver alguns recursos de acessibilidade aqui:</p>

<ul>
 <li>Os cabeçalhos de tabela são definidos usando elementos {{htmlelement ("th")}} - você também pode especificar se eles são cabeçalhos de linhas ou colunas usando o atributo <code>scope</code>. Isso fornece grupos completos de dados que podem ser consumidos pelos leitores de tela como unidades únicas.</li>
 <li>O elemento {{htmlelement ("caption")}} e o atributo de resumo (<code>&lt;table&gt;</code> <code>summary</code>) executam tarefas semelhantes - eles funcionam como texto alternativo para uma tabela, fornecendo ao usuário de leitor de telas um resumo rápido e útil do conteúdo da tabela. <code>&lt;caption&gt;</code> é geralmente mais adequado, pois torna o seu conteúdo acessível para os usuários com visão também, que também poderão achar isso útil. Você não precisa ter os dois.</li>
</ul>

<div class="note">
<p><strong>Nota</strong>: Consulte nossos artigos sobre <a href="/en-US/docs/Learn/HTML/Tables/Advanced">Recursos avançados de acessibilidade para tabelas em HTML</a> para obter mais detalhes sobre tabelas de dados acessíveis.</p>
</div>

<h2 id="Alternativas_em_textos">Alternativas em textos</h2>

<p>Considerando que o conteúdo textual é inerentemente acessível, o mesmo não pode necessariamente ser dito para conteúdo multimídia - conteúdo de imagem / vídeo não pode ser visto por pessoas com deficiência visual, e conteúdo de áudio não pode ser ouvido por pessoas com deficiência auditiva. Abordaremos o conteúdo de vídeo e áudio em detalhes no artigo sobre multimídia acessível mais adiante, mas para este artigo veremos a acessibilidade para o elemento {{htmlelement("img")}}.</p>

<p>Temos um exemplo simples escrito, <a href="http://mdn.github.io/learning-area/accessibility/html/accessible-image.html">accessible-image.html</a>, que apresenta quatro cópias da mesma imagem:</p>

<pre>&lt;img src="dinosaur.png"&gt;

&lt;img src="dinosaur.png"
     alt="Um tiranossauro Rex vermelho: Um dinossauro de duas patas em pé como um humano, com braços pequenos e uma cabeça grande com muitos dentes afiados."&gt;

&lt;img src="dinosaur.png"
     alt="Um tiranossauro Rex vermelho: Um dinossauro de duas patas em pé como um humano, com braços pequenos e uma cabeça grande com muitos dentes afiados."
     title="O dinossauro vermelho da Mozilla."&gt;


&lt;img src="dinosaur.png" aria-labelledby="dino-label"&gt;

&lt;p id="dino-label"&gt;O Tiranossauro Rex: um dinossauro de duas patas de pé como um humano, com braços pequenos e uma cabeça grande com muitos dentes afiados.&lt;/p&gt;
</pre>

<p>A primeira imagem, quando visualizada por um leitor de tela, não oferece muita ajuda ao usuário - o VoiceOver, por exemplo, lê "/dinosaur.png, image". Ele lê o nome do arquivo para tentar fornecer alguma ajuda. Neste exemplo, o usuário pelo menos saberá que é um tipo de dinossauro, mas muitas vezes os arquivos podem ser carregados com nomes de arquivos gerados por máquina (por exemplo, de uma câmera digital) e esses nomes provavelmente não fornecem nenhum contexto ao conteúdo da imagem.</p>

<div class="note">
<p><strong>Nota</strong>: É por isso que você nunca deve incluir conteúdo de texto dentro de uma imagem - os leitores de tela simplesmente não podem acessá-lo. Existem outras desvantagens também - você não pode selecioná-lo e copiá-lo/colá-lo. Apenas não faça isso!</p>
</div>

<p>Quando um leitor de tela encontra a segunda imagem, ele lê todo o atributo <code>alt</code> - "Um tiranossauro Rex vermelho: Um dinossauro de duas patas em pé como um humano, com braços pequenos e uma cabeça grande com muitos dentes afiados".</p>

<p>Isso destaca a importância de não apenas usar nomes de arquivos significativos, caso o <strong>texto alternativo</strong> não esteja disponível, mas também garantir que o texto alternativo seja fornecido em atributos <code>alt</code> sempre que possível. Observe que o conteúdo do atributo <code>alt</code> sempre deve fornecer uma representação direta da imagem e o que ela transmite visualmente. Qualquer conhecimento pessoal ou descrição extra não deve ser incluído aqui, já que não é útil para pessoas que não se depararam com a imagem antes.</p>

<p>Uma coisa a considerar é se as imagens possuem algum significado dentro de seu conteúdo ou se elas são puramente decorativas. Se eles são decorativas, é melhor apenas incluí-las na página como imagens de fundo através de CSS.</p>

<div class="note">
<p><strong>Nota</strong>: Leia <a href="/en-US/docs/Learn/HTML/Multimedia_and_embedding/Images_in_HTML">Imagens em HTML</a> e <a href="/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images">Imagens Responsivas</a> para obter mais informações sobre a implementação de imagens e melhores práticas.</p>
</div>

<p>Se você quiser fornecer informações contextuais extras, deverá colocá-las no texto ao redor da imagem ou dentro de um atributo de título (<code>title</code>), como mostrado acima. Nesse caso, a maioria dos leitores de tela lerá o texto alternativo, o atributo de título e o nome do arquivo. Além disso, os navegadores exibem o texto do título como dicas de ferramentas quando estão sobre o mouse.</p>

<p><img alt="" src="https://mdn.mozillademos.org/files/14333/title-attribute.png" style="display: block; margin: 0 auto;"></p>

<p>Vamos dar uma olhada rápida no quarto método:</p>

<pre class="brush: html">&lt;img src="dinosaur.png" aria-labelledby="dino-label"&gt;

&lt;p id="dino-label"&gt;O dinossauro vermelho da Mozilla.&lt;/p&gt;</pre>

<p>Nesse caso, não estamos usando o atributo <code>alt</code> - em vez disso, apresentamos nossa descrição da imagem como um parágrafo de texto regular, atribuímos um <code>id</code> e, em seguida, usamos o atributo <code>aria-labelledby</code> para nos referirmos a esse <code>id</code>, que faz com que os leitores de tela usem esse parágrafo como o texto/rótulo alternativo para essa imagem. Isso é especialmente útil se você quiser usar o mesmo texto como um rótulo para várias imagens - algo que não é possível com <code>alt</code>.</p>

<div class="note">
<p><strong>Nota</strong>: <code>aria-labelledby</code> é parte da especificação <a href="https://www.w3.org/TR/wai-aria-1.1/">WAI-ARIA</a>, que permite aos desenvolvedores adicionar uma semântica extra à sua marcação para melhorar a acessibilidade do leitor de tela quando necessário. Para saber mais sobre como funciona, leia nosso <a href="/en-US/docs/Learn/Accessibility/WAI-ARIA_basics">artigo básico sobre WAI-ARIA</a>.</p>
</div>

<h3 id="Outros_mecanismos_alternativos_de_textos">Outros mecanismos alternativos de textos</h3>

<p>Imagens também têm outros mecanismos disponíveis para fornecer texto descritivo. Por exemplo, há um atributo <code>longdesc</code> que serve para apontar para um documento da Web separado contendo uma descrição estendida da imagem:</p>

<pre class="brush: html">&lt;img src="dinosaur.png" longdesc="dino-info.html"&gt;</pre>

<p>Isso aparentemente é uma boa ideia, especialmente para infográficos ou gráficos com muitas informações, que talvez possam ser representados como uma tabela de dados acessível (consulte a seção anterior). No entanto, o <code>longdesc</code> não é suportado de forma consistente pelos leitores de tela e o conteúdo é completamente inacessível para usuários que não usam leitores de tela. É sem dúvida muito melhor incluir a descrição longa na mesma página que a imagem ou vinculá-la a um link comum.</p>

<p>O HTML5 inclui dois novos elementos - {{htmlelement ("figure")}} e {{htmlelement ("figcaption")}} - que devem associar uma figura de algum tipo (pode ser qualquer coisa, não necessariamente uma imagem) com uma legenda de figura:</p>

<pre class="brush: html">&lt;figure&gt;
  &lt;img src="dinosaur.png" alt="O dinossauro da Mozilla."&gt;
  &lt;figcaption&gt;Um tiranossauro Rex vermelho: Um dinossauro de duas patas em pé como um humano, com braços pequenos e uma cabeça grande com muitos dentes afiados.&lt;/figcaption&gt;
&lt;/figure&gt;</pre>

<p>Infelizmente, a maioria dos leitores de tela parece não associar ainda as legendas utilizando o elemento "figure" às respectivas imagens, mas a estrutura do elemento é útil para o estilo CSS, além de fornecer uma maneira de inserir uma descrição da imagem.</p>

<h3 id="Atributos_alt_vazios">Atributos "alt" vazios</h3>

<pre class="brush: html">&lt;h3&gt;
  &lt;img src="article-icon.png" alt=""&gt;
  Tiranossauro Rex: O Rei dos dinossauros.
&lt;/h3&gt;</pre>

<p>Pode haver momentos em que uma imagem é incluída no design de uma página, mas seu objetivo principal é a decoração visual. Você notará no exemplo de código acima que o atributo <code>alt</code> da imagem está vazio - isso é para fazer com que os leitores de tela reconheçam a imagem, mas não tentem descrever a imagem (em vez disso, dizem apenas "imagem" ou similar).</p>

<p>A razão para usar um <code>alt</code> vazio ao invés de não incluí-lo é porque muitos leitores de tela anunciam o URL da imagem inteira se nenhum <code>alt</code> for fornecido. No exemplo acima, a imagem está agindo como uma decoração visual para o título ao qual está associada. Em casos como esse, e nos casos em que uma imagem é apenas decoração e não tem valor de conteúdo, você deve colocar um <code>alt</code> vazio em suas imagens. Outra alternativa é usar o atributo ARIA role (role="presentation") - isso também impede que os leitores de telas leiam textos alternativos.</p>

<div class="note">
<p><strong>Nota</strong>: se possível, você deve usar CSS para exibir imagens que são apenas decorativas.</p>
</div>

<h2 id="Resumo">Resumo</h2>

<p>Agora você deve estar familiarizado com a escrita de HTML acessível para a maioria das ocasiões. Nosso artigo básico do WAI-ARIA também preencherá algumas lacunas nesse conhecimento, mas este artigo cuidou do básico. Em seguida, exploraremos CSS e JavaScript e como a acessibilidade é afetada por seu uso bom ou ruim.</p>

<p>{{PreviousMenuNext("Learn/Accessibility/What_is_Accessibility","Learn/Accessibility/CSS_and_JavaScript", "Learn/Accessibility")}}</p>

<h2 id="Neste_módulo">Neste módulo</h2>

<ul>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/What_is_accessibility&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhjKbO6WIg9b4xWjdZSQcnXxP1foyg">O que é acessibilidade?</a></li>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhg8RYEjwUHElGCpA1Q_60OiOtXeLg">HTML: uma boa base para acessibilidade</a></li>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/CSS_and_JavaScript&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhjUFLV1YWbEtSBOMPU_Bt7V-OmzDw">Práticas recomendadas de acessibilidade de CSS e JavaScript</a></li>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/WAI-ARIA_basics&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhj5LnkD6EVmrTiupwf932KoV4VCTw">Noções básicas de WAI-ARIA</a></li>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/Multimedia&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhgWAjBOw_jwWHOvK_zBEob2xQdEmA">Multimídia acessível</a></li>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/Mobile&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhgnwNfnWbOmqpMWRI1c1zBqGFfU1Q">Acessibilidade móvel</a></li>
 <li><a href="https://developer.mozilla.org/en-US/docs/Learn/Accessibility/Accessibility_troubleshooting&amp;xid=17259,15700021,15700124,15700149,15700186,15700190,15700201,15700214&amp;usg=ALkJrhjy6mgQ3Y6D6sT4QvsOicr2-Fmt9Q">Solução de problemas de acessibilidade</a></li>
</ul>