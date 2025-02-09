
markdown
# Green Academy API

Green Academy is a Django REST Framework (DRF) based API for managing courses, users, lessons, user progress, accessibility settings, and interactive elements like comments and quizzes.

---

## Table of Contents
1. [Features](#features)
2. [Requirements](#requirements)
3. [Setup](#setup)
4. [Running the Project](#running-the-project)
5. [API Endpoints](#api-endpoints)
6. [Pagination](#pagination)
7. [Caching](#caching)
8. [Type Checking with Mypy](#type-checking-with-mypy)
9. [Testing](#testing)
10. [Contributing](#contributing)

---

## Features
- *Course Management*: Create, retrieve, and manage courses and lessons.
- *User Management*: Create and manage user accounts and track their progress.
- *Accessibility Settings*: Manage user-specific accessibility preferences.
- *Interactive Elements*: Add comments to lessons and submit quiz attempts.
- *Pagination*: Paginated responses for endpoints returning lists of resources.
- *Caching*: Improved performance using Redis for frequently accessed data.
- *Type Checking*: Static type checking using mypy.

---

## Requirements
- Python 3.8+
- Django 4.2+
- Django REST Framework 3.14+
- Redis (for caching)
- mypy (for type checking)

---

## Setup

### 1. Clone the Repository
bash
git clone https://github.com/ALU-BSE/formative-assessment-1-api-design-and-core-functionality-munyana-jo.git
cd green_academy


### 2. Create a Virtual Environment
bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate


### 3. Install Dependencies
bash
pip install -r requirements.txt


### 4. Set Up the Database
bash
python manage.py migrate


### 5. Create a Superuser (Optional)
bash
python manage.py createsuperuser


### 6. Set Up Redis (for Caching)
Install Redis on your system and ensure it's running. Update the cache settings in settings.py if needed.

---

## Running the Project
Start the development server:
bash
python manage.py runserver


Visit http://127.0.0.1:8000/ in your browser to access the API.

---

## API Endpoints

### Courses
- *GET /courses/*: List all courses (paginated).
- *GET /courses/{id}/*: Retrieve details of a specific course.
- *GET /courses/{id}/lessons/*: List lessons within a course (paginated).

### Users
- *POST /users/*: Create a new user.
- *GET /users/{id}/*: Retrieve a user's profile.
- *PUT /users/{id}/*: Update a user's profile.
- *GET /users/{id}/progress/*: Retrieve a user's course progress.
- *POST /users/{id}/progress/*: Update a user's progress for a course/lesson.

### Accessibility
- *GET /accessibility-settings/*: List available accessibility options.
- *PUT /users/{id}/accessibility-settings/*: Update a user's accessibility preferences.

### Interactive Elements
- *POST /lessons/{id}/comments/*: Add a comment or question to a lesson.
- *GET /lessons/{id}/comments/*: Retrieve comments for a lesson (paginated).
- *POST /lessons/{id}/quiz-attempts/*: Submit quiz answers.
- *GET /lessons/{id}/quiz-results/*: Retrieve quiz results.

---

## Pagination
The API uses *PageNumberPagination* for endpoints that return lists of resources. Clients can control pagination using the following query parameters:
- page: The page number to retrieve (e.g., ?page=2).
- page_size: The number of items per page (e.g., ?page_size=20).

Example response:
json
{
  "count": 100,
  "next": "http://127.0.0.1:8000/courses/?page=2",
  "previous": null,
  "results": [
    {
      "id": 1,
      "title": "Python Basics",
      "description": "Learn Python from scratch."
    },
    ...
  ]
}


---

## Caching
The API uses *Redis* for caching frequently accessed data. For example, the list of courses is cached for 15 minutes to improve performance.

To set up Redis:
1. Install Redis on your system.
2. Update the cache settings in settings.py:
   python
   CACHES = {
       'default': {
           'BACKEND': 'django_redis.cache.RedisCache',
           'LOCATION': 'redis://127.0.0.1:6379/1',
           'OPTIONS': {
               'CLIENT_CLASS': 'django_redis.client.DefaultClient',
           }
       }
   }
   

---

## Type Checking with Mypy
The project uses mypy for static type checking. To run type checks:
bash
mypy green_academy/


Ensure your code passes the type checks without errors.

---

## Testing
You can test the API endpoints using tools like [Postman](https://www.postman.com/) or curl.

### Example: Create a New User
bash
curl -X POST http://127.0.0.1:8000/users/ \
-H "Content-Type: application/json" \
-d '{"name": "John Doe", "email": "john.doe@example.com"}'


### Example: List Courses
bash
curl -X GET http://127.0.0.1:8000/courses/


---

## Contributing
Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a new branch for your feature or bugfix.
3. Submit a pull request with a detailed description of your changes.

---

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
