---
title: Affecter des variables dans SQL Data Warehouse | Microsoft Docs
description: Conseils relatifs à l’affectation de variables Transact-SQL dans Microsoft Azure SQL Data Warehouse, dans le cadre du développement de solutions.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/14/2016
ms.author: jrj;barbkess;sonyama

---
# Affecter des variables dans SQL Data Warehouse
Dans SQL Data Warehouse, les variables sont définies au moyen de l’instruction `DECLARE` ou `SET`.

Pour définir une valeur de variable, il existe différentes méthodes, parfaitement valables :

## Définition de variables via l’instruction DECLARE
Dans SQL Data Warehouse, l’initialisation de variables avec l’instruction DECLARE constitue l’une des méthodes les plus flexibles pour définir une valeur de variable.

```sql
DECLARE @v  int = 0
;
```

De plus, vous pouvez utiliser cette instruction pour définir plusieurs variables à la fois. En effet, vous ne pouvez pas utiliser les éléments `SELECT` et `UPDATE` pour exécuter ceci :

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

De plus, il n’est pas possible d’initialiser et d’utiliser une variable au sein de la même instruction DECLARE. Illustrons notre propos : la commande de l’exemple ci-dessous n’est **pas** autorisée, car l’élément « @p1 » est initialisé, mais également utilisé dans la même instruction DECLARE. Cela entraîne une erreur.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## Définition de valeurs avec l’instruction SET
L’instruction SET est très couramment utilisée pour définir une variable unique.

Tous les exemples ci-dessous illustrent des modes de définition d’une variable avec l’instruction SET, qui sont parfaitement valables :

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Avec l’instruction SET, vous pouvez uniquement définir une variable à la fois. Toutefois, comme indiqué ci-dessus, les opérateurs composés sont autorisés.

## Limitations
Vous ne pouvez pas utiliser les instructions SELECT et UPDATE pour attribuer des variables.

## Étapes suivantes
Pour obtenir des conseils supplémentaires en matière de développement, voir la [vue d’ensemble sur le développement][vue d’ensemble sur le développement].

<!--Image references-->

<!--Article references-->
[vue d’ensemble sur le développement]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

<!---HONumber=AcomDC_0629_2016-->