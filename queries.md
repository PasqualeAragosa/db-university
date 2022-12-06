Seleziono tutti gli studenti neti nel 1990(160)
```sql
SELECT * FROM students WHERE date_of_birth BETWEEN "1990-01-01" AND "1990-12-31";
```

Selezionare tutti i corsi che valgono più di 10 crediti(479)
```sql
SELECT * FROM courses WHERE cfu > 10;
```

Selezionare tutti gli studenti che hanno più di 30 anni(3324)
```sql
SELECT * FROM students WHERE TIMESTAMPDIFF(YEAR, date_of_birth, CURRDATE()) > 30;
```

Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
```sql
SELECT * FROM courses WHERE period = 'I semestre' AND year = 1;
```

Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
```sql
SELECT * FROM exams WHERE date = "2020-06-20" AND hour >= "14:00:00";
```

Selezionare tutti i corsi di laurea magistrale (38)
```sql
SELECT * FROM `degrees` WHERE `level` = "magistrale";
```

Da quanti dipartimenti è composta l'università? (12)
```sql
SELECT * FROM departments;
```
Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
```sql
SELECT * FROM `teachers` WHERE phone IS NULL;
```

Selezionare tutti i corsi del Corso di Laurea in Informatica (22)
```sql
SELECT `courses`.`id`, `courses`.`name`, `courses`.`period`, `courses`.`year`, `courses`.`website`, `courses`.`cfu`, `degrees`.`name` AS `degrees_name`
FROM `courses`
JOIN `degrees` 
ON `courses`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = "Corso di Laurea in Informatica";
```

#############################

GROUP BY:

Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT COUNT(id) AS `enrolment_amount`, year(`enrolment_date`) AS `year`
FROM `students`
GROUP BY year(`enrolment_date`);
```

Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT COUNT(id) AS `amount`, `office_address`
FROM `teachers`
GROUP BY `office_address`;
```

Calcolare la media dei voti di ogni appello d'esame
```sql
SELECT AVG(vote) AS `average_vote`, `exam_id`
FROM `exam_student`
GROUP BY `exam_id`;
```

Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT COUNT(id) AS `amount`, `department_id`
FROM `degrees`
GROUP BY `department_id`;
```


#############################

JOIN:

Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql

```
