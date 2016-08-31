# Mondrian SQL filter on schema

Este repositório implementa a solução para um problema comum:

 - O analista quer restringir o acesso aos dados [rowLevel] baseado em uma dimensão com baixa cardinalidade [ex.: Filial];
 - Mas quando o usuário arrasta uma dimensão com alta cardinalidade, sem uma medida na visão analítica, é possível ver os membros da dimensão. Ex.: Num.Contrato.

 Essa solução depende do Cubeguard - que já tem um branch [pentaho6].

# Exemplo

 Para implantar o exemplo:

  1. Instale o cubeguard no seu branch pentaho6;
  2. Copie o arquivo `endpoints/allschema.ktr` para `cubeguard/endpoints/kettle/`
  3. Atualize a api do cubeguard `http://localhost:8080/pentaho/plugin/cubeguard/api/refresh`;
  4. Habilite o Cubeguard para o schema SteelWheels;
  5. Escolha o endpoint `allschema` para o Schema SteelWheels;
  6. Altere o schema do SteelWheels para o contido na pasta deste repositório `schema/SteelWheelsSales.mondrian.xml`
  7. Abra a transformação `etl/load_security_table.ktr`;
  8. Abra o step de output e no SQL, coloque o conteúdo de `db/example-table.sql`;
  9. Execute a transformação [certifique-se de que o bi-server está rodando e teste a conexão. Essa transf. roda com o usuário de banco `sa`];

Pronto.

# Funcionamento

A query de exemplo, filtra a tabela de fatos, atraavés de inner join.

Se o usuário for `admin`, a consulta faz bypass e traz todos os resultados.

Se o usuário for `suzy` - o único usuário para o qual inserimos regras - então ela só vai ver APAC e EMEA como membros de territory. Assim como todas as outras dimenões vão ester intrinsicamente filtradas, pois estamos restringindo os registros da tabela de fatos.
