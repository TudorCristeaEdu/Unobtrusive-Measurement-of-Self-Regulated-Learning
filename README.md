# Unobtrusive-Measurement-of-Self-Regulated-Learning
The scripts used for the "Unobtrusive Measurement of Self-Regulated Learning:A Clickstream-Based Multi-dimensional Scale". These are all done using Canvas data. Some have prerequisites, which are mentioned below; I also numbered the scripts, so it is easier to follow.

You should start by loading the "course_dim" table which contains the "course_id" of your targeted course. This can be used to filter the requests from the "requests" table. You should also add the "enrollment_dim" table in order to filter for the role (e.g., students) using the "type" column. 

Follow up by loading the "smaller" tables, which contain extra information on the requests and can be used for certain indicators. For our study, we used the "discussion_topic_dim" for discussions and announcements indicators ("type" column for distinguishing), "assignment_dim" for the assignment page clicks, "submission_dim" for submissions of the students (assignment and submissions should be merged to know where each submission goes), "submission_comment_dim" for the comments left by students with/after submitting, "quiz_main" and "quiz_fact" for all clicks within quizzes (these should be merged to get all the information necessary), "files_dim" to get all clicks on learning materials within modules (such as articles, videos etc.), and "conferences_dim" for clicks on video conferences (for us only BigBlueButton was saved). Ideally, you only merge requests with one or two tables at a time and use the resulting table for the indicators that require the smaller table (e.g., indicators about clicks on discussions should be formed from the requests+discussion_dim table). This makes everything more manageable and faster. In our case, the parts we share are present in scripts (1) and (2).
You should not load the whole script, but parts of it, based on the used course. Since we used courses from different years and quartiles, this was necessary. All IDs were changed for anonymization purposes.

If these are done, you can use script (3) to get the indicators for phases one (Task Definition), two (Goal Setting), and four (Adaptation). At the end of the script, there is a function that runs the right script based on the course id for the third learning phase (Enactment). In this case, each course requires their own script; check script (4) for an example. The main problem stems from the way in which Canvas represents files. All materials present within modules, can only be distinguished by extension or name. Since we wanted to look at mandatory and optional materials, it was impossible to form indicators based only on extension (e.g., a .doc file can represent exercises that are mandatory but also optional). Thus, we had to look at each course from an observer perspective and save the files in each category manually. Another problem was that two files were named slightly different but represented the same material (e.g., "Lecture 1a" and "Lecture 1A". This also required manual check and change. For us it was easier this way since we had only four courses and we wanted the differences between courses to be easily expressed. You can copy the scripts and make changes accordingly. If the number of courses is larger, I would recommend finding a better solution. 

Once you have the indicators, you should clean them (removing indicators with very little variance or 0 clicks), replacing NAs with 0s where logical, and transforming them per course. This is done in script (5). The next step is forming the scales using Cronbach's alpha and principal component analysis; script (6). Once you have the final scales, before running correlations, you need to also have the grades and survey responses together. If pseudo-anonymized, you can use the "pseudonym_dim" table in order to make the connection. In our case, the surveys contained the SRL scale from the MSLQ survey. If you want to check for academic performance (grades), you can use the same table to connect everything with the OSIRIS tables. 

The final correlations and regressions (for validity check) can be found in script (7).


PROBLEMS ENCOUNTERED/TIPS: 
- The IDs (for students, courses, discussions etc.) were very long which led to problems if read as numeric. We recommend reading everything as character, and making changes later
- The tables might have different sizes of IDs, although they represent the same thing. For example, in the requests table a discussion might have the ID "12340000000019761", while in the discussion table, that same discussion will show up as "19761". We don't know if you will have the same problem, but I recommend checking this first (the sizes of IDs between merging tables), and if so, removing the first part with str_remove. Fortunately, this worked everywhere with no problem
- Some IDs (e.g., for files) are not present as a column. Thus, you might need to get them from the URL present in the requests table. You can check our examples, but you might need to adapt this
- Some ID columns do not have the same variable name between tables. For example, discussions have "discussion_id" in the requests, but simply "id" in the "discussion_topic_dim". Some use "canvas_id". Check the canvas data portal, it can be really helpful: https://portal.inshosteddata.com/docs 
- Before merging tables, make sure the ID columns have the same class! If you read everything as character, you should not have this problem


Finally, I want to thank Dr. Rianne Conijn (https://github.com/RConijn) for helping with parts of the code. I also want to thank Sonja Kleter for helping with all the slow and manual checks, necessary with such amounts of big data. This would have taken even longer without them!