# Serverless-demo-apirest
Serverless Example using Lambda, Api Gateway and DynamoDB

## Diagrama y flujos de trabajo:

![alt text](https://s3.amazonaws.com/imagenes.cennti.com/serverless-demo-api-rest.png)

## 1. Creamos la base de datos en DynamoDB

### Database Setup
	dynamodb:
		table name: books
		partition key: id
		default settings


##2. Luego creamos un Rol en IAM para que sea usado por Lambda

### Creating the Lambda Role
	role: lambda
	policy: dynamodb:*
	name: LambdaRoleForDynamoDBAccess


## 3. Empezamos la creación de funciones Lambda.


### Create book Lambda: 
	name: create-book
	language: nodejs 10.x
	role: LambdaRoleForDynamoDBAccess
	configure test events


### Update test events:

	{
	  "body": "{\"title\": \"Carlos Cortex Cloud Book\"}"
	}


### Review in dynamodb.


## 5. Creating Get Books Lambda and API:
	name: get-books
	language: nodejs 10.x
	role: LambdaRoleForDynamoDBAccess
	configure test events: default


## 6. Creating a GET API
	go to apigateway service
	type: regional API
	API name: Books
	create resource: /books 
		create method: GET --> get-books lambda function
		deploy API:
			new stage: production

### test new API:

https://cccndgwk5k.execute-api.us-east-1.amazonaws.com/production/books

{
"statusCode": 200,
"body": "[{\"id\":\"9d1f4b369a5619e5e5967ddac2b463db\",\"title\":\"Carlos Cortez Cloud Book\"}]"
}


curl -X GET \
  https://cccndgwk5k.execute-api.us-east-1.amazonaws.com/production/books \
  -H 'cache-control: no-cache'


## 7. Creating a POST API
	go to resource: /books 
		create method: POST
			type: lambda function
			region: us-east-1
			function: create-book
			lambda proxy integration: yes

		deploy API:
			choose stage: production

curl -Xv POST \
  https://cccndgwk5k.execute-api.us-east-1.amazonaws.com/production/books \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{"title":"Be an AWS Architect Guide by CC 2nd Edition"}'



## 8. Get book Lambda: 
	name: get-book
	language: nodejs 10.x
	role: LambdaRoleForDynamoDBAccess
	configure test events:

{
  "pathParameters": {
    "id": "7616bd4945e49e2e1b37536e73d00390"
  }
}


### Test Function:

Response:
{
  "statusCode": 200,
  "body": "{\"id\":\"7616bd4945e49e2e1b37536e73d00390\",\"title\":\"Be an AWS Architect Guide by CC 2nd Edition\"}"
}



## 9. Creating a GET API for single Book
	go to resource: /books 
		create resource: {id}
			create method: GET
			type: lambda function
			region: us-east-1
			function: get-book
			lambda proxy integration: yes

		deploy API:
			choose stage: production

curl -X GET \
  https://cccndgwk5k.execute-api.us-east-1.amazonaws.com/production/books/7616bd4945e49e2e1b37536e73d00390 \
  -H 'cache-control: no-cache' \
  -H 'postman-token: 254956b7-8f94-6121-e9ea-d30107ec34f3'


{"id":"7616bd4945e49e2e1b37536e73d00390","title":"Be an AWS Architect Guide by CC 2nd Edition"}%



## 10. Delete Book Lambda:
	name: delete-book
	language: nodejs 10.x
	role: LambdaRoleForDynamoDBAccess
	configure test events: default


### Test event:

{
  "pathParameters": {
    "id": "7616bd4945e49e2e1b37536e73d00390"
  }
}


### 11. Update Book Lambda:
	name: update-book
	language: nodejs 10.x
	role: LambdaRoleForDynamoDBAccess
	configure test events: default

### Test event:

{
  "pathParameters": {
    "id": "7616bd4945e49e2e1b37536e73d00390",
    "title": "new book v3"
  }
}



## 12. Creating a POST API for Delete book
	go to resource: /books 
		create resource: {id}
			create method: UPDATE
			type: lambda function
			region: us-east-1
			function: delete-book
			lambda proxy integration: yes

		deploy API:
			choose stage: production


curl -X DELETE \
  https://cccndgwk5k.execute-api.us-east-1.amazonaws.com/production/books/3e2ac86035d76287d68e9c456cc2a347 \
  -H 'cache-control: no-cache' \
  -H 'postman-token: bcdb65d1-f9ff-5124-aa93-ac3476812138'



## 13. Creating a POST API for Update book
	go to resource: /books 
		create resource: {id}
			create method: POST
			type: lambda function
			region: us-east-1
			function: update-book
			lambda proxy integration: yes

		deploy API:
			choose stage: production



curl -X POST \
  https://cccndgwk5k.execute-api.us-east-1.amazonaws.com/production/books/54e42d99a2f2760573a9901231ade363 \
  -H 'cache-control: no-cache' \
  -H 'postman-token: 2b8900bc-38eb-e2de-4f69-7f067f5509d2' \
  -d '{"title":"Cloud Lovers v4: lord of the cloud ULTIMATE VERSION...."}'
























