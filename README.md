# Ponderada coleta de métricas

Tutorial sobre implementação de métricas em aplicações ASP.NET Core utilizando a injeção de dependência (DI). Destinado a desenvolvedores que desejam aprofundar seus conhecimentos em monitoramento e gestão de desempenho de aplicativos, integrando métricas de forma eficiente e sustentável. 

A realização dessa atividade foi baseada em um tutorial:

https://learn.microsoft.com/pt-br/dotnet/core/diagnostics/metrics-instrumentation

## Criação do projeto 

Por início é necessário criar uma console aplication com o uso do comando 

```
dotnet new console
```

Depois de criado o projeto ainda no terminal é necessário dar o comando a seguir, para adição de pacotes Nuget ao projeto

```
dotnet add package System.Diagnostics.DiagnosticSource
```

Depois disso já criada a aplicação é necessário substituir o conteúdo inicial do arquivo program.cs, como na imagem a seguir:

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/27900568-1052-4e07-af23-5dc1a608399b)

Concluída essa alteração no program.cs e dando o comando de rodar a aplicação 'dotnet run', obtemos o valor a seguir 
- Press any key to exit

### Exibir nova métrica

Para exibir novas métricas e dando seguimento ao tutorial executamos o comando a seguir

```
dotnet tool update -g dotnet-counters
```

Posterior a isso, e com a aplicação em execução executo o comando a seguir:

```
dotnet-counters monitor -n metric-demo.exe --counters HatCo.Store
```

e obtemos isso que está na imagem a seguir: 

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/83eaf5ff-fbdc-428c-8535-d28f294a49d6)

### Obtenha um Medidor por meio da injeção de dependência

Em projetos ASP.NET Core, recomenda-se usar IMeterFactory para gerenciar métricas de acordo com práticas de injeção de dependência (DI). A partir do .NET 8, o IMeterFactory pode ser automaticamente disponibilizado ou registrado manualmente via AddMetrics. Isso permite integrar métricas de forma isolada, ideal para testes unitários independentes. Basta adicionar IMeterFactory ao construtor e criar métricas com Create para uma arquitetura limpa e testável.

Para iniciar vamos criar uma nova Web API

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/71c28248-897b-401a-861a-7c34f0654b29)

Depois é necessário os comandos iciciais como no anterior, por exemplo 

```
dotnet add package System.Diagnostics.DiagnosticSource
```

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/84827c44-6e73-4163-bf08-fbeaa47c8e9c)

Concluído essa etapa, com tudo funcionando começou a aplicação do código

**Implementação** 

Para adicionar o código criei um arquivo chamado: HatCoMetrics.cs e adicionei o código necessário, como na imagem a seguir: 

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/04db8aeb-aae9-41c6-b761-911abeae5c25)

e depois disso é necessário adicionar a linha a seguir na program.cs

```
builder.Services.AddSingleton<HatCoMetrics>();
```

depois disso foi necessário criar uma models com a class SaleModel

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/070093c0-1606-49a0-aa7a-04e8dc28591e)

Chamar o endpoint na program.cs

```
app.MapPost("/complete-sale", ([FromBody] SaleModel model, HatCoMetrics metrics) =>
{
    metrics.HatsSold(model.QuantitySold);
    return Results.Ok("Venda registrada.");
});
```

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/ae9da26c-9f21-4561-9c4a-768292094334)

E por fim foi feito o teste da rota do endpoint com o uso do postman e o resultado positivo, com a resposta colocado de "Venda Registrada"

![image](https://github.com/mariana2903/Ponderada_Metrics/assets/99264876/de2abd86-4280-4825-ae5f-9b2cac56498c)

Tecnologias Utilizadas <br>
Este tutorial faz uso de várias tecnologias e ferramentas,principalmente as listadas a seguir:

- ASP.NET Core
- System.Diagnostics.DiagnosticSource
- Postman

Conceitos Aprendidos 

- Injeção de Dependência (DI): Compreendemos como a DI facilita a gestão de dependências em aplicações .NET, permitindo uma maior modularidade e facilidade de teste.
- Métricas com IMeterFactory: Aprendemos como criar e gerenciar objetos Meter utilizando IMeterFactory, o que nos permite integrar métricas de forma limpa e conforme as melhores práticas.
- Registro e Uso de Métricas: Demonstramos como registrar e manipular dados de métricas, utilizando a classe HatCoMetrics para coletar e registrar eventos específicos, como vendas de produtos.






