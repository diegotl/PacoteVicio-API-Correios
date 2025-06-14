# 📦 API PacoteVício - Rastreamento de Encomendas Correios, AliExpress, Shopee Xpress e Anjun Express

Documentação da API PacoteVício para rastreamento de objetos dos Correios do Brasil, pacotes do AliExpress, Shopee Xpress e Anjun Express.
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

A API suporta múltiplos serviços de rastreamento, todos com a mesma estrutura de requisição. Basta alterar o endpoint conforme o serviço desejado:

| Serviço         | Endpoint           | Observações                                 |
|-----------------|-------------------|---------------------------------------------|
| Correios        | `/correios`       | Rastreamento dos Correios do Brasil         |
| AliExpress      | `/aliexpress`     | Rastreamento de pacotes AliExpress          |
| Shopee Xpress   | `/shopee`         | Rastreamento de pacotes Shopee Xpress       |
| Anjun Express   | `/anjun`          | Rastreamento de pacotes Anjun Express       |

### Parâmetros Comuns

Todos os endpoints acima aceitam os mesmos parâmetros:

| Parâmetro         | Tipo   | Obrigatório | Descrição                                                                                   |
|-------------------|--------|-------------|---------------------------------------------------------------------------------------------|
| `tracking_code`   | string | Sim         | Código de rastreamento do pacote. Aceita diversos formatos internacionais.                  |
| `confidence_level`| string | Não         | Nível de confiança para tentativas de rastreamento em caso de falha. Valores: `low`, `medium`, `high`. Padrão: `high`. |
| `language`        | string | Não         | Idioma da resposta. Valores: `pt-BR`, `en-US`, `fr-FR`, `zh-CN`. Padrão: `en-US`. |

> **Nota:** O parâmetro `language` só é aceito para AliExpress.

#### Sobre `confidence_level`

Este parâmetro define o nível de esforço da API para tentar obter uma resposta dos Correios em situações de instabilidade do mesmo.

- `low`: não haverá novas tentativas. Garante resposta no pior cenário que não ultrapassará ~10 segundos, ao custo de uma menor chance de sucesso.
- `medium`: serão feitas algumas tentativas. No pior cenário levará ~20 segundos e uma chance maior de sucesso.
- `high`: mais tentativas serão feitas. No pior cenário pode demorar até ~30 segundos, mas tem maior chance de retorno com sucesso.

Escolha e ajuste o timeout de seu cliente conforme a necessidade da sua aplicação. Se o parâmetro for omitido, o valor padrão será `high`.

### Exemplo de Requisição com cURL

```bash
curl -X GET "https://api.pacotevicio.dev/correios?tracking_code=AM101610575BR" \
  --header "X-RapidAPI-Key: SUA_CHAVE_DE_API"
```

Troque `/correios` por `/aliexpress`, `/shopee` ou `/anjun` conforme o serviço desejado.

---

## 📋 Resposta

<details>
<summary><strong>Exemplo de resposta - Correios</strong></summary>

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
          "uf": "PE"
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
</details>

<details>
<summary><strong>Exemplo de resposta - AliExpress</strong></summary>

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
</details>

<details>
<summary><strong>Exemplo de resposta - Shopee</strong></summary>

```json
{
    "sls_tracking_number": "BR2561249217932",
    "need_translate": 0,
    "delivery_type": "SHOPEE_CREDIT",
    "recipient_name": "",
    "phone": "",
    "current_status": "Delivered",
    "tracking_list": [
        {
            "timestamp": 1749140169,
            "status": "Delivered",
            "message": "[LM Hub_MG_Uberlândia] Your parcel has been delivered [Fulano da Silva] [ Receptionist]"
        },
        {
            "timestamp": 1749122864,
            "status": "Delivering",
            "message": "[LM Hub_MG_Uberlândia] Your parcel is being delivered by courier"
        },
        {
            "timestamp": 1749089703,
            "status": "LMHub_Received",
            "message": "[LM Hub_MG_Uberlândia] Your parcel has been received by delivery hub"
        },
        {
            "timestamp": 1749034791,
            "status": "SOC_LHTransporting",
            "message": "Parcel [TO202506041ZAJ7] transporting to [LM Hub_MG_Uberlândia]"
        },
        {
            "timestamp": 1748937958,
            "status": "SOC_Received",
            "message": "[SoC_SP_Santana] Your parcel has been received by sorting center"
        },
        {
            "timestamp": 1748906285,
            "status": "SOC_Pickup_Done",
            "message": "[SoC_SP_Santana] Your parcel has been picked up"
        },
        {
            "timestamp": 1748904099,
            "status": "DOP_Received",
            "message": "Your parcel has been received by drop off point"
        },
        {
            "timestamp": 1748893392,
            "status": "Created",
            "message": "Order has been created"
        }
    ],
    "status_list": [
        {
            "timestamp": 1748893392,
            "code": 1,
            "text": "Created",
            "state_ls": "Created",
            "icon": "Order Created"
        },
        {
            "timestamp": 1748937958,
            "code": 1,
            "text": "Pending_Receive",
            "state_ls": "Pending_Receive",
            "icon": "Picked Up"
        },
        {
            "timestamp": 1749089703,
            "code": 1,
            "text": "Pending",
            "state_ls": "Pending",
            "icon": "Sorting"
        },
        {
            "timestamp": 1749122864,
            "code": 1,
            "text": "Assigned",
            "state_ls": "Assigned",
            "icon": "Courier Delivery"
        },
        {
            "timestamp": 1749140169,
            "code": 1,
            "text": "Delivered",
            "state_ls": "Delivered",
            "icon": "Delivered"
        }
    ]
}
```
</details>

<details>
<summary><strong>Exemplo de resposta - Anjun</strong></summary>

```json
{
    "clCollectOrder": {
        "scan_time": null,
        "bag_sealing_time": null,
        "signer_time": null
    },
    "movementInTheSorter": null,
    "shippingCompany": [
        {
            "date": "2025-01-21T12:22:13.000Z",
            "status": "O pacote foi assinado para",
            "signTypeName": "Objeto entregue pelo próprio",
            "signType": 0,
            "address": "Sorocaba / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-14T08:56:55.000Z",
            "status": "Objeto saiu para entrega ao destinatário",
            "signTypeName": null,
            "signType": null,
            "address": "Sorocaba / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-13T17:00:16.000Z",
            "status": "Objeto saiu para entrega ao destinatário",
            "signTypeName": null,
            "signType": null,
            "address": "Sorocaba / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-13T11:24:46.000Z",
            "status": "Objeto saiu para entrega ao destinatário",
            "signTypeName": null,
            "signType": null,
            "address": "Sorocaba / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-13T11:23:22.000Z",
            "status": "Objeto saiu para entrega ao destinatário",
            "signTypeName": null,
            "signType": null,
            "address": "Sorocaba / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-10T17:25:10.000Z",
            "status": "Objeto chegou ao ponto de entrega",
            "signTypeName": null,
            "signType": null,
            "address": "Sorocaba / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-10T11:37:05.000Z",
            "status": "Objeto saiu do CD",
            "signTypeName": null,
            "signType": null,
            "address": "São Paulo / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-10T08:56:52.000Z",
            "status": "O pacote unitizada está finalizada",
            "signTypeName": null,
            "signType": null,
            "address": "São Paulo / SP",
            "remark": "",
            "limiteDate": ""
        },
        {
            "date": "2025-01-10T08:47:09.000Z",
            "status": "Transferência e armazenagem",
            "signTypeName": null,
            "signType": null,
            "address": "São Paulo / SP",
            "remark": "",
            "limiteDate": ""
        }
    ]
}
```
</details>
