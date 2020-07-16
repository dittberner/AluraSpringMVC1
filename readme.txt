AULA 01

1) Baixe o JBoss Forge acessando a sua página de Download e, em Get Forge for your OS, baixe o zip específico para o seu sistema operacional. Atualmente a versão completa seria a "JBoss Developer Studio".

2) Extraia o zip baixado no passo anterior dentro do seu workspace. Em seguida, abra o seu terminal (ou Prompt de Comando), e acesse o seu workspace.

3) Dentro do seu workspace, inicie o terminal do JBoss Forge, acessando a sua subpasta bin, e executando o aplicativo forge, por exemplo:

alura-Mac-mini:workspace alura$ forge/bin/forge

4) Agora, crie o projeto casadocodigo:

project-new --named casadocodigo

5) Abra o Eclipse e no Project Explorer, clique com o botão direito do mouse e acesse Import -> Import.... Selecione Existing Maven Projects e clique em Next. Em Root Directory, clique em Browse..., selecione o projeto criado acima e clique em Finish.

6) Para rodar a aplicação, é necessário um servidor. Então, faça o download do Tomcat aqui e adicione-o no Eclipse. Para isso, extraia o zip, volte ao Eclipse e, através do atalho CTRL + 3, acesse a view Servers, clique para criar um novo servidor, escolha o Tomcat na versão 7 e aponte para a pasta do Tomcat que você extraiu anteriormente.

7) No Eclipse, na view Servers, associe o projeto casadocodigo com o Tomcat. Basta clicar com o botão da direita do mouse sobre o servidor, selecionar Add and Remove..., clicar no projeto casadocodigo e clicar em Add e em seguida em Finish.

8) Para usar o Spring MVC, adicione a sua dependência ao projeto, no arquivo pom.xml. Abra esse arquivo e, após o fechamento da tag <build> e antes da abertura da tag <properties>, adicione as seguintes dependências:

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>4.1.0.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>tomcat-servlet-api</artifactId>
        <version>7.0.30</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.1</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp.jstl</groupId>
        <artifactId>jstl-api</artifactId>
        <version>1.2</version>
        <exclusions>
            <exclusion>
                <groupId>javax.servlet</groupId>
                <artifactId>servlet-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.glassfish.web</groupId>
        <artifactId>jstl-impl</artifactId>
        <version>1.2</version>
        <exclusions>
            <exclusion>
                <groupId>javax.servlet</groupId>
                <artifactId>servlet-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.6.1</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>1.6.1</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.6.1</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.16</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>

Criando o primeiro Controller

9) Crie a classe HomeController dentro do pacote br.com.casadocodigo.loja.controllers. Em seguida, anote a classe com @Controller, para que o Spring MVC a reconheça como um controller.

@Controller
public class HomeController {

}

10) Agora, dentro da classe HomeController, crie o método home, que será o responsável por atender a home da aplicação, ou seja, o endereço /. Por fim, anote esse método com @RequestMapping("/"), para atender o endereço mencionado anteriormente, imprima a mensagem Exibindo a home da CDC e retorne o nome da página JSP que será exibida, home:

@Controller
public class HomeController {

    @RequestMapping("/")
    public String index() {
        System.out.println("Exibindo a home da CDC");
        return "home";
    }
}

11) Configure a Servlet do Spring MVC para que ela atenda as requisições recebidas pelo servidor. Para isso, crie a classe ServletSpringMVC dentro do pacote br.com.casadocodigo.loja.conf. Em seguida, estenda a classe AbstractAnnotationConfigDispatcherServletInitializer e implemente os métodos conforme abaixo:

public class ServletSpringMvc extends AbstractAnnotationConfigDispatcherServletInitializer{

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[] { AppWebConfiguration.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] {"/"};
    }

}

12) Dentro do método getServletConfigClasses(), está sendo utilizada uma referência da classe AppWebConfiguration. A partir dessa classe, faça a configuração do MVC para a Servlet do Spring. Portanto, crie também essa classe, no mesmo pacote br.com.casadocodigo.loja.conf, com o seguinte código:

@EnableWebMvc
@ComponentScan(basePackageClasses={HomeController.class})
public class AppWebConfiguration {

}

13) Dentro dessa classe, crie o método internalResourceViewResolver, responsável por indicar qual diretório estão as views:

@Bean
public InternalResourceViewResolver internalResourceViewResolver() {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
}

A partir dos métodos setPrefix() e setSuffix(), você define o local (WEB-INF/views/) e extensão (jsp) das views, respectivamente. Por fim, anotando esse método com @Bean, você transforma-o em um Bean, para que o Spring o gerencie.

14) Resta criar o diretório do projeto no qual o Spring vai procurar as views, isto é, as suas páginas. Para isso, dentro do diretório src/main/webapp crie o diretório WEB-INF/views, onde as views serão armazenadas.

15) Agora, dentro do diretório src/main/webapp/WEB-INF/views, crie a página home.jsp, com o seguinte conteúdo:

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Livros de Java, Android, iPhone, PHP, Ruby e muito mais - Casa do Código</title>
</head>
<body>
    <h1>Casa do Código</h1>
    <table>
        <tr>
            <td>Java 8 Prático</td>
            <td>Certificação OCJP</td>
        </tr>
        <tr>
            <td>TDD na Prática - Java</td>
            <td>Google Android</td>
        </tr>
    </table>
</body>
</html>

16) Por fim, inicie o servidor e acesse http://localhost:8080/casadocodigo/.

AULA 02

1) Crie um controller específico para os produtos dentro do seu sistema, chamado ProdutosController, dentro do pacote br.com.caelum.loja.controllers. Nessa classe, crie o método form(), que atende à URL produtos/form. Esse método deve retornar produtos/form, que é justamente o local onde você irá criar o formulário de cadastro de produtos:

@Controller
public class ProdutosController {

    @RequestMapping("/produtos/form")
    public String form() {
        return "produtos/form";
    }
}

2) Agora, crie a view, ou seja, o formulário de cadastro de produtos. Crie a pasta produtos dentro de src/main/webapp/WEB-INF/views/ e dentro dela, crie a página form.jsp com o seguinte conteúdo:

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Livros de Java, Android, iPhone, PHP, Ruby e muito mais - Casa do Código</title>
</head>
<body>
    <form action="/casadocodigo/produtos" method="POST">
        <div>
            <label>Título</label>
            <input type="text" name="titulo" />
        </div>
        <div>
            <label>Descrição</label>
            <textarea rows="10" cols="20" name="descricao"></textarea>
        </div>
        <div>
            <label>Páginas</label>
            <input type="text" name="paginas" />
        </div>
        <button type="submit">Cadastrar</button>
    </form>
</body>
</html>

3) Crie o método gravar() na classe ProdutosController.java, que atenderá à URL casadocodigo/produtos que é justamente o endereço que o formulário está enviando os dados. Esse método será o responsável por receber os dados do formulário, então receba-os por parâmetro e imprima-os dentro do método. O nome dos atributos devem ser exatamente os mesmos valores contidos nos atributos name dos inputs do formulário de cadastro, pois o binding do Spring vincula cada valor de acordo com o seu nome. Por fim, esse método deve retornar produtos/ok, que será a página para onde o usuário será enviado:

@RequestMapping("/produtos")
public String gravar(String titulo, String descricao, int paginas) {
    System.out.println(titulo);
    System.out.println(descricao);
    System.out.println(paginas);
    return "produtos/ok";
}

4) Ao invés de receber diversos parâmetros no método, o ideal é receber uma classe que representa todos esses eles. Então, crie a classe Produto no pacote br.com.casadocodigo.loja.models com os atributos titulo, descricao e paginas, além dos seus getters e setters e de uma implementação do método toString:

public class Produto {

    private String titulo;
    private String descricao;
    private int paginas;

    public String getTitulo() {
        return titulo;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }

    public String getDescricao() {
        return descricao;
    }

    public void setDescricao(String descricao) {
        this.descricao = descricao;
    }

    public int getPaginas() {
        return paginas;
    }

    public void setPaginas(int paginas) {
        this.paginas = paginas;
    }

    @Override
    public String toString() {
        return "Produto [titulo=" + titulo + ", descricao=" + descricao + ", paginas=" + paginas + "]";
    }
}

5) No método gravar, de ProdutosController, ao invés de passar todos aqueles parâmetros, envie apenas um Produto e imprima-o dentro do método:

@RequestMapping("/produtos")
public String gravar(Produto produto) {
    System.out.println(produto);
    return "produtos/ok";
}

6) Agora, crie a view ok.jsp, dentro src/main/webapp/WEB-INF/views/produtos, com o seguinte conteúdo:

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Livros de Java, Android, iPhone, PHP, Ruby e muito mais - Casa do Código</title>
</head>
<body>
    <h1>Produto cadastrado com sucesso!</h1>
</body>
</html>

Spring com JPA

7) Adicione a dependência do Hibernate ao seu projeto. Abra o arquivo pom.xml e dentro da tag <dependencies>, adicione o seguinte XML:

<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-entitymanager</artifactId>
    <version>4.3.0.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>4.3.0.Final</version>
</dependency>
<dependency>
    <groupId>org.hibernate.javax.persistence</groupId>
    <artifactId>hibernate-jpa-2.1-api</artifactId>
    <version>1.0.0.Final</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>4.1.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.15</version>
</dependency>

8) Para transformar as classes modelos como entidades do banco de dados, anote-as com @Entity. Então, faça isso com a classe Produto:

@Entity
public class Produto {

    // restante do código omitido

}

9) Além disso, o Hibernate obriga que toda entidade precisa de um id, ou seja, um campo que contenha um valor único para cada registro. Atualmente, a classe Produto não tem nenhum atributo que possa ser um id, portanto, crie mais um atributo, chamado id, do tipo int, e também gere seus getters e setters:

@Entity
public class Produto {

    private int id;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    // restante do código omitido

}

10) Para o Hibernate entender que o atributo id é o id da entidade, adicione a anotação @Id no atributo. Além disso, popule-o antes de persisti-lo, fazendo com que o próprio banco já atribua um valor do id automaticamente, anotando-o com @GeneratedValue.

Por fim, informe ao Hibernate como ele deve atribuir esse id automaticamente a partir do atributo strategy da anotação @GeneratedValue. Nesse caso, faça com que ele seja auto-incremental, enviando o parâmetro strategy = GenerationType.IDENTITY:

@Entity
public class Produto {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    // restante do código omitido

}

11) Crie a classe JPAConfiguration, no nosso pacote br.com.casadocodigo.loja.conf, que será a responsável por configurar o framework, passando informações relevantes como o banco a ser utilizado, seu usuário e senha, e assim por diante:

public class JPAConfiguration {

    @Bean
    public LocalContainerEntityManagerFactoryBean entityManagerFactory() {

        LocalContainerEntityManagerFactoryBean factoryBean = 
            new LocalContainerEntityManagerFactoryBean();

        JpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();

        factoryBean.setJpaVendorAdapter(vendorAdapter);

        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setUsername("root");
        dataSource.setPassword(""); // modifique para a senha do seu banco
        dataSource.setUrl("jdbc:mysql://localhost:3306/casadocodigo");
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        factoryBean.setDataSource(dataSource);

        Properties props = new Properties();
        props.setProperty("hibernate.dialect", "org.hibernate.dialect.MySQL5Dialect");
        props.setProperty("hibernate.show_sql", "true");
        props.setProperty("hibernate.hbm2ddl.auto", "update");
        factoryBean.setJpaProperties(props);

        factoryBean.setPackagesToScan("br.com.casadocodigo.loja.models");

        return factoryBean;

    }
}

12) Crie a classe ProdutoDAO, no pacote br.com.casadocodigo.loja.daos. Dentro dessa classe, faça a comunicação com o banco de dados, adicionando um atributo do tipo EntityManager, e crie o método gravar(), que recebe um produto como parâmetro, e então, a partir do atributo do tipo EntityManager, persista um objeto a partir do método persist(), enviando o parâmetro do tipo Produto como argumento.

Já que o EntityManager trata-se de um recurso persistente, utilize a anotação @PersistenceContext para que ele seja injetável:

@Repository
public class ProdutoDAO {

    @PersistenceContext
    private EntityManager manager;

    public void gravar(Produto produto) {
        manager.persist(produto);
    }
}

13) Dentro do ProdutosController, crie um atributo do tipo ProdutoDAO e anote com @Autowired, para que ele seja injetado. Em seguida, dentro do método gravar() do controller, use o atributo do tipo ProdutoDAO e chame o método gravar(), enviando o produto recebido por parâmetro:

@Controller
public class ProdutosController {

    @Autowired
    private ProdutoDAO produtoDao;

    @RequestMapping("/produtos/form")
    public String form() {
        return "produtos/form";
    }

    @RequestMapping("/produtos")
    public String gravar(Produto produto) {        
        produtoDao.gravar(produto);

        return "produtos/ok";
    }
}

14) Para o Spring encontrar o DAO, adicione-o na anotação @ComponentScan, na classe AppWebConfiguration:

@EnableWebMvc
@ComponentScan(basePackageClasses={HomeController.class, ProdutoDAO.class})
public class AppWebConfiguration {

    // restante do código omitido

}

15) E para o Spring encontrar a classe JPAConfiguration, adicione-a no método getServletConfigClasses, da classe ServletSpringMVC:

@Override
protected Class<?>[] getServletConfigClasses() {
    return new Class[] { AppWebConfiguration.class, JPAConfiguration.class};
}

Configurando o TransactionManager

16) Para o Spring gerenciar as transações para nós, adicione a anotação @EnableTransactionManagement na classe JPAConfiguration:

@EnableTransactionManagement
public class JPAConfiguration {

    // restante do código omitido

}

17) Em seguida, adicione um bean que será o gerenciador das transações, isto é, a partir desse bean, o Spring fornecerá as transações para o EntityManager. Para isso adicione o código abaixo na classe JPAConfiguration:

@Bean
public JpaTransactionManager transactionManager(EntityManagerFactory emf) {
    return new JpaTransactionManager(emf);
}

18) O ProdutoDAO é um recurso persistente (persiste dados) dentro do sistema, portanto, anote-o com @Repository. E embora o Spring esteja configurado para gerenciar as transações, ainda é necessário indicar que o ProdutoDAO precisa de uma transação. Faça isso anotando-o com @Transactional:

@Repository
@Transactional
public class ProdutoDAO {

    @PersistenceContext
    private EntityManager manager;

    public void gravar(Produto produto) {
        manager.persist(produto);
    }
}

19) Agora, crie a base de dados casadocodigo no MySQL:

mysql -u root
mysql> create database casadocodigo;

20) Por fim, reinicie o Tomcat e acesse a URL http://localhost:8080/casadocodigo/produtos/form e cadastre um produto. Se ele for cadastrado com sucesso, veja-o criado na base de dados.

AULA 03

1) O tipo do preço de cada livro está limitado a 3 opções (ebook, impresso e combo), portanto, crie a Enum TipoPreco, no pacote br.com.casadocodigo.loja.models, com os valores EBOOK, IMPRESSO e COMBO:

public enum TipoPreco {

    EBOOK, IMPRESSO, COMBO;
}
2) Crie a classe Preco no pacote br.com.casadocodigo.loja.models com o atributo valor, do tipo BigDecimal, e tipo, do tipo TipoPreco. Gere também seus getters e setters desses atributos:

public class Preco {

    private BigDecimal valor;
    private TipoPreco tipo;

    public BigDecimal getValor() {
        return valor;
    }

    public void setValor(BigDecimal valor) {
        this.valor = valor;
    }

    public TipoPreco getTipo() {
        return tipo;
    }

    public void setTipo(TipoPreco tipo) {
        this.tipo = tipo;
    }

}
3) Agora, adicione esses preços na classe Produto. Como cada livro pode ter mais de um preço, utilize uma lista. Gere também o getter e setter desse atributo:

@Entity
public class Produto {

    private List<Preco> precos;

    public List<Preco> getPrecos() {
        return precos;
    }

    public void setPrecos(List<Preco> precos) {
        this.precos = precos;
    }

        // restante do código omitido

}
4) Anote a classe Preco com @Embeddable, que a permite ser persistida, desde que ela seja um atributo de uma entidade, e no caso ela é um atributo da classe Produto, que é uma entidade:

@Embeddable
public class Preco {

    // restante do código omitido

}
5) Para indicar que você irá armazenar uma lista de Preco, de uma classe @Embeddable, utilize a anotação @ElementCollection. Portanto, anote o atributo precos da classe Produto:

@ElementCollection
private List<Preco> precos;
6) Para adicionar os campos de preço na view, passe os tipos de preço para a JSP. Para isso, utilize o ModelAndView, que, além de carregar a página, permite enviar objetos para a view. No controller ProdutosController, altere o método form() para retornar um ModelAndView no lugar de uma string. Em seguida, crie um objeto dessa classe que retorna a página produtos/form.

Além de retornar a página, envie o objeto que representa os tipos de preço, através do método addObject, da classe ModelAndView, enviando uma string que indica o nome do objeto como primeiro parâmetro e os valores do objeto como segundo parâmetro:

@RequestMapping("/produtos/form")
public ModelAndView form() {

    ModelAndView modelAndView = new ModelAndView("produtos/form");
    modelAndView.addObject("tipos", TipoPreco.values());

    return modelAndView;    
}
7) Percorra a lista de tipos para exibir os dados no formulário, usando o forEach da JSTL. Então, em form.jsp, antes do botão de cadastro, adicione:

<c:forEach items="${tipos}" var="tipoPreco" varStatus="status">
    <div>
        <label>${tipoPreco}</label>
        <input type="text" name="precos[${status.index}].valor">
        <input type="hidden" name="precos[${status.index}].tipo" value="${tipoPreco}">
    </div>
</c:forEach>
8) E não esqueça de importar a JSTL através da taglib, antes da tag <html>:

<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

AULA 04

1) Com alguns livros cadastrados no banco de dados, chegou a hora de listá-los. Primeiramente, busque-os na base de dados, então crie o método listar() na classe ProdutoDAO, que devolve uma lista de produtos. Dentro desse método, utilize o createQuery() para realizar a query que busca todos os produtos. Por fim, utilize o método getResultList() para retornar a lista de produtos:

public List<Produto> listar() {
    return manager.createQuery("select p from Produto p", Produto.class)
        .getResultList();
}
2) Com a busca de livros no banco de dados pronta, crie o método listar() no ProdutoController, que mapeia o endereço produtos. Esse método irá buscar os livros no banco, criar um objeto do tipo ModelAndView que devolve a view produtos/lista e adicionar a lista de produtos na view. Ao final, retorne o objeto de ModelAndView:

@RequestMapping("produtos")
public ModelAndView listar() {
    List<Produto> produtos = produtoDAO.listar();
    ModelAndView modelAndView = new ModelAndView("produtos/lista");
    modelAndView.addObject("produtos", produtos);

    return modelAndView;
}
3) Agora que todos os produtos contidos no banco de dados estão sendo devolvidos, exiba-os em uma view. Crie a JSP lista.jsp dentro do diretório src/main/webapp/WEB-INF/views/produtos. Dentro desse arquivo, adicione o seguinte código:

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset=UTF-8>
    <title>Livros de Java, Android, iPhone, PHP, Ruby e muito mais - Casa do Código</title>
</head>
<body>
    <h1>Lista de Produtos</h1>
    <table>
        <tr>
            <td>Título</td>
            <td>Descrição</td>
            <td>Páginas</td>
        </tr>
        <c:forEach items="${produtos}" var="produto">
            <tr>
                <td>${produto.titulo}</td>
                <td>${produto.descricao}</td>
                <td>${produto.paginas}</td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>
4) Em ProdutoController, os métodos gravar e listar respondem à mesma URL, então indique a sua intenção utilizando os métodos de requisição do HTTP, como o GET para listar os dados e POST para inserir, por exemplo:

@Controller
public class ProdutosController {

    @RequestMapping(value="produtos", method = RequestMethod.POST)
    public String gravar(Produto produto) {
        // código omitido
    }

    @RequestMapping(value="produtos", method = RequestMethod.GET)
    public ModelAndView listar() {
        // código omitido
    }

        // restante do código omitido

}
5) Reinicie o Tomcat e acesse http://localhost:8080/casadocodigo/produtos. Na listagem dos produtos, os dados que possuem acentos estão com problemas de apresentação. Então defina qual é o encoding da aplicação, através de um filtro do Spring. Para isso, adicione o seguinte método na classe ServletSpringMVC:

@Override
protected Filter[] getServletFilters() {
    CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
    encodingFilter.setEncoding("UTF-8");

    return new Filter[] {encodingFilter};
}
6) Para finalizar, todos os mapeamentos da classe ProdutosController começam com /produtos, mas ficar digitando e dar manutenção neles é complicado. Para resolver isso, adicione a anotação @RequestMapping("/produtos") na classe ProdutosController e apague o prefixo de todos os outros métodos:

@Controller
@RequestMapping("/produtos")
public class ProdutosController {

    @Autowired
    private ProdutoDao produtoDao;

    @RequestMapping("form")
    public ModelAndView form() {
        ModelAndView modelAndView = new ModelAndView("produtos/form");
        modelAndView.addObject("tipos", TipoPreco.values());

        return modelAndView;
    }

    @RequestMapping(method=RequestMethod.POST)
    public String gravar(Produto produto) {
        produtoDAO.gravar(produto);
        return "produtos/ok";
    }

    @RequestMapping(method=RequestMethod.GET)
    public ModelAndView listar() {
        List<Produto> produtos = produtoDao.listar();
        ModelAndView modelAndView = new ModelAndView("produtos/lista");
        modelAndView.addObject("produtos", produtos);

        return modelAndView;
    }
}

AULA 05

1) Após a gravação de um produto, envie o usuário para a listagem de produtos. Para isso, na classe ProdutosController, no método gravar, utilize o ModelAndView, passando a string com o prefixo redirect:

@RequestMapping(method=RequestMethod.POST)
public ModelAndView gravar(Produto produto) {
    produtoDao.gravar(produto);
    return new ModelAndView("redirect:produtos");
}
2) Faça uso do escopo de Flash para enviar uma mensagem de sucesso, que será exibida após o redirect do navegador, na tela de listagem. Adicione o atributo ${sucesso} na JSP lista.jsp:

<body>
    <h1>Lista de Produtos</h1>
    <div>${sucesso}</div>
<!-- restante do código continua abaixo -->
3) Agora, no ProdutosController, receba o RedirectAttributes para adicionar o atributo sucesso com escopo de Flash:

@RequestMapping(method=RequestMethod.POST)
public ModelAndView gravar(Produto produto, RedirectAttributes redirectAttributes) {
    produtoDao.gravar(produto);
    redirectAttributes.addFlashAttribute("sucesso", "Produto cadastrado com sucesso!");

    return new ModelAndView("redirect:produtos");
}

AULA 06

1) Para realizar a validação dos dados, adicione mais algumas dependências ao projeto. Para isso, cole o seguinte XML dentro da tag <dependencies> do arquivo pom.xml:

<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>1.0.0.GA</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>4.2.0.Final</version>
</dependency>
2) Para validar um produto, crie uma classe que será responsável por realizar essa validação, a ProdutoValidation, no pacote br.com.casadocodigo.loja.validation e implemente a interface Validator do Spring. Para adicionar os erros de validação, rejeitar se o campo estiver vazio, você pode usar o método rejectIfEmpty(), da classe ValidationUtils. E no método supports(), você pode usar o método isAssignableFrom() para verificar se classe recebida por parâmetro é de fato um Produto:

public class ProdutoValidation implements Validator {

    @Override
    public boolean supports(Class<?> clazz) {
        return Produto.class.isAssignableFrom(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmpty(errors, "titulo", "field.required");
        ValidationUtils.rejectIfEmpty(errors, "descricao", "field.required");

        Produto produto = (Produto) target;

        if(produto.getPaginas() <= 0) {
            errors.rejectValue("paginas", "field.required");
        }
    }

}
3) Para que o ProdutosController possa reconhecer o validador, crie o método initBinder(), que ficará responsável por vincular o validador com o controller. Esse método recebe um WebDataBinder, e então, anote-o com @InitBinder, para que o Spring o chame automaticamente. A partir do parâmetro WebDataBinder, utilize o método addValidators() para adicionar o ProdutoValidation para o ProdutosController:

@InitBinder
public void initBinder(WebDataBinder binder) {
    binder.addValidators(new ProdutoValidation());
}
4) Com a estrutura para realizar a validação pronta, falta executá-la. Ainda no ProdutosController, faça com que o produto seja validado, então vá até o método gravar() e, no parâmetro do tipo Produto, anote com @Valid:

@RequestMapping(method=RequestMethod.POST)
public ModelAndView gravar(@Valid Produto produto, RedirectAttributes redirectAttributes) {

    produtoDao.gravar(produto);

    redirectAttributes.addFlashAttribute("sucesso", "Produto cadastrado com sucesso!");

    return new ModelAndView("redirect:produtos");
}
5) Além de validar o produto, você precisa saber quais foram os erros obtidos durante a validação. Para pegá-los, adicione o parâmetro BindingResult no método gravar(). Durante a validação, o Spring vai coletar todos os erros e irá adicionar nesse parâmetro mas para que isso funcione, é necessário adicioná-lo logo em seguida do objeto que será validado, em outras palavras, o BindingResult tem que ser declarado logo após o produto:

@RequestMapping(method=RequestMethod.POST)
public ModelAndView gravar(@Valid Produto produto, BindingResult result, 
    RedirectAttributes redirectAttributes) {

    produtoDao.gravar(produto);

    redirectAttributes.addFlashAttribute("sucesso", "Produto cadastrado com sucesso!");

    return new ModelAndView("redirect:produtos");
}
6) Por fim, antes de executar qualquer instrução, faça um if, passando como argumento o método hasErrors(), do BindingResult, e se houver erros, retorne o método form(), ou seja, redirecione o usuário para o formulário. Dessa forma, você fará com que o usuário volte para o formulário todas as vezes que ocorrer uma falha de validação:

@RequestMapping(method=RequestMethod.POST)
public ModelAndView gravar(@Valid Produto produto, BindingResult result, 
        RedirectAttributes redirectAttributes) {

    if (result.hasErrors()) {
        return form();
    }

    produtoDao.gravar(produto);

    redirectAttributes.addFlashAttribute("sucesso", "Produto cadastrado com sucesso!");

    return new ModelAndView("redirect:produtos");
}

AULA 07

1) Para adicionar mensagens de validação no projeto, crie o arquivo que define essas mensagens, conhecido como messages.properties. Ele segue um formato bem parecido com chave/valor. Por exemplo, caso você queira adicionar mensagens para campos que são obrigatórios, basta preencher a chave field.required com o valor Campos obrigatórios. Você também pode definir mensagens mais específicas. Então, na pasta WEB-INF, crie o arquivo messages.properties com o seguinte conteúdo, por exemplo:

field.required = Campo obrigatório
field.required.produto.titulo = O título é obrigatório
field.required.produto.paginas = Informe o número de páginas
field.required.produto.descricao = A descrição é obrigatória
typeMismatch = O tipo de dado é inválido
typeMismatch.produto.paginas = Digite um valor válido. Exemplo: "100"
2) Assim que o arquivo for salvo, clique nele com o botão direito do mouse e selecione Properties. Em Text file encoding, verifique se o encoding é UTF-8, caso não seja, altere-o.

3) Para o Spring encontrar o arquivo messages.properties, crie um novo método na classe AppWebConfiguration, que irá retornar o arquivo com algumas configurações. Crie um novo método como o exemplo abaixo:

@Bean
public MessageSource messageSource() {
    ReloadableResourceBundleMessageSource messageSource =
        new ReloadableResourceBundleMessageSource();

    messageSource.setBasename("/WEB-INF/messages");
    messageSource.setDefaultEncoding("UTF-8");
    messageSource.setCacheSeconds(1);

    return messageSource;
}
4) Para adicionar as mensagens no formulário form.jsp, adicione uma nova taglib:

<%@ taglib uri="http://www.springframework.org/tags/form" prefix="form" %>
5) Além disso, substitua a tag <form> para:

<form:form action="/casadocodigo/produtos" method="POST" commandName="produto">
Caso esteja usando dependências atualizadas e der o erro Unable to find setter method for attribute: [commandName], troque o commandName para modelAttribute.

6) Adicione as mensagens de erro para os campos de título, descrição e páginas:

<form:form action="/casadocodigo/produtos" method="POST" commandName="produto">
    <div>
        <label>Título</label> 
        <input type="text" name="titulo">
        <form:errors path="titulo" />
    </div>
    <div>
        <label>Descrição</label>
        <textarea rows="10" cols="20" name="descricao"></textarea>
        <form:errors path="descricao" />
    </div>

    <div>
        <label>Páginas</label>
        <input type="text" name="paginas">
        <form:errors path="paginas" />
    </div>

    <c:forEach items="${tipos}" var="tipoPreco" varStatus="status">
        <div>
            <label>${tipoPreco}</label>
            <input type="text" name="precos[${status.index}].valor">
            <input type="hidden" name="precos[${status.index}].tipo" value="${tipoPreco}">
        </div>
    </c:forEach>
    <button type="submit">Cadastrar</button>
</form:form>
7) Por fim, para evitar de ficar mexendo na action do formulário, peça para o Spring preencher a URL para você. Para isso, adicione a taglib abaixo:

<%@ taglib uri="http://www.springframework.org/tags" prefix="s" %>
8) E use o método mvcUrl("controller#metodo").build() para criar a URL:

<form:form action="${s:mvcUrl('PC#gravar').build()}" method="POST" commandName="produto">
O controller é referenciado pelas suas letras maiúsculas, então ProdutosController vira PC.

9) Por fim, para que a URL possa ser construída de forma correta, separando os contextos, modifique o @RequestMapping do ProdutosController para /produtos:

@Controller
@RequestMapping("/produtos")
public class ProdutosController {

    // restante do código omitido

}

AULA 08

1) Agora que o livro terá data de lançamento, na classe Produto, crie o atributo dataLancamento, do tipo Calendar e adicione a anotação @DateTimeFormat, para que o Spring consiga converter os valores de texto para Calendar. Além disso, gere o getter e setter desse atributo:

@Entity
public class Produto {

    @DateTimeFormat
    private Calendar dataLancamento;

    public Calendar getDataLancamento() {
        return dataLancamento;
    }

    public void setDataLancamento(Calendar dataLancamento) {
        this.dataLancamento = dataLancamento;
    }

    // restante do código omitido

}
2) No formulário form.jsp, logo após a div das páginas, crie mais uma div:

<div>
    <label>Data de Lançamento</label>
    <input name="dataLancamento" type="text" />
    <form:errors path="dataLancamento" />
</div>
3) Para não ter que configurar o formato da data por anotações, configure-o através do AppWebConfiguration, criando o método abaixo:

@Bean
public FormattingConversionService mvcConversionService() {
    DefaultFormattingConversionService conversionService = 
        new DefaultFormattingConversionService();
    DateFormatterRegistrar registrar = new DateFormatterRegistrar();
    registrar.setFormatter(new DateFormatter("dd/MM/yyyy"));
    registrar.registerFormatters(conversionService);

    return conversionService;
}
4) E para não perder as informações se um campo estiver inválido, troque as tags do formulário no form.jsp de <input type="text" name="nome" /> para <form:input path="nome" />, por exemplo. Ele ficará com a seguinte estrutura:

<form:form action="${s:mvcUrl('PC#grava').build() }" method="post" 
    commandName="produto">    

    <div>
        <label>Título</label>
        <form:input path="titulo" />
        <form:errors path="titulo" />
    </div>
    <div>
        <label>Descrição</label>
        <form:textarea path="descricao" rows="10" cols="20"/>
        <form:errors path="descricao" />
    </div>name
    <div>
        <label>Páginas</label> 
        <form:input path="paginas" />
        <form:errors path="paginas" />
    </div>
    <div>
        <label>Data de lançamento</label>
        <form:input path="dataLancamento" />
        <form:errors path="dataLancamento" />
    </div>
    <c:forEach items="${tipos}" var="tipoPreco" varStatus="status">
        <div>
            <label>${tipoPreco}</label> 
            <form:input path="precos[${status.index}].valor" />
            <form:hidden path="precos[${status.index}].tipo" 
                value="${tipoPreco}" />
        </div>
    </c:forEach>
    <button type="submit">Cadastrar</button>
</form:form>
5) Para isso funcionar, faça o método form() do ProdutoController receber um Produto:

@RequestMapping("form")
public ModelAndView form(Produto produto) {
    ModelAndView modelAndView = new ModelAndView("produtos/form");
    modelAndView.addObject("tipos", TipoPreco.values());

    return modelAndView;
}
6) O método gravar, também do ProdutoController irá apresentar um erro, pois dentro dele há uma chamada para o método form. Então passe um Produto para ele:

@RequestMapping(method=RequestMethod.POST)
public ModelAndView grava(@Valid Produto produto, BindingResult result, 
        RedirectAttributes redirectAttributes) {

    if(result.hasErrors()) {
        return form(produto);
    }

    produtoDao.gravar(produto);

    redirectAttributes.addFlashAttribute("sucesso", "Produto cadastrado com sucesso!");

    return new ModelAndView("redirect:produtos");
}

AULA 09

1) Permita que o usuário envie um sumário do livro em PDF. No form.jsp, dentro do formulário, antes do botão de cadastro, crie um novo campo do tipo file. Além disso, defina o valor do enctype do formulário para multipart/form-data:

<form:form action="${s:mvcUrl("PC#gravar").build()}" method="POST"
    commandName="produto" enctype="multipart/form-data">

    <!-- Outros campos omitidos -->

    <div>
        <label>Sumário</label> 
        <input name="sumario" type="file" />
    </div>
    <button type="submit">Cadastrar</button>
</form:form>
2) Na classe Produto, adicione um atributo chamado sumarioPath do tipo String, juntamente com seu getter e setter:

@Entity
public class Produto {

    private String sumarioPath;

    public String getSumarioPath() {
        return sumarioPath;
    }

    public void setSumarioPath(String sumarioPath) {
        this.sumarioPath = sumarioPath;
    }

    // restante do código omitido
}
3) Configure o Spring para trabalhar com arquivos. Na classe AppWebConfiguration, crie o seguinte método:

@Bean
public MultipartResolver multipartResolver() {
    return new StandardServletMultipartResolver();
}
4) Além disso, altere a configuração das servlets. Então, na classe ServletSpringMVC, sobrescreva o seguinte método:

@Override
protected void customizeRegistration(Dynamic registration) {
    registration.setMultipartConfig(new MultipartConfigElement(""));
}
5) Crie a lógica de salvar um arquivo. Para isso ,crie a classe FileSaver, no pacote br.com.caelum.loja.infra, com um método chamado write, que deverá salvar o arquivo em uma pasta recebida por parâmetro. Lembre-se que você pode usar o método transferTo da classe MultiPartFile para realizar essa tarefa, e para pegar o caminho completo até a pasta, use um HttpServletRequest, utilizando a anotação @Autowired para o Spring injetar essa dependência:

@Component
public class FileSaver {

    @Autowired
    private HttpServletRequest request;

    public String write(String baseFolder, MultipartFile file) {

        try {
            String realPath = request.getServletContext().getRealPath("/" + baseFolder);
            String path = realPath + "/" + file.getOriginalFilename();
            file.transferTo(new File(path));

            return baseFolder + "/" + file.getOriginalFilename();

        } catch (Exception e) {
            throw new RuntimeException(e);    
        }

    }
}
6) Com a classe criada, use-a no controller. Adicione um novo parâmetro no método gravar, em ProdutosController, chamado sumario, do tipo MultipartFile. Dentro do método, chame o método write, da classe FileSaver, salvando o arquivo na pasta arquivos-sumario. Lembre-se que para tudo funcionar, corretamente o Spring deve injetar um FileSaver:

@Controller
@RequestMapping("/produtos")
public class ProdutosController {

    @Autowired
    private FileSaver fileSaver;

    @RequestMapping(method=RequestMethod.POST)
    public ModelAndView gravar(MultipartFile sumario, @Valid Produto produto, 
            BindingResult result, RedirectAttributes redirectAttributes) {

        if(result.hasErrors()) {
            return form(produto);
        }

        String path = fileSaver.write("arquivos-sumario", sumario);
        produto.setSumarioPath(path);

        produtoDao.gravar(produto);

        redirectAttributes.addFlashAttribute("sucesso", "Produto cadastrado com sucesso!");

        return new ModelAndView("redirect:produtos");
    }

    // restante do código omitido
}
7) Crie a pasta arquivos-sumario dentro de src/main/webapp/.

8) Por fim, para o Spring encontrar o FileSaver, adicione-o na anotação @ComponentScan, na classe AppWebConfiguration:

@EnableWebMvc
@ComponentScan(basePackageClasses={HomeController.class, ProdutoDAO.class, 
    FileSaver.class})
public class AppWebConfiguration {

    // restante do código omitido

}

AULA 10

1) Se você ainda não baixou, faça agora o download do código fonte da página JSP aqui (https://s3.amazonaws.com/caelum-online-public/spring-mvc-1-criando-aplicacoes-web/springmvc-arquivos-extras-aula10.zip). Após baixar, extraia o ZIP. Você encontrará dois arquivos JSP e uma pasta com os recursos CSS:

O arquivo JSP limpo mas sem expression languages (detalhe-limpo.jsp).
O arquivo JSP finalizado com todas as alterações já aplicadas nessa aula (detalhe-pronto.jsp).
Uma pasta resources, com os arquivos CSS e imagem da Casa do Código para adicionar no projeto localmente.
2) Escolha qual JSP usar, renomeie-o para detalhe.jsp e copie-o para a pasta src/main/webapp/WEB-INF/views/produtos do seu projeto:

detalhe.jsp

3) Se você usou o arquivo detalhe-limpo.jsp, ainda é preciso incluir as expression languages nos lugares indicados. Lembre-se de alterar os locais onde deseja que as informações de título, id, descrição, número de páginas, sumário, data de lançamento, além do <c:forEach /> para listar os preços do livro:

<form action="/carrinho/add" method="post" class="container">
    <input type="hidden" value="${produto.id}" name="produtoId" >
    <ul id="variants" class="clearfix">
        <c:forEach items="${produto.precos}" var="preco">
            <li class="buy-option">
                <input type="radio" name="tipoPreco" class="variant-radio" 
                    id="tipoPreco" value="${preco.tipo}" checked="checked" />
                <label class="variant-label" >${preco.tipo}</label> 
                <small class="compare-at-price">R$ 39,90</small>
                <p class="variant-price">${preco.valor}</p>
            </li>
        </c:forEach>
    </ul>
    <button type="submit" class="submit-image icon-basket-alt" 
        title="Compre Agora ${produto.titulo}"></button>
</form>
4) Agora, copie toda a pasta resources para a pasta src/main/webapp do seu projeto:

resources

5) Por padrão, o Spring MVC nega o acesso à pasta resources. Consequentemente, o Tomcat não pode carregar os arquivos CSS (e a página fica sem design). Para liberar o acesso, é preciso fazer duas alterações na classe AppWebConfiguration:

A classe deve estender a classe WebMvcConfigurerAdapter:
@EnableWebMvc
@ComponentScan(basePackageClasses= {HomeController.class, ProdutoDAO.class, 
    FileSaver.class})
public class AppWebConfiguration extends WebMvcConfigurerAdapter {

    // restante do código omitido

}
A classe deve implementar o método configureDefaultServletHandling para liberar o acesso:
@Override
public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
    configurer.enable();
}
6) Ajuste o código Java começando pelo ProdutoDAO. Escreva um novo método, chamado find, que recebe o id do produto por parâmetro e retorna o produto com todas as informações preenchidas, inclusive os preços. O método deve executar uma query planejada através do EntityManager, que vai realizar o join entre as tabelas Produto e precos:

public Produto find(Integer id) {
    return manager.createQuery("select distinct(p) from Produto p " + 
        "join fetch p.precos precos where p.id = :id", Produto.class)
            .setParameter("id", id).getSingleResult();
}
7) Ajuste o controller, para carregar a JSP com os dados do produto. Primeiramente, crie o método detalhe, que recebe o id do produto por parâmetro, retorna para a JSP de detalhe e carrega o objeto ModelAndView com um produto:

@RequestMapping("/detalhe")
public ModelAndView detalhe(Integer id){

    ModelAndView modelAndView = new ModelAndView("produtos/detalhe");
    Produto produto = produtoDao.find(id);
    modelAndView.addObject("produto", produto);

    return modelAndView;
}
8) O Spring já traz um sistema de URLs amigáveis, que são URLs que parecem texto e não ficam poluídas com ? e &. Então edite o método detalhe, adicionando uma variável na rota e usando a anotação @PathVariable para mapear essa variável a um parâmetro:

@RequestMapping("/detalhe/{id}")
public ModelAndView detalhe(@PathVariable("id") Integer id){

    ModelAndView modelAndView = new ModelAndView("/produtos/detalhe");
    Produto produto = produtoDao.find(id);
    modelAndView.addObject("produto", produto);

    return modelAndView;
}
9) Por fim, adicione um link na listagem de produtos (lista.jsp) que irá te direcionar para a página de detalhe do produto. Para construir a URL, use o método mvcUrl. Além disso, invoque o método arg para adicionar um parâmetro à URL. Portanto, o link vai ficar com a seguinte estrutura:

<c:forEach items="${produtos}" var="produto">
    <tr>
        <td>
            <a href="${s:mvcUrl('PC#detalhe').arg(0,produto.id).build()}">
                ${produto.titulo}
            </a>
        </td>
        <td>${produto.descricao}</td>
        <td>${produto.paginas}</td>
    </tr>
</c:forEach>
Lembre-se que é preciso fazer o import da taglib para poder usar o mvcUrl:

<%@ taglib uri="http://www.springframework.org/tags" prefix="s"%>

AULA 11

1) Ao abrir a página detalhe.jsp, todas as informações são exibidas corretamente, com exceção da data. Isso acontece porque não foi definido como ela deve ser apresentada. Use a tag formatDate para corrigir esse problema. Ela só aceita o tipo Date, então utilize o atributo time da data de lançamento:

<p>
    Data de publicação: 
        <fmt:formatDate pattern="dd/MM/yyyy" 
            value="${produto.dataLancamento.time}"/>
</p>
Não esqueça também de adicionar a taglib:

<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
2) Comece a criar o carrinho de compras! Todo carrinho de compras possui vários itens, e cada item é composto, além do próprio produto, do tipo que está sendo comprando (Ebook, Impresso, Combo). Portanto, crie a classe CarrinhoItem no pacote br.com.casadocodigo.loja.models, com os atributos produto e tipoPreco, além dos seus getters e setters. E já que para um CarrinhoItem existir, é preciso tanto do produto quanto o tipoPreco, peça-os por parâmetro no construtor:

public class CarrinhoItem {

    private Produto produto;
    private TipoPreco tipoPreco;

    public CarrinhoItem(Produto produto, TipoPreco tipoPreco) {
        this.produto = produto;
        this.tipoPreco = tipoPreco;      
    }

    public Produto getProduto() {
        return produto;
    }

    public void setProduto(Produto produto) {
        this.produto = produto;
    }    

    public TipoPreco getTipoPreco() {
        return tipoPreco;
    }

    public void setTipoPreco(TipoPreco tipoPreco) {
        this.tipoPreco = tipoPreco;
    }    

}
3) Com a representação de um item do carrinho, crie o carrinho de fato. Para isso, crie a classe CarrinhoCompras no pacote br.com.casadocodigo.loja.models, com um atributo que possua um mapa que armazena o item e a quantidade no carrinho. Crie os métodos add, que adiciona o item no mapa, e getQuantidade, que retorna a quantidade de determinado item:

@Component
public class CarrinhoCompras {

    private Map<CarrinhoItem, Integer> itens = new LinkedHashMap<>();

    public void add(CarrinhoItem item) {
        itens.put(item, getQuantidade(item) + 1);
    }

    public Integer getQuantidade(CarrinhoItem item) {
        if(!itens.containsKey(item)) {
            itens.put(item, 0);
        }
        return itens.get(item);
    }

}
4) Para tudo funcionar corretamente, reescreva os métodos hashCode e equals, com os atributos produto e tipoPreco, da classe CarrinhoItem:

public class CarrinhoItem {

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + ((produto == null) ? 0 : produto.hashCode());
        result = prime * result + ((tipoPreco == null) ? 0 : tipoPreco.hashCode());
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        CarrinhoItem other = (CarrinhoItem) obj;
        if (produto == null) {
            if (other.produto != null)
                return false;
        } else if (!produto.equals(other.produto))
            return false;
        if (tipoPreco != other.tipoPreco)
            return false;
        return true;
    }

    // restante do código omitido
}
5) Faça o mesmo para a classe Produto, mas somente com o atributo id:

@Entity
public class Produto {

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + id;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        Produto other = (Produto) obj;
        if (id != other.id)
            return false;
        return true;
    }

    // restante do código omitido
}
6) Com as classes do carrinho prontas, é preciso acessá-lo. Crie o controller CarrinhoComprasController para o carrinho, no pacote br.com.casadocodigo.loja.controllers, e implemente os métodos add, que irá adicionar um item no carrinho, e crieItem, que irá criar um item para ser adicionado no carrinho. Não esqueça de utilizar as classes que foram criadas anteriormente. E para não se preocupar com a criação dos objetos, peça para o Spring injetá-los (@Autowired):

@Controller
@RequestMapping("/carrinho")
public class CarrinhoComprasController {

    @Autowired
    private ProdutoDao produtoDao;

    @Autowired
    private CarrinhoCompras carrinho;

    @RequestMapping("/add")
    public ModelAndView add(Integer produtoId, TipoPreco tipoPreco) {

        ModelAndView modelAndView = new ModelAndView("redirect:/produtos");
        CarrinhoItem carrinhoItem = criaItem(produtoId,tipoPreco);

        carrinho.add(carrinhoItem);

        return modelAndView;   
    }

    private CarrinhoItem criaItem(Integer produtoId, TipoPreco tipoPreco) {

        Produto produto = produtoDao.find(produtoId);
        CarrinhoItem carrinhoItem = new CarrinhoItem(produto, tipoPreco);

        return carrinhoItem;
    }
}
7) Para ver o carrinho funcionando, mostre a quantidade de itens de do lado do link Carrinho. Na classe CarrinhoCompras, crie o método getQuantidade, que conta os itens do carrinho:

public int getQuantidade() {
    return itens.values().stream()
        .reduce(0, (proximo, acumulador) -> proximo + acumulador);
}
E chame-o na JSP detalhe.jsp:

<ul class="clearfix">
    <li>
        <a href="#">
            Seu carrinho (${carrinhoCompras.quantidade})
        </a>
    </li>
    <li>
        <a href="/pages/sobre-a-casa-do-codigo" rel="nofollow">
            Sobre Nós
        </a>
    </li>
</ul>
8) Para enviar o carrinho de compras para a JSP, utilize o método setExposedContextBeanNames, da classe InternalResourceViewResolver, para deixar o carrinho disponível para todas as views. Além disso, para o Spring achar a classe CarrinhoCompras, adicione-a na anotação ComponentScan:

@EnableWebMvc
@ComponentScan(basePackageClasses={HomeController.class, ProdutoDao.class, 
    FileSaver.class, CarrinhoCompras.class})
public class AppWebConfiguration {

    @Bean
    public InternalResourceViewResolver internalResourceViewResolver() {

        InternalResourceViewResolver resolver = 
            new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");

        resolver.setExposedContextBeanNames("carrinhoCompras");

        return resolver;
    }

    // restante do código omitido
}

AULA 12

1) Para a nossa aplicação funcionar corretamente, através da anotação @Scope, altere o escopo da classe CarrinhoCompras para session:

@Component
@Scope(value=WebApplicationContext.SCOPE_SESSION)
public class CarrinhoCompras {

    // código da classe omitido

}
2) E altere o escopo da classe CarrinhoComprasController para request:

@Controller
@RequestMapping("/carrinho")
@Scope(value=WebApplicationContext.SCOPE_REQUEST)
public class CarrinhoComprasController {

    // código da classe omitido
}

AULA 13

1) Se você ainda não baixou, faça o download do código fonte da página JSP para renderizar os itens do carrinho aqui.

Após baixar, extraia o ZIP. Você encontrará dois arquivos JSP e uma imagem:

O arquivo JSP limpo mas sem expression languages (itens-limpo.jsp).
O arquivo JSP finalizado com todas as alterações já aplicadas (itens-pronto.jsp).
Uma imagem excluir.png para adicionar no projeto.
2) Escolha qual JSP usar, renomeie-a para itens.jsp e copie-a para a pasta src/main/webapp/WEB-INF/views/carrinho (que deve ser criada) do seu projeto:

itens.jsp

3) Se você usou o arquivo itens-limpo.jsp, ainda é preciso incluir as expression languages nos lugares indicados. Lembre-se de alterar os locais onde deseja que as informações de URL, título, preço, total, quantidade, além do <c:forEach /> para listar os itens do carrinho:

<tbody>
    <c:forEach items="${carrinhoCompras.itens}" var="item">
        <tr>
            <td class="cart-img-col">
                <img src="http://cdn.shopify.com/s/files/1/0155/7645/products/css-eficiente-featured_large.png?v=1435245145" 
                    width="71px" height="100px" />
            </td>
            <td class="item-title">${item.produto.titulo}</td>
            <td class="numeric-cell">${item.preco}</td>
            <td class="quantity-input-cell">
                <input type="number" min="0" id="quantidade" name="quantidade" 
                    value="${carrinhoCompras.getQuantidade(item)}" />
            </td>
            <td class="numeric-cell">${carrinhoCompras.getTotal(item)}</td>
            <td class="remove-item">
                <form action="" method="POST">
                    <input type="image" src="${contextPath}/resources/imagens/excluir.png" 
                        alt="Excluir" title="Excluir" />
                </form>    
            </td>
        </tr>
    </c:forEach>
</tbody>
4) Copie a imagem excluir.png para a pasta src/main/webapp/resources/imagens da aplicação.

resources

5) Crie o método precoPara na classe Produto para descobrir o preço de um produto de acordo com o tipo:

public BigDecimal precoPara(TipoPreco tipoPreco) {
    return precos.stream()
        .filter(preco -> preco.getTipo().equals(tipoPreco))
            .findFirst().get().getValor();
}
6) Na classe CarrinhoItem, crie os métodos getTotal, apenas realizando uma multiplicação, e getPreco, que chama o método precoPara da classe Produto:

public class CarrinhoItem {

    public BigDecimal getPreco() {
        return produto.precoPara(tipoPreco);
    }

    public BigDecimal getTotal(int quantidade) {
        return this.getPreco().multiply(new BigDecimal(quantidade));
    }

    // restante do código omitido
}
7) Na classe CarrinhoCompras, crie o método getItens, que irá retornar uma coleção de itens. Na mesma classe, crie também dois métodos getTotal, um que apenas repassa a chamada para o método de mesmo nome da classe CarrinhoItem e outro para saber o total que o usuário está pagando:

@Component
@Scope(value=WebApplicationContext.SCOPE_SESSION)
public class CarrinhoCompras {

    public Collection<CarrinhoItem> getItens() {
        return itens.keySet();
    }

    public BigDecimal getTotal(CarrinhoItem item) {
        return item.getTotal(getQuantidade(item));
    }

    public BigDecimal getTotal() {
        BigDecimal total = BigDecimal.ZERO;

        for (CarrinhoItem item : itens.keySet()) {
            total = total.add(getTotal(item));
        }

        return total ;
    }

    // restante do código omitido
}
8) No CarrinhoComprasController, crie o método itens:

@RequestMapping(method=RequestMethod.GET)
public ModelAndView itens(){
    return new ModelAndView("carrinho/itens");
}
9) Ainda em CarrinhoComprasController, altere o método add para que, em vez do usuário ser redirecionado para a página de produtos, ele seja redirecionado para o carrinho:

@RequestMapping("/add")
public ModelAndView add(Integer produtoId, TipoPreco tipo){

    ModelAndView modelAndView = new ModelAndView("redirect:/carrinho");
    CarrinhoItem carrinhoItem = criaItem(produtoId, tipo);

    carrinho.add(carrinhoItem);

    return modelAndView;
}
10) Por fim, todo componente do Spring que possua escopo de sessão precisa implementar a interface Serializable, então faça isso com a classe CarrinhoCompras:

@Component
@Scope(value=WebApplicationContext.SCOPE_SESSION)
public class CarrinhoCompras implements Serializable {

    private static final long serialVersionUID = 1L;

    // restante do código omitido
}

AULA 14

1) Crie a seção de pagamentos, começando pelo PagamentoController, em br.com.casadocodigo.loja.controllers, com o método finalizar, que deve imprimir o total do carrinho e adicionar uma mensagem de flash com o conteúdo Pagamento realizado com sucesso:

@RequestMapping("/pagamento")
@Controller
@Scope(value = WebApplicationContext.SCOPE_REQUEST)
public class PagamentoController {

    @Autowired    
    private CarrinhoCompras carrinho;

    @RequestMapping(value="/finalizar", method=RequestMethod.POST)
    public ModelAndView finalizar(RedirectAttributes model) {

        System.out.println(carrinho.getTotal());
        model.addFlashAttribute("sucesso", "Pagamento realizado com sucesso");

        return new ModelAndView("redirect:/produtos");
    }
}
2) Em itens.jsp, coloque o botão de finalização de compra dentro de um form e altere a sua action:

<tfoot>
    <tr>
        <td colspan="3">
            <form action="${s:mvcUrl('PC#finalizar').build()}" method="post">
                <input type="submit" class="checkout" name="checkout" 
                    value="Finalizar compra" />
            </form>
        </td>
        <td class="numeric-cell">${carrinhoCompras.total}</td>
        <td></td>
    </tr>
</tfoot>
3) Todo controller que precisar acessar o carrinho, você deve lembrar de mudar o seu escopo. Existe uma alternativa para resolver esse problema, você pode mudar o escopo do controller ou adicionar um novo atributo na classe CarrinhoCompras, que é o proxyMode, com o valor TARGET_CLASS:

@Component
@Scope(value=WebApplicationContext.SCOPE_SESSION, 
    proxyMode=ScopedProxyMode.TARGET_CLASS)
public class CarrinhoCompras implements Serializable {

    // código da classe omitido

}
4) Ainda falta conseguir remover os produtos do carrinho. Na classe CarrinhoCompras, crie o método remover, que recebe o id do produto e o seu tipo:

public void remover(Integer produtoId, TipoPreco tipoPreco) {
    Produto produto = new Produto();
    produto.setId(produtoId);
    itens.remove(new CarrinhoItem(produto, tipoPreco));
}
5) Crie o método da classe CarrinhoComprasController que irá invocar o método remover da classe Carrinho e anote-o com @RequestMapping:

@RequestMapping("/remover")
public ModelAndView remover(Integer produtoId, TipoPreco tipoPreco) {
    carrinho.remover(produtoId, tipoPreco);
    return new ModelAndView("redirect:/carrinho");
}
6) Não esqueça também de alterar a action do form da JSP item.jsp:

<td class="remove-item">
    <form action="${s:mvcUrl('CCC#remover').arg(0, item.produto.id).arg(1, item.tipoPreco).build()}" 
        method="POST">
        <input type="image" src="${contextPath}/resources/imagens/excluir.png" 
            alt="Excluir" title="Excluir" />
    </form>    
</td>
7) Agora, envie o pagamento para um serviço. Para isso, será necessário converter algumas classes para JSON, e para facilitar esse processo, use o Jackson. Adicione o seguinte XML dentro do seu arquivo pom.xml:

<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.5.1</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.5.1</version>
</dependency>
8) Com o Jackson adicionado ao projeto, consuma um serviço. Na classe PagamentoController, no método finalizar, envie os dados do pagamento para a URL http://book-payment.herokuapp.com/payment, que recebe um JSON com o formato abaixo:

{"value": 500}
Use a classe RestTemplate para acessar o serviço:

@RequestMapping("/pagamento")
@Controller
@Scope(value = WebApplicationContext.SCOPE_REQUEST)
public class PagamentoController {

    @Autowired
    private CarrinhoCompras carrinho;

    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value="/finalizar", method=RequestMethod.POST)
    public Callable<ModelAndView> finalizar(RedirectAttributes model){
        return () -> {
            String uri = "http://book-payment.herokuapp.com/payment";

            try {
                String response = restTemplate.postForObject(uri, 
                    new DadosPagamento(carrinho.getTotal()), String.class);
                model.addFlashAttribute("sucesso", response);
                System.out.println(response);
                return new ModelAndView("redirect:/produtos");
            } catch (HttpClientErrorException e) {
                e.printStackTrace();
                model.addFlashAttribute("falha", "Valor maior que o permitido");
                return new ModelAndView("redirect:/produtos");
            }
        };
    }
}
9) No pacote br.com.casadocodigo.loja.models, crie a classe DadosPagamento:

public class DadosPagamento {

    private BigDecimal value;

    public DadosPagamento(BigDecimal value) {
        this.value = value;
    }

    public DadosPagamento() {
    }

    public BigDecimal getValue() {
        return value;
    }
}
10) Na classe AppWebConfiguration, crie um novo método que retorne um RestTemplate:

@Bean
public RestTemplate restTemplate() {
    return new RestTemplate();
}
11) Na classe CarrinhoItem, implemente Serializable:

public class CarrinhoItem implements Serializable {

    private static final long serialVersionUID = 1L;

    // restante do código da classe omitido

}
12) Em lista.jsp, exiba a falha, se houver:

<body>
    <h1></h1>

    <div>${sucesso}</div>
    <div>${falha}</div>

<!-- restante do código omitido-->