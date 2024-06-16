#### 基本表设计

1. **用户表 (Users)**

   - `UserID`: 用户ID，主键
   - `Username`: 用户名
   - `Password`: 密码
   - `Role`: 角色（例如：'Student', 'Teacher'）

   ```
   sql复制CREATE TABLE Users (
       UserID INT AUTO_INCREMENT PRIMARY KEY,
       Username VARCHAR(255) NOT NULL,
       Password VARCHAR(255) NOT NULL,
       Role VARCHAR(50) NOT NULL
   );
   ```

2. **课程表 (Courses)**

   - `CourseID`: 课程ID，主键
   - `CourseName`: 课程名称
   - `TeacherID`: 教师ID，外键

   ```
   sql复制CREATE TABLE Courses (
       CourseID INT AUTO_INCREMENT PRIMARY KEY,
       CourseName VARCHAR(255) NOT NULL,
       TeacherID INT,
       FOREIGN KEY (TeacherID) REFERENCES Users(UserID)
   );
   ```

3. **考试表 (Exams)**

   - `ExamID`: 考试ID，主键
   - `CourseID`: 关联的课程ID，外键
   - `ExamTitle`: 考试标题
   - `ExamDate`: 考试日期

   ```
   sql复制CREATE TABLE Exams (
       ExamID INT AUTO_INCREMENT PRIMARY KEY,
       CourseID INT,
       ExamTitle VARCHAR(255) NOT NULL,
       ExamDate DATE NOT NULL,
       FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
   );
   ```

4. **问题表 (Questions)**

   - `QuestionID`: 问题ID，主键
   - `ExamID`: 考试ID，外键
   - `QuestionText`: 问题描述

   ```
   sql复制CREATE TABLE Questions (
       QuestionID INT AUTO_INCREMENT PRIMARY KEY,
       ExamID INT,
       QuestionText TEXT NOT NULL,
       FOREIGN KEY (ExamID) REFERENCES Exams(ExamID)
   );
   ```

5. **答案表 (Answers)**

   - `AnswerID`: 答案ID，主键
   - `QuestionID`: 问题ID，外键
   - `StudentID`: 学生ID，外键
   - `AnswerText`: 学生答案

   ```
   sql复制CREATE TABLE Answers (
       AnswerID INT AUTO_INCREMENT PRIMARY KEY,
       QuestionID INT,
       StudentID INT,
       AnswerText TEXT,
       FOREIGN KEY (QuestionID) REFERENCES Questions(QuestionID),
       FOREIGN KEY (StudentID) REFERENCES Users(UserID)
   );
   ```

#### 视图设计

![image-20240616113713564](https://raw.githubusercontent.com/mitiy001/PicGo/main/blog/202406161936711.png)

#### 注册/登记 SQL 代码

1. **注册新学生**

   ```
   sql复制INSERT INTO Users (Username, Password, Role) VALUES ('new_student', 'password123', 'Student');
   ```

2. **教师注册课程**

   ```
   sql复制INSERT INTO Courses (CourseName, TeacherID) VALUES ('Calculus', (SELECT UserID FROM Users WHERE Username = 'teacher1'));
   ```

3. **学生登记考试**

   ```
   sql复制INSERT INTO Answers (QuestionID, StudentID, AnswerText)
   VALUES (101, (SELECT UserID FROM Users WHERE Username = 'student1'), 'Student answer here');
   ```