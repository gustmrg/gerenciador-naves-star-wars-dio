# Gerenciador de Espaço Naves do Star Wars

### Atividade do Bootcamp da Digital Innovation One

Scripts de criação das tabelas no banco de dados em SQL Server.

Antes de tudo, precisamos criar o banco de dados, para então criar as tabelas.
Podemos fazer isso usando o seguinte comando:

`CREATE DATABASE EstrelaDaMorte;`

Em seguida, selecionamos o banco de dados e criamos a nossa primeira tabela que irá guardar as informações dos Planetas:

```
USE EstrelaDaMorte

CREATE TABLE Planetas(
	IdPlaneta int NOT NULL,
	Nome varchar(50) NOT NULL,
	Rotacao float NOT NULL,
	Orbita float NOT NULL,
	Diametro float NOT NULL,
	Clima varchar(50) NOT NULL,
	Populacao int NOT NULL,
)
GO
ALTER TABLE Planetas ADD CONSTRAINT PK_Planetas PRIMARY KEY (IdPlaneta);
GO
```

A próxima tabela a ser criada será a que guardará os dados das Naves. Executamos o seguinte comando:

```
CREATE TABLE Naves(
	IdNave int NOT NULL,
	Nome varchar(100) NOT NULL,
	Modelo varchar(150) NOT NULL,
	Passageiros int NOT NULL,
	Carga float NOT NULL,
	Classe varchar(100) NOT NULL,
)
GO
ALTER TABLE Naves ADD CONSTRAINT PK_Naves PRIMARY KEY (IdNave);
GO
```

Em seguinda, criamos a tabela dos dados dos Pilotos, adicionando também as chaves que irão fazer os relacionamentos entre tabelas:
```
CREATE TABLE Pilotos(
	IdPiloto int NOT NULL,
	Nome varchar(200) NOT NULL,
	AnoNascimento varchar(10) NOT NULL,
	IdPlaneta int NOT NULL,
)
GO
ALTER TABLE Pilotos ADD CONSTRAINT PK_Pilotos PRIMARY KEY (IdPiloto);
GO
ALTER TABLE Pilotos  ADD  CONSTRAINT FK_Pilotos_Planetas FOREIGN KEY(IdPlaneta)
REFERENCES Planetas (IdPlaneta)
GO
ALTER TABLE Pilotos CHECK CONSTRAINT FK_Pilotos_Planetas
GO
```

É preciso ainda criar uma tabela intermediária para nos ajudar a criar o relacionamento de Pilotos e Naves:

```
CREATE TABLE PilotosNaves(
	IdPiloto int NOT NULL,
	IdNave int NOT NULL,
	FlagAutorizado bit NOT NULL,
)
GO
ALTER TABLE PilotosNaves ADD CONSTRAINT PK_PilotosNaves PRIMARY KEY (IdPiloto, IdNave);
GO
ALTER TABLE PilotosNaves  ADD CONSTRAINT FK_PilotosNaves_Pilotos FOREIGN KEY(IdPiloto)
REFERENCES Pilotos (IdPiloto)
GO
ALTER TABLE PilotosNaves  ADD CONSTRAINT FK_PilotosNaves_Naves FOREIGN KEY(IdNave)
REFERENCES Naves (IdNave)
GO
ALTER TABLE PilotosNaves  ADD CONSTRAINT DF_PilotosNaves_FlagAutorizado  DEFAULT (1) FOR FlagAutorizado
GO
```
Por fim, criaremos uma tabela que irá guardar os dados das Viagens para podermos ter controle de quais naves estão disponíveis ou não.
```
CREATE TABLE HistoricoViagens(
	IdNave int NOT NULL,
	IdPiloto int NOT NULL,
	DtSaida datetime NOT NULL,
	DtChegada datetime NULL
)
GO

ALTER TABLE HistoricoViagens  ADD  CONSTRAINT FK_HistoricoViagens_PilotosNaves FOREIGN KEY(IdPiloto, IdNave)
REFERENCES PilotosNaves (IdPiloto, IdNave)
GO

ALTER TABLE HistoricoViagens CHECK CONSTRAINT FK_HistoricoViagens_PilotosNaves
GO
```