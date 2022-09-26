---
title: 'Tutorial Django Parte 10: Testando uma aplicação web Django'
slug: Learn/Server-side/Django/Testing
translation_of: Learn/Server-side/Django/Testing
---
<div>{{LearnSidebar}}</div>

<div>{{PreviousMenuNext("Learn/Server-side/Django/Forms", "Learn/Server-side/Django/Deployment", "Learn/Server-side/Django")}}</div>

<p class="summary">À medida que websites crescem, eles se tornam mais difíceis de testar manualmente. Não apenas mais para testar, mas, as interações entre componentes tornam-se mais complexas, uma pequena mudança em uma área pode impactar outras áreas, portanto mais mudanças serão necessárias para garantir que tudo permaneça funcionando e erros não sejam introduzidos à medida que mais alterações forem feitas. Uma maneira de mitigar esses problemas é escrever testes automatizados, que podem ser executados facilmente e confiavelmente toda vez que você faz uma alteração. Este tutorial mostra como automatizar testes unitários do seu website utilizando o <em>framework</em> de testes do Django.</p>

<table class="learn-box standard-table">
 <tbody>
  <tr>
   <th scope="row">Pré-requisitos:</th>
   <td>Complete todos os tópicos de tutoriais anteriores, incluindo <a href="/en-US/docs/Learn/Server-side/Django/Forms">Tutorial Django Parte 9: Trabalhando com formulários</a>.</td>
  </tr>
  <tr>
   <th scope="row">Objetivo:</th>
   <td>Entender como escrever testes unitários para websites baseados em Django.</td>
  </tr>
 </tbody>
</table>

<h2 id="Visão_Geral">Visão Geral</h2>

<p>A <a href="https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Tutorial_local_library_website">Local Library</a> atualmente tem páginas para mostrar listas de todos livros e autores, visualização detalhada para itens <code>Book</code> e <code>Author</code>, uma página para renovar <code>BookInstance</code>s, e páginas para criar, atualizar e excluir itens <code>Author</code> (e também registros de <code>Book</code>, se você completou o desafio no <a href="/en-US/docs/Learn/Server-side/Django/Forms">forms tutorial</a>). Mesmo com este site relativamente pequeno, navegar manualmente por cada página e verificar superficialmente se tudo funciona como esperado pode levar vários minutos. À medida que fizemos mudanças e aumentamos o site, o tempo necessário para verificar manualmente se tudo funciona  "devidamente" só aumentará. Se continuássemos como estamos, eventuamente estaríamos gastando a maior parte do tempo testando, e muito pouco tempo aprimorando nosso código.</p>

<p>Testes automatizados podem realmente ajudar com este problema! Os benefícios óbvios são que eles podem ser executados muito mais rápido que testes manuais, podem testar com um nível mais baixo de detalhes, e testa exatamente a mesma funcionalidade (testadores humanos não são nem de longe tão confiáveis!). Por serem rápidos, testes automatizados podem ser executados mais regularmente, e se um teste falhar, eles apontam exatamente para onde o código não está funcionando como esperado .</p>

<p>Além disso, testes automatizados podem atuar como o primeiro "usuário" do mundo real do seu código, forçando você a ser rigoroso ao definir e documentar como seu website deve se comportar. Geralmente, eles são a base para seus exemplos de código e documentação. Por essas razões, alguns processos de desenvolvimento de código iniciam com definição e implementação de teste, o qual após o código é escrito para corresponder ao comportamento necessário (ex. <a href="https://en.wikipedia.org/wiki/Test-driven_development">desenvolvimento guiado por testes</a> e <a href="https://en.wikipedia.org/wiki/Behavior-driven_development">desenvolvimento guiado por comportamento</a>).</p>

<p>Este tutorial mostra como escrever testes automatizados para Django, adicionando um número de testes para o website <em>LocalLibrary</em>.</p>

<h3 id="Tipos_de_teste">Tipos de teste</h3>

<p>Há inúmeros tipos, níveis, e classificações de testes e abordagens de testes. Os testes automatizados mais importantes são:</p>

<dl>
 <dt>Testes unitários</dt>
 <dd>Verifica o comportamento funcional de componentes individuais, geralmente ao nível de classe e função.</dd>
 <dt>Testes de regressão</dt>
 <dd>Testes que reproduzem erros históricos. Cada teste é executado inicialmente para verificar se o erro foi corrigido, e então executado novamente para garantir que não foi reintroduzido após alterações posteriores no código.</dd>
 <dt>Testes de integração</dt>
 <dd>Verifica como agrupamentos de componentes funcionam quando utilizados  juntos. Testes de integração estão cientes das interações necessárias entre componentes, mas não necessariamente das operações internas de cada componente. Eles podem abranger agrupamentos simples de componentes através de todo website.</dd>
</dl>

<div class="note">
<p><strong>Nota: </strong>Outros tipos de testes comuns incluem caixa preta (black box), caixa branca (white box), manual, automatizado, canário (canary), fumaça (smoke), conformidade (conformance), aceitação (acceptance), funcional (functional), sistema (system), <em>performance</em>, carga (load) e testes de <em>stress</em>. Procure-os para mais informaçãos.</p>
</div>

<h3 id="O_que_o_Django_fornece_para_testes">O que o Django fornece para testes?</h3>

<p>Testar um website é uma tarefa complexa, porque isto é composto de várias camadas de lógica – do tratamento de requisições no nível HTTP, consultas de modelos, validação e processamento de formulários, e renderização de <em>template</em>.</p>

<p>Django fornece um <em>framework</em> de teste com uma baixa hierarquia de classes construida na biblioteca padrão <code><a href="https://docs.python.org/3/library/unittest.html#module-unittest" title="(in Python v3.5)">unittest</a></code> de Python. Apesar do nome, este <em>framework</em> de teste é adequado para testes unitários e de integração. O <em>framework</em> Django adiciona métodos e ferramentas de API para ajudar a testar o comportamento web e específico do Django. Isso permite você simular requisições, inserir dados de teste e inspecionar as saídas do seu aplicativo. Django também fornece uma API (<a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#liveservertestcase">LiveServerTestCase</a>) e ferramentas para <a href="https://docs.djangoproject.com/en/2.1/topics/testing/advanced/#other-testing-frameworks">usar diferentes frameworks de teste</a>, por exemplo, você pode integrar com o popular framework <a href="/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment">Selenium</a> para simular um usuário interagindo com um navegador.</p>

<p>Para escrever um teste, você deriva de qualquer uma das classes base de teste de Django (ou <em>unittest</em>) (<a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#simpletestcase">SimpleTestCase</a>, <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#transactiontestcase">TransactionTestCase</a>, <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#testcase">TestCase</a>, <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#liveservertestcase">LiveServerTestCase</a>) e então escreve métodos separados para verificar se a funcionalidade específica funciona como esperado (testes usam métodos "<em>assert</em>" para testar se a expressão resulta em valores <code>True</code> ou <code>False</code>, ou se os dois valores são iguais, etc.). Quando você inicia a execução de um teste, o framework executa os métodos de teste escolhidos em suas classes derivadas. Os métodos de teste são executados independentemente, com configuração comum e/ou comportamento <em>tear-down</em> definido na classe, como mostrado abaixo.</p>

<pre class="brush: python notranslate">class YourTestClass(TestCase):
    def setUp(self):
        # Setup run before every test method.
        pass

    def tearDown(self):
        # Clean up run after every test method.
        pass

    def test_something_that_will_pass(self):
        self.assertFalse(False)

    def test_something_that_will_fail(self):
        self.assertTrue(False)
</pre>

<p>A melhor classe base para maioria dos testes é <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#testcase">django.test.TestCase</a>. Esta classe de teste cria um banco de dados limpo antes dos testes serem executados, e executa todas as funções de teste em sua própria transação. A classe também possui um <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#django.test.Client" title="django.test.Client">Client</a> de teste, que você pode utilizar para simular um usuário interagindo com o código no nível de <em>view</em>. Nas seções a seguir vamos nos concentrar nos testes unitários, criados utilizando a classe base <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#testcase">TestCase</a>.</p>

<div class="note">
<p><strong>Nota:</strong> A classe <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#testcase">django.test.TestCase</a> é muito conveniente, mas pode resultar em alguns testes mais lentos do que necessitam ser (nem todo teste necessita configurar seu próprio banco de dados ou simular interação de <em>view</em>). Uma vez que esteja familiar com o que você pode fazer com essa classe, você pode querer substituir alguns dos seus testes por classes de teste mais simples disponíveis.</p>
</div>

<h3 id="O_que_você_deve_testar">O que você deve testar?</h3>

<p>Você deve testar todos aspectos do seu próprio código, mas nenhuma biblioteca ou funcionalidade oferecida como parte do Python ou Django.</p>

<p>Assim por exemplo, conseidere o <em>model</em> <code>Author</code> definido abaixo. Você não precisa testar explicitamente se <code>first_name</code> e <code>last_name</code> foram armazenados corretamente como <code>CharField</code> no banco de dados, porque isso é algo definido pelo Django (embora, é claro, na prática você inevitávelmente testará esta funcionalidade durante o desenvolvimento). Você também não precisa testar se o <code>date_of_birth</code> foi validado para ser um campo de data, porque isso novamente é algo implementeado no Django.</p>

<p>No entanto, você deve verificar o texto utilizado para os <em>labels</em> (<em>First name, Last name, Date of birth, Died</em>), e o tamanho do campo alocado para o texto (<em>100 caracteres</em>), porque isso faz parte do seu <em>design</em> e algo que pode ser violado/alterado no futuro.</p>

<pre class="brush: python notranslate">class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(null=True, blank=True)
    date_of_death = models.DateField('Died', null=True, blank=True)

    def get_absolute_url(self):
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        return '%s, %s' % (self.last_name, self.first_name)</pre>

<p>Similarmente, você deve verificar se os métodos personalizados  <code style="font-style: normal; font-weight: normal;">get_absolute_url()</code> e <code style="font-style: normal; font-weight: normal;">__str__()</code> se comportam como desejado, porque eles são sua lógica de código/negócios. No caso de <code style="font-style: normal; font-weight: normal;">get_absolute_url()</code> você pode confiar que o método <code>reverse()</code> de Django, foi implementado corretamente, portanto, o que você esta testando é se a <em>view</em> associada foi realmente definida.</p>

<div class="note">
<p><strong>Nota:</strong> Leitores astutos podem notar que também gostariamos de restringir que a data de nascimento e morte como valores sensíveis, e verificar se a morte vem após o nascimento. Em Django, esta restrição seria adicionada a suas classes <em>form</em> (Embora você possa definir validadores para campos do modelo e validadores de modelo, estes só serão usados no nível do formulário se forem chamdos pelo método clean() do model. Isso requer um ModelForm ou o método clean() do modelo precisa ser especificamente chamado).</p>
</div>

<p>Com isso em mente, vamos começar a ver como definir e executar testes.</p>

<h2 id="Visão_geral_da_estrutura_de_teste">Visão geral da estrutura de teste</h2>

<p>Antes de entrarmos nos detalhes de "o que testar", vamos primeiro examinar brevemente <em>onde</em> e <em>como</em> os testes são definidos.</p>

<p>Django usa o módulo <em>unittest</em> com <a href="https://docs.python.org/3/library/unittest.html#unittest-test-discovery" title="(in Python v3.5)">descoberta de teste acoplada</a>, que descrobrirá testes no diretório de trabalho atual em qualquer arquivo nomeado com o padrão <strong>test*.py</strong>. Fornecido o nome do arquivo adequadamente, você pode usar qualquer estrutura que desejar. Recomendamos que você crie um módulo para seu código de teste, e tenha arquivos separados para <em>models</em>, <em>views</em>, <em>forms </em>e qualquer outro tipo de código que você precise testar. Por exemplo:</p>

<pre class="notranslate">catalog/
  /tests/
    __init__.py
    test_models.py
    test_forms.py
    test_views.py
</pre>

<p>Crie uma estrutura de arquivos como mostrado acima em seu projeto <em>LocalLibrary</em>. O <strong>__init__.py</strong> deve ser um arquivo vazio (isso informa ao Python que o diretório é um pacote). Você pode criar os três arquivos de teste copiando e renomeando o arquivo de teste do "esqueleto" <strong>/catalog/tests.py</strong>.</p>

<div class="note">
<p><strong>Nota:</strong> O arquivo de teste <strong>/catalog/tests.py</strong> do "esqueleto", foi criado automaticamente quando nós <a href="/en-US/docs/Learn/Server-side/Django/skeleton_website">construimos o "esqueleto" do website Django</a>. É perfeitamente "legal" colocar todos seus testes dentro dele, mas se você testar devidamente, você acabará rapidamente com um arquivo de teste muito grande e incontrolável.</p>

<p>Exclua o arquivo do "esqueleto", pois não precisamos dele.</p>
</div>

<p>Abra <strong>/catalog/tests/test_models.py</strong>. O arquivo deve importar <code>django.test.TestCase</code>, como mostrado:</p>

<pre class="brush: python notranslate">from django.test import TestCase

# Create your tests here.
</pre>

<p>Frequentemente, você adicionará uma classe de teste para cada <em>model/view/form</em> que deseja testar, com métodos individuais para testar funcionalidades específicas. Em outros casos, você pode desejar ter uma classe separada para testar um caso de uso específico, com funções de teste individuais que testam aspectos desse caso de uso (por exemplo, uma classe para testar se um campo do <em>model</em> é validado corretamente, com funções para testar cada um dos possíveis casos de falha). Novamente, a estrutura depende muito de você, mas é melhor se você for consistente.</p>

<p>Adicione a classe de teste abaixo na parte inferior do arquivo. A classe demonstra como construir uma classe de teste derivando de <code>TestCase</code>.</p>

<pre class="brush: python notranslate">class YourTestClass(TestCase):
    @classmethod
    def setUpTestData(cls):
        print("setUpTestData: Run once to set up non-modified data for all class methods.")
        pass

    def setUp(self):
        print("setUp: Run once for every test method to setup clean data.")
        pass

    def test_false_is_false(self):
        print("Method: test_false_is_false.")
        self.assertFalse(False)

    def test_false_is_true(self):
        print("Method: test_false_is_true.")
        self.assertTrue(False)

    def test_one_plus_one_equals_two(self):
        print("Method: test_one_plus_one_equals_two.")
        self.assertEqual(1 + 1, 2)</pre>

<p>A nova classe define dois métodos que você pode utilizar para aconfiguração de pré-teste (por exemplo, para criar quaisquer modelos ou outros objetos que precisará para to teste):</p>

<ul>
 <li><code>setUpTestData()</code> é chamado uma vez no início da execução do teste para configuração em nível de classe. Você usaria isso para criar objetos que não serão modificados ou alterados em nenhum dos métodos de teste.</li>
 <li><code>setUp()</code> é chamado antes de toda função de teste para configurar qualquer objeto que possa ser modificado pelo teste (toda função de teste receberá uma versão "nova" desses objetos).</li>
</ul>

<div class="note">
<p>As classes de teste também têm um método <code>tearDown()</code>, que não usamos. Este método não é particularmente útil para testes de banco de dados, pois a classe base <code>TestCase</code> cuida da desmontagem do banco de dados para você.</p>
</div>

<p>Abaixo desses, temos vários métodos de teste, que usam funções <code>Assert</code> para testar se as condições são verdadeiras, falsas ou iguais (<code>AssertTrue</code>, <code>AssertFalse</code>, <code>AssertEqual</code>). Se a condição não for avaliada como esperado, então o teste falhará e reportará o erro ao seu console.</p>

<p><code>AssertTrue</code>, <code>AssertFalse</code>, <code>AssertEqual</code> são assertivas padrão fornecidas pelo <strong>unittest</strong>. Existem outras assertivas padão no <em>framework</em> e também <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#assertions">Django especifica assertivas</a> para testar se uma <em>view</em> redireciona (<code>assertRedirects</code>), para testar se um template específico foi utilizado (<code>assertTemplateUsed</code>), etc.</p>

<div class="note">
<p>Você normalmente não deve incluir funções <strong>print()</strong> em seus testes como mostrado acima. Nós fizemos isso aqui apenas para que você posssa ver no console a ordem que as funções de configuração são chamadas (na seção a seguir).</p>
</div>

<h2 id="Como_executar_os_testes">Como executar os testes</h2>

<p>A maneira mais fácil para executar todos os testes é usar o comando:</p>

<pre class="brush: bash notranslate">python3 manage.py test</pre>

<p>Isso descobrirá todos arquivos nomeados com o padrão <strong>test*.py</strong> no diretório atual e executará todos testes definidos usando as classes base apropriadas (aqui temos vários arquivos de teste, mas, atualmente, apenas  <strong>/catalog/tests/test_models.py</strong> contém testes). Por padrão, os testes irão reportar individualmente apenas falhas no teste, seguidos por um resumo do teste.</p>

<div class="note">
<p>Se você obter erros semelhantes a: <code>ValueError: Missing staticfiles manifest entry ...</code> isso pode ocorrer porque o teste não é executado como <em>collectstatic</em> por padrão e seu <em>app</em> está usando uma classe de armazenamento que exige isto (veja <a href="https://docs.djangoproject.com/en/2.1/ref/contrib/staticfiles/#django.contrib.staticfiles.storage.ManifestStaticFilesStorage.manifest_strict">manifest_strict</a> para mais informações). Existem várias maneiras de solucionar esse problema - o mais fácil é simplesmente executar <em>collectstatic</em> antes de executar os testes:</p>

<pre class="brush: bash notranslate">python3 manage.py collectstatic
</pre>
</div>

<p>Execute os testes no diretório raiz de <em>LocalLibrary</em>. Você deve ver uma saída como a abaixo.</p>

<pre class="brush: bash notranslate">&gt; python3 manage.py test

Creating test database for alias 'default'...
<strong>setUpTestData: Run once to set up non-modified data for all class methods.
setUp: Run once for every test method to setup clean data.
Method: test_false_is_false.
setUp: Run once for every test method to setup clean data.
Method: test_false_is_true.
setUp: Run once for every test method to setup clean data.
Method: test_one_plus_one_equals_two.</strong>
.
======================================================================
FAIL: test_false_is_true (catalog.tests.tests_models.YourTestClass)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "D:\Github\django_tmp\library_w_t_2\locallibrary\catalog\tests\tests_models.py", line 22, in test_false_is_true
    self.assertTrue(False)
AssertionError: False is not true

----------------------------------------------------------------------
Ran 3 tests in 0.075s

FAILED (failures=1)
Destroying test database for alias 'default'...</pre>

<p>Aqui vemos que tivemos uma falha no teste e podemos ver exatamente qual função falhou e por quê (essa falha é esperada, porque <code>False</code> não é <code>True</code>!).</p>

<div class="note">
<p><strong>Dica: </strong>A coisa mais importante para aprender com a saída do teste acima é que é muito mais valioso se você utilizar nomes descritivos/informativos para seus objetos e métodos.</p>
</div>

<p>O texto acima mostrado em <strong>negrito</strong> normalmente não apareceria na saída do teste (isso é gerado pelas funções <code>print()</code> em nossos teste). Isso mostra como o método  <code>setUpTestData()</code> é chamdo uma vez para classe e <code>setUp()</code> é chamado antes de cada método.</p>

<p>As próximas seções mostram como você pode executar testes específicos e como controlar quanta infromação os testes exibem.</p>

<h3 id="Mostrando_mais_informações_de_teste">Mostrando mais informações de teste</h3>

<p>Se você deseja obter mais informação sobre a execução do teste, você pode mudar  a verbosidade (<em>verbosity)</em>. Por exemplo, para listar os sucessos do teste, bem como as falhas (e um monte de informações sobre como o banco de dados de teste está configurado) vocêpode definir a <em>verbosity</em> para "2" como mostrado:</p>

<pre class="brush: bash notranslate">python3 manage.py test --verbosity 2</pre>

<p>Os níveis permitidos de <em>verbosity</em> são 0, 1, 2, e 3, com o padrão sendo "1".</p>

<h3 id="Executando_testes_específicos">Executando testes específicos</h3>

<p>Se você desseja executar um subconjunto de seus testes, você pode fazer isso especificando o caminho completo para o(s) pacote(s), módulos, subclasse <code>TestCase</code> ou método:</p>

<pre class="brush: bash notranslate"># Run the specified module
python3 manage.py test catalog.tests

# Run the specified module
python3 manage.py test catalog.tests.test_models

# Run the specified class
python3 manage.py test catalog.tests.test_models.YourTestClass

# Run the specified method
python3 manage.py test catalog.tests.test_models.YourTestClass.test_one_plus_one_equals_two
</pre>

<h2 id="Testes_da_LocalLibrary">Testes da LocalLibrary</h2>

<p>Agora que sabemos como executar nosso testes e que tipo de coisas precisams testar, vamos ver alguns exemplos práticos.</p>

<div class="note">
<p><strong>Nota: </strong>Não escreveremos todos os testes possíveis, mas isso deve lhe dar uma ideia de como testes trabalham e o que mais você pode fazer.</p>
</div>

<h3 id="Models">Models</h3>

<p>Como discutido acima, devemos testar qualquer coisa que faça parte do nosso projeto ou que seja definido por código que escrevemos, mas não bibliotecas/códigos que já foram testados pelo Django ou pela equipe de desenvolvimento do Python.</p>

<p>Por exemplo, considere o <em>model</em> <code>Author</code> abaixo. Aqui devemos testar os <em>labels</em> para todos os campos, porque, embora não tenhamos específicado explicitamente a maioria deles, temos um projeto que diz quais devem ser esses valores. Se não testamos os valores, não sabemos se os <em>labels</em> dos campos  têm os valores pretendidos. Similarmente, enquanto confiamos que o Django criará um campo com o tamanho específicado, vale a pena específicar um teste para este tamanho, para garantir que ele foi implementado como planejado.</p>

<pre class="brush: python notranslate">class Author(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField(null=True, blank=True)
    date_of_death = models.DateField('Died', null=True, blank=True)

    def get_absolute_url(self):
        return reverse('author-detail', args=[str(self.id)])

    def __str__(self):
        return f'{self.last_name}, {self.first_name}'</pre>

<p>Abra nosso <strong>/catalog/tests/test_models.py</strong>, e substitua qualquer código existente pelo seguinte código de teste para o <em>model</em> <code>Author</code>.</p>

<p>Aqui você verá que primeiro importamos <code>TestCase</code> e derivamos nossa classe de teste (<code>AuthorModelTest</code>) a partir dela, usando um nome descritivo para que possamos identificar facilmente quaiquer testes com falha na saída do teste. Nós então chamamos <code>setUpTestData()</code> para criar um objeto autor que iremos usar mas não modificaremos em nenhum dos testes.</p>

<pre class="brush: python notranslate">from django.test import TestCase

from catalog.models import Author

class AuthorModelTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        # Set up non-modified objects used by all test methods
        Author.objects.create(first_name='Big', last_name='Bob')

    def test_first_name_label(self):
        author = Author.objects.get(id=1)
        field_label = author._meta.get_field('first_name').verbose_name
        self.assertEquals(field_label, 'first name')

    def test_date_of_death_label(self):
        author=Author.objects.get(id=1)
        field_label = author._meta.get_field('date_of_death').verbose_name
        self.assertEquals(field_label, 'died')

    def test_first_name_max_length(self):
        author = Author.objects.get(id=1)
        max_length = author._meta.get_field('first_name').max_length
        self.assertEquals(max_length, 100)

    def test_object_name_is_last_name_comma_first_name(self):
        author = Author.objects.get(id=1)
        expected_object_name = f'{author.last_name}, {author.first_name}'
        self.assertEquals(expected_object_name, str(author))

    def test_get_absolute_url(self):
        author = Author.objects.get(id=1)
        # This will also fail if the urlconf is not defined.
        self.assertEquals(author.get_absolute_url(), '/catalog/author/1')</pre>

<p>Os testes de campo verificam se os valores dos <em>labels</em> dos campos (<code>verbose_name</code>)  e se o tamanho dos campos de caracteres são como esperado. Todos esses métodos possuem nomes descritivos e seguem o mesmo padrão:</p>

<pre class="brush: python notranslate"># Get an author object to test
author = Author.objects.get(id=1)

# Get the metadata for the required field and use it to query the required field data
field_label = author._meta.get_field('first_name').verbose_name

# Compare the value to the expected result
self.assertEquals(field_label, 'first name')</pre>

<p>As coisas interessantes a serem observadas aqui:</p>

<ul>
 <li>Não podemos obter <code>verbose_name</code> diretamente utilizando  <code>author.first_name.verbose_name</code>, porque <code>author.first_name</code> é uma <em>string</em> (não um identificador para o objeto <code>first_name</code> que podemos utilizar para acessar suas propriedades). Em vez disso, precisamos utilizar o atributo <code>_meta</code> de <em>author</em> para obter uma instância do campo e usá-la para consultar informações adicionais.</li>
 <li>Optamos por utilizar <code>assertEquals(field_label,'first name')</code> em vez de <code>assertTrue(field_label == 'first name')</code>. A razão para isso é que, se o teste falhar a saída do primeiro informa o que realmente era o <em>label</em>, que torna a depuração do problema um pouco mais fácil.</li>
</ul>

<div class="note">
<p><strong>Nota:</strong> Testes para os rótulos <code>last_name</code> e <code>date_of_birth</code> e também para o teste para o tamanho do <code>last_name</code> field foram omitidos. Adicione suas próprias versões agora, seguindo as convenções de nomeclatura e abordagens mostradas acima.</p>
</div>

<p>Também precisamos testar nossos métodos personalizados. Eles, essencialmente, apenas verificam se o nome do objeto foi construido como esperamos, usando o formato "Last Name", "First Name", e se a URL que obtemos para um item de <code>Author</code> é o que esperávamos.</p>

<pre class="brush: python notranslate">def test_object_name_is_last_name_comma_first_name(self):
    author = Author.objects.get(id=1)
    expected_object_name = f'{author.last_name}, {author.first_name}'
    self.assertEquals(expected_object_name, str(author))

def test_get_absolute_url(self):
    author = Author.objects.get(id=1)
    # This will also fail if the urlconf is not defined.
    self.assertEquals(author.get_absolute_url(), '/catalog/author/1')</pre>

<p>Execute os testes agora. Se você criou o modelo Author como descrevemos no tutorial de modelos, é bem provável que você obtenha um erro para o <em>label</em> <code>date_of_death</code> como mostrado abaixo. O teste está falhando porque foi escrito esperando que a definição do <em>label</em> siga a convenção do Django de não colocar em maíúscula a primeira letra do <em>label</em> (Django faz isso por você).</p>

<pre class="brush: bash notranslate">======================================================================
FAIL: test_date_of_death_label (catalog.tests.test_models.AuthorModelTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "D:\...\locallibrary\catalog\tests\test_models.py", line 32, in test_date_of_death_label
    self.assertEquals(field_label,'died')
AssertionError: 'Died' != 'died'
- Died
? ^
+ died
? ^</pre>

<p>Este é um bug muito pequeno, mas destaca como a escrita de testes pode verificar mais minuciosamente quaislquer suposições que você tenha feito.</p>

<div class="note">
<p><strong>Nota:</strong> Altere o <em>label</em> para o campo date_of_death (/catalog/models.py) para "died" e re-executes os testes.</p>
</div>

<p>Os padrões para testar os outros modelos são semelhantes, portanto não continuaremos discutindo mais isso. Sinta-se livre para criar seus próprios testes para nossos outros modelos.</p>

<h3 id="Forms">Forms</h3>

<p>A filosofia para testar seus <em>forms </em>é a mesma que para testar seus <em>models</em>; você precisa testar qualquer coisa que tenha codificado ou seu projeto especifica, mas não o comportamento do framework subjacente e outras bibliotecas de terceiros</p>

<p>Geralmente, isso significa que você deve testar se os <em>forms</em> têm os campos que você deseja e se esses são exibidos com os <em>labels</em>  e texto de ajuda apropriados. Você não precisa verificar se o Django o tipo de campo corretamente (a menos que você tenha criado seu próprio campo e validação personalizados) — ex. você não precisa testar se um campo de email aceita apenas email. No entanto,  você precisaria testar qualquer validação adicional que você espera que seja executada nos campos e quaisquer mensagens que seu código irá gerar para erros.</p>

<p>Considere nosso <em>form</em> para renovação de livros. Ele tem apenas um campo para data de renovação, que terá um <em>label</em> e um texto de ajuda que precisaremos verificar.</p>

<pre class="brush: python notranslate">class RenewBookForm(forms.Form):
    """Form for a librarian to renew books."""
    renewal_date = forms.DateField(help_text="Enter a date between now and 4 weeks (default 3).")

    def clean_renewal_date(self):
        data = self.cleaned_data['renewal_date']

        # Check if a date is not in the past.
        if data &lt; datetime.date.today():
            raise ValidationError(_('Invalid date - renewal in past'))

        # Check if date is in the allowed range (+4 weeks from today).
        if data &gt; datetime.date.today() + datetime.timedelta(weeks=4):
            raise ValidationError(_('Invalid date - renewal more than 4 weeks ahead'))

        # Remember to always return the cleaned data.
        return data</pre>

<p>Abra nosso arquivo <strong>/catalog/tests/test_forms.py</strong> e substitua qualquer código existente pelo seguinte código de teste para o <em>form</em> <code>RenewBookForm</code>. Nós iniciamos importando nosso <em>form</em> e algumas bibliotecas Python e Django para ajudar testar funcionalidades relacionadas ao tempo. Em seguida, declaramos nossa classe de teste do <em>form,</em> da mesma maneira que fizemos para <em>models</em>,  usando um nome descritivo para a classe de teste derivada de <code>TestCase</code>.</p>

<pre class="brush: python notranslate">import datetime

from django.test import TestCase
from django.utils import timezone

from catalog.forms import RenewBookForm

class RenewBookFormTest(TestCase):
    def test_renew_form_date_field_label(self):
        form = RenewBookForm()
        self.assertTrue(form.fields['renewal_date'].label == None or form.fields['renewal_date'].label == 'renewal date')

    def test_renew_form_date_field_help_text(self):
        form = RenewBookForm()
        self.assertEqual(form.fields['renewal_date'].help_text, 'Enter a date between now and 4 weeks (default 3).')

    def test_renew_form_date_in_past(self):
        date = datetime.date.today() - datetime.timedelta(days=1)
        form = RenewBookForm(data={'renewal_date': date})
        self.assertFalse(form.is_valid())

    def test_renew_form_date_too_far_in_future(self):
        date = datetime.date.today() + datetime.timedelta(weeks=4) + datetime.timedelta(days=1)
        form = RenewBookForm(data={'renewal_date': date})
        self.assertFalse(form.is_valid())

    def test_renew_form_date_today(self):
        date = datetime.date.today()
        form = RenewBookForm(data={'renewal_date': date})
        self.assertTrue(form.is_valid())

    def test_renew_form_date_max(self):
        date = timezone.localtime() + datetime.timedelta(weeks=4)
        form = RenewBookForm(data={'renewal_date': date})
        self.assertTrue(form.is_valid())
</pre>

<p>As primeiras duas funções testam se os campos <code>label</code> e <code>help_text</code> são como esperados. Temos que acessar o campo usando o dicionário de campos (ex. <code>form.fields['renewal_date']</code>). Observe aqui que também precisamos testar se o valor do <em>label</em> é <code>None</code>, porque mesmo que o Django processe o <em>label</em> correto, retornará <code>None</code> se o valor não estiver definido explicitamente.</p>

<p>O restante das funções testam se o form é valido para datas de renovação dentro do intervalo aceitável e inválido para os valores foram do intervalo. Observe como construimos os valores teste de data em torno de nossa data atual (<code>datetime.date.today()</code>) usando <code>datetime.timedelta()</code> (nesse caso, especificando um número de dias ou semanas). Então, apenas criamos o <em>form</em>, passando nossos dados e testando se é válido.</p>

<div class="note">
<p><strong>Nota:</strong> Aqui, na realidade, não usamos o banco de dados ou cliente teste. Considere modificar essses testes para utilizar <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#django.test.SimpleTestCase">SimpleTestCase</a>.</p>

<p>Também precisamos validar que os erros corretos sejam gerados se o form é inválido, no entanto, isso geralmente é feito no processamento da view, portanto trataremos disso na próxima seção.</p>
</div>

<p>Isso é tudo para <em>forms</em>; nós temos alguns outros, mas eles são automaticamente criados pelas nossas <em>views</em> de edição baseada na classe genérica, e devem ser testadas lá! Execute os testes e confirme  se nosso código ainda passa!</p>

<h3 id="Views">Views</h3>

<p>Para validar o comportamento das nossas <em>views</em>, utilzamos <a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/#django.test.Client">Client</a> de teste do Django. Essa classe funciona como um navegador web fictício que podemos usar para simular requisições <code>GET</code> and <code>POST</code> em uma URL e observar a resposta. Podemos ver quase tudo sobre a resposta, desde HTTP de baixo nível (cabeçalhos de resultados e códigos de status) até o <em>template</em> que estamos utilizando para renderizar o HTML e os dados de contexto que estamos passando para ele. Também podemos ver a cadeia de redirecionamentos (se houver) e verificar a URL e o código de status em cada etapa. Isso nos permite verificar se cada <em>view</em> esta fazendo o que é esperado.</p>

<p>Vamos iniciar com uma de nossas <em>views</em> mais simples, que fornece uma lista de todos Autores. Isso é exibido na URL <strong>/catalog/authors/</strong> (uma URL chamada 'authors' na configuração de URL).</p>

<pre class="brush: python notranslate">class AuthorListView(generic.ListView):
    model = Author
    paginate_by = 10
</pre>

<p>Como esta é uma <em>list view</em> genérica, quase tudo é feito para nós pelo Django. Provavelmente, se você confia no Django, então a única coisa que você precisa testar é se a <em>view</em> é acessível na URL correta e pode ser acessada usando seu nome. No entanto, se você está usando um desenvolvimento orientado a testes, você iniciará escrevendo testes que confirmam que a <em>view</em> exibe todos Autores, paginando-os em lotes de 10.</p>

<p>Abra o arquivo <strong>/catalog/tests/test_views.py</strong> e substitua qualquer texto existente pelo seguinte código de teste para <code>AuthorListView</code>. Como antes, importamos nosso <em>model</em> e algumas classe úteis. No método <code>setUpTestData()</code> configuramos vários objetos <code>Author</code> para que possamos testar nossa paginação.</p>

<pre class="brush: python notranslate">from django.test import TestCase
from django.urls import reverse

from catalog.models import Author

class AuthorListViewTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        # Create 13 authors for pagination tests
        number_of_authors = 13

        for author_id in range(number_of_authors):
            Author.objects.create(
                first_name=f'Christian {author_id}',
                last_name=f'Surname {author_id}',
            )

    def test_view_url_exists_at_desired_location(self):
        response = self.client.get('/catalog/authors/')
        self.assertEqual(response.status_code, 200)

    def test_view_url_accessible_by_name(self):
        response = self.client.get(reverse('authors'))
        self.assertEqual(response.status_code, 200)

    def test_view_uses_correct_template(self):
        response = self.client.get(reverse('authors'))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, 'catalog/author_list.html')

    def test_pagination_is_ten(self):
        response = self.client.get(reverse('authors'))
        self.assertEqual(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertTrue(len(response.context['author_list']) == 10)

    def test_lists_all_authors(self):
        # Get second page and confirm it has (exactly) remaining 3 items
        response = self.client.get(reverse('authors')+'?page=2')
        self.assertEqual(response.status_code, 200)
        self.assertTrue('is_paginated' in response.context)
        self.assertTrue(response.context['is_paginated'] == True)
        self.assertTrue(len(response.context['author_list']) == 3)</pre>

<p>Todos os teste usam o cliente (pertenecente a nossa classe derivada <code>TestCase</code>'s) para simular uma requisição <code>GET</code> e obter uma resposta. A primeira versão verifica uma URL específica URL (observe, apenas o caminho específico, sem o domínio), enquanto a segunda gera a URL  a partir do seu nome na configuração da URL.</p>

<pre class="brush: python notranslate">response = self.client.get('/catalog/authors/')
response = self.client.get(reverse('authors'))
</pre>

<p>Uma vez que temos a resposta, consultamos o seu código de status, o <em>template</em> usado, se a resposta é paginada ou não, o número de itens retonado e o número total de itens.</p>

<div class="blockIndicator note">
<p class="brush: python">Nota: Se você definir a variável <code>paginate_by</code> em seu arquivo  <strong>/c</strong><strong>atalog/views.py</strong> para um número diferente de 10, atualize as linhas que testam se o número correto de itens é exibido nos <em>templates</em> paginados acima e nas seções seguintes. Por exemplo, se você definiu a variável para a lista de autor para 5, atualize a linha acima para:</p>

<pre class="brush: python notranslate">self.assertTrue(len(response.context['author_list']) == 5)
</pre>
</div>

<p>A variável mais importante que demonstramos acima é <code>response.context</code>, que é a variável de contexto passada para o <em>template</em> pela <em>view</em>. Isso é incrivelmente útil para testes, porque permite confirmar que nosso template está obtendo todos os dados necessários. Em outras palavras, podemos verificar se estamos utilizando o <em>template</em> pretendido e quais dados o <em>template</em> está obtendo, o que ajuda bastante a verificar que alguns problemas de renderização são apenas devido ao <em>template</em>.</p>

<h4 id="Views_restritas_a_usuários_logados"><em>Views</em> restritas a usuários logados</h4>

<p>Em alguns casos, você desejará testar uma <em>view</em> que é restrita apenas aos usuários logados. Por exemplo, nossa <code>LoanedBooksByUserListView</code> é muito similar a nossa <em>view</em> anterior, mas está disponível apenas para usuários logados e exibe apenas os registros <code>BookInstance</code> que são emprestados pelo usuário atual, têm o status 'emprestado' e são ordenados "mais antigos primeiro".</p>

<pre class="brush: python notranslate">from django.contrib.auth.mixins import LoginRequiredMixin

class LoanedBooksByUserListView(LoginRequiredMixin, generic.ListView):
    """Generic class-based view listing books on loan to current user."""
    model = BookInstance
    template_name ='catalog/bookinstance_list_borrowed_user.html'
    paginate_by = 10

    def get_queryset(self):
        return BookInstance.objects.filter(borrower=self.request.user).filter(status__exact='o').order_by('due_back')</pre>

<p>Adicione o código seguinte ao <strong>/catalog/tests/test_views.py</strong>. Aqui, primeiro usamos <code>SetUp()</code> para criar alguma contas de login de usuário e objetos <code>BookInstance</code> (junto com seus livros associados e outros registros) que usaremos posteriormente nos testes. Metade dos livros são emprestados para cada usuário teste, mas inicialmente definimos o status de todos os livros como  "manutenção". Usamos <code>SetUp()</code> em vez de <code>setUpTestData()</code> porque modificaremos alguns desses objetos depois.</p>

<div class="note">
<p><strong>Nota:</strong> O código <code>setUp()</code> abaixo, cria um livro com uma  <code>Language</code> especificada, mas seu código pode não incluir o <em>model</em> <code>Language</code>, pois foi criado como um desafio. Se esse for o caso, simplesmente comente as partes do código que cria ou importa objetos <em>Language</em>. Você também deve fazer isso na seção  <code>RenewBookInstancesViewTest</code> a seguir.</p>
</div>

<pre class="brush: python notranslate">import datetime

from django.utils import timezone
from django.contrib.auth.models import User # Required to assign User as a borrower

from catalog.models import BookInstance, Book, Genre, Language

class LoanedBookInstancesByUserListViewTest(TestCase):
    def setUp(self):
        # Create two users
        test_user1 = User.objects.create_user(username='testuser1', password='1X&lt;ISRUkw+tuK')
        test_user2 = User.objects.create_user(username='testuser2', password='2HJ1vRV0Z&amp;3iD')

        test_user1.save()
        test_user2.save()

        # Create a book
        test_author = Author.objects.create(first_name='John', last_name='Smith')
        test_genre = Genre.objects.create(name='Fantasy')
        test_language = Language.objects.create(name='English')
        test_book = Book.objects.create(
            title='Book Title',
            summary='My book summary',
            isbn='ABCDEFG',
            author=test_author,
            language=test_language,
        )

        # Create genre as a post-step
        genre_objects_for_book = Genre.objects.all()
        test_book.genre.set(genre_objects_for_book) # Direct assignment of many-to-many types not allowed.
        test_book.save()

        # Create 30 BookInstance objects
        number_of_book_copies = 30
        for book_copy in range(number_of_book_copies):
            return_date = timezone.localtime() + datetime.timedelta(days=book_copy%5)
            the_borrower = test_user1 if book_copy % 2 else test_user2
            status = 'm'
            BookInstance.objects.create(
                book=test_book,
                imprint='Unlikely Imprint, 2016',
                due_back=return_date,
                borrower=the_borrower,
                status=status,
            )

    def test_redirect_if_not_logged_in(self):
        response = self.client.get(reverse('my-borrowed'))
        self.assertRedirects(response, '/accounts/login/?next=/catalog/mybooks/')

    def test_logged_in_uses_correct_template(self):
        login = self.client.login(username='testuser1', password='1X&lt;ISRUkw+tuK')
        response = self.client.get(reverse('my-borrowed'))

        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        # Check we used correct template
        self.assertTemplateUsed(response, 'catalog/bookinstance_list_borrowed_user.html')
</pre>

<p>Para verificar se a <em>view</em> será redirecionada para uma página de login se o usuário não estiver logado, usamos <code>assertRedirects</code>, como demonstrado em <code>test_redirect_if_not_logged_in()</code>. Para verificar se a página é exibida para um usuário logado, primeiro logamos com nosso usuário teste e então acessamos a página novamente e verificamos se obtivemos um <code>status_code</code> de 200 (successo). </p>

<p>O restante dos testes verificam se nossa <em>view</em> retorna apenas livros emprestados ao nosso usuário atual. Copie o código abaixo e cole no final da classe de teste acima.</p>

<pre class="brush: python notranslate">    def test_only_borrowed_books_in_list(self):
        login = self.client.login(username='testuser1', password='1X&lt;ISRUkw+tuK')
        response = self.client.get(reverse('my-borrowed'))

        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        # Check that initially we don't have any books in list (none on loan)
        self.assertTrue('bookinstance_list' in response.context)
        self.assertEqual(len(response.context['bookinstance_list']), 0)

        # Now change all books to be on loan
        books = BookInstance.objects.all()[:10]

        for book in books:
            book.status = 'o'
            book.save()

        # Check that now we have borrowed books in the list
        response = self.client.get(reverse('my-borrowed'))
        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        self.assertTrue('bookinstance_list' in response.context)

        # Confirm all books belong to testuser1 and are on loan
        for bookitem in response.context['bookinstance_list']:
            self.assertEqual(response.context['user'], bookitem.borrower)
            self.assertEqual('o', bookitem.status)

    def test_pages_ordered_by_due_date(self):
        # Change all books to be on loan
        for book in BookInstance.objects.all():
            book.status='o'
            book.save()

        login = self.client.login(username='testuser1', password='1X&lt;ISRUkw+tuK')
        response = self.client.get(reverse('my-borrowed'))

        # Check our user is logged in
        self.assertEqual(str(response.context['user']), 'testuser1')
        # Check that we got a response "success"
        self.assertEqual(response.status_code, 200)

        # Confirm that of the items, only 10 are displayed due to pagination.
        self.assertEqual(len(response.context['bookinstance_list']), 10)

        last_date = 0
        for book in response.context['bookinstance_list']:
            if last_date == 0:
                last_date = book.due_back
            else:
                self.assertTrue(last_date &lt;= book.due_back)
                last_date = book.due_back</pre>

<p>Você também pode adicionar testes de paginação, se desejar!</p>

<h4 id="Testando_views_com_forms">Testando <em>views</em> com <em>forms</em></h4>

<p>Testar views com forms é um pouco mais complicado que nos casos acima, porque você precisa testar mais caminhos de código: exibição inicial, exibição após falha de validação de dados e exibição após validação com sucesso. A boa notícia é que usamos o cliente para testar quase exatamente da mesma maneira que fizemos para <em>views</em> somente de exibição.</p>

<p>Para demonstrar, vamos escrever alguns testes para a <em>view</em> usada para renovar livros (<code>renew_book_librarian()</code>):</p>

<pre class="brush: python notranslate">from catalog.forms import RenewBookForm

@permission_required('catalog.can_mark_returned')
def renew_book_librarian(request, pk):
    """View function for renewing a specific BookInstance by librarian."""
    book_instance = get_object_or_404(BookInstance, pk=pk)

    # If this is a POST request then process the Form data
    if request.method == 'POST':

        # Create a form instance and populate it with data from the request (binding):
        book_renewal_form = RenewBookForm(request.POST)

        # Check if the form is valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required (here we just write it to the model due_back field)
            book_instance.due_back = form.cleaned_data['renewal_date']
            book_instance.save()

            # redirect to a new URL:
            return HttpResponseRedirect(reverse('all-borrowed'))

    # If this is a GET (or any other method) create the default form
    else:
        proposed_renewal_date = datetime.date.today() + datetime.timedelta(weeks=3)
        book_renewal_form = RenewBookForm(initial={'renewal_date': proposed_renewal_date})

    context = {
        'book_renewal_form': book_renewal_form,
        'book_instance': book_instance,
    }

    return render(request, 'catalog/book_renew_librarian.html', context)</pre>

<p>Precisamos testar se a <em>view</em> está disponível apenas para usuários que têm a permissão <code>can_mark_returned </code>, e se eles são direcionados para uma página de erro HTTP 404 se tentarem renovar um <code>BookInstance</code> que não existe. Devemos verificar se o valor inicial do form é propagado com uma data três semanas no futuro e se a validação for bem sucedida somos redirecionados para a <em>view</em> "all-borrowed books". Como parte da verificação dos testes de falha de validação, também verificaremos se nosso <em>form</em> está enviando mensagens de erro apropriadas.</p>

<p>Adicione a primeira parte da classe de teste (mostrada abaixo) na parte inferior de <strong>/catalog/tests/test_views.py</strong>. Isso cria dois usuários e duas instâncias de livro, mas apenas concede a um usuário a permissão necessária para acessar a <em>view</em>. O código para conceder permissões durante os testes é mostrado em negrito:</p>

<pre class="brush: python notranslate">import uuid

from django.contrib.auth.models import Permission # Required to grant the permission needed to set a book as returned.

class RenewBookInstancesViewTest(TestCase):
    def setUp(self):
        # Create a user
        test_user1 = User.objects.create_user(username='testuser1', password='1X&lt;ISRUkw+tuK')
        test_user2 = User.objects.create_user(username='testuser2', password='2HJ1vRV0Z&amp;3iD')

        test_user1.save()
        test_user2.save()

<strong>        permission = Permission.objects.get(name='Set book as returned')
        test_user2.user_permissions.add(permission)
        test_user2.save()</strong>

        # Create a book
        test_author = Author.objects.create(first_name='John', last_name='Smith')
        test_genre = Genre.objects.create(name='Fantasy')
        test_language = Language.objects.create(name='English')
        test_book = Book.objects.create(
            title='Book Title',
            summary='My book summary',
            isbn='ABCDEFG',
            author=test_author,
            language=test_language,
        )

        # Create genre as a post-step
        genre_objects_for_book = Genre.objects.all()
        test_book.genre.set(genre_objects_for_book) # Direct assignment of many-to-many types not allowed.
        test_book.save()

        # Create a BookInstance object for test_user1
        return_date = datetime.date.today() + datetime.timedelta(days=5)
        self.test_bookinstance1 = BookInstance.objects.create(
            book=test_book,
            imprint='Unlikely Imprint, 2016',
            due_back=return_date,
            borrower=test_user1,
            status='o',
        )

        # Create a BookInstance object for test_user2
        return_date = datetime.date.today() + datetime.timedelta(days=5)
        self.test_bookinstance2 = BookInstance.objects.create(
            book=test_book,
            imprint='Unlikely Imprint, 2016',
            due_back=return_date,
            borrower=test_user2,
            status='o',
        )</pre>

<p>Adicione os seguintes testes na parte inferior da classe de teste. Eles verificam se apenas usuários com a permissão correta (testuser2) podem aceesar a <em>view</em>. Verificamos todos os casos: quando o usuários não está logado, quando um usuário está logado mas não tem as permissões corretas, quando o usuário possui permissões, mas não é o tomador do empréstimo (deve ter êxito), e o que acontece quando eles tentam acessar uma <code>BookInstance</code> que não existe. Também verificamos se o <em>template</em> correto é utilizado.</p>

<pre class="brush: python notranslate">   def test_redirect_if_not_logged_in(self):
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        # Manually check redirect (Can't use assertRedirect, because the redirect URL is unpredictable)
        self.assertEqual(response.status_code, 302)
        self.assertTrue(response.url.startswith('/accounts/login/'))

    def test_redirect_if_logged_in_but_not_correct_permission(self):
        login = self.client.login(username='testuser1', password='1X&lt;ISRUkw+tuK')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 403)

    def test_logged_in_with_permission_borrowed_book(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance2.pk}))

        # Check that it lets us login - this is our book and we have the right permissions.
        self.assertEqual(response.status_code, 200)

    def test_logged_in_with_permission_another_users_borrowed_book(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))

        # Check that it lets us login. We're a librarian, so we can view any users book
        self.assertEqual(response.status_code, 200)

    def test_HTTP404_for_invalid_book_if_logged_in(self):
        # unlikely UID to match our bookinstance!
        test_uid = uuid.uuid4()
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk':test_uid}))
        self.assertEqual(response.status_code, 404)

    def test_uses_correct_template(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 200)

        # Check we used correct template
        self.assertTemplateUsed(response, 'catalog/book_renew_librarian.html')
</pre>

<p>Adicione o próximo método de teste, como mostrado abaixo. Isso verifica se a data inicial para o form é três semanas no futuro. Observe como podemos acessar o valor do valor inicial do campo do form (mostrado em negrito).</p>

<pre class="brush: python notranslate">    def test_form_renewal_date_initially_has_date_three_weeks_in_future(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        response = self.client.get(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}))
        self.assertEqual(response.status_code, 200)

        date_3_weeks_in_future = datetime.date.today() + datetime.timedelta(weeks=3)
        self.assertEqual(response<strong>.context['form'].initial['renewal_date']</strong>, date_3_weeks_in_future)
</pre>

<div class="warning">
<p>Se você usar a classe <em>form</em> <code>RenewBookModelForm(forms.ModelForm)</code> em vez da classe <code>RenewBookForm(forms.Form)</code>, então o nome do campo do <em>form</em> será <strong>'due_back' </strong>em vez de <strong>'renewal_date'</strong>.</p>
</div>

<p>O próximo teste (adicione isso a classe também) verifica se a <em>view</em> redireciona para uma lista de todos livros emprestados, se a renovação for bem-sucedida. O que difere aqui é que pela primeira vez mostramos como você pode fazer <code>POST</code> de dados usando o cliente. Os dados do <em>post</em> são o segundo argumento da função <em>post</em>, e são especificados como um dicionário de chave/valores.</p>

<pre class="brush: python notranslate">    def test_redirects_to_all_borrowed_book_list_on_success(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        valid_date_in_future = datetime.date.today() + datetime.timedelta(weeks=2)
        <strong>response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future})</strong>
        self.assertRedirects(response, reverse('all-borrowed'))
</pre>

<div class="warning">
<p>A view <em>all-borrowed</em> foi adicionada como um <em>desafio</em>, e seu código pode, em vez disso, direcionar para a página inicial '/'. Nesse caso, modifique as últimas duas linhas do código de teste para que sejam como o código abaixo. O <code>follow=True</code> na solicitação, garante que a solicitação retorna a URL final de destino (portanto verifique <code>/catalog/</code> em vez de <code>/</code>).</p>

<pre class="brush: python notranslate"> response = self.client.post(reverse('renew-book-librarian', kwargs={'pk':self.test_bookinstance1.pk,}), {'renewal_date':valid_date_in_future}, <strong>follow=True</strong> )
 <strong>self.assertRedirects(</strong><strong>response, '/catalog/')</strong></pre>
</div>

<p>Copie as última duas funções para a classe, como visto abaixo. Elas testam novamente as requisições <code>POST</code>, mas nesse caso, com datas inválidas de renovação. Utilizamos <code>assertFormError()</code> para verificar se as mensagens de erro são as esperadas.</p>

<pre class="brush: python notranslate">    def test_form_invalid_renewal_date_past(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        date_in_past = datetime.date.today() - datetime.timedelta(weeks=1)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}), {'renewal_date': date_in_past})
        self.assertEqual(response.status_code, 200)
        <strong>self.assertFormError(</strong><strong>response, 'form', 'renewal_date', 'Invalid date - renewal in past')</strong>

    def test_form_invalid_renewal_date_future(self):
        login = self.client.login(username='testuser2', password='2HJ1vRV0Z&amp;3iD')
        invalid_date_in_future = datetime.date.today() + datetime.timedelta(weeks=5)
        response = self.client.post(reverse('renew-book-librarian', kwargs={'pk': self.test_bookinstance1.pk}), {'renewal_date': invalid_date_in_future})
        self.assertEqual(response.status_code, 200)
        <strong>self.assertFormError(</strong><strong>response, 'form', 'renewal_date', 'Invalid date - renewal more than 4 weeks ahead')</strong>
</pre>

<p>Os mesmos tipos de técnicas podem ser usadas para testar a outra <em>view</em>.</p>

<h3 id="Templates"><em>Templates</em></h3>

<p>Django fornece APIs de teste para verificar se o template correto esta sendo chamado por suas views, e para permitir que você verifique se a informação correta está sendo enviada. Entretanto, não há suporte específico à API para testar no Django que sua saída HTML seja renderizada conforme esperado.</p>

<h2 id="Outras_ferramentas_de_teste_recomendadas">Outras ferramentas de teste recomendadas</h2>

<p>O framework de teste do Django pode ajudar você a escrever eficazes testes unitários e de integração — nós apenas arranhamos a superfície do que o framework <strong>unittest</strong> pode fazer, muito menos as adições de Django (por exemplo, confira como você pode usar <a href="https://docs.python.org/3.5/library/unittest.mock-examples.html">unittest.mock</a> para corrigir bibliotecas de terceiros para que você possa testar mais detalhadamente seu próprio código).</p>

<p>Embora existam inúmeras outras ferramentas de teste que você pode utilizar, destacaremos apenas duas:</p>

<ul>
 <li><a href="http://coverage.readthedocs.io/en/latest/">Coverage</a>: Essa ferramenta Python reporta quando do seu código é realmente executado pelos seus testes. É particularmente útil quando você começando e está tentando descobrir o que exatamente deve testar.</li>
 <li><a href="/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment">Selenium</a> é um framework para automatizar testes em um navegador real. Ele permite simular um usuário real interagindo com o site e fornece uma excelente estrutura para o sistema testar seu site (a próxima etapa do teste de integração).</li>
</ul>

<h2 id="Desafie-se">Desafie-se</h2>

<p>Existem muito mais <em>models</em> e <em>views</em> que podemos testar. Como uma tarefa simples, tente criar um caso de teste para a <em>view</em> <code>AuthorCreate</code>.</p>

<pre class="brush: python notranslate">class AuthorCreate(PermissionRequiredMixin, CreateView):
    model = Author
    fields = '__all__'
    initial = {'date_of_death':'12/10/2016'}
    permission_required = 'catalog.can_mark_returned'</pre>

<p>Lembre-se de que você precisa verificar qualquer coisa que você especificar ou que faça parte do projeto. Isso incluirá quem tem acesso, a data inicial, o <em>template</em> utilizado e para onde a view é redirecionada quando bem-sucedida.</p>

<h2 id="Resumo">Resumo</h2>

<p>Escrever código de teste não é divertido nem glamuroso, e é consequentemente muitas vezes deixado por último (ou nem isso) ao criar um site. No entanto, é uma parte essencial para garantir que seu código esteja seguro para <em>release</em> após fazer alterações e de baixo custo de manutenção.</p>

<p>Neste tutorial, mostramos como escrever e executar testes para seus <em>models</em>, <em>forms</em> e <em>views</em>. Mais importante ainda, fornecemos um breve resumo do que você deve testar, que geralmente é a coisa mais difícil de resolver quando você está iniciando. Há muito mais para conhecer, mas mesmo com o que você já aprendeu, poderá criar testes unitários eficazes para seus websites.</p>

<p>O próximo e último tutorial mostra como você pode implantar seu maravilhoso (e totalmente testado!) website Django.</p>

<h2 id="Veja_também">Veja também</h2>

<ul>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/testing/overview/">Escrevendo e executando testes</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/intro/tutorial05/">Escrevendo seu primeiro app Django, parte 5 &gt; Introduzindo testes automatizados</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/testing/tools/">Refererências de ferramentas de teste</a> (Django docs)</li>
 <li><a href="https://docs.djangoproject.com/en/2.1/topics/testing/advanced/">Tópicos avançados de testes</a> (Django docs)</li>
 <li><a href="http://toastdriven.com/blog/2011/apr/10/guide-to-testing-in-django/">Um Guia para testes no Django</a> (Toast Driven Blog, 2011)</li>
 <li><a href="http://test-driven-django-development.readthedocs.io/en/latest/index.html">Workshop: Desenvolvimento web Orientado a Testes com Django</a> (San Diego Python, 2014)</li>
 <li><a href="https://realpython.com/blog/python/testing-in-django-part-1-best-practices-and-examples/">Testando no Django (Parte 1) - Melhores práticas e Exemplos</a> (RealPython, 2013)</li>
</ul>

<p>{{PreviousMenuNext("Learn/Server-side/Django/Forms", "Learn/Server-side/Django/Deployment", "Learn/Server-side/Django")}}</p>

<h2 id="Neste_módulo">Neste módulo</h2>

<ul>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Introduction">Introdução ao Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/development_environment">Configurando um ambiente de desenvolvimento Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Tutorial_local_library_website">Tutorial Django: Website de uma Biblioteca Local</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/skeleton_website">Tutorial Django Parte 2: Criando a base do website</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Models">Tutorial Django Parte 3: Usando models</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Admin_site">Tutorial Django Parte 4: Django admin site</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Home_page">Tutorial Django Parte 5: Criando nossa página principal</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Generic_views">Tutorial Django Parte 6: Lista genérica e detail views</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Sessions">Tutorial Django Parte 7: Framework de Sessões</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Authentication">Tutorial Django Parte 8: Autenticação de Usuário e permissões</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Forms">Tutorial Django Parte 9: Trabalhando com formulários</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Testing">Tutorial Django Parte 10: Testando uma aplicação web Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/Deployment">Tutorial Django Parte 11: Implantando Django em produção</a></li>
 <li><a rel="nofollow" title="A página ainda não foi criada.">Segurança de aplicações web Django</a></li>
 <li><a href="/pt-BR/docs/Learn/Server-side/Django/django_assessment_blog">DIY Django mini blog</a></li>
</ul>