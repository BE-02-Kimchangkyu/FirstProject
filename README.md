-- AIF_Member 테이블 생성
CREATE TABLE AIF_Member (
    member_id CHAR(36) PRIMARY KEY,
    member_email VARCHAR(100) NOT NULL UNIQUE,
    mem_password VARCHAR(255) NOT NULL,
    auth_group ENUM('Admin', 'Manager', 'Staff', 'General') NOT NULL DEFAULT 'General',
    created_at DATETIME NOT NULL,
    modified_at DATETIME
);

-- AIF_Group 테이블 생성
CREATE TABLE AIF_Group (
    group_id INT AUTO_INCREMENT PRIMARY KEY,
    group_name VARCHAR(20) NOT NULL UNIQUE,
    created_at DATETIME NOT NULL,
    modified_at DATETIME
);

-- Member_Role 테이블 생성
CREATE TABLE Member_Role (
    user_id CHAR(36),
    auth_group_id INT,
    PRIMARY KEY (user_id, auth_group_id),
    FOREIGN KEY (user_id) REFERENCES AIF_Member(member_id),
    FOREIGN KEY (auth_group_id) REFERENCES AIF_Group(group_id)
);

-- Refresh_Token 테이블 생성
CREATE TABLE Refresh_Token (
    refresh_token_Id INT AUTO_INCREMENT PRIMARY KEY,
    member_id CHAR(36) NOT NULL,
    refresh_token VARCHAR(100),
    FOREIGN KEY (member_id) REFERENCES AIF_Member(member_id)
);

-- AI_Image 테이블 생성
CREATE TABLE AI_Image (
    img_id INT AUTO_INCREMENT PRIMARY KEY,
    created_by CHAR(36) NOT NULL,
    img_url VARCHAR(255) NOT NULL,
    keyword_input TEXT,
    style_code VARCHAR(20),
    created_at DATETIME NOT NULL,
    FOREIGN KEY (created_by) REFERENCES AIF_Member(member_id)
);

-- Survey_Question 테이블 생성
CREATE TABLE Survey_Question (
    question_id INT AUTO_INCREMENT PRIMARY KEY,
    question_content VARCHAR(255) NOT NULL,
    created_at DATETIME NOT NULL,
    modified_at DATETIME
);

-- Survey_Answer 테이블 생성
CREATE TABLE Survey_Answer (
    survey_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id CHAR(36),
    question_id INT NOT NULL,
    answer_choice VARCHAR(100),
    created_at DATETIME NOT NULL,
    modified_at DATETIME,
    FOREIGN KEY (user_id) REFERENCES AIF_Member(member_id),
    FOREIGN KEY (question_id) REFERENCES Survey_Question(question_id)
);
