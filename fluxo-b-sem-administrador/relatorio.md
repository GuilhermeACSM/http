# Relatório — Laboratório de Inspeção HTTP/HTTPS — Fluxo B (sem privilégio administrativo)

> **Como usar este template:** substitua os campos `[...]` pelas suas respostas,
> anexe as capturas de tela na pasta `evidencias/` e referencie-as onde indicado.
> Preserve a formatação markdown (tabelas, blocos de código) para facilitar a correção.
>
> **Observação:** toda a análise prática deste relatório é feita sobre tráfego **HTTP em texto claro**. A análise de HTTPS é teórica, baseada na fundamentação do `readme.md` do repositório.

---

## Identificação

| Campo       | Valor                  |
|-------------|------------------------|
| Nome        | Anna Clara Sbrama dos Santos |
| Nome        | Guilherme Augusto Corrêa Salgado Moreira |
| Disciplina  | Redes de Computadores  |
| Turma       | Sistemas para Internet 2° Ciclo |
| Data        | 30/04/2026   |
| Fluxo       | **B — Aluno sem privilégio de administrador** |
| SO utilizado | Windows 11 |
| Ferramenta de proxy | Fiddler Classic per-user |
| Navegador(es)       | Firefox 125 |
| HTTPS-First Mode / HTTPS-Only desabilitado? | sim |

---

## Atividade 1 — Primeira captura (`http://example.com`)

**Captura de tela:** <img width="1918" height="1069" alt="image" src="https://github.com/user-attachments/assets/bc84069a-cc5a-48cc-a02b-985308120d9a" />

**Request-line enviada:**

```http
GET http://www.textfiles.com HTTP/1.1
```

**Status-line recebida:**

```http
HTTP/1.1 200 OK
```

### Pergunta 1.1
> Quantos cabeçalhos o navegador enviou no request? Liste-os.

**Resposta:**
5 cabeçalhos

Cabeçalhos:
- Content-Type: text/html;charset=utf-8
- Content-Length: 7126
- Date: Thu, 30 Apr 2026 14:36:34 GMT
- Keep-Alive: timeout=60
- Connection: keep-alive


### Pergunta 1.2
> Qual foi o `Content-Length` da resposta? Se ele não apareceu, registre `Transfer-Encoding`, versão do protocolo ou outro indício observado. O corpo retornado é HTML, texto puro, JSON ou binário? Como você descobriu?

**Resposta:** Content-Length: 7126. HTML, descobri indo no TextView do Fiddler Classic.

---

## Atividade 2 — Anatomia de um GET (`http://httpbin.org/get?...`)

**Captura de tela:** <img width="1919" height="1069" alt="image" src="https://github.com/user-attachments/assets/cddc4e05-9374-4010-802e-c1e4f8ad49ef" />

**Request-line completa:**

```http
GET http://httpbin.org/forms/post HTTP/1.1
```

**Cabeçalhos-chave capturados:**

| Cabeçalho    | Valor                    |
|--------------|--------------------------|
| `Host`       | httpbin.org |
| `User-Agent` | Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:148.0) Gecko/20100101 Firefox/148.0 |
| `Accept`     | text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 |

**Campos do JSON de resposta:**

```json
{
  "args": {
    "aluno": "GuilhermeAugustoCorreaSalgadoMoreira", 
    "curso": "redes"
  }, 
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "en-US,en;q=0.9", 
    "Cache-Control": "no-cache", 
    "Host": "httpbin.org", 
    "Pragma": "no-cache", 
    "Priority": "u=0, i", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:148.0) Gecko/20100101 Firefox/148.0", 
    "X-Amzn-Trace-Id": "Root=1-69f3689d-43dd15612ada4cf910c7ba47"
  }, 
  "origin": "187.58.19.47", 
  "url": "http://httpbin.org/get?aluno=GuilhermeAugustoCorreaSalgadoMoreira&curso=redes"
}

```

### Pergunta 2.1
> O valor do campo `origin` corresponde a qual elemento da rede? Por que normalmente não é o IP local?

**Resposta:** Corresponde ao IP público. Pois é devido ao uso de NAT pelo roteador ou provedor de internet.

### Pergunta 2.2
> Compare o `User-Agent` enviado com o que aparece no JSON da resposta. Coincidem?

**Resposta:** Sim, coincidem.

### Pergunta 2.3
> Em `http://httpbin.org/headers`, liste até três cabeçalhos que o servidor vê mas **não aparecem** no Raw do request. De onde vêm? Se não encontrar três, explique por que o resultado pode variar.

**Resposta:** Apenas um cabeçalho extra é visível em relação ao que o navegador enviou originalmente: X-Amzn-Trace-Id. O número de cabeçalhos extras depende inteiramente de quais e quantos intermediários existem no caminho entre computador e o servidor final.

| Cabeçalho visto pelo servidor | Origem provável | Observação |
|-------------------------------|-----------------|------------|
| "X-Amzn-Trace-Id": "Root=1-69f3689d-43dd15612ada4cf910c7ba47" | Infraestrutura da AWS | O site é hospedado pela AWS. |
| [...]                         | [...]           | [...]      |
| [...]                         | [...]           | [...]      |

---

## Atividade 3 — POST e envio de formulário (`http://httpbin.org/forms/post` → `/post`)

**Captura de tela:** <img width="1918" height="1071" alt="image" src="https://github.com/user-attachments/assets/6215eec1-f7de-4948-88a3-818c137db38e" />

**Request-line do POST:**

```http
POST http://httpbin.org/post HTTP/1.1
```

**Cabeçalhos do request:**

| Cabeçalho        | Valor |
|------------------|-------|
| `Content-Type`   | "application/x-www-form-urlencoded" |
| `Content-Length` | "160" |

**Corpo completo do request:**

```
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "comments": "entregar no sindico", 
    "custemail": "asdasdasd@gmail.com", 
    "custname": "Gulherme", 
    "custtel": "111111111111111", 
    "delivery": "16:30", 
    "size": "medium", 
    "topping": [
      "bacon", 
      "cheese"
    ]
  }, 
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", 
    "Accept-Encoding": "gzip, deflate", 
    "Accept-Language": "en-US,en;q=0.9", 
    "Content-Length": "160", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "Origin": "http://httpbin.org", 
    "Priority": "u=0, i", 
    "Referer": "http://httpbin.org/forms/post", 
    "Upgrade-Insecure-Requests": "1", 
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:148.0) Gecko/20100101 Firefox/148.0", 
    "X-Amzn-Trace-Id": "Root=1-69f36f7d-65dfe10a1d6792eb65130c74"
  }, 
  "json": null, 
  "origin": "187.58.19.47", 
  "url": "http://httpbin.org/post"
}
```

**Trecho do JSON de resposta (campo `form`):**

```json
"form": {
    "comments": "entregar no sindico", 
    "custemail": "asdasdasd@gmail.com", 
    "custname": "Gulherme", 
    "custtel": "111111111111111", 
    "delivery": "16:30", 
    "size": "medium", 
    "topping": [
      "bacon", 
      "cheese"
    ]
  }
```

### Pergunta 3.1
> Qual o formato do corpo? Como esse formato codifica caracteres especiais (espaço, acentos)?

**Resposta:** "application/x-www-form-urlencoded". Funciona através de Percent-enconding (URL enconding).

### Pergunta 3.2
> Comparando **Request → WebForms** e **Request → Raw**: qual das duas corresponde literalmente aos bytes enviados no socket TCP?

**Resposta:** Request -> **Request → Raw**

### Pergunta 3.3 — Composer
> Envie manualmente via Composer um `POST` para `http://httpbin.org/post` com JSON. Registre a resposta. Qual campo do JSON confirma que o servidor interpretou o JSON?

**Captura de tela:** <img width="1917" height="1079" alt="image" src="https://github.com/user-attachments/assets/1827ef7a-4d03-4eb2-8526-50156dc22e81" />

**Response JSON (trecho relevante):**

```json
{"protocolo":"HTTP","versao":"1.1"}
```

**Resposta:** É o campo "json"

---

## Atividade 4 — Catálogo de status codes (`http://httpbin.org/...`)

**Captura de tela (lista do Fiddler com as 7 sessões):** 
<img width="1919" height="606" alt="Evidência 1 método GET status 200" src="https://github.com/user-attachments/assets/491086e8-d05a-4d16-9180-1d378434738a" /> <br> 

<img width="1346" height="567" alt="Evidência 2 método GET status 301" src="https://github.com/user-attachments/assets/281e6798-5e38-4054-86ab-863eecf0882a" /> <br>  

<img width="1170" height="503" alt="Evidência 3 método GET status 404" src="https://github.com/user-attachments/assets/cc7c2e44-7ab6-4d6b-9283-fdc7ae777a15" /> <br>  

<img width="1290" height="606" alt="Evidência 4 método GET status 418" src="https://github.com/user-attachments/assets/f7ba6e09-444e-49b9-bc89-a3d4714a9f20" /> <br>  

<img width="1180" height="542" alt="Evidência 5 método GET status 500" src="https://github.com/user-attachments/assets/31d1eac9-f3b1-4b9a-9d9b-7253854187fe" /> <br>   

<img width="1133" height="522" alt="Evidência 6 método GET status 503" src="https://github.com/user-attachments/assets/909ca78f-dc5f-42ed-8bd6-5acfebf67295" /> <br>   

<img width="996" height="552" alt="Evidência 7 método GET status 304" src="https://github.com/user-attachments/assets/22adf90f-36fe-4f3c-9f4d-abfdbc57f772" /> <br>  

| # | Método | URL | Status-line | `Content-Length` / `Transfer-Encoding` | Body presente? |
|---|--------|-----|-------------|-----------------------------------------|----------------|
| 1 | GET    | `http://httpbin.org/status/200` | HTTP/1.1 200 OK | Content-Length: 0 | não |
| 2 | GET    | `http://httpbin.org/redirect-to?status_code=301&url=/get` | HTTP/1.1 301 MOVED PERMANENTLY | Content-Length: 0 | não |
| 3 | GET    | `http://httpbin.org/status/404` | HTTP/1.1 404 NOT FOUND | Content-Length: 0 | não |
| 4 | GET    | `http://httpbin.org/status/418` | HTTP/1.1 418 I'M A TEAPOT | Content-Length: 135 | sim |
| 5 | GET    | `http://httpbin.org/status/500` | HTTP/1.1 500 INTERNAL SERVER ERROR | Content-Length: 0 | não |
| 6 | GET    | `http://httpbin.org/status/503` | HTTP/1.1 503 SERVICE UNAVAILABLE | Content-Length: 0 | não |
| 7 | GET    | `http://example.com/` com `If-Modified-Since` | HTTP/1.1 304 Not Modified | Ausente | não |

### Pergunta 4.1
> Em qual dos status o corpo está ausente/tamanho zero? Isso é obrigatório pela especificação ou depende do servidor?

**Resposta:** O corpo está ausente nos status `200`, `301`, `404`, `500`, `503` e `304`. 
Isso é **obrigatório pela especificação** apenas para o status `304 Not Modified` onde é proibido enviar corpo. Para os demais status citados (`200`, `301`, `404`, `500`, `503`), a especificação permite o envio de um corpo. Portanto, nesses casos, o tamanho zero **depende da implementação do servidor**.

### Pergunta 4.2
> No `301`, qual cabeçalho da resposta informa para onde ir? O que aconteceria se estivesse ausente?

**Resposta:** O cabeçalho responsável por informar o novo destino é o **`Location`**. Se ele estivesse ausente, o cliente HTTP (como o navegador) não saberia para qual URL seguir. O redirecionamento automático falharia e o usuário ficaria "preso" na resposta do 301 sem chegar ao destino final pretendido.

### Pergunta 4.3
> Diferença semântica entre `200`, `304` e `404` do ponto de vista do cache do navegador.

**Resposta:** 
* **200 (OK):** O servidor ignora o cache antigo (ou o cache não existia) e envia o recurso completo e atualizado, que o navegador pode salvar no cache para usos futuros.

* **304 (Not Modified):** O servidor valida que o recurso não sofreu alterações desde o último acesso. O navegador economiza rede ao não baixar o corpo da resposta e apenas carrega a versão que já estava guardada no seu cache local.

* **404 (NOT FOUND):** O servidor avisa que o recurso não existe na URL solicitada. O navegador entende que a busca falhou e não utiliza o cache para carregar a página (embora possa armazenar em cache o próprio "erro 404" por um curto período para evitar novas requisições inúteis ao mesmo link quebrado).

---

## Atividade 5 — Identificação de cabeçalhos (`http://httpbin.org/response-headers?...` + `/gzip`)

**Captura de tela (Inspectors → Headers):**
<img width="1914" height="1074" alt="response-headers" src="https://github.com/user-attachments/assets/353e26d3-576e-48dd-af94-1f1a29dcdfa2" /><br>

<img width="1919" height="1076" alt="gzip" src="https://github.com/user-attachments/assets/ca0c66a9-279c-4811-9e29-f28c2a6ce65d" /><br>

| Cabeçalho                    | Req/Resp | Valor capturado | Função em uma frase |
|------------------------------|----------|------------------|----------------------|
| `Host`                       | Req    | httpbin.org        | Especifica o domínio do servidor alvo da requisição. |
| `User-Agent`                 | Req    | Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:150.0) Gecko/20100101 Firefox/150.0 | Identifica o software cliente e o sistema operacional fazendo a requisição. |
| `Accept`                     | Req    | Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 | Informa ao servidor os tipos de mídia (formatos) que o cliente consegue entender. |
| `Accept-Encoding`            | Req    | gzip, deflate      | Indica quais algoritmos de compressão o cliente suporta para a resposta. |
| `Cookie`                     | Req    | teste=1            | Envia de volta ao servidor dados de estado previamente armazenados pelo cliente. |
| `Server`                     | Resp   | gunicorn/19.9.0    | Identifica o software do servidor web que gerou a resposta. |
| `Content-Type`               | Resp   | application/json   | Indica o tipo de mídia (formato) do corpo da mensagem que está sendo enviada. |
| `Content-Encoding`           | Resp   | gzip               | Indica qual algoritmo de compressão foi aplicado ao corpo da resposta. |
| `Set-Cookie`                 | Resp   | teste=1            | Instrução do servidor para o cliente armazenar um cookie em sua máquina. |
| `Cache-Control`              | Resp   | max-age=3600       | Define diretivas e regras sobre como a resposta deve ser armazenada em cache. |
| `Strict-Transport-Security`  | Resp   | Ausente            | Força o cliente a se comunicar com o servidor apenas via conexão segura (HTTPS). |

### Pergunta 5.1
> `Content-Encoding: gzip`/`br` apareceu? Compare `Content-Length`, quando presente, com o conteúdo visível. O que explica a diferença?

**Resposta:** Sim, na requisição para o endpoint `/gzip`, o servidor retornou `Content-Encoding: gzip`. O valor do `Content-Length` (tamanho em bytes transmitido pela rede) é menor do que o tamanho do texto visível na aba **TextView**. Isso ocorre porque o `Content-Length` representa o tamanho do arquivo **compactado** que trafegou na rede.

### Pergunta 5.2
> Cliente envia `Accept: application/json` mas o recurso só existe em `text/html`. Qual status code esperar?

**Resposta:** Segundo a especificação HTTP, se o servidor não for capaz de fornecer uma resposta com as características de formato aceitas pelo cliente (declaradas no cabeçalho `Accept`), ele deve retornar o código de status **`406 Not Acceptable`**. Isso indica ao cliente que o recurso existe, mas não no formato que o cliente declarou conseguir processar.

### Pergunta 5.3
> `Strict-Transport-Security` apareceu nas respostas HTTP? Por que esse cabeçalho está ausente neste fluxo? (Consulte a RFC 6797.) Qual é seu papel contra downgrades para HTTP puro?

**Resposta:** Não, o cabeçalho não apareceu. Ele está ausente porque todas as requisições destes testes foram feitas via protocolo **HTTP puro (não criptografado)** no endereço `http://httpbin.org`. Segundo a RFC 6797, os servidores não devem enviar o cabeçalho HSTS em conexões HTTP inseguras (e se enviarem, o cliente deve ignorar, pois a rede não é confiável). 
O papel do HSTS (HTTP Strict Transport Security) é proteger os usuários contra ataques de *downgrade* (como o SSL Stripping). Uma vez que o cliente recebe esse cabeçalho através de uma conexão HTTPS válida, ele memoriza que aquele domínio específico só pode ser acessado via HTTPS. A partir daí, qualquer tentativa futura de acessar o site via `http://` será bloqueada ou convertida automaticamente para `https://` pelo próprio navegador de forma interna, sem nem deixar a requisição insegura sair para a rede.

---

## Atividade 6 — HTTP vs HTTPS (análise sem decriptação)

**Captura de tela HTTP (`neverssl.com`):**
<img width="1910" height="1074" alt="neverssl.com" src="https://github.com/user-attachments/assets/1f17ab5d-0f7a-47a8-955f-267c6a7a60f1" /><br>

**Captura de tela HTTPS (`https://httpbin.org/get`, apenas CONNECT):**
<img width="1919" height="1079" alt="httpbin.org/get" src="https://github.com/user-attachments/assets/e6713213-d689-46a1-9e6e-c886815ff603" /><br>

### Pergunta 6.1
> Que método HTTP aparece na sessão do `https://httpbin.org/get`? O que ele faz e por que existe?

**Resposta:** O método que aparece é o CONNECT. Ele existe para permitir que o cliente solicite ao proxy a criação de um túnel TCP bidirecional direto com o servidor de destino (na porta 443). Uma vez que esse túnel é estabelecido (com a resposta 200 Connection Established), o proxy para de ler os cabeçalhos HTTP e passa a apenas repassar os bytes criptografados de um lado para o outro, sem saber o conteúdo da comunicação.

### Pergunta 6.2
> Tabela comparativa dos campos visíveis ao Fiddler em cada caso:

| Campo                          | Visível em HTTP? | Visível em HTTPS (sem decriptação)? |
|--------------------------------|------------------|-------------------------------------|
| Método                         | Sim              | Não (apenas o método CONNECT inicial) |
| URL completa (path + query)    | Sim              | Não                                   |
| Cabeçalhos de request          | Sim              | Não                                   |
| Corpo de request               | Sim              | Não                                   |
| Status code                    | Sim              | Não (apenas o 200 Connection Established do túnel) |
| Cabeçalhos de response         | Sim              | Não                                   |
| Corpo de response              | Sim              | Não (apenas bytes cifrados/ilegíveis) |
| Host (via SNI, no `CONNECT`)   | Sim              | Sim                                   |
| IP e porta de destino          | Sim              | Sim                                   |

### Pergunta 6.3 (teórica)
> O que você **veria** no Fiddler se tivesse privilégio de administrador e pudesse habilitar *Decrypt HTTPS traffic*? Indique telas/abas e justifique por que essa inspeção exige a instalação de um certificado raiz.

**Resposta:** Veríamos o tráfego HTTPS em texto claro, exatamente como ocorre no HTTP puro. A URL completa e os cabeçalhos apareceriam legíveis na aba Inspectors → Raw, e o corpo das requisições/respostas ficaria visível nas abas Response → JSON / TextView. Essa inspeção exige a instalação de um certificado raiz porque o Fiddler atua como um Man-in-the-Middle (MITM), interceptando a conexão e emitindo certificados "falsos" para cada site visitado. Para que o navegador aceite esses certificados falsos sem bloquear a navegação, o certificado raiz do Fiddler precisa ser explicitamente adicionado ao armazenamento de confiança do sistema operacional.

### Pergunta 6.4
> Por que a técnica de decriptação dos *debugging proxies* **não** funcionaria contra um usuário se um atacante a tentasse sem instalar o certificado?

**Resposta:** Porque a criptografia HTTPS baseia-se em uma cadeia de confiança. Sem a "cooperação do usuário" (que é a instalação voluntária do certificado raiz do proxy/atacante na máquina), o ataque falha. Se um atacante tentar interceptar a conexão e enviar seus certificados "falsos" sem essa raiz de confiança instalada, o navegador da vítima detectará imediatamente que o certificado não foi emitido por uma Autoridade Certificadora confiável e bloqueará o acesso à página exibindo um alerta severo de segurança, impedindo o vazamento de dados.

---

## Atividade 7 — Cookies e sessão (`http://httpbin.org/cookies/...`)

**Captura de tela da sequência:** `evidencias/atv7_cookies.png`

| # | URL | `Set-Cookie` recebido | `Cookie` enviado |
|---|-----|-----------------------|-------------------|
| 1 | `/cookies/set?...`       | [...] | [nenhum / ...] |
| 2 | `/cookies` (1ª visita)   | [...] | [...]          |
| 3 | `/cookies` (reload 1)    | [...] | [...]          |
| 4 | `/cookies` (reload 2)    | [...] | [...]          |

### Pergunta 7.1
> `Set-Cookie` aparece uma vez ou em toda requisição? Justifique.

**Resposta:** [...]

### Pergunta 7.2
> Que atributos o `Set-Cookie` trouxe? Explique cada um presente. Para atributos não observados, registre `não observado`.

**Resposta:**

| Atributo | Valor | Função |
|----------|-------|--------|
| [...]    | [...] | [...]  |

### Pergunta 7.3
> O atributo `Secure` pode aparecer num cookie recebido por HTTP puro? Qual seria o comportamento esperado? Relacione com o fato de que todo o tráfego desta atividade é visível em texto claro.

**Resposta:** [...]

### Pergunta 7.4
> Na aba **Inspectors → Cookies**, o cookie armazenado coincide com o campo `cookies` do JSON?

**Resposta:** [...]

---

## Atividade 8 — Manipulação com breakpoints

**Captura de tela da edição do User-Agent:** `evidencias/atv8_ua_edit.png`

**JSON de resposta após edição:**

```json
{
  "user-agent": "[valor forjado]"
}
```

### Pergunta 8.1
> O servidor pode detectar que o `User-Agent` foi forjado? Discuta.

**Resposta:** [...]

### Pergunta 8.2
> Após editar a status-line de `200 OK` para `404 Not Found`, o que o navegador exibe? Comente o papel do proxy como MITM.

**Captura de tela:** `evidencias/atv8_status_edit.png`

**Resposta:** [...]

### Pergunta 8.3
> Confirme que todos os breakpoints foram desabilitados.

- [ ] Breakpoints desabilitados ao final (Shift+F11)

---

## Atividade 9 — Redirecionamento HTTP → HTTPS

**Captura de tela:** `evidencias/atv9_redir.png`

**Status-line da resposta a `http://httpbin.org/redirect-to?status_code=301&url=https%3A%2F%2Fhttpbin.org%2Fget`:**

```http
[colar aqui, ex: HTTP/1.1 301 Moved Permanently]
```

**Cabeçalho `Location` da resposta:**

```
Location: [colar aqui]
```

### Pergunta 9.1
> Código de status e cabeçalho que direcionaram o navegador para `https://`.

**Resposta:** [...]

### Pergunta 9.2
> Além do redirecionamento 3xx, qual outro mecanismo/cabeçalho faz o navegador passar a forçar HTTPS em visitas futuras? Cite a RFC.

**Resposta:** [...]

### Pergunta 9.3
> Se esse cabeçalho fosse enviado por uma resposta servida via HTTP puro, o navegador deveria obedecer? Justifique com base na RFC.

**Resposta:** [...]

---

## Questões de Verificação

### 1. Ordem dos elementos em uma mensagem HTTP/1.1. O que separa cabeçalhos do corpo?

[resposta]

### 2. Por que `Host` é obrigatório em HTTP/1.1 mas era opcional em HTTP/1.0?

[resposta]

### 3. Diferença entre `401 Unauthorized` e `403 Forbidden`.

[resposta]

### 4. Um `POST` enviado duas vezes produz o mesmo efeito? E um `PUT`? Justifique em termos de idempotência.

[resposta]

### 5. Por que HTTPS permite ainda que um observador saiba qual site está sendo visitado? (SNI, DNS)

[resposta]

### 6. O que muda com `Content-Encoding: gzip`? Onde os dados são compactados e descompactados?

[resposta]

### 7. Impacto prático de `Cache-Control: no-store`.

[resposta]

### 8. Como um debugging proxy decifra HTTPS sem violar a criptografia, e por que isso exige cooperação do usuário (e por que, justamente, você não pôde executar essa etapa)?

[resposta]

### 9. Exemplo de cabeçalho de request que o navegador envia automaticamente, sem a página pedir.

[resposta]

### 10. Se fosse automatizar a inspeção via script, qual ferramenta alternativa escolheria? Por quê?

[resposta]

### 11. (Exclusiva do Fluxo B) Três cabeçalhos de segurança que não aparecem ou não fazem sentido em respostas HTTP puro. Para cada um, o que aconteceria se enviado por um servidor HTTP? (Cite RFC 6797 para HSTS.)

**Resposta:**

| Cabeçalho | Comportamento esperado sobre HTTP | Referência |
|-----------|-----------------------------------|-----------|
| [...]     | [...]                             | [...]     |
| [...]     | [...]                             | [...]     |
| [...]     | [...]                             | [...]     |

---

## Reflexão final (opcional, até 10 linhas)

> O que você aprendeu que não conhecia antes deste laboratório? Há algum
> cabeçalho, código de status ou comportamento que passou a olhar com
> mais atenção? Alguma dificuldade que recomendaria evitar para a próxima turma?

[reflexão]

---

## Encerramento — justificativa de segurança (Fluxo B)

**Parágrafo: por que a remoção de certificado é dispensável neste fluxo e por que seria obrigatória para o aluno administrador:**

[redigir, em até 5 linhas, com base na seção 4.6 do readme.md]

- [ ] HTTPS-First Mode / HTTPS-Only Mode reabilitado no navegador
- [ ] Fiddler / mitmproxy / HTTP Toolkit fechado (porta de proxy liberada)
- [ ] Configuração de proxy removida do navegador (se aplicável)

---

## Checklist de entrega

- [ ] Todos os campos `[...]` substituídos
- [ ] Pasta `evidencias/` com capturas nomeadas por atividade (incluindo Atv. 9)
- [ ] 11 questões de verificação respondidas
- [ ] Atividade 9 (redirecionamento HTTP→HTTPS) documentada
- [ ] Justificativa de encerramento redigida
- [ ] Arquivo compactado como `NOME_RA_LAB_HTTP_FLUXOB.zip`
- [ ] Submetido no Microsoft Teams dentro do prazo
