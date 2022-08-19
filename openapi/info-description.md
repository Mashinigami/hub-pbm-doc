# Introdução

Este documento visa o mapeamento das chamadas e seus respectivos
retornos com a descrição das variáveis (dos requests e responses) tal como exemplo das mesmas

## Intenção Compra

A intenção de compra compreende em realizar busca dos produtos e saber se os mesmos
tem oportunidades de desconto nos pbms enviados.

### Sem dados do comprador

Caso seja informado sem os dados do cliente será feito uma consulta generica, onde retornará por
exemplo a obrigatóriedade dos campos cpf ou cartão caso seja necessários, junto da possível
quantidade autorizada para aquele produto.

### Com dados do comprador

Caso seja informado os dados do comprador, o sistema fará a consulta de desconto com o laborarório,
nessa etapa é esperado alguns possíveis retornos:

#### Aceite termo lgpd
Pode ser retornado na variavel urlAceiteTermo um link para que o comprador aceite o termo lgpd 

#### Solicitação de dados adicionais
Um segundo caso seria a necessidade de dados adicionais do comprador para autorização com o laboratório, caso não tenha sido enviado,
os campos são informados em resposta caso seja enviado pelo menos o cpf e/ou cartão do programa. 

Nesses casos deve ser refeito a chamada de intenção de compra com os dados necessários ou aceite do termo realizado. 

## Pré-autorização

Nessa etapa devem ser enviados os produtos que o cliente aceitou, preencheu os requisitos, e retornaram com sucesso 
da chamada de intenção de compra, caso o cliente mude alguma coisa no carrinho de compra, essa chamada deve ser
refeita com o novo carrinho atualizado, é retornado um id que será usado na chamada de confirmação.

## Confirmação pré autorização

Após a confirmação do final da venda deve ser enviado a confirmação do id do carrinho retornado na ultima
etapa, junto do corpo da mensagem caso seja uma venda com desconto em folha, deve ser informado a variavel
**folhaPagamento = 1**