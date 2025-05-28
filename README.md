# 📦 API PacoteVício - Rastreamento de Encomendas Correios e AliExpress

Documentação da API PacoteVício para rastreamento de objetos dos Correios do Brasil e pacotes do AliExpress.
Veja mais informações na [página oficial da API PacoteVício](http://pacotevicio.dev).

## 🔗 Acesso à API

A API é fornecida através da plataforma RapidAPI.
Oferecemos um plano gratuito com até 1.000 requisições/mês, o que deve atender à maioria das necessidades.
- [Página no RapidAPI](https://rapidapi.com/pacotevicio-pacotevicio-default/api/correios-rastreamento-de-encomendas)

## 🛠️ Como Utilizar

### 1. Obter uma Chave de API

Para utilizar esta API, é necessário obter uma chave de API através do RapidAPI:

1. Acesse o [RapidAPI](https://rapidapi.com/pacotevicio-pacotevicio-default/api/correios-rastreamento-de-encomendas)
2. Escolha o plano desejado (para iniciar recomendamos o BASIC que é gratuito)
3. Crie uma conta caso seja necessário
4. Sua chave de API (X-RapidAPI-Key) estará disponível na [área de testes](https://rapidapi.com/pacotevicio-pacotevicio-default/api/correios-rastreamento-de-encomendas/playground/apiendpoint_19d15e2c-d3a9-422f-9da1-05881c97f70d)


## 💻 Endpoints Disponíveis

### 2.1. Correios - `/correios`

Utilize este endpoint para consultar o status de encomendas dos Correios do Brasil.

#### Exemplo de Requisição com cURL:

```bash
curl -X GET "https://api.pacotevicio.dev/correios?tracking_code=AM101610575BR" \
  --header "X-RapidAPI-Key: SUA_CHAVE_DE_API"
```

#### Parâmetros - Correios

| Parâmetro         | Tipo   | Obrigatório | Descrição                                                                                     |
|-------------------|--------|-------------|-----------------------------------------------------------------------------------------------|
| `tracking_code`   | string | Sim         | Código de rastreamento do objeto. Deve conter 13 caracteres: 2 letras iniciais, 9 números e 2 letras finais. |
| `confidence_level`| string | Não         | Nível de confiança para tentativas de rastreamento em caso de falha. Valores possíveis: `low`, `medium`, `high`. Padrão: `high`. |

##### Sobre `confidence_level`

Este parâmetro define o nível de esforço da API para tentar obter uma resposta dos Correios em situações de instabilidade do mesmo.

- `low`: não haverá novas tentativas. Garante resposta no pior cenário que não ultrapassará ~10 segundos, ao custo de uma menor chance de sucesso.
- `medium`: serão feitas algumas tentativas. No pior cenário levará ~20 segundos e uma chance maior de sucesso.
- `high`: mais tentativas serão feitas. No pior cenário pode demorar até ~30 segundos, mas tem maior chance de retorno com sucesso.

Escolha e ajuste o timeout de seu cliente conforme a necessidade da sua aplicação. Se o parâmetro for omitido, o valor padrão será `high`.

### 2.2. AliExpress - `/aliexpress`

Utilize este endpoint para consultar o status de pacotes do AliExpress.

#### Exemplo de Requisição com cURL:

```bash
curl -X GET "https://api.pacotevicio.dev/aliexpress?tracking_code=LP00123456789CN" \
  --header "X-RapidAPI-Key: SUA_CHAVE_DE_API"
```

#### Parâmetros - AliExpress

| Parâmetro         | Tipo   | Obrigatório | Descrição                                                                                   |
|-------------------|--------|-------------|---------------------------------------------------------------------------------------------|
| `tracking_code`   | string | Sim         | Código de rastreamento do pacote AliExpress. Aceita diversos formatos de código de rastreamento internacional. |
| `confidence_level`| string | Não         | Nível de confiança para tentativas de rastreamento em caso de falha. Valores possíveis: `low`, `medium`, `high`. Padrão: `high`. |
| `language`        | string | Não         | Idioma da resposta. Valores possíveis: `pt-BR` (Português), `en-US` (Inglês), `fr-FR` (Francês), `zh-CN` (Chinês). Padrão: `en-US`. |

## 📋 Resposta

### 3.1. Resposta - Correios

A resposta será um JSON contendo o status da encomenda dos Correios.

#### Exemplo (simplificado):

```json
{
  "codObjeto": "AM101610575BR",
  "tipoPostal": {
    "sigla": "AM",
    "descricao": "ETIQUETA LOGICA PAC",
    "categoria": "ENCOMENDA PAC",
    "tipo": "N"
  },
  "dtPrevista": "20/03/2025",
  "modalidade": "F",
  "eventos": [
    {
      "codigo": "BDE",
      "tipo": "01",
      "dtHrCriado": {
        "date": "2025-03-03 23:30:03.000000",
        "timezone_type": 3,
        "timezone": "America/Sao_Paulo"
      },
      "descricao": "Objeto entregue ao destinatário",
      "unidade": {
        "codSro": "50630977",
        "tipo": "Unidade de Tratamento",
        "endereco": {
          "cidade": "Recife",
          "uf": "PE",
        }
      },
      "unidadeDestino": null,
      "descricaoFrontEnd": "ENTREGUE",
      "finalizador": "S",
      "rota": "CONTEXTO",
      "descricaoWeb": "ENTREGUE",
      "detalhe": "Nossa entrega atendeu às suas expectativas? Conte pra gente: https://survey3.medallia.com/?correios-nps-sms-sro&obj=AM101610575BR",
    },
    {
      "codigo": "PO",
      "tipo": "09",
      "dtHrCriado": {
        "date": "2025-02-24 15:51:29.000000",
        "timezone_type": 3,
        "timezone": "America/Sao_Paulo"
      },
      "descricao": "Objeto postado após o horário limite da unidade",
      "unidade": {
        "codSro": "65995970",
        "tipo": "Agência dos Correios",
        "endereco": {
          "cidade": "Feira Nova do Maranhao",
          "uf": "MA",
        }
      },
      "unidadeDestino": null,
      "descricaoFrontEnd": "Postado depois do horário",
      "finalizador": "N",
      "rota": "NORMAL",
      "descricaoWeb": "POSTAGEM",
      "detalhe": "Sujeito a encaminhamento no próximo dia útil",
    }
  ],
  "situacao": "E",
  "autoDeclaracao": false,
  "encargoImportacao": false,
  "percorridaCarteiro": false,
  "bloqueioObjeto": false,
  "arEletronico": false,
  "atrasado": false
}
```

### 3.2. Resposta - AliExpress

A resposta será um JSON contendo o status do pacote AliExpress.

#### Exemplo (simplificado):

```json
{
    "mailNo": "LP00123456789CN",
    "originCountry": "Mainland China",
    "destCountry": "Brazil",
    "status": "CLEAR_CUSTOMS",
    "statusDesc": "In customs ",
    "mailNoSource": "AE",
    "globalEtaInfo": {
        "etaDesc": "Estimated delivery by",
        "deliveryMinTime": 1749006268984,
        "deliveryMaxTime": 1750475068984
    },
    "detailList": [
        {
            "time": 1748410407000,
            "timeStr": "2025-05-28 13:33:27",
            "desc": "",
            "standerdDesc": "Import customs clearance complete",
            "descTitle": "Carrier note:",
            "timeZone": "GMT-3",
            "actionCode": "CC_IM_SUCCESS"
        },
        {
            "time": 1747839077000,
            "timeStr": "2025-05-21 22:51:17",
            "desc": "",
            "standerdDesc": "[Shatian Town] Processing at sorting center",
            "descTitle": "Carrier note:",
            "timeZone": "GMT+8",
            "actionCode": "SC_INBOUND_SUCCESS"
        },
        {
            "time": 1747805584000,
            "timeStr": "2025-05-21 13:33:04",
            "desc": "",
            "standerdDesc": "Received by logistics company",
            "descTitle": "Carrier note:",
            "timeZone": "GMT+8",
            "actionCode": "PU_PICKUP_SUCCESS"
        }
    ],
    "daysNumber": "8\tday(s)"
}
```
