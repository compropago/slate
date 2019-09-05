---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php
  - ruby
  - python
  - csharp
  - java
  - javascript

toc_footers:
#  - <a href='#'>Sign Up for a Developer Key</a>
#  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introducción

ComproPago te ofrece un API tipo REST para integrar pagos en SPEI en tu comercio electrónico o tus aplicaciones.

La autenticación es vía HTTP por medio de llaves -API Keys- tanto en modo de pruebas como en modo activo. El API cuenta con métodos para accesar a los diferentes recursos -API Endpoints- los cuales actualmente te permiten realizar diversas operaciones, desde generar órdenes de pago hasta enviar instrucciones de pago vía SMS.

# Autenticación

> Para autentificar, use este código:

```shell
curl "api_endpoint_here" \
-u sk_live_xxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxx
```

```php
<?php
# Using SPEI-SDK for PHP
require 'vendor/autoload.php';

# Importar objeto Client
use SpeiSdk\Client;

# Configuración de las llaves de SPEI en el cliente
$client = new Client(
    'pk_live_xxxxxxxxxxxxxxxxxx',
    'sk_live_xxxxxxxxxxxxxxxxxx'
);
```

```ruby
# Using SPEI-SDK for Ruby
require 'spei_sdk'

# Configuración de las llaves de SPEI en el cliente
client = Client.new(
    'pk_live_xxxxxxxxxxxxxxxxxx',
    'sk_live_xxxxxxxxxxxxxxxxxx'
)
```

```python
# Using SPEI-SDK for Python
from spei_sdk import Client

# Configuración de las llaves de SPEI en el cliente
client = Spei(
    'pk_live_xxxxxxxxxxxxxxxxxx',
    'sk_live_xxxxxxxxxxxxxxxxxx'
)
```

```csharp
// Using SPEI-SDK for C# .Net
using SpeiSdk;

// Configuración de las llaves de SPEI en el cliente
var client = new Spei(
    "pk_live_xxxxxxxxxxxxxxxxxx",
    "sk_live_xxxxxxxxxxxxxxxxxx"
);
```

```java
// Using SPEI-SDK for Java
import speisdk.Client;

// Configuración de las llaves de SPEI en el cliente
Client client = new Client(
    "pk_live_xxxxxxxxxxxxxxxxxx",
    "sk_live_xxxxxxxxxxxxxxxxxx"
);
```

> Asegurate de remplazar `sk_live_xxxxxxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxxxxx` por tus llaves del API.

Ponemos a tu disposición dos pares de llaves -API Keys- en la sección [panel/configuración](https://panel.compropago.com/panel/configuracion), con ellas puedes interactuar con nuestro sistema en sus diferentes modos de operación.

Es posible utilizar más de una llave al mismo tiempo; es decir, puedes realizar cargos en modo activo o producción y al mismo tiempo tu equipo de desarrollo puede hacer cargos de prueba sin afectar la configuración de tu aplicación.

La autenticación de tu aplicación con ComproPago es por medio de HTTP Basic Authentication: proporciona alguna de tus llaves como username, no necesitas proporcionar un password.

`sk_live_xxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxx`

<aside class="notice">
Debes remplazar <code>sk_live_xxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxx</code> por tus llaves del API.
</aside>

# SPEI

## Crear orden de pago

> Ejemplo para crear una orden:

```shell
curl "https://api.compropago.com/v2/orders" \
-u sk_live_xxxxxxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxxxxx \
-H 'Content-Type: application/json'
```

```php
<?php
# Arreglo con información de la orden
$orderInformation = [
    "product" => [
        "id" => "10001",
        "price" => 1499.99,
        "name" => "SPEI Order from PHP",
        "currency" => "MXN",
        "url" => "http://dummyurl.com/prod10001.jpg"
    ],
    "customer" => [
        "id" => "12345",
        "name" => "Nombre del Cliente",
        "email" => "cliente@dominio.com",
        "phone" => "55222999888"
    ],
    "payment" =>  [
        "type" => "SPEI"
    ],
    "expiresAt" => strtotime("+2 day"),
];

# Llamada al método del API para generar una orden de pago
$order = $client->createOrder($orderInformation);
```

```ruby
# Arreglo con información de la orden
order_information = [
    :product => [
        :id => "10001",
        :price => 1499.99,
        :name => "SPEI Order from Ruby",
        :currency => "MXN",
        :url => "http://dummyurl.com/prod10001.jpg"
    ],
    :customer => [
        :id => "12345",
        :name => "Nombre del Cliente",
        :email => "cliente@dominio.com",
        :phone => "55222999888"
    ],
    :payment =>  [
        :type => "SPEI"
    ],
    :expiresAt => (DateTime.now + 2).to_time.to_i
]

# Llamada al método del API para generar una orden de pago
order = client.create_order(order_information)
```

```python
# Diccionario con información de la orden
order_information = {
    'product': {
        'id': '10001',
        'price': 1499.99,
        'name': 'SPEI Order from Python',
        'currency': 'MXN',
        'url': 'http://dummyurl.com/product_0001.jpg'
    },
    'customer': {
        'id': '12345',
        'name': 'Nombre del Cliente',
        'email': 'cliente@dominio.com',
        'phone': '55222999888'
    },
    'payment': {
        'type': 'SPEI'
    },
    'expiresAt': (datetime.now() + timedelta(days=2)).timestamp()
}

# Llamada al método del API para generar una orden de pago
order = client.create_order(order_information)
```

```csharp
// Diccionario con información de la orden
var order_information = new Dictionary
{
    {
        "product", {
            { "id", "10001" },
            { "price", 1499.99 },
            { "name", "SPEI Order from C# .NET" },
            { "currency", "MXN" },
            { "url", "http://dummyurl.com/prod10001.jpg" }
        }
    },
    {
        "customer", {
            { "id", "12345" },
            { "name", "Nombre del Cliente" },
            { "email", "cliente@dominio.com" },
            { "phone", "55222999888" }
        }
    },
    {
        "payment", {
            { "TYPE", "SPEI" }
        }
    },
    { "expiresAt", DateTimeOffset.Now.AddDays(2).ToUnixTimeSeconds() },
};


// Llamada al método del API para generar una orden de pago
var order = client.CreateOrder(order_information);
```

```java
// Objeto JSON con información de la orden
JSONObject order_product = new JSONObject();
order_product.put("id", "10001");
order_product.put("price", 1499.99);
order_product.put("name", "SPEI Order from Java");
order_product.put("currency", "MXN");
order_product.put("url", "http://dummyurl.com/prod10001.jpg");
JSONObject order_customer = new JSONObject();
order_customer.put("id", "12345");
order_customer.put("name", "Nombre del Cliente");
order_customer.put("email", "cliente@dominio.com");
order_customer.put("phone", "55222999888");
JSONObject order_payment = new JSONObject();
order_payment.put("type", "SPEI");
JSONObject order_information = new JSONObject();
order_information.put("product", order_product);
order_information.put("customer", order_customer);
order_information.put("payment", order_payment);
order_information.put("expiresAt", System.currentTimeMillis()/1000 + (2*60*60*24));

// Llamada al método del API para generar una orden de pago
Order order = client.CreateOrder(order_information);
```

> The above command returns JSON structured like this:

```json
{
    "object": "orders.create",
    "code": 200,
    "status": "success",
    "message": "success.create",
    "request": 1559853010380,
    "url": "/v2/orders",
    "map": "76ab15b2-f7c3-4a95-a662-b5cc4a2a9938",
    "data": {
        "id": "ch_ca001318-7f70-47a9-90a2-df3e755d9be3",
        "shortId": "481AD8F9",
        "product": {
            "id": "SMGCURL1",
            "price": 150,
            "name": "SAMSUNG GOLD CURL",
            "currency": "MXN",
            "url": "http://dummyurl.com/smgcul1.jpg"
        },
        "customer": {
            "name": "Waldix",
            "email": "waldix@compropago.com",
            "phone": "5513899832"
        },
        "payment": {
            "type": "SPEI",
            "fee": 10.25,
            "iva": 1.64,
            "clabe": "646180146400219781",
            "instructions": [
                {
                    "step": 1,
                    "description": "Agrega la cuenta CLABE en tu banca en linea para banco STP."
                },
                {
                    "step": 2,
                    "description": "Una vez dada de alta la cuenta CLABE, realiza el deposito por la cantidad exacta."
                },
                {
                    "step": 3,
                    "description": "Ya que realizaste el pago, recibiras un comprobante de transacción exitosa y nuestro sistema confirmara automáticamente el pago."
                }
            ]
        },
        "status": "PENDING",
        "testMode": false,
        "exchange": {
            "rate": 1,
            "finalAmount": 150,
            "originAmount": 150,
            "finalCurrency": "MXN",
            "originCurrency": "MXN"
        },
        "extras": {},
        "client": {
            "name": "ms-api",
            "version": "2.0.0"
        },
        "createdAt": 1559853010,
        "updatedAt": 1559853010,
        "expiresAt": 1560139200
    }
}
```

Generación de orden de pago para SPEI.
Nota. Si envias un id en el objeto customer, siempre que envies este identificador te regresaremos la misma cuenta CLABE con la finalidad de que el cliente no tenga que estar dando de alta varias cuentas en su banca en linea.

### HTTP Request

`POST https://api.compropago.com/v2/orders`

### Query Parameters

Parameter | Type | Description
--------- | ---- | -----------
product | Object | Requerido. Objeto del producto a pagar.
id | string | Requerido. Identificador del producto o servicio.
price | float | Requerido. Precio del producto o servicio.
name | string | Requerido. Nombre del producto o servicio.
url | string | Opcional. Url de la imagen del producto o servicio.
customer | Object | Requerido. Objeto del cliente.
id | string | Requerido. Id del cliente.
name | string | Requerido. Nombre del cliente.
email | string | Requerido. Email del cliente.
phone | string | Opcional. Teléfono del cliente.
payment | Object | Requerido. Objeto del pago.
type | string | Requerido. Tipo de pago *default "SPEI".

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->

## Verificar orden de pago

```shell
curl "https://api.compropago.com/v2/orders/ch_c1c0983d-6318-4f65-838c-8ff60afacf18" \
-u sk_live_xxxxxxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxxxxx
```

```php
<?php
# Obtener el ID de la orden generada anteriormente
$orderId = $order['data']['id'];

# Llamada al método del API para recuperar la información de la orden de pago
$verified = $compropagoSpei->verifyOrder($orderId);
```

```ruby
# Obtener el ID de la orden generada anteriormente
order_id = order.data.id

# Llamada al método del API para recuperar la información de la orden de pago
verified = client.verify_order(order_id)
```

```python
# Obtener el ID de la orden generada anteriormente
order_id = order.data.id

# Llamada al método del API para recuperar la información de la orden de pago
verified = client.verify_order(order_id)
```

```csharp
// Obtener el ID de la orden generada anteriormente
string order_id = order.data.id;

// Llamada al método del API para recuperar la información de la orden de pago
var verified = client.VerifyOrder(order_id);
```

```java
// Obtener el ID de la orden generada anteriormente
String order_id = order.data.id;

// Llamada al método del API para recuperar la información de la orden de pago
Order verified = client.VerifyOrder(order_id);
```

> El comando anterior regresa el JSON estructurado de la siguiente manera:

```json
{
      "id": "ch_c1c0983d-6318-4f65-838c-8ff60afacf18",
      "shortId": "3B92A8FC",
      "product": {
          "id": "SMGCURL1",
          "price": 12.56,
          "name": "SAMSUNG GOLD CURL",
          "currency": "MXN"
      },
      "customer": {
          "name": "Ernesto Perez",
          "email": "luis_mtzg@msn.com"
      },
      "payment": {
          "type": "SPEI",
          "fee": 8.19,
          "iva": 1.31,
          "clabe": "000000000000000000",
          "instructions": [
              {
                  "step": 1,
                  "description": "Agrega la cuenta CLABE en tu banca en linea para banco STP."
              },
              {
                  "step": 2,
                  "description": "Una vez dada de alta la cuenta CLABE, realiza el deposito por la cantidad exacta."
              },
              {
                  "step": 3,
                  "description": "Ya que realizaste el pago, recibiras un comprobante de transacción exitosa y nuestro sistema confirmara automáticamente el pago."
              }
          ]
      },
      "status": "PENDING",
      "testMode": false,
      "exchange": {
          "rate": 1,
          "finalAmount": 12.56,
          "originAmount": 12.56,
          "finalCurrency": "MXN",
          "originCurrency": "MXN"
      },
      "extras": {},
      "api": {
          "name": "ms-api",
          "version": "2.0.0"
      },
      "createdAt": 1522082069321,
      "updatedAt": 1522082069321,
      "expiresAt": 1522254869321
  }
```

Verifica y obtiene los detalles de una orden existente.

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`GET https://api.compropago.com/v2/orders/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
ID | string | Requerido. Id de la orden de pago generada.

## Crear cuenta CLABE

```shell
curl "https://api.compropago.com/v2/createClabe" \
-u sk_live_xxxxxxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxxxxx \
-X POST \
-d '{
        "customer": {
            "id": "1",
            "email": "customer@email.com",
            "phone": "5555555555"
        },
        "payment": {
            "frecuency": "unlimited",
            "exp_date": null,
            "amount": null
        }
    }'
```

```php
<?php
# Arreglo con información de la CLABE
$clabe_information = [
    "customer" => [
        "id" => "1",
        "email" => "customer@email.com",
        "phone" => "5555555555"
    ],
    "payment" => [
        "frecuency" => "unlimited",
        "exp_date" => null,
        "amount" => null,
    ],
];

$clabe = $client->create_clabe($clabe_information);
```

```ruby
# Arreglo con información de la CLABE
clabe_information = [
    :customer => [
        :id => "1",
        :email => "customer@email.com",
        :phone => "5555555555"
    ],
    :payment => [
        :frecuency => "unlimited",
        :exp_date => None,
        :amount => None
    ]
]

clabe = client.create_clabe(clabe_information)
```

```python
# Diccionario con información de la CLABE
clabe_information = {
    'customer': {
        'id': '1',
        'email': 'customer@email.com',
        'phone': '5555555555'
    },
    'payment': {
        'frecuency': 'unlimited',
        'exp_date': None,
        'amount': None
    }
}

# Llamada al método del API
clabe = client.create_clabe(clabe_information)
```

```csharp
// Diccionario con información de la CLABE
var clabe_information = new Dictionary
{
    {
        "customer", {
            { "id": "1" },
            { "email": "customer@email.com" },
            { "phone": "5555555555" }
        }
    },
    {
        "payment", {
            { "frecuency": "unlimited" },
            { "exp_date": null },
            { "amount": null }
        }
    }
};

// Llamada al método del API
var clabe = client.CreateClabe(clabe_information);
```

```java
// Objeto JSON con información de la CLABE
JSONObject clabe_customer = new JSONObject();
clabe_customer.put("id", "1");
clabe_customer.put("email", "customer@email.com");
clabe_customer.put("phone", "5555555555");
JSONObject clabe_payment = new JSONObject();
clabe_payment.put("frecuency", "unlimited");
clabe_payment.put("exp_date", null);
clabe_payment.put("amount", null);
JSONObject clabe_information = new JSONObject();
clabe_information.put("customer", clabe_customer);
clabe_information.put("payment", clabe_payment);

// Llamada al método del API
Clabe clabe = client.CreateClabe(clabe_information);
```

> El comando anterior regresa el JSON estructurado de la siguiente manera:

```json
{
    "object": "orders.createClabe",
    "code": 200,
    "status": "success",
    "message": "success.create",
    "request": 1559855232441,
    "url": "/v2/createClabe",
    "map": "dd91e4b5-4a3f-472e-a3bb-4d53f52902f6",
    "data": {
        "customer": {
            "id": "1",
            "email": "customer@email.com",
            "phone": "5555555555"
        },
        "payment": {
            "clabe": "000000000000000000",
            "frecuency": "unlimited",
            "amount": null,
            "instructions": [
                {
                    "step": 1,
                    "description": "Agrégala en tu banca en línea seleccionando como banco: STP."
                },
                {
                    "step": 2,
                    "description": "Realiza transferencias a cualquier hora por los montos que desees."
                },
                {
                    "step": 3,
                    "description": "Por cada operación exitosa recibirás un comprobante de la transacción."
                }
            ]
        },
        "status": "ACTIVE",
        "testMode": false,
        "createdAt": 1559855232,
        "updatedAt": 1559855232,
        "expiresAt": null
    }
}
```

Generación de cuentas CLABE ligadas a un Id del cliente y con la posibilidad de recibir todos los pagos que se envien a esta.

### HTTP Request

`POST https://api.compropago.com/v2/createClabe`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
customer | object | Requerido. Objeto de la información del cliente.
id | string | Requerido. Id del cliente.
email | string | Requerido. Email del cliente, se le enviara un correo con la generación de la cuenta CLABE.
phone | string | Opcional. Telefono del cliente donde enviaremos las instrucciones de su CLABE por SMS.
payment | object | Requerido. Objeto con las configuraciones de la cuenta.
frecuency | string | Requerido. Opciones: `unlimited` - se recibiran todos los pagos que lleguen a la cuenta CLABE generada.
exp_date | epoch | Opcional. Fecha de expiración para la recepción de pagos en esta cuenta CLABE
amount | float | Opcional. Recepción de pagos solo con el monto configurado

## Crear cuentas y subcuentas

```shell
curl "https://api.compropago.com/v2/createAccount" \
-u sk_live_xxxxxxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxxxxx \
-X POST \
-d '{
        "name": "Condominio Polanco",
        "accountId": "ae3d2531-ac31-406b-86dc-fjzwe8212h",
        "customers":[ 
            {
                "id": "ae3d2531-ac31-3292sd-23sdja-823j",
                "name": "Departamento_101",
                "email": "email@gmail.com",
                "phone": "5555555555"
            },
            {
                "id": "kdsakdas-823jasd-823jsa-923kaas",
                "name": "Departamento_102",
                "email": "email@hotmail.com",
                "phone": "5555555555"
            }
        ]
    }'
```

```php
<?php
# Información de la cuenta
$account_information = [
    'name' => 'Condominio Polanco',
    'accountId' => 'ae3d2531-ac31-406b-86dc-fjzwe8212h',
    'customers' => [
        'id' => 'ae3d2531-ac31-3292sd-23sdja-823j',
        'name' => 'Departamento_101',
        'email' => 'email@gmail.com',
        'phone' => '5555555555'
    ],
];

# Llamada al método del API
$account = $client->create_account($account_information);
```

```ruby
# Arreglo con información de la cuenta
account_information = [
    :name => 'Condominio Polanco',
    :accountId => 'ae3d2531-ac31-406b-86dc-fjzwe8212h',
    :customers => [
        :id => 'ae3d2531-ac31-3292sd-23sdja-823j',
        :name => 'Departamento_101',
        :email => 'email@gmail.com',
        :phone => '5555555555'
    ],
];

# Llamada al método del API
account = client.create_account(account_information);
```

```python
# Diccionario con información de la cuenta
account_information = {
    'name': 'Condominio Polanco',
    'accountId': 'ae3d2531-ac31-406b-86dc-fjzwe8212h',
    'customers': [
        'id': 'ae3d2531-ac31-3292sd-23sdja-823j',
        'name': 'Departamento_101',
        'email': 'email@gmail.com',
        'phone': '5555555555'
    ]
}

# Llamada al método del API
account = client.create_account(account_information)
```

```csharp
// Diccionario con información de la cuenta
var account_information = new Dictionary
{
    { 'name', 'Condominio Polanco' },
    { 'accountId', 'ae3d2531-ac31-406b-86dc-fjzwe8212h' },
    { 'customers', {
        { 'id': 'ae3d2531-ac31-3292sd-23sdja-823j' },
        { 'name': 'Departamento_101' },
        { 'email': 'email@gmail.com' },
        { 'phone': '5555555555' }
    }
};

// Llamada al método del API
var account = client.CreateAccount(account_information)
```

```java
// Objeto JSON con información de la cuenta
JSONObject account_customer = new JSONObject();
account_information.put("id", "ae3d2531-ac31-3292sd-23sdja-823j");
account_information.put("name", "Departamento_101");
account_information.put("email", "email@gmail.com");
account_information.put("phone", "5555555555");
JSONObject account_information = new JSONObject();
account_information.put("name", "Condominio Polanco");
account_information.put("accountId", "ae3d2531-ac31-406b-86dc-fjzwe8212h");
account_information.put("customers", account_customer);

// Llamada al método del API
Account account = client.CreateAccount(account_information);
```

> El comando anterior regresa el JSON estructurado de la siguiente manera:

```json
{
    "object": "orders.createAccount",
    "code": 200,
    "status": "success",
    "message": "success.create",
    "request": 1561416434701,
    "url": "/v2/createAccount",
    "map": "97281aa7-04b0-4cc7-bc7f-e97c7a609f3f",
    "data": {
        "name": "Condominio Polanco",
        "accountId": "ae3d2531-ac31-406b-86dc-fjzwe8212h",
        "customers": [
            {
                "id": "ae3d2531-ac31-3292sd-23sdja-823j",
                "name": "Departamento_101",
                "email": "email@gmail.com",
                "phone": "5555555555",
                "clabe": "646180146400231992"
            },
            {
                "id": "kdsakdas-823jasd-823jsa-923kaas",
                "name": "Departamento_102",
                "email": "email@gmail.com",
                "phone": "5555555555",
                "clabe": "646180146400252269"
            }
        ],          
        "instructions": [
            {
                "step": 1,
                "description": "Agrégala en tu banca en línea seleccionando como banco: STP."
            },
            {
                "step": 2,
                "description": "Realiza transferencias a cualquier hora por los montos que desees."
            },
            {
                "step": 3,
                "description": "Por cada operación exitosa recibirás un comprobante de la transacción."
            }
        ],
        "status": "ACTIVE",
        "testMode": false,
        "createdAt": 1561416434,
        "updatedAt": 1561416434
    }
}
```

Generación de cuentas y subcuentas ligadas a una CLABE para el abono de depósitos mediante SPEI.

### HTTP Request

`POST https://api.compropago.com/v2/createAccount`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
name | string | Requerido. Nombre de la cuenta.
accountId | string | Requerido. Identificador de la cuenta, tiene que ser único por cuenta.
customers | array | Requerido. Arreglo de subcuentas.
id | string | Requerido. Identificador de la subcuenta, tiene que ser único por subcuenta.
name | string | Requerido. Nombre de subcuenta.
email | string | Requerido. Email de subcuenta, se le enviara un correo con la generación de la cuenta CLABE.
phone | string | Opcional. Teléfono de la subcuenta a la cual se le enviaran las instrucciones de pago vía SMS.

## Listar cuentas

```shell
curl "https://api.compropago.com/v2/accounts" \
-u sk_live_xxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxx
```

```php
<?php
# Llamada al método del API
$accounts = $client->accounts->getAll();
```

```ruby
# Llamada al método del API
accounts = client.accounts.getAll()
```

```python
# Llamada al método del API
accounts = client.accounts.get_all()
```

```csharp
// Llamada al método del API
accounts = client.Accounts.GetAll();
```

```java
// Llamada al método del API
Account accounts = client.accounts.GetAll();
```

> El comando anterior regresa el JSON estructurado de la siguiente manera:

```json
{
    "object": "orders.get_accounts",
    "code": 200,
    "status": "success",
    "message": "success.get_accounts",
    "request": 1563056627957,
    "url": "/v2/accounts",
    "map": "68d68437-4dc2-4381-bfea-8e3cd86c03f7",
    "data": [
        {
            "accountId": "ae3d2531-ac31-406b-86dc-fjzwe8212h",
            "name": "Condominio Zacatepec",
            "balance": 0
        },
        {
            "accountId": "ae3d2531-ac31-406b-86dc-a0d6272sfr234",
            "name": "Condominio Urales",
            "balance": 0
        },
        {
            "accountId": "ae3d2531-ac31-406b-86dc-a0d6272c37f5",
            "name": "Condominio Torreon",
            "balance": 0
        }
    ]
}
```

Listado de cuentas conformadas por id, referencia, nombre y saldo.

### HTTP Request

`GET https://api.compropago.com/v2/accounts`

## Listar subcuentas

```shell
curl "https://api.compropago.com/v2/accounts/<ID>" \
-u sk_live_xxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxx
```

```php
<?php
# ID
$id = 'ae3d2531-ac31-406b-86dc-fjzwe8212h';

# Llamada al método del API
$sub_accounts = $client->accounts->get($id);
```

```ruby
# ID
id = 'ae3d2531-ac31-406b-86dc-fjzwe8212h'

# Llamada al método del API
sub_accounts = client.accounts.get(id)
```

```python
# ID
id = 'ae3d2531-ac31-406b-86dc-fjzwe8212h'

# Llamada al método del API
sub_accounts = client.accounts.get(id)
```

```csharp
// ID
string id = 'ae3d2531-ac31-406b-86dc-fjzwe8212h';

// Llamada al método del API
var sub_accounts = client.Accounts.Get(id);
```

```java
// ID
String id = 'ae3d2531-ac31-406b-86dc-fjzwe8212h';

// Llamada al método del API
SubAccount sub_accounts = client.accounts.get(id);
```

> El comando anterior regresa el JSON estructurado de la siguiente manera:

```json
{
    "object": "orders.get_account",
    "code": 200,
    "status": "success",
    "message": "success.get_account",
    "request": 1563057051977,
    "url": "/v2/accounts/170",
    "map": "b5f76f16-76dd-471b-ad85-abb1834e3acf",
    "data": [
        {
            "id": "ae3d2531-ac31-3292sd-23sdja-823j",
            "name": "Departamento_101",
            "clabe": "646180146400237132"
        },
        {
            "id": "kdsakdas-823jasd-823jsa-923kaas",
            "name": "Departamento_102",
            "clabe": "646180146400237145"
        }
    ]
}
```

Listado de subcuentas conformadas por nombre y CLABE.

### HTTP Request

`GET https://api.compropago.com/v2/accounts/<ID>`

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
ID | string | Requerido. Id de la subcuenta que se quiere consultar.

## Listar transacciones recibidas en la subcuenta

```shell
curl "https://api.compropago.com/v2/accounts/<ID>/transactions?clabe=<CLABE>" \
-u sk_live_xxxxxxxxxxxxxx:pk_live_xxxxxxxxxxxxxxx
```

```php
<?php
# ID
$id = '170';
$clabe = '002180123123123123';

# Llamada al método del API
$transactions = $client->accounts->transactions($id, $clabe);
```

```ruby
# ID
id = '170'
clabe = '002180123123123123'

# Llamada al método del API
transactions = client.accounts.transactions(id, clabe)
```

```python
# ID
id = '170'
clabe = '002180123123123123'

# Llamada al método del API
transactions = client.accounts.transactions(id, clabe)
```

```csharp
// ID
string id = '170';
string clabe = '002180123123123123';

// Llamada al método del API
var transactions = client.Accounts.Transactions(id, clabe);
```

```java
// ID
String id = '170';
String clabe = '002180123123123123';

// Llamada al método del API
Transaction transactions = client.accounts.transactions(id, clabe);
```

> El comando anterior regresa el JSON estructurado de la siguiente manera:

```json
{
    "object": "orders.get_transactions",
    "code": 200,
    "status": "success",
    "message": "success.get_transactions",
    "request": 1564093161840,
    "url": "/v2/accounts/186/transactions",
    "map": "a018844d-5bc9-43aa-b9e4-2dfd65ce7353",
    "data": [
        {
            "id": "b079d0fe-7719-4ffd-a899-46044410d940",
            "date": "1564003560",
            "amount": 10,
            "status": "paid",
            "description": "Abono",
            "reference": "085903161360320595"
        },
        {
            "id": "a12200d1-1395-43ef-a12f-7854ca5ef146",
            "date": "1564003560",
            "amount": -0.1,
            "status": "paid",
            "description": "Comisión en % por operación",
            "reference": "085903161360320595"
        },
        {
            "id": "935adeb9-9513-487c-a07c-25a5a32f42e9",
            "date": "1564003560",
            "amount": -8,
            "status": "paid",
            "description": "Comisión fija por operación",
            "reference": "085903161360320595"
        },
        {
            "id": "821cef90-6644-4d3d-8b09-945a19bb69c9",
            "date": "1564003560",
            "amount": -0.15,
            "status": "paid",
            "description": "Comisión en % por operación",
            "reference": "085903161360320595"
        }
    ]
}
```

Listado de transacciones recibidas en la subcuenta, filtradas por la CLABE.

### HTTP Request

`GET https://api.compropago.com/v2/accounts/<ID>/transactions?clabe=<CLABE>`

### URL Parameters

Parameter | Type | Description
--------- | ------- | -----------
ID | string | Requerido. Id de la subcuenta que se quiere consultar.
CLABE | string | Opcional. Filtro para consultar las transaccions por cuenta CLABE.