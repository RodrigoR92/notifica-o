# Serviço de Notificação por E-mail

Aplicação REST para enviar notificações de tarefas por e-mail. A mensagem é renderizada com Thymeleaf e inclui o nome da tarefa, a data do evento e sua descrição.

## Tecnologias

- Java 21
- Spring Boot
- Spring Web MVC
- Spring Mail
- Thymeleaf
- Gradle

## Requisitos

- JDK 21
- Acesso a um servidor SMTP. Para a configuração padrão, use uma conta Gmail com uma senha de app.

## Configuração

O serviço lê as configurações de e-mail do `src/main/resources/application.yaml`. Defina as credenciais SMTP e os dados do remetente antes de iniciar a aplicação. Por exemplo, é possível usar variáveis de ambiente:

| Variável | Descrição |
| --- | --- |
| `SPRING_MAIL_HOST` | Host SMTP |
| `SPRING_MAIL_PORT` | Porta SMTP |
| `SPRING_MAIL_USERNAME` | Usuário da conta SMTP |
| `SPRING_MAIL_PASSWORD` | Senha ou senha de app SMTP |
| `ENVIO_EMAIL_REMETENTE` | Endereço exibido como remetente |
| `ENVIO_EMAIL_NOMEREMETENTE` | Nome exibido como remetente |

O servidor HTTP inicia na porta `8082`.

## Executar

Na raiz do repositório, entre na pasta do projeto:

```shell
cd notificacao
```

No Windows:

```powershell
.\gradlew.bat bootRun
```

No Linux ou macOS:

```shell
./gradlew bootRun
```

## Endpoint

Envie uma requisição `POST` para `http://localhost:8082/email` com um corpo JSON. Os campos usados para compor o e-mail são `emailUsuario`, `nomeDaTarefa`, `descricao` e `dataEvento`. A data deve seguir o formato `dd-MM-yyyy HH:mm:ss`.

Exemplo:

```http
POST /email HTTP/1.1
Host: localhost:8082
Content-Type: application/json
```

```json
{
  "emailUsuario": "pessoa@example.com",
  "nomeDaTarefa": "Enviar relatório",
  "descricao": "Revisar e enviar o relatório mensal.",
  "dataEvento": "30-10-2026 14:30:00"
}
```

Com `curl`:

```shell
curl -X POST http://localhost:8082/email \
  -H "Content-Type: application/json" \
  -d "{\"emailUsuario\":\"pessoa@example.com\",\"nomeDaTarefa\":\"Enviar relatório\",\"descricao\":\"Revisar e enviar o relatório mensal.\",\"dataEvento\":\"30-10-2026 14:30:00\"}"
```

Quando o envio é concluído, a API responde com HTTP `200 OK` e corpo vazio.

## Testes

Na pasta `notificacao`, execute:

```shell
./gradlew test
```

No Windows, use `.\gradlew.bat test`.
