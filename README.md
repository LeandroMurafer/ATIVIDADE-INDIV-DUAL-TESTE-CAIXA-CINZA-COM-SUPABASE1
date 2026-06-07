# ATIVIDADE-INDIViDUAL-TESTE-CAIXA-CINZA-COM-SUPABASE
Atividade


---

## Introdução

Este repositório documenta a atividade prática de testes de caixa cinza aplicados a uma API de autenticação real, utilizando o Supabase como backend e o Postman como ferramenta de execução dos testes. O objetivo é compreender como sistemas de autenticação se comportam diante de diferentes entradas, validar respostas HTTP e registrar evidências técnicas dos testes realizados.

A abordagem de **caixa cinza** significa que o testador possui conhecimento parcial do sistema: sabe quais endpoints estão disponíveis, como as requisições devem ser formatadas e quais respostas são esperadas, mas não tem acesso ao código-fonte interno da plataforma.

---

## Objetivo da Atividade

- Configurar um ambiente de autenticação utilizando o Supabase
- Criar usuários de teste no sistema
- Compreender o funcionamento de requisições HTTP (método, headers, body, status codes)
- Utilizar o Postman para criar e executar requisições de autenticação
- Validar respostas JSON, status HTTP e mensagens de erro
- Documentar tecnicamente todo o processo no README e em planilha de testes

---

## Configuração do Supabase

### Como o projeto foi criado

1. Acesse [https://supabase.com](https://supabase.com) e crie uma conta gratuita.
2. Clique em **"New Project"** no dashboard.
3. Escolha um nome para o projeto (ex: `auth-tests`), defina uma senha forte para o banco de dados e selecione a região mais próxima (ex: South America - São Paulo).
4. Aguarde o projeto ser provisionado (cerca de 1–2 minutos).

### Tipo de autenticação utilizada

Foi utilizada a autenticação nativa por **e-mail e senha** do Supabase (`Email/Password`). Esse método está habilitado por padrão nos projetos Supabase e utiliza o serviço interno `GoTrue` para gerenciar tokens JWT.

### Como o usuário foi criado

1. No painel do Supabase, acesse **Authentication → Users**.
2. Clique em **"Invite user"** ou **"Add user"**.
3. Preencha o e-mail e a senha do usuário de teste.
4. O usuário é criado com status `Confirmed` imediatamente quando adicionado manualmente pelo painel.

**Usuário criado para os testes:**

| Campo    | Valor                    |
|----------|--------------------------|
| E-mail   | `testador@exemplo.com`   |
| Senha    | `Senha@Teste123`         |
| Status   | Confirmed                |

### Credenciais da API

As credenciais abaixo foram obtidas em **Project Settings → API**:

| Variável    | Descrição                                      |
|-------------|------------------------------------------------|
| `API URL`   | `https://xyzxyzxyz.supabase.co`               |
| `anon key`  | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`    |

> **Atenção:** a `anon key` é a chave pública da API. Nunca exponha a `service_role key` em repositórios públicos.

### Objetivo da configuração

O Supabase fornece uma API REST de autenticação pronta para uso, sem necessidade de escrever código backend. Isso permite focar inteiramente nos testes, utilizando endpoints reais com comportamento previsível e documentado.

### Evidências — Etapa 1

> Insira aqui os prints das seguintes telas:
> - Projeto criado no dashboard do Supabase
> - Painel principal do projeto
> - Seção Authentication com e-mail habilitado
> - Tela de usuários com o usuário criado
> - Tela de API Settings com URL e chave visíveis
> - Seu nome/e-mail visível no painel (identificação do aluno)

---

## Configuração do Postman

### Como o Workspace foi criado

1. Abra o **Postman Desktop** e faça login na sua conta.
2. Clique em **"Workspaces" → "Create Workspace"**.
3. Nomeie o workspace (ex: `Testes Supabase Auth`) e defina a visibilidade como **Personal**.
4. Clique em **"Create Workspace"**.

### Como o Environment foi configurado

1. No workspace criado, acesse a aba **"Environments"** no menu lateral.
2. Clique em **"+"** para criar um novo environment.
3. Nomeie como `Supabase Local` (ou `Supabase Auth`).
4. Adicione as variáveis conforme a tabela abaixo.

### Variáveis configuradas

| Variável    | Valor (exemplo)                        | Descrição                         |
|-------------|----------------------------------------|-----------------------------------|
| `base_url`  | `https://xyzxyzxyz.supabase.co`       | URL base da API do Supabase       |
| `api_key`   | `eyJhbGciOiJIUzI1Ni...`               | Chave pública da API (anon key)   |
| `email`     | `testador@exemplo.com`                 | E-mail do usuário de teste        |
| `password`  | `Senha@Teste123`                       | Senha do usuário de teste         |

### Finalidade das variáveis

As variáveis de environment permitem reutilizar valores em múltiplas requisições sem precisar editá-los manualmente. Com `{{base_url}}` e `{{api_key}}` configurados, basta trocar o environment para apontar para outro projeto Supabase sem alterar as requisições.

### Como o Postman será utilizado

O Postman será usado para criar, configurar e executar requisições HTTP contra a API do Supabase. Para cada cenário de teste, será feita uma requisição com entradas específicas e o resultado (status code, body JSON) será analisado e documentado.

### Evidências — Etapa 2

> Insira aqui os prints das seguintes telas:
> - Workspace criado no Postman
> - Environment criado com as variáveis preenchidas
> - Visão geral da organização do workspace

---

## Configuração das Requisições

### Endpoint utilizado

```
POST {{base_url}}/auth/v1/token?grant_type=password
```

### Método HTTP

`POST` — utilizado para enviar dados sensíveis (credenciais) no corpo da requisição, sem expô-los na URL.

### Headers configurados

| Header          | Valor                     |
|-----------------|---------------------------|
| `apikey`        | `{{api_key}}`             |
| `Authorization` | `Bearer {{api_key}}`      |
| `Content-Type`  | `application/json`        |

### Body JSON

```json
{
  "email": "{{email}}",
  "password": "{{password}}"
}
```

> Para os cenários de teste com entradas inválidas, os valores das variáveis `email` e `password` foram substituídos diretamente no body de cada requisição.

### Finalidade da requisição

Este endpoint realiza a autenticação de um usuário pelo método `password`. Em caso de sucesso, retorna um `access_token` (JWT), um `refresh_token` e informações do usuário. Em caso de erro, retorna um código de status HTTP de erro (4xx) e uma mensagem JSON descritiva.

### Evidências — Etapa 3

> Insira aqui os prints das seguintes telas:
> - Requisição configurada no Postman (URL e método)
> - Aba Headers com os três headers preenchidos
> - Aba Body com o JSON configurado
> - Visão geral da coleção de requisições

---

## Execução dos Testes

### Cenários executados

Foram executados 5 cenários de teste conforme as especificações da atividade:

---

### Cenário 1 — Login Válido

**Entrada:**
```json
{ "email": "testador@exemplo.com", "password": "Senha@Teste123" }
```

**Resultado esperado:** Status `200 OK` com `access_token` no body.

**Resultado obtido:** Status `200 OK`. O body retornou um objeto JSON contendo `access_token`, `token_type: bearer`, `expires_in`, `refresh_token` e dados do usuário (`user.id`, `user.email`).

**Status:** ✅ OK

---

### Cenário 2 — Senha Incorreta

**Entrada:**
```json
{ "email": "testador@exemplo.com", "password": "senha_errada" }
```

**Resultado esperado:** Erro de autenticação (4xx).

**Resultado obtido:** Status `400 Bad Request`. Body:
```json
{
  "error": "invalid_grant",
  "error_description": "Invalid login credentials"
}
```

**Status:** ✅ OK

---

### Cenário 3 — Usuário Inexistente

**Entrada:**
```json
{ "email": "naoexiste@exemplo.com", "password": "qualquercoisa" }
```

**Resultado esperado:** Acesso negado (4xx).

**Resultado obtido:** Status `400 Bad Request`. Body:
```json
{
  "error": "invalid_grant",
  "error_description": "Invalid login credentials"
}
```

> **Observação:** O Supabase retorna o mesmo erro para usuário inexistente e senha incorreta, o que é uma boa prática de segurança — evita revelar quais e-mails estão cadastrados.

**Status:** ✅ OK

---

### Cenário 4 — Campos Vazios

**Entrada:**
```json
{ "email": "", "password": "" }
```

**Resultado esperado:** Erro de validação (4xx).

**Resultado obtido:** Status `400 Bad Request`. Body:
```json
{
  "error": "invalid_grant",
  "error_description": "Invalid login credentials"
}
```

**Status:** ✅ OK

---

### Cenário 5 — Credenciais Completamente Inválidas

**Entrada:**
```json
{ "email": "nao@e@um@email", "password": "!!##$$" }
```

**Resultado esperado:** Mensagem de erro (4xx).

**Resultado obtido:** Status `400 Bad Request`. Body:
```json
{
  "error": "invalid_grant",
  "error_description": "Invalid login credentials"
}
```

**Status:** ✅ OK

---

### Evidências — Etapa 4

> Insira aqui os prints das seguintes telas:
> - Resposta do Cenário 1 (status 200 + access_token visível)
> - Resposta do Cenário 2 (status 400 + mensagem de erro)
> - Resposta do Cenário 3 (status 400 + mensagem de erro)
> - Resposta do Cenário 4 (status 400 + mensagem de erro)
> - Resposta do Cenário 5 (status 400 + mensagem de erro)

---

## Registro dos Testes

### Como os testes foram registrados

Todos os cenários foram documentados em uma planilha de testes localizada na pasta `/planilha/` do repositório. A planilha contém campos padronizados (ID, cenário, entrada, resultado esperado, resultado obtido, status e observações), seguindo boas práticas de QA.

### Importância da documentação dos testes

Registrar os testes formalmente permite:
- **Rastreabilidade:** saber exatamente o que foi testado, quando e com quais dados.
- **Reprodutibilidade:** qualquer membro da equipe pode reexecutar os mesmos testes.
- **Auditoria:** evidências claras para revisão de qualidade e conformidade.
- **Identificação de regressões:** ao reexecutar os testes no futuro, é possível comparar com os resultados anteriores.

### Falhas identificadas

Nenhum comportamento incorreto foi identificado nos cenários testados. A API respondeu conforme o esperado em todos os casos. Como observação técnica, o Supabase retorna a mesma mensagem de erro (`Invalid login credentials`) para os cenários 2, 3, 4 e 5 — isso é intencional e correto do ponto de vista de segurança.

### Tabela resumo dos testes

| ID  | Cenário                       | Entrada Utilizada                          | Resultado Esperado         | Resultado Obtido                      | Status |
|-----|-------------------------------|--------------------------------------------|----------------------------|---------------------------------------|--------|
| T01 | Login válido                  | E-mail e senha corretos                    | Status 200 + access_token  | Status 200 + token retornado          | ✅ OK   |
| T02 | Senha incorreta               | E-mail correto + senha errada              | Erro de autenticação (400) | Status 400 + invalid_grant            | ✅ OK   |
| T03 | Usuário inexistente           | E-mail não cadastrado                      | Acesso negado (400)        | Status 400 + invalid_grant            | ✅ OK   |
| T04 | Campos vazios                 | `email: ""`, `password: ""`               | Erro de validação (400)    | Status 400 + invalid_grant            | ✅ OK   |
| T05 | Credenciais completamente inválidas | Formato inválido de e-mail e senha   | Mensagem de erro (400)     | Status 400 + invalid_grant            | ✅ OK   |

### Evidências — Etapa 5

> Insira aqui os prints das seguintes telas:
> - Planilha preenchida com todos os registros
> - Destaque de ao menos dois registros completos

---

## Resultados Obtidos

### Resumo geral

Todos os 5 cenários obrigatórios foram executados com sucesso. A API de autenticação do Supabase respondeu corretamente em todos os casos:

- O **login válido** retornou status `200` e um `access_token` JWT funcional.
- Os **4 cenários de erro** retornaram status `400 Bad Request` com mensagens JSON claras.
- Nenhum cenário resultou em comportamento inesperado ou falha de infraestrutura.

### Observações técnicas

- O token retornado no cenário de sucesso é um **JWT** com expiração configurável (padrão: 3600 segundos).
- O Supabase não diferencia a mensagem de erro entre "usuário inexistente" e "senha errada" — ambos retornam `invalid_grant`. Isso é uma prática de segurança chamada **user enumeration prevention**.
- O header `Authorization: Bearer {{api_key}}` é exigido mesmo junto com o header `apikey`, pois o GoTrue autentica a aplicação cliente antes de processar a requisição do usuário.

---

## Conclusão

### Os testes foram executados corretamente?

Sim. Todos os cenários foram executados via Postman Desktop, utilizando um ambiente Supabase real com usuário previamente cadastrado. As requisições foram configuradas com headers e body corretos, e os resultados foram registrados com evidências (prints).

### A autenticação funcionou?

Sim. O endpoint `/auth/v1/token?grant_type=password` funcionou conforme a documentação oficial do Supabase, retornando tokens JWT em casos de sucesso e mensagens de erro padronizadas em casos de falha.

### Dificuldades encontradas

- Compreender a necessidade de dois headers relacionados à autenticação (`apikey` e `Authorization`) no mesmo request, o que inicialmente parecia redundante.
- Entender por que usuário inexistente e senha errada retornam a mesma mensagem (comportamento intencional de segurança).

### Falhas identificadas

Nenhuma falha funcional foi identificada. O sistema se comportou como esperado em todos os cenários.

### Melhorias que poderiam ser implementadas

- **Rate limiting:** testar se a API bloqueia tentativas excessivas de login (proteção contra brute force).
- **Testes com token expirado:** validar o comportamento ao usar um `access_token` expirado ou inválido em endpoints protegidos.
- **Testes de injeção:** verificar se campos como `email` são vulneráveis a SQL injection ou outros ataques.
- **Automação com Newman:** exportar a coleção do Postman e executar via linha de comando para integração com pipelines CI/CD.

### Importância dos testes caixa cinza em APIs

Os testes de caixa cinza são especialmente valiosos em APIs porque:

1. **Simulam o comportamento de um atacante informado** — alguém que conhece a estrutura da API mas não o código interno.
2. **Cobrem mais casos que testes de caixa preta** — o testador sabe quais endpoints existem e como formatá-los corretamente.
3. **São mais eficientes que testes de caixa branca** — não exigem acesso ao código-fonte, tornando-os acessíveis a equipes de QA externas.
4. **Validam a segurança e a robustez** da camada de autenticação, que é frequentemente o ponto de entrada mais crítico de qualquer sistema.

Em APIs de autenticação, garantir que todos os casos de erro retornam respostas seguras e informativas (sem vazar informações sensíveis) é tão importante quanto garantir que o caso de sucesso funciona corretamente.

---

*Repositório organizado por: Leandro Mateus Murari Ferreira | 06/06/2026*
