```csharp
1 Passo - Criei as tabelas fornecidas pelo SQLite, sendo elas:

Produtos:
CREATE TABLE "produtos" (
	"id"	INTEGER NOT NULL,
	"descricao"	TEXT NOT NULL,
	"unidade"	TEXT NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);

pedidos: 
CREATE TABLE "pedidos" (
	"id"	INTEGER NOT NULL,
	"data"	DATA NOT NULL,
	"fornecedores_id"	ID,
	PRIMARY KEY("id" AUTOINCREMENT)
);

fornecedores:
CREATE TABLE "fornecedores" (
	"id"	INTEGER NOT NULL,
	"razao_social"	TEXT NOT NULL,
	"cnpj"	TEXT NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);

pedidos_itens:
pedidos itens:
CREATE TABLE "pedidos_itens" (
	"id"	INTEGER NOT NULL,
	"pedidos_id"	INTEGER NOT NULL,
	"produtos_id"	INTEGER NOT NULL,
	"quantidade"	FLOAT NOT NULL,
	"valor"	FLOAT NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);


*2 Passo: 

Imaginei as páginas do projeto, ficariam igual nessa imagem: https://imgur.com/FrXDn9U -- !!!!
quando adicionado uma descrição ou uma unidade no Banco de Dados, aparece na Pagina 2 em ordem de tabela.*


3 Passo:

Criei as funções para inserir os dados de cada tabela, sendo elas:

Produto:
public string InsertProduct(string description, string unit)
{
    string query = "INSERT INTO product (description, unit) VALUES ('" + description + "', '" + unit + "')";
    return query;
}

novo pedido:
public string InsertPedido(DateTime data, string fornecedorId)
{
    string query = "INSERT INTO pedidos (data, fornecedor_id) VALUES ('" + data.ToString("yyyy-MM-dd HH:mm:ss") + "', '" + fornecedorId + "')";
    return query;
}


fornecedores: 
public string InsertFornecedor(string razaoSocial, string cnpj)
{
    string query = "INSERT INTO fornecedores (razao_social, cnpj) VALUES ('" + razaoSocial + "', '" + cnpj + "')";
    return query;
}


pedidos_itens:
public string InsertPedidoItem(string pedidoId, string produtoId, int quantidade, decimal valor)
{
    string query = "INSERT INTO pedidos_itens (pedidos_id, produtos_id, quantidade, valor) VALUES ('" + pedidoId + "', '" + produtoId + "', " + quantidade + ", " + valor.ToString("0.00", CultureInfo.InvariantCulture) + ")";
    return query;
}

```

```sql

4 Passo:

Criei as funções que fazem a query solicitar os dados e apresentarem as informações, sendo elas:

Função para inserir um novo produto:
public string InsertProduct(string description, string unit)
{
    string query = "INSERT INTO products (description, unit) VALUES (@description, @unit)";
    return query;
}

using (SQLiteConnection conn = new SQLiteConnection(connectionString))
{
    conn.Open();
    using (SQLiteCommand cmd = new SQLiteCommand(InsertProduct(description, unit), conn))
    {
        cmd.Parameters.AddWithValue("@description", description);
        cmd.Parameters.AddWithValue("@unit", unit);
        int result = cmd.ExecuteNonQuery();
        if (result > 0)
        {
            Console.WriteLine("Produto inserido com sucesso!");
        }
        else
        {
            Console.WriteLine("Falha ao inserir produto.");
        }
    }
}

Função para obter informações de um fornecedor pelo ID:
public string GetFornecedorById(string id)
{
    string query = "SELECT * FROM fornecedores WHERE id = @id";
    return query;
}
using (SQLiteConnection conn = new SQLiteConnection(connectionString))
{
    conn.Open();
    using (SQLiteCommand cmd = new SQLiteCommand(GetFornecedorById(id), conn))
    {
        cmd.Parameters.AddWithValue("@id", id);
        using (SQLiteDataReader reader = cmd.ExecuteReader())
        {
            while (reader.Read())
            {
                Console.WriteLine("ID: {0}", reader["id"]);
                Console.WriteLine("Razão social: {0}", reader["razao_social"]);
                Console.WriteLine("CNPJ: {0}", reader["cnpj"]);
            }
        }
    }
}

Função para obter informações de todos os itens de um pedido pelo ID do pedido:
public string GetItensByPedidoId(string id)
{
    string query = "SELECT * FROM pedidos_itens WHERE pedidos_id = @id";
    return query;
}
using (SQLiteConnection conn = new SQLiteConnection(connectionString))
{
    conn.Open();
    using (SQLiteCommand cmd = new SQLiteCommand(GetItensByPedidoId(id), conn))
    {
        cmd.Parameters.AddWithValue("@id", id);
        using (SQLiteDataReader reader = cmd.ExecuteReader())
        {
            while (reader.Read())
            {
                Console.WriteLine("Pedido ID: {0}", reader["pedidos_id"]);
                Console.WriteLine("Produto ID: {0}", reader["produtos_id"]);
                Console.WriteLine("Quantidade: {0}", reader["quantidade"]);
                Console.WriteLine("Valor: {0}", reader["valor"]);
            }
        }
    }
}

Função para obter informações de todos os pedidos de um fornecedor pelo ID do fornecedor:
public string GetPedidosByFornecedorId(string id)
{
    string query = "SELECT * FROM pedidos WHERE fornecedor_id = @id";
    return query;
}
using (SQLiteConnection conn = new SQLiteConnection(connectionString))
{
    conn.Open();
    using (SQLiteCommand cmd = new SQLiteCommand(GetPedidosByFornecedorId(id), conn))
    {
        cmd.Parameters.AddWithValue("@id", id);
        using (SQLiteDataReader reader = cmd.ExecuteReader())
        {
            while (reader.Read())
            {
                Console.WriteLine("ID: {0}", reader["id"]);
                Console.WriteLine("Data: {0}", reader["data"]);
                Console.WriteLine("Fornecedor ID: {0}", reader["fornecedor_id"]);
            }
        }
    }
}
```
