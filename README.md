## Inclusão de Funcionários e Consultas SQL

### Inclua suas próprias informações no departamento de tecnologia da empresa

```sql
INSERT INTO funcionarios(funcionario_id,primeiro_nome,sobrenome,email,senha,telefone,data_contratacao,cargo_id,salario,gerente_id,departamento_id) 
VALUES (208,'Jéssica','Souza','jessisouzatech@gmail.com','65703','11958300920','2020-03-31',9,50000.00,NULL,13);
```

1.Quantos funcionários temos ao total na empresa?

```sql
SELECT COUNT(*) from momento.funcionarios;
````

2.Quantos funcionários temos no departamento de finanças?

```sql
SELECT COUNT(*) from momento.funcionarios WHERE departamento_id = 10;
```

3.Qual a média salarial do departamento de tecnologia?

```sql
SELECT SUM(salario) FROM funcionarios WHERE departamento_id = 5;
```

4.Adição de um novo departamento: Inovações

```sql
INSERT INTO departamentos (departamento_id, departamento_nome, escritorio_id)
VALUES (14, 'Inovações',  (SELECT escritorio_id FROM escritorios WHERE pais_id = 'BR'));
```

5.Contratação de três novos funcionários para o departamento de Inovações

```sql
SET @avg_salario = (SELECT AVG(salario) FROM funcionarios WHERE departamento_id = 1);
INSERT INTO funcionarios (funcionario_id, primeiro_nome, sobrenome, email, senha, telefone, data_contratacao, cargo_id, salario, gerente_id, departamento_id)
VALUES 
(NULL, 'Maria', 'Rosário', 'maria.rosario@gmail.com', 'PS987654321', '1234567890', '2022-01-01', 9, @avg_salario, NULL, 14),
(NULL, 'Agatha', 'Souza', 'agatha.souza@gmail.com', 'PS987654321', '1234567890', '2022-01-01', 9, @avg_salario, NULL, 14),
(NULL, 'Yasmim', 'Silva', 'yasmim.silva@gmail.com', 'PS987654321', '1234567890', '2022-01-01', 14, @avg_salario, NULL, 14);
```

6.Adição de dependentes dos novos funcionários

```sql
INSERT INTO dependentes (dependente_id, primeiro_nome, sobrenome, relacionamento, funcionario_id)
VALUES 
(NULL, 'Pedro', 'Ribeiro', 'Cônjuge', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Maria' AND sobrenome = 'Rosário')),
(NULL, 'Griselda', 'Ribeiro Rosário', 'Filha', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Maria' AND sobrenome = 'Rosário')),
(NULL, 'Helen', 'Souza', 'Cônjuge', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Agatha' AND sobrenome = 'Souza')),
(NULL, 'Beatriz', 'Souza', 'Filha', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Agatha' AND sobrenome = 'Souza')),
(NULL, 'Millena', 'Silva', 'Filha', (SELECT funcionario_id FROM funcionarios WHERE primeiro_nome = 'Yasmim' AND sobrenome = 'Silva'));
```

7.Informe todas as regiões em que a empresa atua acompanhadas de seus países.

```sql

SELECT r.regiao_nome, p.pais_nome
FROM regioes r
JOIN paises p ON r.regiao_id = p.regiao_id
ORDER BY r.regiao_nome, p.pais_nome;
```

8.Joe Sciarra é filho de quem?

```sql
SELECT * FROM dependentes d INNER JOIN funcionarios f ON d.funcionario_id = f.funcionario_id WHERE d.primeiro_nome = 'Joe' AND d.sobrenome = 'Sciarra';
```

9.Jose Manuel possui filhos?

```sql
SELECT * FROM dependentes d INNER JOIN funcionarios f ON d.funcionario_id = f.funcionario_id WHERE f.primeiro_nome = 'Jose Manuel';
```

10.Qual região possui mais países?

```sql
SELECT r.regiao_nome, COUNT(p.pais_nome) AS num_paises
FROM regioes r
JOIN paises p ON r.regiao_id = p.regiao_id
GROUP BY r.regiao_nome
ORDER BY num_paises DESC LIMIT 1;
```

11.Exiba o nome de cada funcionário acompanhado de seus dependentes.

```sql
SELECT f.primeiro_nome AS nome_responsavel, f.sobrenome AS sobrenome_responsavel, d.primeiro_nome AS nome_dependente, d.sobrenome AS sobrenome_dependente, d.relacionamento
FROM funcionarios f
INNER JOIN dependentes d ON f.funcionario_id = d.funcionario_id;
```

12.Karen Partners possui um(a) cônjuge?

```sql
SELECT * FROM dependentes d INNER JOIN funcionarios f ON d.funcionario_id = f.funcionario_id
```
