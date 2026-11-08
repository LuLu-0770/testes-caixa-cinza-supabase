# Introdução

**Aluno:** Luan Gomes Marques
**Disciplina:** [NOME DA DISCIPLINA]
**Data:** 07/11/2026

Este repositório documenta a atividade de **testes de caixa cinza** aplicados à API de autenticação do **Supabase**, executados com o **Postman Desktop**. No teste de caixa cinza o testador conhece parcialmente o sistema (endpoint, headers, formato do body e das respostas HTTP), mas não tem acesso ao código-fonte interno. Nenhuma interface gráfica foi desenvolvida: todo o trabalho foi feito por requisições HTTP.

**Estrutura do repositório**

```
/README.md
/postman/    -> collection e environment exportados (chaves mascaradas)
/prints/     -> evidências (prints) produzidas durante a atividade
/planilha/   -> testes.xlsx (registro dos testes)
```

# Objetivo da Atividade

- Configurar um ambiente de autenticação no Supabase;
- Criar usuários para testes;
- Compreender o funcionamento de requisições HTTP;
- Utilizar o Postman para executar e validar testes funcionais;
- Validar status HTTP, JSON, mensagens de erro e access token;
- Documentar tecnicamente todo o processo.

# Configuração do Supabase

**Como o projeto foi criado:** foi criada uma conta em supabase.com e, na organização **Luan**, um novo projeto chamado **luangomes0770@gmail.com's Project** (referência `ubhbxqpxvgelvbpzxdrs`), hospedado na região **US East (North Virginia)** (`us-east-1`).

**Tipo de autenticação:** e-mail e senha (*Email provider*), habilitado em *Authentication → Providers*.

**Como o usuário foi criado:** em *Authentication → Users → Add user → Create new user*, com o e-mail `[SEU E-MAIL DE TESTE]`, com a opção *Auto Confirm User* marcada para permitir o login imediato.

**Objetivo da configuração:** disponibilizar uma API de autenticação real e um usuário válido, para que os cenários de login (válidos e inválidos) pudessem ser testados.

**Dados utilizados nos testes**

| Item | Valor |
|---|---|
| Nome do projeto | `luangomes0770@gmail.com's Project` |
| Project Reference | `ubhbxqpxvgelvbpzxdrs` |
| Organização | `Luan` |
| Região | US East (North Virginia) - `us-east-1` |
| API URL | `https://ubhbxqpxvgelvbpzxdrs.supabase.co` |
| API KEY (anon/publishable) | `sb_publishable_...` (configurada localmente no Postman) |
| Usuário de teste | `luangomes0770@gmail.com` |

> A chave completa e a senha foram omitidas por segurança.

**Arquivos de teste:** importe `postman/supabase-auth-tests.postman_collection.json` e `postman/supabase-testes.postman_environment.json`. Preencha `api_key` com a chave `sb_publishable_` e `password` com a senha do usuário diretamente no Postman. Não envie a senha no chat.

**Evidências**

| Evidência | Print |
|---|---|
| Projeto criado | ![Projeto criado](prints/01-projeto-criado.png) |
| Painel do Supabase | ![Painel](prints/02-painel-supabase.png) |
| Autenticação habilitada | ![Auth habilitada](prints/03-auth-habilitada.png) |
| Usuário criado | ![Usuário](prints/04-usuario-criado.png) |
| Credenciais da API (chave parcialmente oculta) | ![Credenciais](prints/05-credenciais-api.png) |

# Configuração do Postman

**Workspace:** preencher com o nome exibido no seletor de Workspaces do Postman.

**Environment:** importar `postman/supabase-testes.postman_environment.json`; o nome inicial é `Supabase - Testes Luan`.

**Variáveis e finalidade**

| Variável | Descrição | Finalidade |
|---|---|---|
| `base_url` | URL da API do Supabase | Evita repetir a URL em cada requisição |
| `api_key` | API KEY (anon/publishable) | Identifica o projeto nos headers |
| `email` | Usuário criado | E-mail usado no login |
| `password` | Senha criada | Senha usada no login |

**Uso nos testes:** o Postman envia as requisições `POST` ao endpoint de autenticação, e os valores das variáveis são substituídos automaticamente (`{{variavel}}`). Cada cenário é uma requisição dentro de uma Collection.

**Evidências**

| Evidência | Print |
|---|---|
| Workspace criado | ![Workspace](prints/06-workspace.png) |
| Environment criado | ![Environment](prints/07-environment.png) |
| Variáveis configuradas | ![Variáveis](prints/08-variaveis.png) |
| Organização do Postman (collection) | ![Organização](prints/09-organizacao-postman.png) |

# Configuração das Requisições

- **Endpoint:** `{{base_url}}/auth/v1/token?grant_type=password`
- **Método HTTP:** `POST`
- **Finalidade:** autenticar um usuário por e-mail e senha; em caso de sucesso, retorna `access_token`, `refresh_token` e dados do usuário.

**Headers utilizados**

| Header | Valor |
|---|---|
| apikey | `{{api_key}}` (chave `sb_publishable_`) |
| Content-Type | application/json |

Não é necessário enviar `Authorization: Bearer {{api_key}}` no login por senha. Não use a chave `sb_secret_` nem `service_role` nesta Collection.

**Body JSON utilizado**

```json
{
  "email": "usuario@email.com",
  "password": "123456"
}
```

No Postman o body usa as variáveis do environment: `"email": "{{email}}"` e `"password": "{{password}}"`.

**Evidências**

| Evidência | Print |
|---|---|
| Endpoint configurado | ![Endpoint](prints/10-endpoint.png) |
| Headers | ![Headers](prints/11-headers.png) |
| Body JSON | ![Body](prints/12-body.png) |
| Requisição criada no Postman | ![Requisição](prints/13-requisicao.png) |

# Execução dos Testes

A Collection contém os 5 cenários obrigatórios e 1 cenário extra. Os resultados ainda precisam ser coletados no Postman; não foram inventados.

**Tabela de Cenários**

| ID | Cenário | Entrada Utilizada | Resultado Esperado | Resultado Obtido | Status |
|---|---|---|---|---|---|
| CT01 | Login válido | E-mail e senha corretos | Status 200 + access token | Pendente: executar no Postman e registrar status e campos retornados, sem copiar tokens. | [OK/FALHA] |
| CT02 | Senha incorreta | E-mail correto, senha errada | Erro de autenticação | Pendente: executar no Postman e registrar status e mensagem. | [OK/FALHA] |
| CT03 | Usuário inexistente | E-mail não cadastrado | Acesso negado | Pendente: executar no Postman e registrar status e mensagem. | [OK/FALHA] |
| CT04 | Campos vazios | E-mail e senha vazios | Erro de validação | Pendente: executar no Postman e registrar status e mensagem. | [OK/FALHA] |
| CT05 | Credenciais inválidas | E-mail malformado | Mensagem de erro | Pendente: executar no Postman e registrar status e mensagem. | [OK/FALHA] |
| CT06 (extra) | Sem header `apikey` | Requisição sem `apikey` | Status 401 | [PREENCHER] | [OK/FALHA] |

**Detalhamento e evidências**

### CT01 - Login válido
- Entrada: [descreva]
- Validações: status HTTP, JSON retornado, presença de `access_token` (parcialmente ocultado no print).
- Resultado: [PREENCHER]

![CT01](prints/14-ct01-login-valido.png)

### CT02 - Senha incorreta
- Entrada: [descreva]
- Resultado: Pendente: executar no Postman e registrar status e mensagem.

![CT02](prints/15-ct02-senha-incorreta.png)

### CT03 - Usuário inexistente
- Entrada: [descreva]
- Resultado: Pendente: executar no Postman e registrar status e mensagem.

![CT03](prints/16-ct03-usuario-inexistente.png)

### CT04 - Campos vazios
- Entrada: [descreva]
- Resultado: Pendente: executar no Postman e registrar status e mensagem.

![CT04](prints/17-ct04-campos-vazios.png)

### CT05 - Credenciais inválidas
- Entrada: [descreva]
- Resultado: Pendente: executar no Postman e registrar status e mensagem.

![CT05](prints/18-ct05-credenciais-invalidas.png)

### CT06 (extra) - Sem apikey
- Resultado: [PREENCHER]

![CT06](prints/19-ct06-sem-apikey.png)

**Erros/observações encontrados durante os testes:** [PREENCHER: ex. erro `email_not_confirmed` no primeiro login, mensagens iguais para senha errada e usuário inexistente, etc.]

# Registro dos Testes

**Como foram registrados:** cada teste foi registrado na planilha [`planilha/testes.xlsx`](planilha/testes.xlsx), com os campos: ID do teste, cenário, entrada, resultado esperado, resultado obtido, status (Sucesso/Falha) e observações.

**Importância da documentação:** permite rastrear o que foi testado, repetir os testes (reprodutibilidade), comparar resultado esperado com obtido, identificar falhas e servir de evidência técnica para a equipe.

**Falhas identificadas:** Pendente: descrever somente dificuldades realmente observadas durante a execução.

**Evidências**

![Planilha preenchida](prints/20-planilha-preenchida.png)

Exemplos de registros realizados:

| ID | Cenário | Resultado obtido | Status |
|---|---|---|---|
| CT01 | Login válido | [PREENCHER] | [Sucesso/Falha] |
| CT02 | Senha incorreta | [PREENCHER] | [Sucesso/Falha] |

# Resultados Obtidos

- Total de testes executados: **[N]**
- Testes com sucesso: **[N]**
- Testes com falha: **[N]**

| Item validado | Resultado |
|---|---|
| Status HTTP | [PREENCHER] |
| Estrutura do JSON de resposta | [PREENCHER] |
| Mensagens de erro | [PREENCHER] |
| Access token retornado no login válido | [PREENCHER] |
| Comportamento geral da autenticação | [PREENCHER] |

# Conclusão

A conclusão deve ser escrita após a execução, com base nos resultados reais. Não afirmar que a autenticação funcionou ou que não houve falhas antes de verificar as respostas.

**Os testes foram executados corretamente?** Pendente de execução.

**A autenticação funcionou?** Pendente de execução.

**Dificuldades encontradas:** Pendente de relato.

**Falhas identificadas:** Pendente de análise dos resultados.

**Melhorias possíveis:** automatizar os testes com scripts do Postman/Newman; incluir testes de limite de tentativas (rate limit); política de senha forte; autenticação multifator (MFA); testes de expiração e renovação de token (`refresh_token`).

**Importância dos testes de caixa cinza em APIs:** como se conhece o contrato da API (endpoint, headers, body e respostas) mas não o código interno, é possível testar o comportamento e a segurança de forma realista e eficiente, validando status, mensagens e tratamento de erros sem depender da implementação.
