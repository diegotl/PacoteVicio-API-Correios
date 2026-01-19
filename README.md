# 📦 API PacoteVício - Rastreamento de Encomendas Correios, AliExpress, Shopee Xpress, Anjun Express e J&T Express

Documentação da API PacoteVício para rastreamento de objetos dos Correios do Brasil, pacotes do AliExpress, Shopee Xpress, Anjun Express e J&T Express.
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

| Serviço | Endpoint | Observações |
|---|---|---|
| Correios | `/correios` | Rastreamento dos Correios do Brasil |
| AliExpress | `/aliexpress` | Rastreamento de pacotes AliExpress |
| Shopee Xpress | `/shopee` | Rastreamento de pacotes Shopee Xpress |
| Anjun Express | `/anjun` | Rastreamento de pacotes Anjun Express |
| J&T Express | `/jtexpress` | Rastreamento de pacotes J&T Express |

### Parâmetros Comuns

Todos os endpoints acima aceitam os mesmos parâmetros (exceto onde notado):

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `tracking_code` | string | Sim | Código de rastreamento do pacote. Aceita diversos formatos internacionais. |
| `confidence_level`| string | Não | Nível de confiança para tentativas de rastreamento em caso de falha. Valores: `low`, `medium`, `high`. Padrão: `high`. |
| `language` | string | Não | Idioma da resposta. Valores: `pt-BR`, `en-US`, `fr-FR`, `zh-CN`. Padrão: `en-US`. |
| `document` | string | Sim* | Obrigatório **apenas** para J&T Express. Deve conter o CPF do destinatário (apenas números). |

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

Troque `/correios` por `/aliexpress`, `/shopee`, `/anjun` ou `/jtexpress` conforme o serviço desejado.

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
    "lastTrackStatus": "signed",
    "nodeDataList": [
        {
            "address": "Sorocaba / SP",
            "codeNumber": null,
            "collectType": null,
            "content": null,
            "dateTime": "21-01-2025 09:22:13",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "14",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": "Objeto entregue pelo próprio",
            "signType": 1,
            "signTypeName": "Objeto entregue pelo próprio",
            "siteName": null,
            "status": "O pacote foi assinado para",
            "statusCode": "signed",
            "statusDetail": null
        },
        {
            "address": "Sorocaba / SP",
            "codeNumber": null,
            "collectType": null,
            "content": null,
            "dateTime": "14-01-2025 05:56:55",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "13",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": null,
            "status": "Objeto saiu para entrega ao destinatário",
            "statusCode": "delivering",
            "statusDetail": null
        },
        {
            "address": "Sorocaba / SP",
            "codeNumber": null,
            "collectType": null,
            "content": null,
            "dateTime": "13-01-2025 14:00:16",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "13",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": null,
            "status": "Objeto saiu para entrega ao destinatário",
            "statusCode": "delivering",
            "statusDetail": null
        },
        {
            "address": "Sorocaba / SP",
            "codeNumber": null,
            "collectType": null,
            "content": null,
            "dateTime": "13-01-2025 08:24:46",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "13",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": null,
            "status": "Objeto saiu para entrega ao destinatário",
            "statusCode": "delivering",
            "statusDetail": null
        },
        {
            "address": "Sorocaba / SP",
            "codeNumber": null,
            "collectType": null,
            "content": null,
            "dateTime": "13-01-2025 08:23:22",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "13",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": null,
            "status": "Objeto saiu para entrega ao destinatário",
            "statusCode": "delivering",
            "statusDetail": null
        },
        {
            "address": "Sorocaba / SP",
            "codeNumber": "Sorocaba / SP",
            "collectType": null,
            "content": null,
            "dateTime": "10-01-2025 14:25:10",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "12",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": "SP-R-D086",
            "status": "Objeto chegou ao ponto de entrega",
            "statusCode": "delivery_site_in_storage",
            "statusDetail": null
        },
        {
            "address": "São Paulo / SP",
            "codeNumber": 9,
            "collectType": null,
            "content": null,
            "dateTime": "10-01-2025 08:37:05",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "09",
            "nextSiteName": "SP-R-D086",
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": null,
            "status": "Objeto saiu do CD",
            "statusCode": "transfer_out_storage",
            "statusDetail": null
        },
        {
            "address": "São Paulo / SP",
            "codeNumber": 7,
            "collectType": null,
            "content": null,
            "dateTime": "10-01-2025 05:47:09",
            "deliverdNamePhoto": null,
            "deliverdPhoto": null,
            "localTrackCode": "08",
            "nextSiteName": null,
            "operateUserName": null,
            "problemType": "",
            "signDesc": null,
            "signType": null,
            "signTypeName": null,
            "siteName": null,
            "status": "Transferência e armazenagem",
            "statusCode": "transfer_in_storage",
            "statusDetail": null
        }
    ],
    "providerNumber": null,
    "trackNo": "AJ250101341570001"
}
```
</details>

<details>
<summary><strong>Exemplo de resposta - J&T Express</strong></summary>

```json
{
    "keyword": "888030556767025",
    "details": [
        {
            "scanTime": "2026-01-17 12:55:57",
            "scanTypeName": "快件签收",
            "customerTracking": "[Teresina] O pacote foi assinado! O signatário é [Recebimento por terc], se você tiver alguma dúvida, entre em contato com: 0800XXXXXXX, se você tiver algum problema ou reclamação, ligue para o número da filial: 0800XXXXXXX",
            "status": "Pedido Entregue",
            "sigPicUrl": "aza180low1825del/lite-ylappbc/SIGNING_SCAN_LIST/20260117/880495994808124166_861674_110_20260117125543742_0649.jpeg,aza180low1825del/lite-ylappbc/SIGNING_SCAN_LIST/20260117/880496046121240381_861674_110_20260117125555976_7052.jpeg",
            "electronicSignaturePicUrl": "https://pro-jmsbr-file.jtjms-br.com/aza180low1825del/lite-ylappbc/SIGNING_SCAN_LIST/20260117/880495954601526018_861674_110_20260117125534125_0254.jpeg?sv=2023-11-03&spr=https&se=2026-01-20T08%3A57%3A48Z&sr=b&sp=r&sig=FaWbbPP%2BybdKEbFcJyZ3b26BCmYJCoUhnLoAxJ0a8lI%3D",
            "signList": [
                "https://pro-jmsbr-file.jtjms-br.com/aza180low1825del/lite-ylappbc/SIGNING_SCAN_LIST/20260117/880495994808124166_861674_110_20260117125543742_0649.jpeg?sv=2023-11-03&spr=https&se=2026-01-20T08%3A57%3A48Z&sr=b&sp=r&sig=SHPFfkcqXMbbVbKK4NpvUA1Kkp1yMTbMQuuyYmYmnRc%3D",
                "https://pro-jmsbr-file.jtjms-br.com/aza180low1825del/lite-ylappbc/SIGNING_SCAN_LIST/20260117/880496046121240381_861674_110_20260117125555976_7052.jpeg?sv=2023-11-03&spr=https&se=2026-01-20T08%3A57%3A48Z&sr=b&sp=r&sig=TtdTY5G%2FNMrz%2FCgQo8%2FudrAoDSvCOgoAZAir3GWbbH4%3D"
            ],
            "code": 100,
            "remark1": "Recebimento por terc",
            "remark2": "1"
        },
        {
            "scanTime": "2026-01-16 21:29:26",
            "scanTypeName": "出仓扫描",
            "customerTracking": "O entregador [Teresina] [THE 02-PI], M****A está enviando a encomenda. Se você tiver algum problema ou reclamação, ligue para o número da filial: 0800XXXXXXX",
            "status": "Em Rota de Entrega",
            "code": 94
        },
        {
            "scanTime": "2026-01-12 23:01:34",
            "scanTypeName": "快件揽收",
            "customerTracking": "[Guarulhos] [PA PEGAKI-GRU-SP] Seu entregador L****s retirou a encomenda. Se você tiver qualquer problema ou reclamação, ligue para o número da filial: 0800XXXXXXX",
            "status": "Coletado",
            "code": 10
        }
    ]
}
```
</details>
