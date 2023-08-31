# Ecommerce-Ifood

## Banco de Dados para Projeto Final do Curso Ciência de Dados by DIO - IFood


- [Desafio Parte 1: Criando o Diagrama no MysSQL Workbench](#diagrama-do-banco-de-dados)


- [Desafio Parte 2: Criando o Script das Tabelas e Constraints](#script-das-tabelas-e-constraints)


- [Desafio Parte 3: Inserindo Dados no Banco via MySQL](#inserindo-dados-no-banco-via-mysql)


- [Desafio Parte 4: Criando Queries](#desafio-parte-4---criando-queries)


## Desafio Parte 1 - Diagrama do Banco de Dados
[DIAGRAMA DO BANCO DE DADOS]![image](https://github.com/wellingtonb3/ecommerce-ifood/assets/130426959/ee1cd04e-43a8-4763-93ff-4bc6a208a4f8)

[VOLTAR](#ecommerce-ifood)

## Desafio Parte 2 - Script das Tabelas e Constraints

### Criando o Banco de Dados

```mysql
-- criação do banco de dados para o cenário de E-commerce

create database ecommerce1;
use ecommerce1;
```



### Criando a Tabela Cliente

```
create table clients(
	idClient int auto_increment primary key,
        type_client ENUM('Pessoa Física', 'Pessoa Jurídica') not null,
        cpf char(11),
        fname varchar(10),
        minit char(3),
        lname varchar(20),
        cnpj char(14),
        businessname varchar(255),
        address varchar(30),
        phone_number varchar(11) not null,
        constraint unique_cpf_client unique (cpf),
        constraint unique_cnpj_client unique (cnpj)
);

alter table clients auto_increment = 1;

desc clients;
```

------------------------------------------------------------------------

### Criando a Tabela Produto

```python
create table product(
	idProduct int auto_increment primary key,
	pname varchar (10) not null, 
	category enum("Eletrônicos","Vestuario","Brinquedos","Alimentos","Móveis") not null,
	for_kids bool default false,
	review float default 0,
	dimensions varchar(10)
);

alter table product auto_increment = 1;
```


--------------------------------------------------------------------------

### Criando a Tabela Pedido
```python
 create table orders(
	idOrder int auto_increment primary key,
	idOrderClient int,
	orderstatus ENUM("Cancelado", "Confirmado", "Em processamento") default "Em processamento",
	orderdescription varchar(255),
	freight float default 10,
	delivery ENUM ('Em preparação','Enviado', 'Entregue', 'Extraviado') default "Em preparação",
        delivery_number varchar(20),
        constraint fk_orders_client foreign key (idOrderClient) references clients(idClient)                
);

alter table orders auto_increment = 1;

desc orders;
```

-------------------------------------------------------------------------


### Criando a tabela Estoque
```python

create table productstock(
	idProdStock int auto_increment primary key,
	stocklocation varchar(255),
	quantity int default 0
);

alter table productstock auto_increment = 1;
```


-------------------------------------------------------------------------

### Criando a tabela Fornecedor
```python

create table supplier(
	idSupplier int auto_increment primary key,
	businessname varchar(255) not null,
	cnpj char(15) not null,
        phone_number varchar(11) not null,
        constraint unique_supplier unique (cnpj)
);
alter table supplier auto_increment = 1;
desc supplier;
```


-----------------------------------------------------------------------

### Criando a tabela Vendedor CNPJ
```python

create table seller(
	idSeller int auto_increment primary key,
	businessname varchar(255) not null,
        companyname varchar(255) not null,
        address varchar(255),
	cnpj char(15),
        cpf char(9),
        contact char(11) not null,
        constraint unique_cnpj_seller unique (cnpj),
        constraint unique_cpf_seller unique (cpf)
);

alter table seller auto_increment = 1;
```


-----------------------------------------------------------------------

### Criando a tabela Produtos Vendedor
```python

create table product_seller(
	idPseller int,
	idProduct int,
	prodquantity int default 1,
	primary key (idPseller, idProduct),
	constraint fk_product_seller foreign key (idPseller) references seller (idSeller),
	constraint fk_product_product foreign key (idProduct) references product (idProduct)
);

desc product_seller;
```


-------------------------------------------------------------------------

### Criando a tabela Produto / Pedido
```python

create table productorder(
	idPOproduct int,
	idPOorder int,
	poquantity int default 1,
	postatus enum ("Disponível","Sem estoque") default "Disponível",
	primary key (idPOproduct, idPOorder),
	constraint fk_productorder_seller foreign key (idPOproduct) references product(idProduct),
	constraint fk_productorder_product foreign key (idPOorder) references orders(idOrder)
);
```


--------------------------------------------------------------------------

### Criando a tabela Localização do Estoque
```python

create table stocklocation(
	idLproduct int,
	idLstock int,
	location varchar(255) not null,
	primary key (idLproduct, idLstock),
	constraint fk_stock_location_product foreign key (idLproduct) references product(idProduct),
	constraint fk_stock_location_stock foreign key (idLstock) references productStock(idProdStock)
    );
```



---------------------------------------------------------------------------


### Criando a tabela Produto Fornecedor
```python
create table productsupplier(
	idPsSupplier int,
	idPsProduct int,
	quantity int not null,
	primary key (idPsSupplier, idPsProduct),
	constraint fk_product_supplier_supplier foreign key (idPsSupplier) references supplier(idSupplier),
	constraint fk_product_supplier_product foreign key (idPsProduct) references product (idProduct)
);
desc productsupplier;
```


-----------------------------------------------------------------------------

### Criando a tabela Pagamento
```python
create table payment(
	idPayment int auto_increment primary key,
	idOPayment int,
	total_value decimal(10,2) not null,
	payment_date date not null,
	type_payment enum ("Boleto", "Cartão de Crédito", "Dois Cartões") not null default "Cartão de Crédito",
	card_number VARCHAR(16),
	expiration_date date,
	security_code varchar(3),
	bank_slipcode varchar(20),
	constraint fk_order_payment foreign key (idOPayment) references orders (idOrder),
	constraint fk_client_payment foreign key (idPayment) references clients (idClient)
);
alter table payment auto_increment = 1;

desc payment;
```


--------------------------------------------------------------------------

Comandos úteis
```
-- show tables;
-- show databases;
-- use information_schema;
-- show tables;
-- desc table_constraints;
-- desc referential_constraints;
-- select * from referential_constraints;
-- select * from referential_constraints where constraint_schema = 'ecommerce1';''''
```

[VOLTAR](#ecommerce-ifood)


## Desafio Parte 3 - Inserindo Dados no Banco via MySQL


-- inserção de dados
```
use ecommerce1;
show tables;
```
---------------------------------------------------------------------------

```
desc clients;
-- idClient, type_client, cpf, fname, minit, lname, cnpj, businessname, address, phone_number
insert into clients (type_client, cpf, fname, minit, lname, cnpj, businessname, address, phone_number) values
	('Pessoa_Física', '29537472910','Maria', 'E', 'Ferreira', null , null, 'Av Treze 41, Jamba - Gusmão', '31 45983385'),
        ('Pessoa_Jurídica', null, null, null, null, 45284901000321, 'Acording System', 'Rua Jota 32, Pamonhas - Jequi', '41 45983210'),
	('Pessoa_Física', '29347472910','Maria', 'E', 'Ferreira', null , null, 'Av Treze 41, Jamba - Gusmão', '31 45983385'),
        ('Pessoa_Física', '48302769178', 'José', 'G', 'Alandra', null, null, 'Rua Santa 52, Gara - Jarandi', '11 49603377'),
        ('Pessoa_Jurídica', null, null, null, null, 47295789000156, 'Pururu Aços', 'Rua Jota 32, Pamonhas - Jequi', '21 55983567'),
        ('Pessoa_Física', '48702769331', 'José', 'G', 'Alandra', null, null, 'Rua Dio 36, Ilha - Betiquita', '15 39563355'),
	('Pessoa_Jurídica', null, null, null, null, 29367912000177, 'Casa Móveis', 'Rua Gari 26, Camba - Jetu', '21 46333567');
          
select * from clients;
```

---------------------------------------------------------------------------

```
desc product;
-- idProduct, pname, category ('Eletrônicos','Vestuario','Brinquedos','Alimentos','Móveis'), for_kids boolean, review, dimensions(10)

insert into product (pname, category, for_kids, review, dimensions) values
	('Gabinete', 'Eletrônicos', false, '5', null),
        ('Bombons', 'Alimentos', false, '5', null),
        ('GeForce', 'Eletrônicos', false, '5', null),
        ('Estante', 'Móveis', false, '3', null),
        ('Super Mar', 'Brinquedos', true, '4', null),
        ('Calça Big', 'Vestuario', false, '4', null);

select * from product;
```

---------------------------------------------------------------------------

```
desc orders;
-- idOrder, idOrderClient, orderstatus, orderdescription, freight, delivery, delivery_number

insert into orders (idOrderClient, orderstatus, orderdescription, freight, delivery, delivery_number) values
	(8, 'Em processamento', 'Webserver3', 22, 'Em preparação', null ),
        (9, 'Confirmado', 'Webserver2', 27, 'Enviado', 'BC234654367BR'),
        (10, 'Em processamento', 'WebServer1', 12, 'Em preparação', 'AB376347656BR'),
        (11, 'Cancelado', 'Webserver2', 19, null, null);

select * from orders;
```

------------------------------------------------------------------------

```
desc productorder;
-- idPOproduct, idOorder, poquantity, postatus

insert into productorder (idPOproduct, idPOorder, poquantity, postatus) values
	(1,5,2,default),
        (2,6,1,default),
        (3,7,1,default);

select * from productorder;
```

---------------------------------------------------------------------------

```
desc productstock;
insert into productstock (stocklocation, quantity) values
	('São Paulo', 1000),
        ('Santa Catarina', 400),
        ('São Paulo', 250),
        ('Minas Gerais', 1000),
        ('Rio de Janeiro', 230),
        ('Alagoas', 3000);

select * from productstock;
```
                        
---------------------------------------------------------------------

```
desc stocklocation;
insert into stocklocation (idLproduct, idLstock, location) values
	(1, 2, 'SP'),
        (2, 5, 'MG');

select * from stocklocation;
```

----------------------------------------------------------------------

```
desc supplier;
-- idSupplier, businessname, cnpj, phone_number

insert into supplier (businessname, cnpj, phone_number)values
	('Lan Distribuidora',386556730001-54,'34 34664455'),
        ('Max Produtos',824556730001-54,'34 34664455'),
        ('STC Alimentos' ,453956730001-54,'34 34664455');

select * from supplier;
```

---------------------------------------------------------------------------

```
desc seller;
-- idSeller, businessname, companyname, address, cnpj, cpf, contact

insert into seller (businessname, companyname, address, cnpj, cpf, contact) values
	('Randi ltda', 'Devox', 'Rua Treze, 415 - Landau - Bahia', 543743540001-56, 346756712, 7134556677),
        ('DVX S/C', 'Ritx', 'Av Eng Heitor, 741 - Amazu - Santa Catarina', 562347450001-65, 744756722, 7139673474),
        ('Lauaz ltda ME', 'Sac', 'Rua Treze, 658 - Landau - Alagoas', 456783540001-16, 256756755, 5157445655);

select * from seller;   
```

-------------------------------------------------------------------------

```
desc product_seller;
-- idPseller, idProduct, prodquantity

insert into product_seller (idPseller, idProduct, prodquantity) values
	(1, 2, 4500),
        (1, 3, 1500),
        (3, 4, 440),
        (2, 5, 500),
        (1, 611, 100);

select * from product_seller;
```

----------------------------------------------------------------------

```
desc productsupplier;

insert into productsupplier (idPsSupplier, idPsProduct, quantity) values
	(1, 2, 4500),
        (1, 3, 1500),
        (3, 4, 440),
        (2, 5, 500),
        (1, 6, 100);

select * from productsupplier;
```

-------------------------------------------------------------------------

```
desc payment;

insert into payment (idOPayment, total_value, payment_date, type_payment, card_number, expiration_date, security_code, bank_slipcode) values
        (5, 229.00, '2023-04-12', default, 4533432275643544, '2027-05-01', 321, null),
        (6, 124.00, '2023-02-28', 'Boleto', null, null, null, 42376598765367845345),
        (7, 329.00, '2023-04-14', default, 1267432275643544, '2028-05-01', 221, null),					
        (8, 125.00, '2023-03-28', 'Boleto', null, null, null, 16776598765367845345);

select * from payment;
```

---------------------------------------------------------------------------


[VOLTAR](#ecommerce-ifood)


## Desafio Parte 4 - Criando Queries











[VOLTAR](#ecommerce-ifood)




