# Projeto MVC em PHP

Este projeto implementa o padrão de arquitetura **MVC (Model-View-Controller)** utilizando **PHP puro**, sem dependências externas. O objetivo é fornecer uma estrutura simples e flexível para construir aplicações web baseadas nesse padrão.

## Estrutura de Diretórios

A estrutura de diretórios é organizada da seguinte forma:

```
- app/
    |
    Models/
    |
    Views/
    |
    Controllers/
    |
    public/
        |
        index.php => core mvc
        |
        src/
    |
    autoload.php => carrega classes automaticamente quando necessárias
    |
    routes.php => arquivo de configuração de rotas
```

## Descrição

- **Models (M)**: Responsável pela lógica de dados. (No momento, o projeto não possui um diretório dedicado a modelos, pois é uma estrutura simples.)
- **Views (V)**: Responsável pela apresentação dos dados.
- **Controllers (C)**: Gerencia a interação entre o modelo e a visão. Um controlador recebe as requisições e define qual a resposta a ser exibida.
- **public/index.php**: Core mvc.
- **public/src/**: Pasta para os assets frontend. ```<img src="src/logo.jpg">```

![scheme](https://i.imgur.com/JJJhIkS.png)

## Exemplo de `Rota`

No arquivo `routes.php`, é definida uma rota simples:

```
['/' => 'HomeController@index?GET'],
```
#### Como Funciona?
- **/** > URL da rota
- **HomeController** > Este e o controlador associada à rota.
- **index** > O método chamado no controlador HomeController.
- **GET** > o método HTTP que a rota irá responder.

### Explicação do Fluxo
- **Requisição do Usuário**: O usuário faz uma requisição GET para a URL /.
- **Roteamento**: O core verifica a rota em `routes.php` e encontra que a requisição `GET` para `/` deve ser tratada pela ação `index` do controlador `HomeController`.
- **Controller**: O método `index()` do controlador é chamado, onde pode ser manipulado um `Model` e retornar uma view para o usuário.
- **View**: O controlador retorna a view associada (por exemplo, `Views/index.php`).

## Estrutura do Controller

O **Controller** é responsável por processar as requisições para a página inicial e renderizar a view correspondente.

O controlador `HomeController.php` está localizado no diretório `/Controllers`, e o código é o seguinte:

```php
namespace Controllers;

use Models\UserModel;

class HomeController extends BaseController
{
    function index()
    {
        $db = new UserModel;
        $users = $db->getAll();
        return $this->view('index', ['users' => $users]);
    }
}
```

É possivel passar variáveis para essa view da seguinte forma:

```php
return $this->view('index', ['var1' => '1234']);
```

Estará disponivel da view como:
```html
<?php echo $var1 ?> // 1234

```

## Estrutura do Modelo

O modelo `UserModel.php` está localizado no diretório `/Models`, e o código é o seguinte:

```php
namespace Models;

class UserModel extends BaseModel
{
    function getAll()
    {
         $result = $this->query('SELECT * FROM user');
         $data = [];
         while($row = $result->fetchArray(SQLITE3_ASSOC)){
             $data[] = $row;
        }
        return $data;
    }
}
```

## Conclusão

Este projeto implementa uma estrutura básica do padrão **MVC (Model-View-Controller)** utilizando **PHP puro**. Ele fornece uma base sólida e simples para o desenvolvimento de aplicações web, permitindo que você organize a lógica da aplicação, a manipulação de dados e a exibição de forma clara e modular.

### Pontos Importantes:
- **Modelo (Model)**: A classe `UserModel` é responsável por interagir com o banco de dados SQLite, oferecendo uma maneira simples de acessar os dados. Ela pode ser expandida para incluir mais métodos de manipulação de dados (como inserção, atualização e exclusão).
- **Controlador (Controller)**: O controlador, como o `HomeController`, gerencia as requisições do usuário, interage com os modelos e passa os dados necessários para as views.
- **Visão (View)**: A visão, por meio da classe `RenderView`, exibe os dados de forma formatada para o usuário.

Embora este seja um projeto simples e básico, ele pode ser facilmente expandido com novas funcionalidades, como:
- Melhorias no core do projeto.
- Adição de mais modelos para outras tabelas e entidades.
- Implementação de autenticação de usuários e controle de sessões.
- Expansão das funcionalidades de consulta no banco de dados.
- Uso de outros métodos HTTP (como POST, PUT, DELETE) para operações de CRUD.
- Melhoria na estrutura das views e templates dinâmicos.

### Iniciar com servidor web embutido PHP

```bash
php -S localhost:8000 -t public 
```

http://localhost:8000


### Iniciar com docker-compose
Crie um arquivo `docker-compose.yml` na raiz do projeto

cole o codigo abaixo:
```docker-compose
services:
  app:
    image: php:8.4-cli
    volumes:
      - ./:/app
    working_dir: /app
    ports:
      - "8000:8000"
    command: ["php", "-S", "0.0.0.0:8000", "-t", "public"]
```

**Inicie o servidor**

```bash
docker compose up
```

http://localhost:8000
