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
SELECT `students`.`name`, `students`.`surname`,  `degrees`.`name` AS `degrees_name`
FROM `students`
JOIN `degrees`
ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```

Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT `degrees`.`name`, `degrees`.`level`,  `departments`.`name` AS `departments_name`
FROM `degrees`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze'
AND `degrees`.`level` = 'magistrale';
```

Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT `teachers`.`name`, `teachers`.`surname`, `courses`.`name`
FROM `teachers`
JOIN `course_teacher`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
JOIN `courses` 
ON `course_teacher`.`course_id` = `courses`.`id`
WHERE `teachers`.`id` = 44;
```

Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT `students`.`surname`, `students`.`name`, `degrees`.`name`, `departments`.`name`
FROM `students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname` ASC;
```

Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT `degrees`.`name` AS `corso_laurea`, `courses`.`name` AS `corso`, `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`
FROM `degrees`
JOIN `courses`
ON `degree_id` = `degrees`.`id`
JOIN `course_teacher`
ON `course_id` = `courses`.`id`
JOIN `teachers`
ON `teacher_id` = `teachers`.`id`;
```

Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT DISTINCT `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`, `departments`.`name` AS `department_name`
FROM `departments`
JOIN `degrees`
ON `department_id` = `departments`.`id`
JOIN `courses`
ON  `degree_id` = `degrees`.`id`
JOIN `course_teacher`
ON `course_id` = `courses`.`id`
JOIN `teachers`
ON `teacher_id` = `teachers`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami
```sql
SELECT `students`.`name` AS `student_name`, `students`.`surname` AS `student_surname`, COUNT (`exam_student`.`vote`) AS `attempts`
FROM `students`
JOIN `exam_student`
ON `students`.`id` = `student_id`
JOIN `exams`
ON `exam_id` = `exams`.`id`
JOIN `courses`
ON `course_id` = `courses`.`id`;
```