# DIO - Desafio Agendados de Tarefas 

Este desafio de projeto foi desenvolvido no módulo de API e Entity Framework, da trilha .NET da DIO do Bootcamp Deal Group AI Centric .NET.


## Contexto
Você precisa construir um sistema gerenciador de tarefas, onde você poderá cadastrar uma lista de tarefas que permitirá organizar melhor a sua rotina.

Essa lista de tarefas precisa ter um CRUD, ou seja, deverá permitir a você obter os registros, criar, salvar e deletar esses registros.

A sua aplicação deverá ser do tipo Web API ou MVC, fique a vontade para implementar a solução que achar mais adequado.

A sua classe principal, a classe de tarefa, deve ser a seguinte:

![Diagrama da classe Tarefa](diagrama.png)

Não se esqueça de gerar a sua migration para atualização no banco de dados.

## Métodos esperados
É esperado que você crie o seus métodos conforme a seguir:


**Swagger**


![Métodos Swagger](swagger.png)


**Endpoints**


| Verbo  | Endpoint                | Parâmetro | Body          |
|--------|-------------------------|-----------|---------------|
| GET    | /Tarefa/{id}            | id        | N/A           |
| PUT    | /Tarefa/{id}            | id        | Schema Tarefa |
| DELETE | /Tarefa/{id}            | id        | N/A           |
| GET    | /Tarefa/ObterTodos      | N/A       | N/A           |
| GET    | /Tarefa/ObterPorTitulo  | titulo    | N/A           |
| GET    | /Tarefa/ObterPorData    | data      | N/A           |
| GET    | /Tarefa/ObterPorStatus  | status    | N/A           |
| POST   | /Tarefa                 | N/A       | Schema Tarefa |

Esse é o schema (model) de Tarefa, utilizado para passar para os métodos que exigirem

```json
{
  "id": 0,
  "titulo": "string",
  "descricao": "string",
  "data": "2022-06-08T01:31:07.056Z",
  "status": "Pendente"
}
```


## Solução
Este desafio foi incrível e desafiador! Pude exercitar o conhecimento adquirito neste módulo e assim firmar melhor o aprendizado! Aqui neste repositório você encontrar a solução para o desafio proposto!


## 🛠️ Tecnologias

- .NET 8.0  
- ASP.NET Core Web API  
- Entity Framework Core  
- SQLite (ou outro banco)  
- Swagger UI


## 📋 Pré-requisitos
Certifique-se de ter as seguintes ferramentas instaladas:

- 🔹 **.NET 6.0** ou superior
- 🔹 **IDE**: Visual Studio 2022, VS Code ou similar
- 🔹 **Banco de Dados**: SQL server management studio; SQL Server 2022 Express


## 🚀 Como Executar
```bash
# 1. Clone o repositório
git clone https://github.com/erasmobezerra/desafio-agendador-de-tarefas.git

# 2. Navegue até a pasta do projeto
cd ./desafio-agendador-de-tarefas

# 3. Tenha certeza que seu SQL Server está em execução 

# 4. Restaurar os pacotes
dotnet restore

# 5. Aplicar as migrations (se necessário)
dotnet ef database update

# 6. Execute o projeto e a página do Swagger será aberta automaticamente no seu navegador de internet padrão no endereço: https://localhost:7295/swagger 
dotnet whatch run
```

## 📬 Contato
<div align="left">

**Erasmo Bezerra**

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:erasmo.ads.tech@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/erasmobezerra/)

</div>

---

🙏 Agradeço profundamente à **Digital Innovation One** por proporcionar este aprendizado gratuito e de qualidade. Um reconhecimento especial ao professor **[Leonardo Buta](https://www.linkedin.com/in/leonardo-buta/)** pela excelente didática e orientação durante todo o processo.

<div align="center">
  <p>⭐ Se este projeto foi útil para você, considere dar uma estrela!</p>
</div>
