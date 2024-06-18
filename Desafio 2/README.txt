Primeira coisa a ser feita, após ter criado o banco de dados no mysql
fui inserir os dados na tabela porém, todos os comandos de insert deram
o "Error Code 1452", em que basicamente eu teria que ter a os valores da chave estrangeria já 
inserida em outra tabela para conseguir inserir os dados na tabela 
que eu queria, então tive que usar o comando SET FOREIGN_KEY_CHECKS =0;
para inserir os dados para depois desabilitar.

Alterações da tabela:

dependent -> Nome da coluna Essn alterado para Ssn

dept_location ->

employee -> posição das colunas Lname, Ssn e Super_ssn alteradas para facilitar visualização dos dados importantes
            nome da coluna Dno alterado para Dnumber

project -> tipo da coluna Salary alterado para decimal fixo
	   nome da coluna Dnum alterado para Dnumber

works_on -> nome da coluna Pno alterado para Pnumber
	    nome da coluna Essn alterado para Ssn

Departament -> coluna mgr_ssn alterado para Ssn



Identificado o colaborador James como gerente. Porque? os colaboradores John, Joyce, Ramesh, Ahmad e Alicia,
tem Super_ssn em comum, que levam a apenas dois colaboradores que são: Franklin e Jennifer, ou seja, são gerentes dos mesmos, 
e os dois gerentes tem um Super-ssn em comum, o James, o qual não tem Super_ssn, ou seja, o último da hierarquia (gerente)

Identificado um valor de horas zerado na tabela works_on, que por coincidência é o unico com Ssn = ao Ssn do gerente. 
o número do projeto é o 20, que é de reorganização, e os únicos presentes são os dois sub-gerentes e o próprio gerente, o qual não tem
horas atribuidas.


Todos os departamentos tem gerente.

Coluna Adress da tabela Employee dividada por "-" nas colunas Adress-Number,
Adress-Street, Adress-City e Adress-State


Mescladas as colunas employee com department, percebo que alguns colaboradores estão sem departamento
, os colaboradores sem departamentos são os colaboradores que estão 
na parte mais baixa da hierarquia, ou seja, os seus departamentos são
os mesmos departamentos dos seus Super_ssn, então refiz ela utilizando a mesclagem por Dnumber, 
mesclagem realizada e colunas irrelevantes excluidas da tabela mesclada.

Porque mesclagem e não Atribuição? A mesclagem é utilizada para criar novas tabelas como se fossem um "join" e criar tabelas com uma visão inter-relacionada. 
A atribuição é utilizada em casos em que é preciso criar cálculos adicionais e adicionar ou modificar colunas existentes a fim de fazer novas métricas


Consulta SQL para saber o nome dos gerentes de cada colaborador: 
Select f.Fname as colab_name, e.Fname as manager_name  from employee as e inner join employee as f where e.Ssn = f.Super_ssn;

Criada a coluna Full_Name utilizando a função DAX CONCATENATE(CONCATENATE('azure_ningas employee'[Fname]," "),'azure_ningas employee'[Lname])

Criada a coluna mesclada de Departament e dept_locations e criada a coluna Dept_loc com a concatenação da Dname e Dlocation
para diminuir futuros problemas entre relacionamento muito para muitos

Criada a coluna gerente através de uma mesclagem de employee com employee. 

Colunas não utilizadas removidas