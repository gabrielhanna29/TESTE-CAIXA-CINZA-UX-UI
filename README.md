# Introdução

Este repositório documenta a atividade prática de **testes caixa cinza em API de autenticação**, utilizando o **Supabase** como provedor de autenticação, o **Postman Desktop** para execução das requisições e o **GitHub** para versionamento e documentação.

- **Aluno:** [SEU NOME COMPLETO]
- **RA / Matrícula:** [SEU RA]
- **Disciplina / Turma:** [DISCIPLINA] / [TURMA]
- **Data:** [DATA DA REALIZAÇÃO]

### Estrutura do repositório

```
/README.md
/postman/     -> coleção e environment exportados do Postman
/prints/      -> evidências (capturas de tela reais)
/planilha/    -> planilha de registro dos testes
```

---

# Objetivo da Atividade

Compreender e aplicar **testes de caixa cinza** em uma API de autenticação. No teste caixa cinza o testador conhece parcialmente a estrutura do sistema (endpoint, headers, formato do body e respostas HTTP esperadas), mas não acessa o código-fonte interno. Ao final, a atividade deve demonstrar:

- configuração de um ambiente de autenticação no Supabase;
- criação de usuário de teste;
- entendimento de requisições HTTP (método, endpoint, headers, body, status);
- execução e validação de testes no Postman;
- documentação técnica completa do processo.

---

# Configuração do Supabase

### Como o projeto foi criado
1. Acessei `https://supabase.com` e criei uma conta com [GitHub / e-mail].
2. Em **New project**, criei o projeto **[NOME DO PROJETO]** na organização [NOME DA ORGANIZAÇÃO], região [REGIÃO], definindo uma senha para o banco de dados.
3. Aguardei o provisionamento e acessei o painel (Dashboard) do projeto.

### Tipo de autenticação utilizada
Autenticação por **e-mail e senha** (provider *Email*), em **Authentication → Providers → Email**, habilitado. [Descreva se desativou "Confirm email" para facilitar os testes.]

### Como o usuário foi criado
Em **Authentication → Users → Add user → Create new user**, cadastrei o usuário `[EMAIL DE TESTE]` com a senha definida por mim, marcando **Auto Confirm User** para que o login funcione sem confirmação por e-mail.

### Credenciais da API
Em **Project Settings → API** obtive:
- **Project URL** (API URL): `https://[PROJECT-REF].supabase.co`
- **anon public key** (API KEY): `XXXXX` *(mascarada neste documento)*

### Objetivo da configuração
Disponibilizar um serviço de autenticação real, com um usuário válido conhecido, para que o endpoint de login possa ser testado com cenários positivos e negativos.

### Evidências
| Evidência | Print |
|---|---|
| Projeto criado | ![projeto](prints/01-projeto-criado.png) |
| Painel do Supabase | ![painel](prints/02-painel-supabase.png) |
| Autenticação por e-mail habilitada | ![auth](prints/03-auth-email-habilitada.png) |
| Usuário criado | ![usuario](prints/04-usuario-criado.png) |
| Credenciais da API (chave parcialmente oculta) | ![api](prints/05-credenciais-api.png) |
| Identificação do aluno no ambiente | ![aluno](prints/06-identificacao-aluno.png) |

---

# Configuração do Postman

### Workspace
Instalei o **Postman Desktop** e criei o workspace **[NOME DO WORKSPACE]** (Workspaces → Create Workspace), para manter a coleção e o environment desta atividade isolados.

### Environment
Criei o environment **Supabase Auth - Testes Caixa Cinza** com as variáveis:

| Variável | Descrição |
|---|---|
| `base_url` | URL da API do Supabase (`https://[PROJECT-REF].supabase.co`) |
| `api_key` | API KEY (anon public key) do projeto |
| `email` | E-mail do usuário de teste criado no Supabase |
| `password` | Senha do usuário de teste |

### Finalidade das variáveis
Separar os dados de configuração das requisições: se a URL, a chave ou as credenciais mudarem, altera-se apenas o environment, e as requisições usam `{{base_url}}`, `{{api_key}}`, `{{email}}` e `{{password}}`. Também evita expor segredos dentro das requisições.

### Como o Postman será utilizado
Será criada uma coleção com uma requisição `POST` por cenário de teste. Cada uma contém *tests* (scripts em `pm.test`) que validam automaticamente status HTTP e conteúdo da resposta. Arquivos exportados em [`/postman`](postman/).

### Evidências
| Evidência | Print |
|---|---|
| Workspace criado | ![workspace](prints/07-workspace.png) |
| Environment criado | ![env](prints/08-environment.png) |
| Variáveis configuradas | ![vars](prints/09-variaveis.png) |
| Organização do Postman (coleção) | ![org](prints/10-organizacao-postman.png) |

---

# Configuração das Requisições

- **Endpoint:** `{{base_url}}/auth/v1/token?grant_type=password`
- **Método HTTP:** `POST`
- **Finalidade:** autenticar o usuário por e-mail e senha (*password grant*). Em caso de sucesso, o Supabase devolve `access_token` (JWT), `refresh_token`, `token_type`, `expires_in` e os dados do usuário.

### Exemplo de Header

| Header | Valor |
|---|---|
| `apikey` | `XXXXX` (usa `{{api_key}}`) |
| `Authorization` | `Bearer XXXXX` (usa `Bearer {{api_key}}`) |
| `Content-Type` | `application/json` |

### Exemplo do Body JSON

```json
{
  "email": "usuario@email.com",
  "password": "123456"
}
```

### Evidências
| Evidência | Print |
|---|---|
| Endpoint configurado | ![endpoint](prints/11-endpoint.png) |
| Headers | ![headers](prints/12-headers.png) |
| Body JSON | ![body](prints/13-body.png) |
| Requisição criada no Postman | ![req](prints/14-requisicao-criada.png) |

---

# Execução dos Testes

Foram executados os 5 cenários obrigatórios, todos contra o mesmo endpoint, variando apenas o body.

### Tabela de Cenários

| Cenário | Entrada Utilizada | Resultado Esperado | Resultado Obtido | Status |
|---|---|---|---|---|
| CT01 - Login válido | e-mail e senha corretos | Status 200 + access token | [PREENCHER com status e trecho real da resposta] | [OK/FALHA] |
| CT02 - Senha incorreta | e-mail correto, senha errada | Erro de autenticação | [PREENCHER] | [OK/FALHA] |
| CT03 - Usuário inexistente | e-mail não cadastrado | Acesso negado | [PREENCHER] | [OK/FALHA] |
| CT04 - Campos vazios | `email: ""`, `password: ""` | Erro de validação | [PREENCHER] | [OK/FALHA] |
| CT05 - Credenciais inválidas | e-mail mal formatado, senha curta | Mensagem de erro | [PREENCHER] | [OK/FALHA] |

### O que foi validado em cada teste
Status HTTP, corpo JSON da resposta, mensagens de erro, presença/ausência do `access_token` e o comportamento geral da autenticação.

### Erros encontrados
[DESCREVA os erros/comportamentos inesperados que você realmente observou. Se nenhum, escreva "Nenhum comportamento inesperado".]

### Evidências
| Evidência | Print |
|---|---|
| Teste válido (200 + `access_token`) | ![ct01](prints/15-ct01-login-valido.png) |
| Senha incorreta | ![ct02](prints/16-ct02-senha-incorreta.png) |
| Usuário inexistente | ![ct03](prints/17-ct03-usuario-inexistente.png) |
| Campos vazios | ![ct04](prints/18-ct04-campos-vazios.png) |
| Credenciais inválidas | ![ct05](prints/19-ct05-credenciais-invalidas.png) |
| Resultado dos *tests* do Postman (Test Results) | ![results](prints/20-test-results.png) |

> No print do CT01, ocultar parte do `access_token`.

---

# Registro dos Testes

### Como os testes foram registrados
Cada execução foi registrada na planilha [`planilha/registro_testes.xlsx`](planilha/registro_testes.xlsx) com os campos: ID do teste, Cenário, Entrada, Resultado esperado, Resultado obtido, Status (Sucesso/Falha) e Observações.

### Importância da documentação dos testes
O registro torna os testes **rastreáveis e repetíveis**, permite comparar o resultado esperado com o obtido, serve de evidência para auditoria e facilita a identificação de regressões quando a API mudar.

### Falhas identificadas
[LISTE as falhas reais registradas na planilha, ou escreva que nenhuma falha foi identificada.]

### Evidências
![planilha](prints/21-planilha-preenchida.png)

Exemplos de registros:

| ID | Cenário | Entrada | Esperado | Obtido | Status | Observações |
|---|---|---|---|---|---|---|
| CT01 | Login válido | [..] | 200 + token | [..] | [..] | [..] |
| CT02 | Senha incorreta | [..] | Erro de autenticação | [..] | [..] | [..] |

---

# Resultados Obtidos

| Total de testes | Sucesso | Falha |
|---|---|---|
| 5 | [N] | [N] |

[Resuma em 3 a 5 linhas o que os resultados mostram: ex.: o login válido retornou 200 com token JWT; os cenários inválidos retornaram status 4xx sem `access_token`, com mensagens de erro no JSON.]

---

# Conclusão

- **Os testes foram executados corretamente?** [Sim/Não e justificativa]
- **A autenticação funcionou?** [Descreva com base nos resultados reais]
- **Dificuldades encontradas:** [ex.: localizar a API key, configurar variáveis, confirmar o usuário]
- **Falhas identificadas:** [as reais, ou "nenhuma"]
- **Melhorias possíveis:** [ex.: testes de bloqueio por tentativas excessivas (rate limit), expiração e renovação do token via `refresh_token`, senha fraca, uso de e-mail com espaços/maiúsculas, automação da coleção por Newman em CI]
- **Importância dos testes caixa cinza em APIs:** permitem validar o contrato (endpoint, headers, body, status) e o comportamento de segurança da autenticação com conhecimento parcial do sistema, encontrando falhas de validação e de controle de acesso sem depender do código interno nem de interface gráfica.
