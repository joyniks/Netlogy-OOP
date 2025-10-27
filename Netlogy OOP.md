from Tools.scripts.make_ctype import values


class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def rate_lecture(self, lecturer, course_lect, rate_stud_lect ):
        if isinstance(lecturer, Lecturer) and course_lect in self.courses_in_progress and course_lect in lecturer.courses_attached:
            if course_lect in lecturer.grades:
                lecturer.grades[course_lect] += [rate_stud_lect]
            else:
                lecturer.grades[course_lect] = [rate_stud_lect]
        else:
            return 'Ошибка'

    def __str__(self):
        return  f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за домашние задания: {self.grades}\nКурсы в процессе изучения:{self.courses_in_progress}\nЗавершенные курсы:{self.finished_courses} '



class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'




class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def average(self):
        total_subject_grades = 0
        count_subject_grades = 0
        for subject, value in self.grades():
            total_subject_grades += sum(value)
            count_subject_grades += len(value)
        return total_subject_grades / count_subject_grades

    def __str__(self):
        return  f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {self.average(lecturer.grades)}'

class Reviewer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)

    def __str__(self):
        return  f'Имя: {self.name}\nФамилия: {self.surname}'



# из квиза
best_student = Student('Ruoy', 'Eman', 'your_gender')
best_student.courses_in_progress += ['Python']

cool_mentor = Reviewer('Some', 'Buddy')
cool_mentor.courses_attached += ['Python']

cool_mentor.rate_hw(best_student, 'Python', 10)
cool_mentor.rate_hw(best_student, 'Python', 10)
cool_mentor.rate_hw(best_student, 'Python', 10)

print(best_student.grades)

# из 1 задания
lecturer = Lecturer('Иван', 'Иванов')
reviewer = Reviewer('Пётр', 'Петров')
print(isinstance(lecturer, Mentor)) # True
print(isinstance(reviewer, Mentor)) # True
print(lecturer.courses_attached)    # []
print(reviewer.courses_attached)    # []

# из 2 задания
lecturer = Lecturer('Иван', 'Иванов')
reviewer = Reviewer('Пётр', 'Петров')
student = Student('Алёхина', 'Ольга', 'Ж')

student.courses_in_progress += ['Python', 'Java']
lecturer.courses_attached += ['Python', 'C++']
reviewer.courses_attached += ['Python', 'C++']

print(student.rate_lecture(lecturer, 'Python', 7))  # None
print(student.rate_lecture(lecturer, 'Java', 8))  # Ошибка
print(student.rate_lecture(lecturer, 'С++', 8))  # Ошибка
print(student.rate_lecture(reviewer, 'Python', 6))  # Ошибка

print(lecturer.grades)  # {'Python': [7]}

# из 3 задания

some_reviewer = Reviewer('Some', 'Buddy')

print(some_reviewer)
# Имя: Some
# Фамилия: Buddy

some_lecturer = Lecturer('Some', 'Buddy')

print(some_lecturer)
# Имя: Some
# Фамилия: Buddy
# Средняя оценка за лекции: 9.9

some_student = Student('Ruoy', 'Buddy', 'Ж')

print(some_student)
# Имя: Ruoy
# Фамилия: Eman
# Средняя оценка за домашние задания: 9.9
# Курсы в процессе изучения: Python, Git
# Завершенные курсы: Введение в программирование

