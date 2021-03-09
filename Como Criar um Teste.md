# Como criar um teste de COVID
---

### Resumo

Para criar um teste é necessário primeiramente criar uma `ordem de pagamento`. Esta ordem de pagamento - também chamado de `pedido` - retorna um código, pelo qual você poderá criar ordens de logística.

Após criar uma ordem de logística, informamos através de uma `função de callbak` quais testes podem ser criados e a lista de barcodes que podem ser associadas. Estes barcodes são as identificações únicas dos testes.

Ao final do processo enviamos o resultado do teste através de uma função de callback, identificado pelo barcode do teste.

### Criar uma ordem de pagamento

A criação de uma ordem de pagamento é o primeiro passo para criar um teste de COVID da mendelics. Esta ordem de pagamento é necessária para a contabilidade de testes contratuatilizados e a prestação de contas via Equipe Financeira da Mendelics.

Para ver detalhes de como criar criar uma ordem de pagamento, abra [o link](https://endpointsportal.api-mendelics-dev.cloud.goog/docs/api-dev-1zjrwr95p92zz.apigateway.api-mendelics-dev.cloud.goog/0/routes/erp/payments/insert/post) com os detalhes do respectivo endpoint. 

Ao final da criação de uma ordem de pagamento você está habilitado a criar uma ordem de logística. Ordens de logística só são criadas com uma ordem de pagamento associada.

### Criar uma ordem de logística

Uma ordem de logística é necessária para que a Equipe de Logística da Mendelics possa alocar e enviar os tubos necessários para que você colete as amostras dos testes a serem criados.

Para ver detalhes de como criar uma ordem de logística, abra [o link](https://endpointsportal.api-mendelics-dev.cloud.goog/docs/api-dev-1zjrwr95p92zz.apigateway.api-mendelics-dev.cloud.goog/0/routes/logistica/order/request_from_covid_payment/post) com os detalhes do respecitvo endpoint.

Após a criação da ordem de logística, a Mendelics iniciará o processo de alocação de material para o requisitante. Este processo pode demorar um pouco, porém assim que encerrado você será notificado através de uma função de callback. Nesta função você receberá a lista com os códigos de tubo.

A função de callback enviará um request contendo um BODY com o seguinte formato JSON:

```
{
    "logistica_codigo": "<logistica_codigo>",
    "barcodes": ["<barcode1>", "<barcode2>", ..., "<barcode3>"],
    "observacao": "",
}
```

O campo `logistica_codigo` indica o identificador único desta ordem de logística. O campo `barcodes` é uma lista de strings contendo uma série de códigos únicos de material alocados para você. O campo `observacao` contém detalhes extras que o time de Logística pode dar sobre sua ordem de logística, mas em geral virá vazio.

**Detalhe importante: a notificação via callback pode acontecer mais de uam vez para o mesmo código de logística. É importante considerar apenas a notificação mais recente.**

### Criar um teste

Ao receber uma lista de barcodes via callback da ordem de logística você estará autorizado apra criar um teste de covid para cada um destes barcodes. Para ver detalhes sobre como criar um teste de covid entre no [link do endpoint de criação de teste](https://endpointsportal.api-mendelics-dev.cloud.goog/docs/api-dev-1zjrwr95p92zz.apigateway.api-mendelics-dev.cloud.goog/0/routes/diagnostics/tests/create/post).

**Detalhe importante: A criação de um teste com este barcode precisa ser feita antes de enviar a amostra para nosso laboratório. Caso uma amostra chegue sem cadastro prévio a Equipe de Triagem invalidará a amostra.**


### Recebendo os resultados


Após criar e enviar o teste para a Mendelics, o laboratório analisará o conteúdo e enviará o resultado através de uma função de callback. A função de callback enviará um request contendo um BODY com o seguinte formato JSON:

```
{
    "sample_code": "<barcodeX>",
    "type": "<type>",
    "result": "<result>",
}
```

O campo "sample_code" possui um dos barcodes enviados por você. O campo "type" indica o tipo do teste criado(podendo ser `covid` ou `covid_whitelabel`) e o campo "result" recebe um valor de três possíveis: `positive`(COVID detectado), `negative`(COVID não detectado), `non_compliance`(Problema com a amostra).
