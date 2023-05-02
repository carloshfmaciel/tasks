# Atividades

## Task 1 - Criar banco de dados e tabela no spammer
Responsavel: SRE
Criar banco de dados chamado "store-geolocation"
Criar tabela "store"
- ID - number - pk
- name - varchar
- address - varchar
- neighborhood - varchar
- city - varchar
- state - varchar
- country - varchar
- zipcode - varchar
- latitude_x - float8
- longitude_y - float8
- is_active - boolean

## Task 2 - Criar repositorio no bitbucket e conceder acessos para membros da equipe
Responsavel: Verificar
Criar repositorio chamado "store-geolocation-services"

## Task 3 - Criar ambientes(HOMOLOGAÇÃO E PROD), assim também como os respectivos pipelines de deploy
Responsavel: SRE
- Criar ambientes de Homolog e PROD, assim também como as respectivas URLs para acesso aos endpoints REST do projeto
- Criar pipeline no respectivo CI/CD(ArgosCD ou SpinMaker), para homolog(branch develop) e prod(branch master) assim também como as devidas configurações de docker necessárias no projeto.

## Task 4 - Migrar e Revisar Base de dados de Loja
Responsavel: Carlos Maciel
- Criar script para inserir dados das lojas originados das planilhas excel no banco de dados "store" dentro do spammer. 
- Revisar se todas as lojas possuem valores X e Y de coordenadas de localização.
- As que não possuírem, necessário preencher.

## Task 5 - Criar projeto Java inicial utilizando spring boot 3, lombok, spring web, spring data jpa.
Responsavel: Carlos Maciel
- Nome do projeto: store-geolocation-services
- Comitar no repositorio do Bitbucket

## Task 6 - Criar endpoint para pesquisa das lojas mais proximas baseadas no cep
Responsavel: Carlos Maciel

A API deverá obrigatoriamente receber como "path url" um cep válido. Deverá ocorrer uma validação para verificar se o CEP é valido(Basicamente se possui 8 digitos numéricos). Em seguida, com o cep informado, obter as coordenadas de latitude e longitude do mesmo(x, y) através da API Google Maps.
Com os valores de x,y do google pesquisar na base de dados as lojas mais próximas, pesquisar na base de dados baseado nos seus respectivos (x,y) e aplicando o cálculo de distância(Haversine) as lojas mais próximas do cep informado, listando somente uma loja por categoria e na ordem abaixo:
- Categorias e Ordem
	- Hipermercado
	- Bairro
	- Express
	- Atacadão
	
Código Fonte Java da função de Haversine(cálculo de distância)
https://gist.github.com/vananth22/888ed9a22105670e7a4092bdcf0d72e4

Endpoint:
```
GET /stores/zipcode/{zipcode}
```

Resposta Sucesso - HTTP Code 200

Body Resposta
```
{
	"stores": [
		{
			category: string,
			address: {
				"street": string,
				"number": string,
				"neighborhood": string,
				"city": string,
				"hasGasStation": boolean,
				"hasDrugStore": boolean,
				"openingHours": [
					"monday": string
					"tuesday": string,
					"wednesday": string,
					"thursday": string,
					"friday": string,
					"saturday": string,
					"sunday": string
				],
				"distanceKm": number,
				"googleMapsLink": string
			}
		}
	]
}
```

Resposta Sucesso - HTTP Code 404 - Quando nenhuma loja for encontrada para o cep informado

Sem Body Resposta

Resposta Erro - HTTP Code 400 - Quando não for informado um cep valido

Body Resposta
```
{
	"errorCode": "INVALID_CEP"
}
```

Resposta Erro - HTTP Code 500 - Quando ocorrer um erro imprevisto

Body Resposta
```
{
	"errorCode": "INTERNAL_SERVER_ERROR"
}
```

## Task 7 - Documentar no Confluence a API GET /stores/zipcode/{zipcode}

## Task 8 - Implementar Testes Unitários referente a funcionalidade do endpoint GET /stores/zipcode/{zipcode}
Utilizando Junit e Mockito realizar testes que cubram todos os critérios de aceite da funcionalidade.


## Task 9 - Implementar verificação de autenticação nas requisições
Responsavel: Carlos Maciel
- Para que a API não fique aberta, necessário implementar integração com os recursos de OAuth2, onde deverá ser informado um acess_token no header "Authorization" da aplicação. A aplicação através do SPring Security irá verificar no Authorization Server se o token é valido.

## Task 10 - Implementar recurso primário de consulta do CEP

Visando economizar chamadas para a api do Google Maps, gravar em uma base local, resultados de consultas previamente feitas.

- Criar no spammer tabela chamada "store_geolocation"
Campos
- ID - PK - numeric
- zipcode
- latitude_x
- longitude_y

- Na API de consulta de lojas, ao receber uma requisição, pesquisar primeiro na tabela "store_geolocation" se já não existe um registro para o cep informado.
Se existir, obter os valores de X e Y, realizar o cálculo de distancia e obter as lojas mais próximas.
Se não existir, pesquisar na API do Google e gravar na tabela o resultado(cep, latitude_x  longitude_y).


