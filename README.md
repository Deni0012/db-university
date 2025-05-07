# DB University

## 1. Selezionare tutti gli studenti nati nel 1990 (160)
SELECT 
	*
FROM 
	`students`
WHERE
	YEAR(`date_of_birth`) = 1990;

## 2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
SELECT 
    *
FROM
    `courses`
WHERE
    `cfu` > 10;

## 3. Selezionare tutti gli studenti che hanno più di 30 anni
SELECT 
    *
FROM
    `students`
WHERE
    `date_of_birth` < DATE_SUB(CURDATE(), INTERVAL 30 YEAR);

## 4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)
SELECT 
    *
FROM
    `courses`
WHERE
    `year` = 1 AND `period` LIKE 'I %';

## 5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)
SELECT 
    *
FROM
    `exams`
WHERE
    `date` = '2020-06-20'
        AND `hour` > '14:00';

## 6. Selezionare tutti i corsi di laurea magistrale (38)
SELECT 
    *
FROM
    `degrees`
WHERE
    `level` = 'magistrale';

## 7. Da quanti dipartimenti è composta l'università? (12)
SELECT 
    COUNT(*)
FROM
    `departments`;

<!-- oppure -->

SELECT 
    *
FROM
    `departments`;
    

## 8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
SELECT 
    *
FROM
    `teachers`
WHERE
    `phone` IS NULL;


# Esercizi Personali
<!-- ESERCIZI PERSONALI -->
## 9. Quanti sono gli insegnanti che hanno un numero di telefono? (50)
SELECT 
    *
FROM
    `teachers`
WHERE
    `phone` IS NOT NULL;

## 10. Selezionare tutti i corsi di laurea triennale (37)
SELECT 
    *
FROM
    `degrees`
WHERE
    `level` = 'triennale';

## 11. Selezionare tutti gli studenti che hanno meno di 30 anni
SELECT 
    *
FROM
    `students`
WHERE
    `date_of_birth` > DATE_SUB(CURDATE(), INTERVAL 30 YEAR);

## 12. Selezionare tutti i corsi che non hanno un website (676)
SELECT 
    *
FROM
    `courses`
WHERE
    `website` IS NULL;

## 13. Selezionare tutti i corsi che hanno un website (641)
SELECT 
    *
FROM
    `courses`
WHERE
    `website` IS NOT NULL;


# Giorno Due 

# GROUP BY

## 1. Contare quanti iscritti ci sono stati ogni anno

SELECT 
    COUNT(*), YEAR(`enrolment_date`) AS `registration_for_year`
FROM
    `students`
GROUP BY `registration_for_year`;

## 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT 
    COUNT(*), `office_address`
FROM
   `teachers`
GROUP BY `office_address`;

## 3. Calcolare la media dei voti di ogni appello d'esame

SELECT 
    `exam_id`, AVG(vote) AS `average_vote`
FROM
    `exam_student`
GROUP BY `exam_id`
ORDER BY `average_vote` DESC;

## 4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT 
    `department_id`, COUNT(*) AS `type_of_course`
FROM
    `degrees`
GROUP BY `department_id`;

------------------------

# INNER JOIN 

## 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT 
    `students`.*, `degrees`.`name`
FROM
    `students`
        INNER JOIN
    `degrees` ON `degrees`.`id` = `students`.`degree_id`
WHERE
    `degrees`.`name` = 'Corso di Laurea in Economia';

## 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

SELECT 
    `degrees`.`id`, `degrees`.`level`, `departments`.*
FROM
    `degrees`
        INNER JOIN
    `departments` ON `degrees`.`id` = `degrees`.`department_id`
WHERE
    `departments`.`name` = 'Dipartimento di Neuroscienze'
        AND degrees.level = 'triennale';

## 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT 
    *
FROM
    `teachers`
        INNER JOIN
    `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
        INNER JOIN
    `courses` ON `course_teacher`.`course_id` = courses.id
WHERE
    teachers.id = 44;

## 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT 
    *
FROM
    `students`
        INNER JOIN
    `degrees` ON `degrees`.`id` = `degrees`.`department_id`
ORDER BY `students`.`surname` , `students`.`name`;

## 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

## 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

## 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.