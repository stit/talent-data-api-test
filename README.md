# ST IT Cloud - Development Test LV. 3

## Objetivo

Desenvolver uma API, utilizando a **stack definida durante o processo seletivo**, para entregar de acordo com os requisitos descritos abaixo.

Esse teste deve avaliar a qualidade técnica na manipulacão de dados, otimizacão de performance, trabalho com arquivos, permissionamento e tratamento de erros

**Faz parte dos critérios de avaliacão a pontualidade da entrega. Implemente até onde for possível dentro do prazo acordado.**

**Os dados de produtos foram gerados de forma aleatória, utilizando a biblioteca FakerJS**

## Caso

Você deverá implementar um catalogo interno de produtos, no formato de API, para que os vendedores possam consultar e buscar os produtos que têm permissão paara ver. 

Os produtos estão organizados da seguinte forma, e essa estrutura está disponível no arquivo `organization.json`.

```
|
- STUFF A
---- STUFF A01
-------- <department>
-------- ...
-------- <department>
---- ...
---- STUFF A0n
-------- <department>
-------- ...
-------- <department>
- STUFF B
---- STUFF B01
-------- <department>
-------- ...
-------- <department>
---- ...
---- STUFF B0n
-------- <department>
-------- ...
-------- <department>
- STUFF C
---- STUFF C01
-------- <department>
-------- ...
-------- <department>
---- ...
---- STUFF C0n
-------- <department>
-------- ...
-------- <department>
```

Os vendedores tem 4 perfis de acessos: `junior`, `middle`, `senior` e `intern`. Cada perfil poderá ver os produtos de acordo com o seu nível (campo `level` de organization), conforme regra a seguir:

- `senior` - level 0, 1 e 2
- `middle` - level 1 e 2
- `junior` - level 2

- `intern` - level 0, 1 e 2, porém somente sob a organization `STUFF A`

## Requisitos

A API deve expor os seguintes endpoints através de HTTP(S), e caso a API utilize outra porta que não as padrões dos protocolos, deve ser informado na entrega do teste.
 
### `POST /login`

Esse endpoint deve receber os dados de autenticacão e cruzar com os dados do arquivo fornecido `users.json`, caso a combinacão de usuário e senha existam no arquivo, o endpoint retorna um token com a roles do usuário via claims. Caso não exista, deve informar que o acesso não pode ser autorizado.

#### Expected Request

- Body
```
{ "email": <USER_EMAIL>, "password": <USER_PASSWORD> }
```

#### Expected Response

```
{ "token": <JWT TOKEN> }
```


### `GET /products/:organizationName?tags=<tag1>,<tag2>,...`

Esse endpoint deve retornar o catalogo de produtos de acordo com o formato descrito abaixo, repeitando o nível de acesso do usuário autenticado através do token JWT.

Caso o usuário tente acessar alguma organization que não tem acesso, o sistema deve informar que o acesso não é permitido.

Respeitando a estrutura de árvore, o catálogo de produtos deve retornar todos os produtos que estão dentro da organization solicitada e também os que estão abaixo na estrutura.

A querystring tags é opicional, e deve ser usada para filtrar os produtos através do campo `tags` do arquivo `products.txt`, de forma inclusiva. Ou seja, ainda respeitando a estrutura de árvore e permissionamento, ao passar uma ou mais tags, o catalogo deve retornar todos os produtos que tem pelo menos uma das tags passadas.

#### Expected Request

- Headers
```
Authorization: Bearer <JWT TOKEN>
```
- Querystring
  - tags - lista de tags para filtrar os produtos
```
tags=<tag1>,<tag2>,...
```
- Parameters
  - organizationName - nome de organization (referencia ao campo `name` do arquivo `organization.json`)
```
exemplos: STUFF A, STUFF B01 ou Garden

Aqui pode ser passado o nome de qualquer entrada de organization, independente do nível
```

#### Expected Response

```
{ 
    "total": <TOTAL NUMBER OF RETURNED PRODUCTS>,
    "products": [
        <PRODUCT>,
        <PRODUCT>,
        <PRODUCT>,
        ...
    ]
}
```

## Requisitos não-funcionais

- A autenticacão do usuário deve ser feita através de alguma implementacão de JWT.

- Se julgar necessário manter sessão, deve ser feito de forma independente de server.

- Instruções para instalacão e execucão da API, incluindo as dependências de libs, runtimes, e etc.

- A API deve ser escalável horizontalmente.

## Bonus! (Não obrigatório)

- Não carregar os dados do arquivo `products.txt` de forma integral em memória. SUGESTÃO: Utilize streams.
- Implementacão de testes unitários.
- Implementacão de testes integrados através de feature files.
- Execucão em container.
- Publicacão da API em algum servico cloud.

## Critérios de Avaliacão

- Pontualidade da entrega
- Atendimento dos requisitos
- Aderência à stack solicitada
- Dados respondidos corretamente
- Uso correto dos códigos HTTP para casos de sucesso e erro
- Qualidade do código. Levaremos em consideracão a utilizacão de padrões de desenvolvimento como GoF Design Patterns, GRASP, DRY, KIS e SOLID. Também avaliaremos a clareza do código, e a quantidade de *code smells*.
- Argumentacão dos trade-offs e escolhas técnicas, como estruturacão do projeto, algoritmo para trabalhar com os dados e etc.

