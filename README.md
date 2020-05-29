# TODOs
1. add support and use cases for question:answer = 1:M relationship (currently modeled as 1:1 in question_answer table)
2. add support for multiline answers
3. rename column answer_stats.timestamp => answer_stats.answered_at
4. add new column answer_stats.answer_time and populate this column when saving correct and incorrect answers
5. add new tags column for questions
6. print exit message on ctrl + d
7. query questions that 
    - were unanswered
    - were answered incorrectly
    - were answered correctly
    
# Limitations
- command line doesn't allow to edit previous line of a multiline answer

# Work in Progress
query to get unasnwered questions ids
```
SELECT * FROM question_answer WHERE id NOT IN (
	SELECT id FROM answer_stats JOIN (
		SELECT question_id, MAX(timestamp) as timestamp
		FROM answer_stats 
		WHERE correct_answer = 1 
		GROUP BY question_id
	) AS latest_correct_answers 
	ON answer_stats.question_id = latest_correct_answers.question_id
	AND answer_stats.timestamp = latest_correct_answers.timestamp
)
```
