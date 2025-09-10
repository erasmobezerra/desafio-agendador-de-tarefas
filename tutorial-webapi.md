## 1\. Conceito de WEB API e Principais Usos

### O que é uma WEB API?

Uma **WEB API** (Application Programming Interface) é uma interface que permite que diferentes sistemas de software se comuniquem entre si através da web, utilizando o protocolo HTTP/HTTPS.

Pense nela como um "garçom" em um restaurante: o cliente (uma aplicação mobile, um site, outro serviço) faz um pedido (requisição HTTP), o garçom (a API) anota o pedido e o leva para a cozinha (o servidor/banco de dados), e depois retorna com o prato pronto (a resposta, geralmente em formato JSON ou XML).

No ecossistema .NET, as WEB APIs são criadas utilizando o framework **ASP.NET Core**, que é moderno, de alta performance, multiplataforma (roda em Windows, macOS e Linux) e open-source. Com o .NET 8.0, a criação de APIs se tornou ainda mais simplificada e poderosa.

### Principais Características de uma WEB API .NET

* **Padrão RESTful:** As Web APIs modernas seguem os princípios do **RESTful**, o que facilita a integração com diversos tipos de clientes — como aplicações web, mobile e dispositivos IoT — além de promover uma arquitetura **limpa, escalável e desacoplada**. O estilo REST utiliza os verbos HTTP de forma semântica e padronizada para representar operações sobre recursos:

  * `GET`: Recupera dados, como uma lista de produtos ou os detalhes de um cliente.
  * `POST`: Cria um novo recurso, por exemplo, ao cadastrar um novo cliente.
  * `PUT`: Atualiza completamente um recurso existente, como modificar todos os dados de um produto.
  * `DELETE`: Remove um recurso, como excluir um cliente do sistema.

* **Alta segurança:** Utilizam autenticação e autorização robustas com JWT, OAuth2, entre outros.

* **Focada em Dados:** O formato de dados mais comum para troca de informações é o **JSON** (JavaScript Object Notation), por ser leve, legível e de fácil interpretação por diversas linguagens de programação.

* **Modularidade e Baixo Acoplamento:** O ASP.NET Core permite construir APIs com componentes altamente modulares. Isso significa que você pode incluir apenas os pacotes e funcionalidades que realmente precisa, o que melhora a performance, segurança e manutenção do sistema.

* **Hospedagem em Nuvem:** São otimizadas para ambientes de nuvem, como Azure, AWS ou Google Cloud, com alta escalabilidade e baixo consumo de recursos.

### Principais Usos

1. **Aplicações SPA (Single Page Applications):** Frameworks de frontend como Angular, React e Vue.js consomem dados de uma WEB API para renderizar as telas e interagir com o usuário sem a necessidade de recarregar a página inteira.
2. **Aplicações Mobile (iOS/Android):** Aplicativos nativos precisam de um backend para buscar e salvar dados, autenticar usuários e executar regras de negócio. A WEB API é a ponte perfeita para essa comunicação.
3. **Microsserviços:** Em arquiteturas de microsserviços, a aplicação é dividida em serviços menores e independentes que se comunicam entre si através de APIs.
4. **Integração entre Sistemas (B2B):** Uma empresa pode expor uma API para que seus parceiros comerciais possam consumir seus serviços ou dados de forma automatizada.
5. **Aplicações IoT (Internet of Things):** Dispositivos inteligentes (sensores, câmeras, etc.) enviam e recebem dados de uma plataforma central através de APIs.

-----

### 2\. Estrutura e Arquitetura de uma WEB API

Uma WEB API em .NET 8.0, especialmente quando conectada a um banco de dados, possui uma arquitetura bem definida para organizar o código e separar as responsabilidades. Vamos aos componentes principais:

#### a) Models (Modelos)

Os **Models** são classes C\# que representam as entidades de domínio do seu projeto. Eles definem a estrutura dos dados com os quais sua aplicação irá trabalhar. Por exemplo, se você está criando uma API para um sistema de gerenciamento de tarefas, você teria um `Model` chamado `Tarefa`.

* **Função:** Definir as propriedades e o formato dos seus dados.
* **Exemplo:** Uma classe `Produto` com propriedades como `Id`, `Nome`, `Preco` e `Estoque`.

O Entity Framework Core (ORM que usaremos) utiliza esses modelos para mapear as tabelas e colunas do banco de dados.

#### b) Controller (Controlador)

O **Controller** é o cérebro da sua API. É uma classe que recebe as requisições HTTP, processa a lógica necessária e retorna uma resposta. Cada método público em um Controller é chamado de **Action** e corresponde a um *endpoint* (uma URL) que pode ser acessado.

* **Função:** Orquestrar o fluxo da requisição. Ele recebe a chamada, utiliza o *Context* para interagir com o banco de dados, aplica regras de negócio e devolve uma resposta HTTP (como `200 OK`, `201 Created`, `404 Not Found`, etc.).
* **Exemplo:** Um `ProdutosController` com Actions como `GetProdutos()`, `GetProdutoPorId(int id)`, `CreateProduto(Produto produto)`.

#### c) Context (Contexto do Banco de Dados)

A classe de **Contexto**, geralmente herdada de `DbContext` do Entity Framework Core, é a ponte entre seus *Models* e o banco de dados. Ela representa uma sessão com o banco de dados e permite que você consulte e salve dados.

* **Função:** Configurar a conexão com o banco de dados e mapear quais *Models* devem se tornar tabelas. É através do objeto de contexto que você executa operações de Leitura, Escrita, Atualização e Deleção (CRUD).
* **Exemplo:** Uma classe `ApplicationDbContext` que contém uma propriedade `public DbSet<Produto> Produtos { get; set; }`. Esta linha informa ao Entity Framework que existe uma tabela `Produtos` no banco de dados correspondente ao modelo `Produto`.

#### d) Migration (Migração)

As **Migrations** são um recurso do Entity Framework Core para gerenciar a evolução do esquema do seu banco de dados ao longo do tempo. Quando você cria ou altera um *Model*, em vez de alterar o banco de dados manualmente, você cria uma *Migration*.

* **Função:** Gerar código C\# que descreve as alterações a serem feitas no banco de dados (criar tabela, adicionar coluna, remover índice, etc.). Esse código pode ser aplicado para atualizar o banco de dados de forma segura e versionada. Isso garante que a estrutura do banco de dados esteja sempre sincronizada com os seus *Models*.
* **Processo:** Você executa um comando (`add-migration`) para gerar o arquivo de migração e outro (`update-database`) para aplicar as alterações ao banco de dados.

#### e) appsettings.json - O "Painel de Controle" da sua aplicação

Pense no `appsettings.json` como o **"painel de controle"** da sua aplicação em em tempo de execução. Ele contém configurações que a aplicação utiliza para funcionar, como strings de conexão com bancos de dados, chaves de APIs externas, configurações de logging, parâmetros personalizados da aplicação entre outros.

A grande vantagem é que você pode alterar essas configurações sem precisar recompilar o código. Além disso, é comum ter arquivos específicos para diferentes ambientes, como `appsettings.Development.json` e `appsettings.Production.json` Esses arquivos complementam ou sobrescrevem o appsettings.json principal, dependendo do ambiente em que a aplicação está sendo executada — o que facilita a gestão de configurações entre desenvolvimento, testes e produção.

#### f) `launchSettings.json` - A Configuração do seu Ambiente de Desenvolvimento

O arquivo `launchSettings.json` define como o projeto deve ser iniciado durante o desenvolvimento, seja pelo Visual Studio ou via comando `doner run`. Ele especifica detalhes como perfins de esexução, URLs utilizadas pela aplicação local, e variáveis de ambiente específicas para testes.

`Importante`: esse arquivo não é incluído no deploy da aplicação — ele serve exclusivamente para o ambiente de desenvolvimento local. Você o encontrará na pasta Properties do projeto.

-----

### 3\. Tutorial: Criando uma API de Gerenciamento de Produtos com .NET 8.0

Vamos criar uma API simples para gerenciar uma lista de produtos.

**Pré-requisitos:**

* [.NET 8.0 SDK](https://www.google.com/search?q=https.dotnet.microsoft.com/download/dotnet/8.0) instalado.
* Um editor de código como o [Visual Studio 2022](https://visualstudio.microsoft.com/vs/) ou [VS Code](https://code.visualstudio.com/).
* SQL Server (pode ser a versão Express ou Developer, que são gratuitas).

#### Passo 1: Criando o Projeto

Abra o terminal ou o prompt de comando e execute os seguintes comandos:

```bash
dotnet new webapi -n ApiProdutos
cd ApiProdutos
```

Isso cria um novo projeto de WEB API chamado `ApiProdutos` e navega para dentro da pasta.

#### Passo 2: Instalando as Dependências (Pacotes NuGet)

Vamos instalar os pacotes necessários para o projeto: 

```bash
# Adiciona suporte ao EF Core no projeto
dotnet add package Microsoft.EntityFrameworkCore.SqlServer

# Permitem que ferramentas como dotnet ef funcionem corretamente durante o desenvolvimento.
dotnet add package Microsoft.EntityFrameworkCore.Design

# Permite via CLI (linha de comando) aplicar migrations e interagir com o Entity Framework 
dotnet tool install --global dotnet-ef

# Para a documentação da API (Swagger/OpenAPI)
dotnet add package Swashbuckle.AspNetCore
```

#### Passo 3: Criando o Model

Crie uma pasta chamada `Models` na raiz do projeto. Dentro dela, crie um arquivo `Produto.cs`:

**Models/Produto.cs**

```csharp
namespace ApiProdutos.Models
{
    public class Produto
    {
        public int Id { get; set; }
        public string Nome { get; set; }
        public decimal Preco { get; set; }
        public int Estoque { get; set; }
    }
}
```

#### Passo 4: Criando o Contexto do Banco de Dados

Crie uma pasta chamada `Context` na raiz do projeto. Dentro dela, crie um arquivo `AppDbContext.cs`:

**Context/AppDbContext.cs**

```csharp
using ApiProdutos.Models;
using Microsoft.EntityFrameworkCore;

namespace ApiProdutos.Context
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
        {
        }

        public DbSet<Produto> Produtos { get; set; }
    }
}
```

#### Passo 5: Configurando a Conexão com o Banco de Dados

Abra o arquivo `appsettings.Development.json` e adicione a sua *connection string* do SQL Server.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=SEU_SERVIDOR;Database=DbApiProdutos;Trusted_Connection=True;TrustServerCertificate=True;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

> **Atenção:** Substitua `SEU_SERVIDOR` pelo nome da sua instância do SQL Server (ex: `localhost\\SQLEXPRESS` ou `.` para o servidor local padrão). O banco `DbApiProdutos` será criado automaticamente.

Agora, vamos registrar o `AppDbContext` e o serviço de banco de dados no arquivo `Program.cs`.

**Program.cs**

```csharp
using ApiProdutos.Context;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// 1. Adicionar a string de conexão e o DbContext
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(connectionString));


// Add services to the container.
builder.Services.AddControllers();
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

#### Passo 6: Criando e Aplicando a Migration

Com o `Model` e o `DbContext` configurados, podemos criar a primeira migration para gerar a tabela `Produtos` no banco de dados.

No terminal, na raiz do projeto, execute os seguintes comandos:

```bash
# 1. Cria a migration (isso gerará uma pasta 'Migrations' com o código)
dotnet ef migrations add CriacaoInicial

# 2. Aplica a migration ao banco de dados (isso cria o banco e a tabela)
dotnet ef database update
```

Após executar esses comandos, você pode verificar no seu SQL Server que o banco `DbApiProdutos` e a tabela `Produtos` foram criados.


### A Configuração do seu Ambiente de Desenvolvimento

O arquivo `launchSettings.json` como o **"controle remoto" do seu ambiente de desenvolvimento**. Este arquivo diz ao Visual Studio (ou ao comando `dotnet run`) como iniciar o seu projeto. **Importante: este arquivo não é publicado (deploy) com sua aplicação; ele só tem efeito na sua máquina local.**

Ele fica localizado na pasta `Properties` do seu projeto.

#### Estrutura do `launchSettings.json` para o `ApiProdutos` (exemplo típico)

```json
{
  "$schema": "https://json.schemastore.org/launchsettings.json",
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3076",
      "sslPort": 44343
    }
  },
  "profiles": {
    "TrilhaApiDesafio": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:7295;http://localhost:5181",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "launchUrl": "swagger",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}

```
Esse arquivo configura como sua API será iniciada localmente, com dois perfis:
- Um para rodar direto com `dotnet run` (`TrilhaApiDesafio`)
- Outro para rodar com IIS Express

Ambos usam o ambiente de desenvolvimento e abrem o Swagger automaticamente para facilitar os testes.



#### Passo 7: Criando o Controller

Finalmente, vamos criar o Controller que irá expor os endpoints para manipular os produtos. Na pasta `Controllers`, crie um novo arquivo `ProdutosController.cs`.

**Controllers/ProdutosController.cs**

```csharp
using ApiProdutos.Context;
using ApiProdutos.Models;
using Microsoft.AspNetCore.Mvc;
using System.Linq;

namespace ApiProdutos.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class ProdutosController : ControllerBase
    {
        private readonly AppDbContext _context;

        public ProdutosController(AppDbContext context)
        {
            _context = context;
        }

        [HttpGet("{id}")]
        public IActionResult ObterPorId(int id)
        {
            var produto = _context.Produtos.Find(id);

            if (produto == null)
                return NotFound($"Produto com ID {id} não encontrado.");

            return Ok(produto);
        }

        [HttpGet("ObterTodos")]
        public IActionResult ObterTodos()
        {
            var produtos = _context.Produtos.ToList();
            return Ok(produtos);
        }

        [HttpGet("ObterPorNome")]
        public IActionResult ObterPorNome(string nome)
        {
            var produtos = _context.Produtos
                .Where(p => p.Nome.Contains(nome))
                .ToList();

            return Ok(produtos);
        }

        [HttpPost]
        public IActionResult Criar(Produto produto)
        {
            if (string.IsNullOrWhiteSpace(produto.Nome))
                return BadRequest(new { Erro = "O nome do produto não pode ser vazio." });

            _context.Produtos.Add(produto);
            _context.SaveChanges();

            return CreatedAtAction(nameof(ObterPorId), new { id = produto.Id }, produto);
        }

        [HttpPut("{id}")]
        public IActionResult Atualizar(int id, Produto produto)
        {
            var produtoBanco = _context.Produtos.Find(id);

            if (produtoBanco == null)
                return NotFound();

            if (string.IsNullOrWhiteSpace(produto.Nome))
                return BadRequest(new { Erro = "O nome do produto não pode ser vazio." });

            produtoBanco.Nome = produto.Nome;
            produtoBanco.Estoque = produto.Estoque;
            produtoBanco.Preco = produto.Preco;

            _context.Produtos.Update(produtoBanco);
            _context.SaveChanges();

            return Ok(produtoBanco);
        }

        [HttpDelete("{id}")]
        public IActionResult Deletar(int id)
        {
            var produtoBanco = _context.Produtos.Find(id);

            if (produtoBanco == null)
                return NotFound();

            _context.Produtos.Remove(produtoBanco);
            _context.SaveChanges();

            return NoContent();
        }
    }
}
```

#### Passo 8: Testando a API

Execute o projeto com o comando:

```bash
dotnet watch run
```

Conforme configurado no arquivo de configuração de ambiente launchSettings.json, ao iniciar a execução do projeto, a página do swagger será aberta automaticamente no seu navegador de internet padrão no endereço: https://localhost:7295/swagger 

Você verá a interface do Swagger, que documenta todos os endpoints que criamos. Você pode expandir cada um deles, preencher os parâmetros e clicar em "Try it out" -\> "Execute" para testar sua API em tempo real\!

* **Teste o `POST`:** Crie um novo produto.
* **Teste o `GET`:** Busque a lista de todos os produtos para ver o que você criou.
* **Teste os outros endpoints** para editar e deletar o produto.

Este tutorial cobre o ciclo completo e a arquitetura fundamental de uma WEB API .NET 8.0. A partir daqui, você pode evoluir o projeto adicionando validações, tratamento de erros mais robusto, autenticação e muito mais. Espero que tenha sido claro e útil\!

---
