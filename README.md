A Question-Answer Platform for St. Lawrence University
CS 345: Database Systems
Khang Duc Le, Nevaan Perera

Information is everywhere. There is so much information that, it is extremely difficult to find specific ideas, resources and opportunities suited to an individual’s needs. This is especially true for college campuses, and St. Lawrence University is no exception. Yes, as students, we can always look for answers from our friends, community assistants and campus faculty, however, asking questions anonymously, and gaining perspective from many campus phases is the most desired outcome. Therefore, we propose a question and answer platform aimed specifically for St. Lawrence University students, staff and faculty. Answers could be ‘up voted’ or liked and the best answer will be highlighted. Questions could be asked in many forms from text, checkbox, or radio-dials. Users can tag specific questions, upload pictures or videos and even specify their organization/department. Students can search for questions by name, tag, year asked or organization. All the questions will be stored in a database so anyone can search and view the questions and answers, at any point in time.
Therefore, creating a database that stores and efficiently retrieves information is of paramount importance; thus, we propose a database design for such an application. Information stored in our database would include questions, answers, users, organization name, pictures, videos and tags. We hope to incorporate expandability and flexibility in our record system, that is tailored specifically towards St. Lawrence University and its culture. We propose creating a relational database which will help reduce data redundancy, and enable us to enforce referential integrity. Our database will be a wealth of knowledge concerning college life, and will help bridge the gap of asymmetric information, provide a myriad of recourses to students and enable students to gain insight into many issues and ideas on campus.
---------------------------------------------------------------------------
Following are all the tables needed for our application. 
---------------------------------------------------------------------------

--1
Create table Users (
	user_id varchar(23),
	first varchar not null,
	last varchar not null,
	password varchar not null,
	username varchar not null,
	following_num int not null,
	count_topics int not null,
	position varchar not null,
	num_q int not null,
	num_a int not null,
	primary key (user_id)
	);
--2
Create table Following (
	user_id varchar(23),
	friend_id varchar(23),
	date_start varchar not null,
	primary key (user_id, friend_id),
	foreign key (user_id) references Users,
	foreign key (friend_id) references Users(user_id)
	);
--3
Create table Question (
	q_id varchar not null,
	u_id varchar not null,
	date_ques varchar not null,
	type_q varchar not null 
		check (type_q in ('Multiple Choice', 'Poll', 'Text')),
	count_ans int not null check (count_ans >= 0),
	count_tags int not null,
	text varchar not null,
	primary key (q_id),
	foreign key (u_id) references Users
	);
--4
Create table Answer (
	q_id varchar not null,
	a_id varchar not null,
	u_ans varchar(23),
	date_ans varchar not null,
	count_upvotes int not null check (count_upvotes >= 0),
	count_comments int not null check (count_comments >= 0)
	primary key (a_id),
	foreign key (q_id) references Question,
	foreign key (u_ans) references Users(u_id)
	);
--5	
Create table U_Topics (
	user_id varchar not null,
	topic_id varchar not null,
	date_following varchar not null,
	primary key (user_id, topic_name),
	foreign key (user_id) references Users,
	foreign key (topic_id) references Topics
	);
	
--6 (topics of interests of the Users) 	
Create table Topics (
	topic_id varchar not null,
	name varchar not null,
	primary key (topic_id)
	);
	
-- 7
Create table U_Expertise (
	user_id varchar not null,
	exp_id varchar not null,
	year_of_exp numeric (2,0) check (year_of_exp > 0
		and year_of_exp < 70),
	primary key (exp_id, user_id),
	foreign key (exp_id) references Expertises_type,
	foreign key (user_id) references Users
	);
--8
Create table Expertises_type (
	exp_id varchar not null,
	name varchar not null,
	primary key(exp_id)
	);
	
	--9
Create table Organization (
	group_id varchar not null,
	name varchar not null,
	establish_date varchar not null,
	count_mem int not null,
	leader_id varchar not null,
	primary key(group_id),
	foreign key (leader_id) references Users(user_id)
	);
--10
Create table GroupMembership (
	u_id varchar not null,
	group_id varchar not null,
	date_enter varchar not null,
	primary key(group_id, mem_id),
	foreign key(group_id) references ORGANIZATION,
	foreign key(u_id) references Users(user_id)
	);
--11 1 ans - many comments but 1 comments only 1 ans
Create table Comments (
	a_id varchar not null,
	comment_id varchar not null,
	u_comment varchar (23),
	date_comment varchar not null,
	primary key(comment_id),
	foreign key(a_id) references Answer,
	foreign key(u_comment) references Users(u_id)
	);
--12
Create table tags (
	tags_id varchar not null;
	tags_name varchar not null;
	primary key (tags_id)
	);
--13
Create table Question_tags (
	tags_id varchar not null,
	q_id varchar not null,
	primary key (tags_id, q_id);
	foreign key (tags_id) references tags,
	foreign key (q_id) references Question
	);
--14
Create table Bookmark (
	u_id varchar not null,
	q_booked varchar not null,
	date_booked varchar not null,
	primary key (u_id, q_booked),
	foreign key (u_id) references Users,
	foreign key (q_booked) references Question(q_id)
	);

--15
Create table Answer_Reputation (
	num_a varchar not null,
	reputation varchar not null 
		check (reputation in ('BEGINNER', 'AMATUER', 'PRO')),
	primary key (num_a),
	foreign key (num_a) references Users
	);
	
	
	
	