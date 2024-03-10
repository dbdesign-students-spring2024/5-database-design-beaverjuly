# Data Normalization and Entity-Relationship Diagramming

![](./images/University%20ERD.drawio.svg)

# Entities and Attributes:

## Courses
- **Entity**: Courses
  - **Attributes**:
    - `course_id` (Primary Key)
    - `course_name`

## Professors
- **Entity**: Professors
  - **Attributes**:
    - `professor_id` (Primary Key)
    - `professor_name`
    - `professor_email`

## Classrooms
- **Entity**: Classrooms
  - **Attributes**:
    - `classroom_id` (Primary Key)
    - `classroom_location`

## Sections
- **Entity**: Sections
  - **Attributes**:
    - `section_id` (Primary Key)
    - `due_date`
  - **Note**: `course_id`, `professor_id`, and `classroom_id` are foreign keys and represented as relationships, not attributes within the Sections entity.

## Assignments
- **Entity**: Assignments
  - **Attributes**:
    - `assignment_id` (Primary Key)
    - `assignment_topic`
    - `relevant_reading`
    - `due_date`
    - `professor_id` (Foreign Key)

## Student
- **Entity**: Students
  - **Attributes**:
    - `student_id` (Primary Key)
    - `student_name`
    - `student_email`

## Grades
- **Entity**: Grades
  - **Attributes**:
    - The combination of `assignment_id` and `student_id` serve as a composite key
    - `grade_value`
  - **Note**: `assignment_id` and `student_id` are foreign keys and are represented as relationships.

# Relationships:

## Sections to Courses
- A many-to-one relationship where each Section is subsumed under one Course, while one course is associated with many sections. 
- The `course_id` in the Sections table serves as a foreign key that references the `course_id` in the Courses table.

## Sections to Students
- A many-to-many relationship where each Section has many Students; and each Student attends many Sections.

## Sections to Professors
- A many-to-one relationship where each Section is associated with one Professor; while one Professor can teach multiple Sections. 
- The `professor_id` in the Sections table serves as a foreign key that references the `professor_id` in the Professors table.

## Sections to Classrooms
- A one-to-one relationship where each Section is associated with one unique Classroom. 
- The `classroom_id` in the Sections table serves as a foreign key that references the `classroom_id` in the Classrooms table.

## Assignments to Sections
- A many-to-many relationship where multiple Assignments are associated with one Section, and one assignment can be simultaneously assigned to several Sections. 
- An additional junction table is needed (demonstrated later)

## Grades to Assignments
- A one-to-one relationship where each Grade is associated with one Assignment. 
- The `assignment_id` in the Grades table serves as a foreign key that references the `assignment_id` in the Assignments table.

## Student to Grades
- A one-to-many relationship where one student can have many grades; while each grade is associated with only one Student. 
- Each record in the Grades table is associated with one `student_id`.

## Student to Professor
- Students and Professors do not have a direct relationship except through shared Sections.  

## Assignment to Professor
- A one-to-many relationship where each professor give multiple unique assignments; and each assignment belongs only to the Professor who published it. 
- `professor_id` is a direct foreign key in the Assignments table.

# Normal Form Compliance Analysis

## Courses Table
| course_id | course_name              |
|-----------|--------------------------|
| 101       | Database Systems         |
| 102       | Introduction to Python   |

- **2NF**: Non-key attribute `course_name` is fully functionally dependent on the primary key `course_id`.
- **3NF**: Non-key attribute `course_name` depends directly on the primary key `course_id` and not on any other non-key attribute.
- **4NF**: Attribute `course_name` is uniquely determined by `course_id` and doesn't lead to any multi-valued dependencies.
- **Conclusion**: complies with 2NF, 3NF, and 4NF.


## Professors Table
| professor_id | professor_name | professor_email  |
|--------------|----------------|------------------|
| 1            | Melvin         | l.melvin@foo.edu |
| 2            | Logston        | e.logston@foo.edu|
| 3            | Nevarez        | i.nevarez@foo.edu|

- **2NF**: Fully functionally dependent on the primary key.
- **3NF**: No transitive dependencies.
- **4NF**: No multi-valued dependencies.
- **Conclusion**: Complies with 2NF, 3NF, and 4NF.


## Classrooms Table
| classroom_id | classroom_location |
|--------------|--------------------|
| 1            | WWH 101            |
| 2            | 60FA 314           |
| 3            | WWH 201            |

- **2NF**: Fully functionally dependent on the primary key.
- **3NF**: No transitive dependencies.
- **4NF**: No multi-valued dependencies.
- **Conclusion**: Complies with 2NF, 3NF, and 4NF.


## Sections Table
| section_id | course_id | professor_id | classroom_id | due_date  |
|------------|-----------|--------------|--------------|-----------|
| 1          | 101       | 1            | 1            | 23.02.21  |
| 2          | 102       | 2            | 2            | 18.11.21  |

- **2NF**: 
  - Each non-key attribute (`course_id`, `professor_id`, `classroom_id`, `due_date`) is fully functionally dependent on the primary key (`section_id`), not on just a part of it.

- **3NF**: 
  - Every non-key attribute (`course_id`, `professor_id`, `classroom_id`, `due_date`) is only dependent on `section_id`, which is the primary key, and there are no transitive dependencies.

- **4NF**: 
  - Each attribute is functionally independent of the others; for example, `course_id`, `professor_id`, and `classroom_id` do not depend on each other but only on `section_id`.

- **Conclusion**: complies with 2NF, 3NF, and 4NF.


## Assignments Table
| assignment_id | assignment_topic                | relevant_reading    | due_date  | professor_id |
|---------------|---------------------------------|---------------------|-----------|------------|
| 1             | Data normalization              | Deumlich Chapter 3  | 23.02.21  | 7          |
| 2             | Single table queries            | Dümmlers Chapter 11 | 18.11.21  | 4          |

- **2NF**: All non-key attributes (`assignment_topic`, `relevant_reading`, `due_date`, `professor_id`) are fully dependent on the primary key (`assignment_id`).
- **3NF**: All attributes depend only on the primary key.
- **4NF**: No multi-valued dependencies are observed.
- **Conclusion**: Complies with 2NF, 3NF, and 4NF. 

## Students Table
| student_id | student_name | student_email     |
|------------|--------------|-------------------|
| 1          | John Doe     | john.doe@mail.com |
| 2          | Jane Doe     | jane.doe@mail.com |
| 7          | Max Payne    | max.payne@mail.com|

- **2NF**: All attributes are fully functionally dependent on the primary key.
- **3NF**: All attributes depend only on the primary key.
- **4NF**: No multi-valued dependencies are present.
- **Conclusion**: Complies with 2NF, 3NF, and 4NF.

## Grades Table
| assignment_id | student_id | grade_value |
|---------------|------------|-------------|
| 1             | 1          | 80          |
| 2             | 7          | 25          |
| 1             | 4          | 75          |

- **2NF**: Uses a composite key; all attributes are dependent on the entire key.
- **3NF**: `grade_value` depends only on the composite key (no transitive dependencies).
- **4NF**: No multi-valued dependencies as each grade is associated uniquely with a student and an assignment.
- **Conclusion**: Complies with 2NF, 3NF, and 4NF.


# Additional Junction Tables for Many-to-Many Relationships

## 1. To represent relationship between Assignments & Sections Tables:
### AssignmentSection 
| assignment_id | section_id |
|---------------|------------|
| 1             | 1          |
| 1             | 2          |
| 2             | 1          |
| 3             | 2          |

### Normal Form Analysis:

- No partical dependencies, transitive dependencies, or multi-valued dependencies
- In fact, there are no non-key attributes at all, just the composite key.

- **Conclusion**:
complies with 2NF, 3NF, and 4NF.

## 2. To represent relationship between Students & Sections Tables:
### SectionStudents Table
| section_id | student_id |
|------------|------------|
| 1          | 1          |
| 1          | 2          |
| 2          | 1          |
| 2          | 3          |

### Normal Form Analysis:
- Again, no non-key attributes at all, just the composite key.
- Thus it complies with 2NF, 3NF, and 4NF.
