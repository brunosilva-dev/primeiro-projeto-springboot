# SPRING

- `Criar projeto maven diretamento do vscode`

> - Acesse com CTRL + SHIFT + P</br></br>
> - Digite maven</br></br>
> - Clique em Spring Initializr e siga os passos que o sistema mostra. Caso tenha dúvida acesse a documentação no site

- `Executando uma aplicação`

> - Digite o comando `mvn spring-boot:run` isso fará com que o projeto seja executado no vscode</br></br>
> - Existe outra forma para executar a aplicação onde é pedindo ao maven para que gere o `.jar` e executar através do comando `mvn clean install`.</br></br>
> - Para executar o arquivo `.jar` digite o comando `java -jar target/nome-arquivo.jar`</br></br>
> - Uma outra forma de se executar é clicando em `JAVA PROJECTS` e na frente do nome do projeto clicar no simbolo de debug</br></br>
> - E a última forma é executar clicando em `RUN` dentro do método main da classe principal que é o mais indicado

- `API REST`

> - Recurso(URL)</br></br>
> - Tipo de Operação HTTP METHODS (GET,POST,PUT,DELETE)
>   > - GET - Buscar uma informação
>   > - POST - Inerir uma informação
>   > - PUT - Atualizar uma informação
>   > - DELETE - Remover uma informação
> - Estado Representacional XML ou JSON {}</br></br>
> - Comunicação Cliente/Servidor </br></br>
> - Tipos de parametros na requisição
>   > - Body
>   > - Path Params (Faz parte da requisição e são obrigatórios)
>   > - Query params - São parametros de consultas onde serão adiocionados pelo caracter e podem ser opcionais
>   > - Header params - Parametros do cabeçalho da requisição
>   >   > - Authorization
>   >   > - Pages

- `ROTA/CONTROLLER`

> - Quando criar um controller, sempre colocar as annotation `@RequestMapping("/caminhoDaRota")` e `@RestController()` onde informa que essa primeira classe é uma controller que vai receber as requisições. A rest controller é inserida para definir que a classe será uma controller.
> - Após definir o tipo de recurso, será preciso definir o que o método irá fazer, no exemplo estamos utilizando `GetMapping("/primeiroMetodo")`.

```java
package br.com.neomeca.primeiroprojetospringboot;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo")
  public String primeiroMetodo() {
    return "Primeiro método API REST";
  }

}
```

- `Component Scan`

> - Na classe principal(responsável por subir a aplicação), inserir outra annotation que é `@ComponentScan(basePackages = "br.com.nomeDoPacote")`, isso vai fazer com que o compilador interprete tudo o que estiver até o caminho do component informado

```java
package br.com.neomeca.primeiroprojetospringboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = "br.com.nomeDoPacote")
public class PrimeiroProjetoSpringbootApplication {

	public static void main(String[] args) {
		SpringApplication.run(PrimeiroProjetoSpringbootApplication.class, args);
	}

}
```

> - Mas também existe outra forma que possa gerenciar a package colocando `@ComponentScan(basePackageClasses = "Controller.class")`

```java
package br.com.neomeca.primeiroprojetospringboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackageClasses = "Controller.class")
public class PrimeiroProjetoSpringbootApplication {

	public static void main(String[] args) {
		SpringApplication.run(PrimeiroProjetoSpringbootApplication.class, args);
	}

}
```

- `Path Params`

> - A utilização do Path Params deve-se a necessidade de quando for buscar algo especifico dentro do método e para isso no método criado deve-se incluir o parametro dentro de {} ficando assim `@GetMapping("/primeiroMetodo/{id}")` e para passar o parametro dentro do método existe duas formas, onde a primeira fica assim `public String primeiroMetodo(@PathVariable String id)` e a segunda caso queira passar o nome do id `public String primeiroMetodo(@PathVariable(name="id") String id)`

```java
package br.com.neomeca.primeiroprojetospringboot.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo/{id}")
  public String primeiroMetodo(@PathVariable String id) {
    return "Primeiro método API REST/Primeira rota criada e o id é " + id;
  }

}

```

> - Outro tipo de parametros que pode receber também é o da própria requisição, chamados de `Query Params`

```java
package br.com.neomeca.primeiroprojetospringboot.controller;

import java.util.Map;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo/{id}")
  public String primeiroMetodo(@PathVariable String id) {
    return "Primeiro método API REST/Primeira rota criada e o id é " + id;
  }

  @GetMapping("/metodoComQueryParams")
  public String metodoComQueryParams(@RequestParam String id) {
    return "O Parametro com metodoComQueryParams é " + id;
  }

  @GetMapping("/metodoComQueryParams2")
  public String metodoComQueryParams2(@RequestParam Map<String, String> allParams) {
    return "O Parametro com metodoComQueryParams é " + allParams.entrySet();
  }
}
```

- `Body Params`

```java
package br.com.neomeca.primeiroprojetospringboot.controller;

import java.util.Map;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo/{id}")
  public String primeiroMetodo(@PathVariable String id) {
    return "Primeiro método API REST/Primeira rota criada e o id é " + id;
  }

  @GetMapping("/metodoComQueryParams")
  public String metodoComQueryParams(@RequestParam String id) {
    return "O Parametro com metodoComQueryParams é " + id;
  }

  @GetMapping("/metodoComQueryParams2")
  public String metodoComQueryParams2(@RequestParam Map<String, String> allParams) {
    return "O Parametro com metodoComQueryParams é " + allParams.entrySet();
  }

  @PostMapping("/metodoComBodyParams")
  public String metodoComBodyParams(@RequestBody String username) {
    return "metodoComBodyParams " + username;
  }
}

```

- `Record` permite criar uma classe sem ter a necessidade de getters and setters

```java
package br.com.neomeca.primeiroprojetospringboot.controller;

import java.util.Map;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo/{id}")
  public String primeiroMetodo(@PathVariable String id) {
    return "Primeiro método API REST/Primeira rota criada e o id é " + id;
  }

  @GetMapping("/metodoComQueryParams")
  public String metodoComQueryParams(@RequestParam String id) {
    return "O Parametro com metodoComQueryParams é " + id;
  }

  @GetMapping("/metodoComQueryParams2")
  public String metodoComQueryParams2(@RequestParam Map<String, String> allParams) {
    return "O Parametro com metodoComQueryParams é " + allParams.entrySet();
  }

  @PostMapping("/metodoComBodyParams")
  public String metodoComBodyParams(@RequestBody Usuario usuario) {
    return "metodoComBodyParams " + usuario.username();
  }

  record Usuario(String username) {
  }
}

```

- `Header Params`

> - São os parametros que vem no header da requisição

```java
package br.com.neomeca.primeiroprojetospringboot.controller;

import java.util.Map;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo/{id}")
  public String primeiroMetodo(@PathVariable String id) {
    return "Primeiro método API REST/Primeira rota criada e o id é " + id;
  }

  @GetMapping("/metodoComQueryParams")
  public String metodoComQueryParams(@RequestParam String id) {
    return "O Parametro com metodoComQueryParams é " + id;
  }

  @GetMapping("/metodoComQueryParams2")
  public String metodoComQueryParams2(@RequestParam Map<String, String> allParams) {
    return "O Parametro com metodoComQueryParams é " + allParams.entrySet();
  }

  @PostMapping("/metodoComBodyParams")
  public String metodoComBodyParams(@RequestBody Usuario usuario) {
    return "metodoComBodyParams " + usuario.username();
  }

  @PostMapping("/metodoComHeaders")
  public String metodoComHeaders(@RequestHeader("name") String name) {
    return "metodoComHeaders " + name;
  }

  @PostMapping("/metodoComListHeaders")
  public String metodoComListHeaders(@RequestHeader Map<String, String> headers) {
    return "metodoComListHeaders " + headers.entrySet();
  }

  record Usuario(String username) {
  }
}

```

- `Response Entity`

```java
package br.com.neomeca.primeiroprojetospringboot.controller;

import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController()
@RequestMapping("/controller")
public class Controller {

  @GetMapping("/primeiroMetodo/{id}")
  public String primeiroMetodo(@PathVariable String id) {
    return "Primeiro método API REST/Primeira rota criada e o id é " + id;
  }

  @GetMapping("/metodoComQueryParams")
  public String metodoComQueryParams(@RequestParam String id) {
    return "O Parametro com metodoComQueryParams é " + id;
  }

  @GetMapping("/metodoComQueryParams2")
  public String metodoComQueryParams2(@RequestParam Map<String, String> allParams) {
    return "O Parametro com metodoComQueryParams é " + allParams.entrySet();
  }

  @PostMapping("/metodoComBodyParams")
  public String metodoComBodyParams(@RequestBody Usuario usuario) {
    return "metodoComBodyParams " + usuario.username();
  }

  @PostMapping("/metodoComHeaders")
  public String metodoComHeaders(@RequestHeader("name") String name) {
    return "metodoComHeaders " + name;
  }

  @PostMapping("/metodoComListHeaders")
  public String metodoComListHeaders(@RequestHeader Map<String, String> headers) {
    return "metodoComListHeaders " + headers.entrySet();
  }

  @GetMapping("/metodoResponseEntity/{id}")
  public ResponseEntity<Object> metodoResponseEntity(@PathVariable Long id) {
    var usuario = new Usuario("brunosilva");
    if (id > 5) {
      return ResponseEntity.status(HttpStatus.OK).body(usuario);
    }
    return ResponseEntity.badRequest().body("Número menor que 5");
  }

  record Usuario(String username) {
  }
}

```

- `IOC Di`

> - Gerencia os componentes dentro da própria aplicação. Quando for utilizar a injeção de components, deve-se usar o `Autowired` pelo é o gerenciamento do próprio spring

```java
package br.com.neomeca.primeiroprojetospringboot.ioc_di;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/component")
public class MeuControllerComponent {

  @Autowired
  MeuComponent meuComponent;

  @GetMapping("/")
  public String chamandoComponent() {

    var resultado = meuComponent.chamarMeuComponent();
    return resultado;
  }
}

```
