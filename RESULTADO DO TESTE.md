```C#
// Produtos: //

CREATE TABLE "produtos" (
	"id"	INTEGER NOT NULL,
	"descricao"	TEXT NOT NULL,
	"unidade"	TEXT NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);

// pedidos: //

CREATE TABLE "pedidos" (
	"id"	INTEGER NOT NULL,
	"data"	DATA NOT NULL,
	"fornecedores_id"	ID,
	PRIMARY KEY("id" AUTOINCREMENT)
);

// fornecedores: //

CREATE TABLE "fornecedores" (
	"id"	INTEGER NOT NULL,
	"razao_social"	TEXT NOT NULL,
	"cnpj"	TEXT NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);

// pedidos itens: //

CREATE TABLE "pedidos_itens" (
	"id"	INTEGER NOT NULL,
	"pedidos_id"	INTEGER NOT NULL,
	"produtos_id"	INTEGER NOT NULL,
	"quantidade"	FLOAT NOT NULL,
	"valor"	FLOAT NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);

```

