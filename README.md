# TP2 SQL Oracle
## Exercice 6

1. Ecrivez pour le département des ressources humaines une interrogation produisant l'adresse de tous les départements. Utilisez les tables `LOCATIONS` et `COUNTRIES`. Affichez dans les résultats l'ID de lieu, la rue, la ville, le département et le pays. Utilisez une jointure naturelle (NATURAL JOIN) pour produire les résultats.

```sql
SELECT 
    location_id,
    street_address,
    city,
    state_province,
    country_name
FROM locations
NATURAL JOIN countries;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/1.png)

2. Le département des ressources humaines a besoin d'un état de tous les employés. Ecrivez une interrogation permettant d'afficher le nom, ainsi que le numéro et le nom de département, pour tous les employés.

```sql
SELECT
    e.last_name,
    d.department_id,
    d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/2.png)

3. Le département des ressources humaines a besoin d'un état des employés de Toronto. Affichez le nom, le poste, ainsi que le numéro et le nom de département, pour tous les employés qui travaillent à Toronto.

```sql
SELECT
    e.last_name,
    e.job_id,
    d.department_id,
    d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id
INNER JOIN locations l
ON d.location_id = l.location_id
WHERE City = 'Toronto';
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/3.png)

4. Créez un état permettant d'afficher le nom et le numéro des employés, ainsi que le nom et le numéro de leur manager. Intitulez respectivement les colonnes `Employee`, `Emp#`, `Manager et Mgr#`. Enregistrez l'instruction SQL sous le nom ex_06_04.sql. Exécutez l'interrogation.

```sql
SELECT
    e.last_name as Employee,
    e.employee_id as "Emp#",
    m.last_name as "Manager",
    m.employee_id as "Mgr#"
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/4.png)

## Exercice 7

1. Créez un état qui affiche le numéro d'employé, le nom et le salaire de tous les employés qui gagnent plus que le salaire moyen. Triez les résultats par ordre croissant sur la base du salaire.

```sql
SELECT
    e.employee_id,
    e.last_name,
    e.salary,
    b.mean_salary
FROM employees e,(SELECT ROUND(AVG(salary),0) as mean_salary
                FROM employees) b
WHERE e.salary > b.mean_salary
ORDER BY salary;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/5.png)

2. Ecrivez une interrogation qui affiche le numéro d'employé et le nom de tous les employés qui travaillent dans un département comprenant un employé dont le nom contient la lettre "u". Enregistrez l'instruction SQL sous le nom ex_07_03.sql. Exécutez votre interrogation.

```sql
SELECT
    employee_id,
    last_name
FROM employees
WHERE department_id IN 
(
    SELECT DISTINCT
    department_id
    FROM employees
    WHERE last_name LIKE '%u%'
);
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/6.png)

3. Le département des ressources humaines a besoin d'un état qui affiche le nom, l'ID de département et l'ID de poste de tous les employés dont l'ID de lieu de département est 1700.

```sql
SELECT 
    e.last_name,
    e.department_id,
    e.job_id
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id
WHERE d.location_id = 1700;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/7.png)

4. Créez pour les ressources humaines un état affichant le nom et le salaire de tous les employés dont le manager est King.

```sql
SELECT
    last_name,
    salary
FROM employees 
WHERE manager_id IN 
    (SELECT
            employee_id
     FROM employees
     WHERE last_name = 'King');
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/8.png)

5. Créez pour les ressources humaines un état affichant l'ID de département, le nom et l'ID de poste de tous les employés du département Executive

```sql
SELECT
    department_id,
    last_name,
    job_id
FROM employees
WHERE department_id IN 
    (SELECT department_id
    FROM departments
    WHERE department_name = 'Executive');
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/9.png)

6. Modifiez l'interrogation ex_07_03.sql pour afficher le numéro d'employé, le nom et le salaire de tous les employés qui gagnent plus que le salaire moyen et qui travaillent dans un département comprenant un employé dont le nom contient la lettre "u".

```sql
SELECT
    employee_id,
    last_name,
    salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees)
AND department_id IN (SELECT DISTINCT department_id FROM employees WHERE last_name LIKE '%u%');
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/10.png)

## Exercice 8

1. Le département des ressources humaines a besoin de la liste des ID des départements qui ne contiennent pas l'ID de poste `ST_CLERK`. Utilisez les opérateurs ensemblistes pour créer cet état.

```sql
SELECT department_id
FROM departments
MINUS
SELECT department_id
FROM employees
WHERE job_id = 'ST_CLERK'
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/11.png)

2. Le département des ressources humaines a besoin de la liste des pays dans lesquels il n'existe aucun département. Affichez l'ID et le nom des pays. Utilisez les opérateurs ensemblistes pour créer cet état.

```sql
SELECT c.country_id,c.country_name
FROM countries c
MINUS
SELECT DISTINCT c.country_id,c.country_name
FROM countries c
LEFT JOIN locations l
ON c.country_id = l.country_id
LEFT JOIN departments d
ON l.location_id = d.location_id
WHERE d.department_id IS NOT NULL
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/12.png)

3. Produisez la liste des postes des départements 10, 50 et 20, dans cet ordre. Affichez l'ID de poste et l'ID de département à l'aide des opérateurs ensemblistes.

```sql
SELECT DISTINCT job_id, department_id
FROM employees
WHERE department_id = 10
UNION ALL
SELECT DISTINCT job_id, department_id
FROM employees
WHERE department_id = 50
UNION ALL
SELECT DISTINCT job_id, department_id
FROM employees
WHERE department_id = 20
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/13.png)

4. Créez un état répertoriant l'ID d'employé et l'ID de poste des employés dont l'intitulé de poste actuel est identique à l'intitulé de poste initial lors de leur embauche par l'entreprise. (Ces employés ont changé de poste, puis sont revenus à leur poste d'origine.)

```sql
SELECT employee_id,job_id
FROM employees
INTERSECT
SELECT employee_id,job_id
FROM job_history
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/14.png)

5. Le département des ressources humaines a besoin d'un état avec les spécifications suivantes : Nom et ID de département de tous les employés de la table `EMPLOYEES`, qu'ils appartiennent ou non à un département. ID et nom de tous les départements de la table `DEPARTMENTS`, qu'ils comptent des employés ou non. Pour ce faire, écrivez une interrogation composée.

```sql
SELECT last_name,department_id,TO_CHAR(NULL)
FROM employees
UNION
SELECT TO_CHAR(NULL),department_id,department_name
FROM departments
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/15.png)

## Exercice 9

1. Créer un script ex_09_01.sql pour générer la création de la table `MY_EMPLOYEE`, avec la structure suivante :

![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/16.png)

```sql
CREATE TABLE MY_EMPLOYEE
(
    ID NUMBER(4) NOT NULL,
    LAST_NAME VARCHAR2(25),
    FIRST_NAME VARCHAR2(25),
    USERID VARCHAR2(8),
    SALARY NUMBER(9,2)
)
```

2. Créez une instruction INSERT permettant d'ajouter à la table `MY_EMPLOYEE` la première ligne de données du tableau ci-après. N'énumérez pas les colonnes dans la clause INSERT. N'entrez pas encore toutes les lignes. 

```sql
INSERT INTO MY_EMPLOYEE VALUES(1,'Patel','Ralph','rpatel',895);
```

3. Insérez dans la table MY_EMPLOYEE la deuxième ligne de données du tableau qui précède. Cette fois, énumérez les colonnes de façon explicite dans la clause INSERT

```sql
INSERT INTO MY_EMPLOYEE(ID,LAST_NAME,FIRST_NAME,USERID,SALARY) VALUES(2,'Dancs','Betty','bdancs',860);
```
4. Vérifiez les ajouts effectués dans la table.

```sql
SELECT *
FROM MY_EMPLOYEE
WHERE ID IN (1,2);
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/17.png)

5. Ecrivez dans un fichier script réutilisable et dynamique une instruction INSERT permettant de charger les lignes restantes dans la table MY_EMPLOYEE. Le script doit afficher une invite pour toutes les colonnes (`ID`, `LAST_NAME`, `FIRST_NAME`, `USERID` et `SALARY`).

```sql
INSERT INTO MY_EMPLOYEE(ID,LAST_NAME,FIRST_NAME,USERID,SALARY)
VALUES (:ID,:LAST_NAME,:FIRST_NAME,:USERID,:SALARY);
```

6. Insérez dans la table les deux lignes suivantes du tableau de données fourni à l'étape 3 en exécutant l'instruction INSERT du script que vous avez créé.

![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/18.png)

![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/19.png)

7. Vérifiez les ajouts effectués dans la table

```sql
SELECT * FROM MY_EMPLOYEE;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/20.png)

8. Rendez définitifs les ajouts de données. Mettez à jour et supprimez des données dans la table MY_EMPLOYEE.

```sql
COMMIT;
```

9. Remplacez le nom de l'employé 3 par Drexler.

```sql
UPDATE MY_EMPLOYEE
SET LAST_NAME = 'Drexler'
WHERE ID = 3;
```
Vérifiez les modifications apportées à la table

```sql
SELECT * FROM MY_EMPLOYEE WHERE ID = 3;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/21.png)

10. Remplacez par 1 000 $ le salaire de tous les employés qui ont un salaire inférieur à 900 $.

```sql
UPDATE MY_EMPLOYEE
SET SALARY = 1000
WHERE SALARY < 900;
```
Vérifiez les modifications apportées à la table
```sql
SELECT * FROM MY_EMPLOYEE;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/22.png)

11. Supprimez Betty Dancs de la table `MY_EMPLOYEE`.

```sql
DELETE FROM MY_EMPLOYEE
WHERE LAST_NAME = 'Dancs' AND FIRST_NAME = 'Betty';
```
12. Vérifiez les modifications apportées à la table
```sql
SELECT * FROM MY_EMPLOYEE;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/23.png)

## Exercice 10

1. Créez la table `DEPT` conformément aux indications du tableau ci-après. Enregistrez l'instruction dans un script nommé ex_10_01.sql, puis exécutez ce script pour créer la table.

![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/24.png)

```sql
CREATE TABLE DEPT
(
    ID NUMBER(7),
    NAME VARCHAR2(25),
    CONSTRAINT ID_PKK PRIMARY KEY (ID)
);
```
Vérifiez que la table a bien été créée.
```sql
DESCRIBE DEPT;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/25.png)

2. Remplissez la table `DEPT` avec les données de la table `DEPARTMENTS`. Inclure uniquement les colonnes dont vous avez besoin

```sql
INSERT INTO DEPT(ID,NAME)
SELECT DEPARTMENT_ID,DEPARTMENT_NAME
FROM DEPARTMENTS;
```
```sql
SELECT * FROM DEPT;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/26.png)

3. Créez la table `EMP` conformément aux indications du tableau ci-après. Enregistrez l'instruction dans un script nommé ex_10_03.sql, puis exécutez ce script pour créer la table.
 
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/27.png)

```sql
CREATE TABLE EMP
(
    ID NUMBER(7),
    LAST_NAME VARCHAR2(25),
    FIRST_NAME VARCHAR2(25),
    DEPT_ID NUMBER(7),
    CONSTRAINT DEPT_ID_FK FOREIGN KEY (DEPT_ID) REFERENCES DEPT(ID)
);
```
Vérifiez que la table a bien été créée.
```sql
DESCRIBE EMP;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/28.png)

4. Créez la table `EMPLOYEES2` en utilisant la structure de la table `EMPLOYEES`. Inclure uniquement les colonnes `EMPLOYEE_ID`, `FIRST_NAME`, `LAST_NAME`, `SALARY` et `DEPARTMENT_ID`. Intitulez les colonnes de la nouvelle table respectivement `ID`, `FIRST_NAME`, `LAST_NAME`, `SALARY` et `DEPT_ID`.

```sql
DESCRIBE EMPLOYEES;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/29.png)

```sql
CREATE TABLE EMPLOYEES2
(
    ID NUMBER(6,0),
    FIRST_NAME VARCHAR2(20) NOT NULL,
    LAST_NAME VARCHAR2(25) NOT NULL,
    SALARY NUMBER(8,2),
    DEPT_ID NUMBER(4,0),
    CONSTRAINT EMPLOYEE2_ID_PK PRIMARY KEY (ID),
    CONSTRAINT EMPLOYEES2_SALARY_CK CHECK (SALARY > 0),
    CONSTRAINT EMPLOYEE2_DEPT_ID_FK FOREIGN KEY (DEPT_ID) REFERENCES DEPARTMENTS(DEPARTMENT_ID)
);
```
```sql
DESCRIBE EMPLOYEES2;
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/30.png)

5. Placez la table `EMPLOYEES2` en mode lecture seule.

```sql
ALTER TABLE EMPLOYEES2 READ ONLY;
```

6. Essayez d'insérer la ligne suivante dans la table `EMPLOYEES2` :

![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/31.png)

```sql
INSERT INTO EMPLOYEES2(ID,FIRST_NAME,LAST_NAME,SALARY,DEPT_ID)
VALUES(34,'Grant','Marcie',5678,10);
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/32.png)

7. Remettez la table `EMPLOYEES2` en mode lecture-écriture. Essayez à nouveau d'insérer la même ligne.

```sql
ALTER TABLE EMPLOYEES2 READ WRITE;
```
```sql
INSERT INTO EMPLOYEES2(ID,FIRST_NAME,LAST_NAME,SALARY,DEPT_ID)
VALUES(34,'Grant','Marcie',5678,10);
```
![Cover](https://github.com/Angelofz/TP2-SQL-Oracle/blob/main/image/33.png)

8. Supprimez la table EMPLOYEES2.

```sql
DROP TABLE EMPLOYEES2;
```
