## TDD com Spring Boot

 TDD com Spring Boot e CRUD básico orientado a testes.
O que vamos precisar para testar nossa aplicação web?
No caso do spring boot vamos adicionar somente uma dependência.

Agora vamos criar um application.properties para testes.
Colocar este arquivo no diretório /src/test/resources

Eu gosto muito da ideia de criar um banco de testes, mas você também pode usar banco em memória.
Agora vamos criar a classe que vai levantar o container do spring boot para executar os testes de integração.

Lembrando de passar o nome do seu arquivo de configuração que salvamos em src/test/resources, se você não especificar ele vai usar o padrão que fica em src/main/resources.
Feito isso agora estamos prontos para usar testes de integração em nossa aplicação.
Vamos fazer um CRUD simples de Salário Mínimo.
Atributos do objeto:
Integer id;
String estado;
String salario;
Date dataInicio;
Date dataFinal;
Vamos começar criando uma classe chamada SalarioMinimoControllerTest em src/test/java.

Vamos lá entender nossa classe.
linha 6 : primeira coisa vamos herdar nossa classe de configuração que criamos anteriormente para levantar o container do spring.
linha 9: vamos usar o mockmvc com spring para testar nossa aplicação.
linha 14: o método setUp vai ser executado antes de qualquer método que vamos ter nesta classe de testes, exemplo: se tivermos dois testes antes ele vai passar pelo método setUp.
linha 16: aqui ele injeta o controller para que funcione como se tivesse executando sua aplicação normalmente.
Pronto já entendemos uma estrutura básica para se testar um controller.
Mas temos que seguir a risca o ciclo do TDD, ai eu te pergunto em algum momento criamos um classe SalarioMinimoController? Não! se rodarmos esse teste ele vai quebrar, ai entra o refatore, e rodamos novamente.
Vamos criar a classe SalarioMinimoController em src/main/java.

Agora vamos rodar o teste

Beleza! passou.
O padrão de requisição que vamos implementar é o seguinte
GET localhost:8080/salarios
GET localhost:8080/salarios/novo
POST localhost:8080/salarios
PUT localhost:8080/salarios/{salario_id}/editar
DELETE localhost:8080/salarios/{salario_id}/excluir
Vamos começar na sequência das URL’s
GET localhost:8080/salarios
Essa é aquela típica tela que tem um link para uma página de formulário e uma tabela listando os salários com links pra edição e exclusão.
Então vamos lá ! Começando pelo Teste.

criamos o método GETIndexSalarioMinimoController
linha 25: este teste ele vai solicitar a URL e vai esperar um retorno de sucesso.

Pergunta mapeamos a URL /salarios em nosso controller? Não! então mais uma vez refatore e execute o teste novamente.
vamos criar esse mapeamento em nosso controller.

Observação: coloquei a anotação ResponseBody por que eu teria que criar um view e não to afim :P
Agora rodemos o teste novamente :D

Passou! já deu para entender o ciclo de desenvolvimento usando TDD né?
cria o test, executa e falha, refatora executa novamente e sucesso!
Nos próximos passos vou pular esse ciclo para o artigo não ficar muito longo.
GET localhost:8080/salarios/novo
Essa é aquela tela que carregamos o formulário para inserção do salário minimo.
Criamos o teste testGETSaveSalarioMinimoController()

Controller

POST localhost:8080/salarios
Teste

Aqui entra o que eu comentei anteriormente que o foco do artigo é para aplicações monolíticas para conseguir fazer o bind que preencha o modelo do teste para o controller você tem que passar param por param do modelo como nas linhas 5 a 8.
No caso de testar uma API REST basta você criar um JSON completo do seu objeto e em vez de usar o método param usar o .content(JSON).
Controller
Nosso controller ficará assim

Entidade para o POST

PUT localhost:8080/salarios/{salario_id}/editar
Edição do salário mínimo.
Test

Lembrando que estamos sempre salvando no banco teste para não ficar lixo no banco eu costumo configurar um tearDown que é similar ao setUP em vez de rodar antes de cada método ele roda depois de cada método, onde eu vou excluir esses registros que eu salvei para testes.
tearDown()

Controller PUT

Para não prolongar muito não vou implementar o método delete fica ai uma tarefa para vocês, é praticamente igual a este ultimo teste do PUT só vai mudar a URL do controller.
##