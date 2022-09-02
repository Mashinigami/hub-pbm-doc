# Introdução

Este documento visa o mapeamento das chamadas e seus respectivos
retornos com a descrição das variáveis (dos requests e responses) tal como exemplo das mesmas

## Versões doc

### 2.1.6

- Removido variavel loja.cnpj em todas as requests
- Atualizado descrição produtoList[].precoBruto
- Atualizado descrição produtoList[].precoLiquido
- Adicionado variavel retorno data.status.TRN.status

### 2.1.5

- Alterado endpoint responsável por fazer o cancelamento de venda.
  O novo endpoint para cancelamento de venda é hubCancelarAutorizacao.
- Alterado valor exemplo variavel canalSolicitante, anteriormente "SITE", agora "ECOMMERCE"
- Renomeado variavel data.produtoList[].produtoRetornoMap.TRN.percentualDescontoUnitario para
  data.produtoList[].produtoRetornoMap.TRN.valorDescontoUnitario

### 2.1.4

- Removido variavel cliente.cpfCnpj, no envio da confirmação pre-autorização
- Adicionado variavel cliente.TRN, no envio da confirmação pre-autorização
- Movido variavel cliente.convenioClienteWrapper para dentro da variavel cliente.TRN, ficando cliente.TRN.convenioClienteWrapper,
  no envio da confirmação da pre-autorização
- Adicionado informação de todos os campos previstos no cadastro do cliente

### 2.1.3

- Alterado tipo da variavel canalSolicitante de number para string
- Adicionado variavel de retorno data.produtoList[].produtoRetornoMap.TRN.valorDescontoIndustria
- Adicionado variavel de retorno data.produtoList[].produtoRetornoMap.TRN.percentualDescontoAutorizado
- Adicionado variavel de retorno data.produtoList[].produtoRetornoMap.TRN.valorDescontoAdicionalMultiplicado
- Adicionado variavel de retorno data.produtoList[].produtoRetornoMap.TRN.percentualDescontoUnitario

### 2.1.2

- Adicionado variavel de envio receita.nomeMedico (fluxo intencao compra)
- Adicionado variavel de envio receita.uf (fluxo intencao compra)
- Adicionado variavel de envio receita.conselho (fluxo intencao compra)
- Adicionado variavel de envio receita.numeroConselho (fluxo intencao compra)

### 2.1.1

- Renomeado valorDescontoMaximoNovoPaciente para percentualDescontoNovoPaciente

### 2.1.0

- Adicionado variavel requiredFields.field (nome do campo obrigatório)
- Adicionado variavel requiredFields.type (tipo do campo obrigatório)

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

## Autorização

Após o faturamento da venda deve ser feito a autorização enviando o id para o endpoints de autorização, caso a
venda não tenha sido finalizada enviar uma chamada para o endpoint de cancelamento (verbo Delete)

## Status da transação

A variavel data.status.TRN.status irá indicar o que aconteceu com a solicitação realizada no pbm 

## Campos cadastro cliente

- cpf
- data de nascimento
- nome completo
- genero
- cep
- unidade federativa
- cidade
- bairro
- tipo do endereço (rua, avenida,...)
- logradouro
- numero
- complemento
- telefone
- email
- aceito dos termos (industria, material informativo,...)
- tipo do profissional de saude (CRM, CRO)
- codigo do profissional de saude
- estado do profissional de saude
- nome do profissional de saude
