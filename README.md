# webhook-eduzz
O webhook permite que você receba notificações a cada alteração de status de uma compra. Sempre que uma compra tiver seu status atualizado ( Pago, Cancelado, Reembolsado, etc... ) nós iremos enviar uma chamada via POST para uma URL pré-configurada nas configurações do seu conteúdo com alguns parâmetros para que você possa realizar suas decisões comerciais.

Consideramos como **sucesso** todas as requisições que retornam o [status HTTP 200](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 

## Lista de parâmetros

Parâmetro     | Descrição	| Tipo de Variável
------------- | -------------	| -----------------
api_key | Token de segurança do produtor ou afiliado | String
aff_cod | Id do afiliado | Int
aff_document_number | Documento do afiliado | String
aff_email | E-mail do afiliado | String
aff_name |  Nome do afiliado | String
aff_value | Valor da comissão do afiliado | Float
billet_url | Página com a chave gerada para o produto | String    
cus_address | Endereço do cliente | String
cus_address_city | Cidade do cliente | String
cus_address_comp | Complemento do endereço do cliente | String
cus_address_country | País do cliente | String
cus_address_district | Bairro do cliente | String
cus_address_number | Numero do endereço do cliente | String
cus_address_state | Estado do clieente | String
cus_address_zip_code | CEP do cliente | String
cus_cel | Celular do cliente | String
cus_tel | Telefone do cliente | String
cus_tel2 | Segundo telefone do cliente | String
cus_cod | Id do cliente na Eduzz | Int
cus_email | E-mail do cliente | String
cus_name | Nome do cliente | String
cus_taxnumber | Documento do cliente | String    
eduzz_value | Taxa cobrada pela Eduzz | Float
page_checkout_url | URL do Checkout | String  
pro_document_number | Documento do produtor | String
pro_email | E-mail do produtor | String
pro_name | Nome do produtor | String
pro_value | Valor da comissão do produtor | Float 
product_cod | ID do produto | Int
product_name | Nome do produto | String
product_refund | Prazo em dias para reembolso do produto |  Int    
discount_coupon_code | Código do Cupom de Desconto utilizado (ou Vazio caso não utilizou) | String
recurrence_cod | Id do contrato | Int
recurrence_count | Quantidade de cobranças já realizadas | Int
recurrence_interval | Intervalo da recorrencia | Int
recurrence_interval_type | Tipo de intervalo (por exemplo, se o campo anterior o valor for 1 e esse campo o valor for 'month', significa que faz 1 cobrança por mes...se o campo anterior for 1 e esse for 'day', a cobrança é diária, valores possíveis: day, month e year) | String 
recurrence_plan | Nome do plano da recorrencia | String
recurrence_startdate | Data de inicio da cobrança | String
recurrence_status | Id do status do contrato | Int
recurrence_status_name | Nome do status do contrato | String    
recurrence_type | Indica se é a assinatura 2.0 | String
sku_reference | Referência externa | String    
tracker_trk |  Parâmetro genérico 1 | String
tracker_trk2 | Parâmetro genérico 2 | String
tracker_trk3 | Parâmetro genérico 3 | String
tracker_utm_campaign | UTM Campaing | String
tracker_utm_content | UTM Content | String
tracker_utm_medium | UTM Medium | String
tracker_utm_source | UTM Source | String
trans_barcode | codigo de barra do boleto|String
trans_cod | Id da fatura|Int
trans_createdate | Data de criação da fatura|String
trans_createtime | Hora de criação da fatura | String
trans_currency | Moeda utilizada na transação|String
trans_duedate | Data limite para o pagamento da fatura|String
trans_duetime | Horario limite para o pagamento da fatura|String
trans_orderid | ID da Ordem|Int
trans_paid | Valor pago|Float
trans_paiddate | Data de pagamento|String
trans_paidtime | Hhora de pagamento|String
trans_paymentmethod | Forma de pagamento (Ver tabela de forma de pagamento)|Int
trans_status | Status do pagamento (Ver tabela de status)|Int
trans_value | Valor da venda|Float
trans_items_quantity | Quantidade de itens da venda|Float
utm_campaign | UTM Campaing|String
utm_content | UTM Content|String
utm_medium | UTM Medium|String
utm_source | UTM Source|String



## Tabela de status da fatura

ID  | Status | Descrição
--- | ------ | -----------
1 | Aberta | Fatura aberta, cliente gerou boleto, mas ainda não foi compensado 
3 | Paga | Compra foi paga, o cliente já esta apto a receber o produto 
4 | Cancelada | Fatura Cancelada pela Eduzz
6 | Aguardando Reembolso | Cliente solicitou reembolso, porem o mesmo ainda não foi efetuado
7 | Reembolsado | Cliente já foi reembolsado pela eduzz
8 | Em Análise | Cliente efetuou o pagamento, porém esta em análise pela instituição financeira
11 | Em Recuperacao | Fatura entrou para o processo de recuperação

## Tabela de formas de pagamento
ID	| Forma de pagamento
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


## Exemplo php
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



