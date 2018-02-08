# webhook-eduzz
O webhook permite que você receba notificações a cada alteração de status de uma compra. Sempre que uma compra tiver seu status atualizado ( Pago, Cancelado, Reembolsado, etc... ) nós iremos enviar uma chamada via POST para uma URL pré-configurada nas configurações do seu conteúdo com alguns parâmetros para que você possa realizar suas decisões comerciais.

Consideramos como **sucesso** todas as requisições que retornam o [status HTTP 200](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 

## Lista de parâmetros

Parâmetro     | Descrição	| Tipo de Variável
------------- | -------------	| -----------------
api_key | Token de segurança do produtor ou afiliado | String
trans_cod     | ID da Fatura na Eduzz ( Venda ) | Int
trans_value | Valor da compra | Float
trans_paid    | Valor pago | Float
trans_status  | Status do pagamento ( Ver tabela de status ) | Int
trans_paymentmethod | Forma de pagamento ( Ver tabela de forma de pagamento ) | Int
trans_createdate | Data de Criação da Fatura | String
trans_createtime | Hora de Criação da Fatura | String
trans_paiddate | Data de Pagamento da Fatura | String
trans_barcode | Código de Barra do Boleto | Int
trans_currency | Moeda Utilizada na Transação | String
trans_duedate | Data Limite para o Pagamento da Fatura | String
trans_duetime | Horário Limite para o Pagamento da Fatura | String
trans_paiddate | Data de Pagamento | String
trans_paidtime | Hora de Pagamento | String
product_cod   | ID do produto ( Conteúdo ) | Int
product_name | Nome do produto | String
product_refund | Prazo em Dias para Reembolso | Int
cus_cod	| ID do Cliente na Eduzz | Int
cus_taxnumber | Documento do Cliente | String
cus_name | Nome do Cliente | String
cus_email | E-mail do Cliente | String
cus_tel | Telefone 1 do Cliente | String
cus_tel2 | Telefone 2 do Cliente | String
cus_cel | Celular do Cliente | String
cus_address | Endereço do Cliente | String
cus_address_number | Número do Endereço do cliente | String
cus_address_country | País do Cliente | String
cus_address_district | Bairro do Cliente | String
cus_address_comp | Complemento do Cliente | String
cus_address_city | Cidade do Cliente  | String
cus_address_state | Estado do Cliente | String
cus_address_zip_code | CEP do Cliente | String
aff_cod | ID do Afiliado na Eduzz | Int
aff_document_number | CPF/CNPJ do Afiliado | String
aff_name | Nome do Afiliado | String
aff_email | E-mail do Afiliado | String
aff_value | Valor da comissão do afiliado  | Float
billet_url | Página com a chave gerada para o produto | String
page_checkout_url | Página do checkout do produto | String
tracker_trk | Parâmetro genérico 1 | String 
tracker_trk2 | Parâmetro genérico 2 | String
tracker_trk3 | Parâmetro genérico 3 | String
pro_document_number | Documento do Produtor (CPF/CNPJ) | Int
pro_email | E-mail do Produtor | String
pro_name | Nome do Produtor | String
pro_value | Comissão Paga ao Produtor | Float
recurrence_cod | ID do Contrato | Int
recurrence_count | Quantidade de Cobranças já Realizadas | Int
recurrence_interval | Intervalo da Recorrência | Int
recurrence_interval_type | Tipo de Intervalo | String
recurrence_plan | Nome do Plano da Recorrência | String
recurrence_startdate | Data de Inicio da Cobrança | String
recurrence_status | ID do Status do Recorrência | Int
recurrence_status_name | Nome do Status do Contrato | String
    



## Tabela de Status da Fatura

ID  | Status | Descrição
--- | ------ | -----------
1 | Aberta | Fatura aberta, cliente gerou boleto, mas ainda não foi compensado 
3 | Paga | Compra foi paga, o cliente já esta apto a receber o produto 
4 | Cancelada | Fatura Cancelada pela Eduzz
6 | Aguardando Reembolso | Cliente solicitou reembolso, porem o mesmo ainda não foi efetuado
7 | Reembolsado | Cliente já foi reembolsado pela eduzz
8 | Em Análise | Cliente efetuou o pagamento, porém esta em análise pela instituição financeira
11 | Em Recuperacao | Fatura entrou para o processo de recuperação

## Tabela de Status da Recorrência/Contrato

ID  | Status | Descrição
--- | ------ | -----------
1 | Em Dia | No momento em que a parcela da assinatura for paga.
2 | Aguardando Pagamento | Caso o cliente não pague a assinatura até o vencimento. Esse Status permanece durante 3 dias e o cliente é cobrado diariamente ainda com acesso.
3 | Suspenso | A qualquer momento o cliente ou produtor podem suspender uma assinatura, podendo reativá-la. O cliente não será cobrado.
4 | Cancelado | A qualquer momento o cliente ou produtor podem cancelar uma assinatura. Nesta etapa não é possível reativar a assinatura.
7 | Atrasado | Após 3 dias sem pagamento, os dados de acesso do cliente são interrompidos até o próximo pagamento.
9 | Finalizado | Após ser concluída uma assinatura com prazo de validade.
10 | Trial | É um período de experiência no início da assinatura. Esta etapa é opcional e determinada pelo Produtor.

## Tabela de Tipo de Intervalo da Recorrência/Contrato

ID  | Descrição
--- | -----------
day | Diária
month | Mensal
year | Anual


## Tabela de Formas de Pagamento
ID	| Forma de Pagamento
----	| -----
1 	| Boleto Bancário
9 	| Paypal
13 	| Visa
14	| Amex
15 	| Mastercard
16 	| Diners
17 	| Débito Banco do Brasil
18 	| Débito Bradesco
19 	| Débito Itaú
21 	| Hipercard
22 	| Débito Banrisul
23 	| Hiper
24 	| Elo
25 	| Paypal Internacional


## Exemplo PHP
```php
<?php

	//Exemplo webhook Eduzz

	foreach ($_POST as $key => $value)	
		$$key = $value;


	if( $api_key == 'SUACHAVEDEAPI' )
	{
		
		switch ($trans_status)
		{
			case '3' :
						#Pagou
						libera_acesso();
				break;
			case '6':   #Aguardando reembolso
			case '7':   #Reembolsado
						remove_acesso();
						break;
				#...
				#...
				#...
			default:
				echo "STATUS DESCONHECIDO";
				break;
		}
	}
	else
		echo "ACESSO INVALIDO";

	function libera_acesso()
	{
		echo "ACESSO LIBERADO";
	}

	function remove_acesso()
	{
		echo "ACESSO REMOVIDO";
	}

?>
```

## Exemplo Webhook Enviado

```js
{
    "aff_cod": "",
    "aff_document_number": "",
    "aff_email": "",
    "aff_name": "",
    "aff_value": "",
    "api_key": "3d77c9f8b1",
    "billet_url": "http:\/\/g.eduzz.com\/checkout\/cart\/393351cc409172e74de46cf26cda3680",
    "cus_address": "Rua Odette Ribeiro Giardini",
    "cus_address_city": "Sorocaba",
    "cus_address_comp": "NI",
    "cus_address_country": "BR",
    "cus_address_district": "Jardim Piazza Di Roma II",
    "cus_address_number": "75",
    "cus_address_state": "SP",
    "cus_address_zip_code": "18051821",
    "cus_cel": "11-1111111111",
    "cus_cod": "677122",
    "cus_email": "rogerio.adriano@gmail.com_x",
    "cus_name": "Rog\u00e9rio Lima",
    "cus_taxnumber": "26192623805",
    "cus_tel": "1532177179",
    "cus_tel2": "15981134009",
    "eduzz_value": "",
    "page_checkout_url": "http:\/\/g.eduzz.com\/1882",
    "pro_document_number": "37068141888",
    "pro_email": "jezao@me.com",
    "pro_name": "Jez\u00e3o Maloca",
    "pro_value": "",
    "product_cod": "1882",
    "product_name": "Produto Teste GRUPP",
    "product_refund": "7",
    "recurrence_cod": "143969",
    "recurrence_count": "2",
    "recurrence_interval": "1",
    "recurrence_interval_type": "month",
    "recurrence_plan": "Produto Teste GRUPP",
    "recurrence_startdate": "2016-03-28 16:24:15",
    "recurrence_status": "10",
    "recurrence_status_name": "Trial",
    "recurrence_type": "new",
    "sku_reference": "",
    "tracker_trk": "",
    "tracker_trk2": "",
    "tracker_trk3": "",
    "tracker_utm_campaign": "",
    "tracker_utm_content": "",
    "tracker_utm_medium": "",
    "tracker_utm_source": "",
    "trans_barcode": "",
    "trans_cod": "1832416",
    "trans_createdate": "20180130",
    "trans_createtime": "10:42:09",
    "trans_currency": "BRL",
    "trans_duedate": "20170301",
    "trans_duetime": "00:00:00",
    "trans_orderid": "",
    "trans_paid": "0.00",
    "trans_paiddate": "",
    "trans_paidtime": "",
    "trans_paymentmethod": "11",
    "trans_status": "4",
    "trans_value": "10.00",
    "utm_campaign": "",
    "utm_content": "",
    "utm_medium": "",
    "utm_source": ""
}
```
