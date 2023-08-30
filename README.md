### Ecommerce-Ifood

## Banco de Dados para Projeto Final do Curso Ciência de Dados by DIO - IFood


- [Desafio Parte 1: Criando o Diagrama no MysSQL Workbench](#diagrama-do-banco-de-dados)


- [Desafio Parte 2: Criando o Script das Tabelas e Constraints](#script-das-tabelas-e-constraints)


- [Tópico 3: Tecnologias Inovadoras](#tópico-3-tecnologias-inovadoras)

## Diagrama do Banco de Dados
[DIAGRAMA DO BANCO DE DADOS]![image](https://github.com/wellingtonb3/ecommerce-ifood/assets/130426959/ee1cd04e-43a8-4763-93ff-4bc6a208a4f8)

[VOLTAR](#ecommerce-ifood)

## Script das Tabelas e Constraints

-- criação do banco de dados para o cenário de E-commerce
-- drop database ecommerce1;
create database ecommerce1;
use ecommerce1;


------------------------------------------------------------------------


```python criar tabela cliente
-- create table clients(
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

desc clients;```

--------------------------------------------------------------------------


```python-- criar tabela produto
	create table product(
	idProduct int auto_increment primary key,
	pname varchar (10) not null, 
	category enum("Eletrônicos","Vestuario","Brinquedos","Alimentos","Móveis") not null,
	for_kids bool default false,
	review float default 0,
	dimensions varchar(10)
);

alter table product auto_increment = 1;```


--------------------------------------------------------------------------


```python-- criar tabela pedido

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

desc orders;```


-------------------------------------------------------------------------


```python-- criar tabela estoque

	create table productstock(
	idProdStock int auto_increment primary key,
	stocklocation varchar(255),
	quantity int default 0
);

alter table productstock auto_increment = 1;```


-------------------------------------------------------------------------


```python-- criar tabela fornecedor

	create table supplier(
	idSupplier int auto_increment primary key,
	businessname varchar(255) not null,
	cnpj char(15) not null,
        phone_number varchar(11) not null,
        constraint unique_supplier unique (cnpj)
);
alter table supplier auto_increment = 1;
desc supplier;```


-----------------------------------------------------------------------


```python-- criar tabela vendedor CNPJ

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

alter table seller auto_increment = 1;```


-----------------------------------------------------------------------


```python-- criar tabela produtos_vendedor

create table product_seller(
	idPseller int,
	idProduct int,
	prodquantity int default 1,
	primary key (idPseller, idProduct),
	constraint fk_product_seller foreign key (idPseller) references seller (idSeller),
	constraint fk_product_product foreign key (idProduct) references product (idProduct)
);

desc product_seller;```


-------------------------------------------------------------------------


```python-- criar tabela Produto/pedido

create table productorder(
	idPOproduct int,
	idPOorder int,
	poquantity int default 1,
	postatus enum ("Disponível","Sem estoque") default "Disponível",
	primary key (idPOproduct, idPOorder),
	constraint fk_productorder_seller foreign key (idPOproduct) references product(idProduct),
	constraint fk_productorder_product foreign key (idPOorder) references orders(idOrder)
);```


--------------------------------------------------------------------------


```python-- criar tabela Localização do Estoque
create table stocklocation(
	idLproduct int,
	idLstock int,
	location varchar(255) not null,
	primary key (idLproduct, idLstock),
	constraint fk_stock_location_product foreign key (idLproduct) references product(idProduct),
	constraint fk_stock_location_stock foreign key (idLstock) references productStock(idProdStock)
    );```



---------------------------------------------------------------------------



```python-- criar tabela Produto Fornecedor
create table productsupplier(
	idPsSupplier int,
	idPsProduct int,
	quantity int not null,
	primary key (idPsSupplier, idPsProduct),
	constraint fk_product_supplier_supplier foreign key (idPsSupplier) references supplier(idSupplier),
	constraint fk_product_supplier_product foreign key (idPsProduct) references product (idProduct)
);
desc productsupplier;```


-----------------------------------------------------------------------------


```python-- criar tabela Pagamento
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

desc payment;```


--------------------------------------------------------------------------


-- show tables;
-- show databases;
-- use information_schema;
-- show tables;
-- desc table_constraints;
-- desc referential_constraints;
-- select * from referential_constraints;
-- select * from referential_constraints where constraint_schema = 'ecommerce1';''''


[VOLTAR](#ecommerce-ifood)

# Minha Página Interessante

[VOLTAR](#ecommerce-ifood)

## Introdução
Bem-vindo à minha página interessante! Nesta página, vou compartilhar informações fascinantes sobre diversos tópicos.

## Tópico 1: Curiosidades sobre o Universo
Nesta seção, exploraremos fatos incríveis sobre o cosmos.as


### A Via Láctea
Saiba mais sobre a nossa galáxia e suas características.

### Buracos Negros
Descubra como os buracos negros se formam e seu papel no espaço.

## Tópico 2: Maravilhas Naturais
A natureza é repleta de fenômenos incríveis.

### Aurora Boreal
Aprenda sobre o espetáculo de luzes no céu ártico.

### Cachoeiras Gigantes
Conheça algumas das cachoeiras mais impressionantes do mundo.

## Tópico 3: Tecnologias Inovadoras
Explore as últimas inovações tecnológicas.asdfasdfs
#tópico-1-curiosidades-sobre-o-universo

### Inteligência Artificial
Descubra como a IA está transformando nosso mundo.

### Realidade Virtual
Saiba como a realidade virtual está revolucionando a forma como interagimos com a tecnologia.

## Conclusão
Espero que você tenha desfrutado das informações apresentadas nesta página. Sinta-se à vontade para explorar mais sobre esses tópicos incríveis!

---











[VOLTAR](#ecommerce-ifood)




















































### ecommerce-ifood2

G
DFG
F
F
F
F
F
F
F
F
F
F


F
F
F
F
F
F
F
F
F
F
F
F
F
### ecommerce-ifood2
