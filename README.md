# EngageRocket Assignment

[Question 1](#q1)

[Question 3](#q3)

## <a name="q1"></a>Question 1 - TDD

For the first 2 test cases: 

```
def sort(array)
  return array
end
```

It returns the original array, so the first 2 cases will pass.

For the third test case:

```
def sort(array)
  if array[0] > array[1]
    array = swap(array, 0, 1)
  end

  return array
end

def swap(array, i, j)
  temp = array[i]
  array[i] = array[j]
  array[j] = temp

  return array
end
```

This algorithm simply compares and swaps 2 adjacent items into ascending order. Since the third test case only has 2 items, it will pass.

For the fourth test case:

```
def sort(array)
  for i in 0...array.size-1
    if array[i] > array[i+1]
      array = swap(array, i, i+1)
    end
  end

  return array
end

def swap(array, i, j)
  temp = array[i]
  array[i] = array[j]
  array[j] = temp

  return array
end
```

The algorithm has been generalized to an array of any size. It will finalize the position of the biggest item in the array. Since the test case only has 1 item that needs to be shifted (3), it will pass.

The next test case should require shifting more than 1 item. This can be demonstrated using:

```
assert_equal([1,2,3], sort(3,2,1))
```

This test case requires shifting both 3 and 1. The next iteration of the code is:

```
def sort(array)
  for i in 0...array.size-1
    for j in 0...array.size-(1+i)
      if array[j] > array[j+1]
        array = swap(array, j, j+1)
      end
    end
  end

  return array
end

def swap(array, i, j)
  temp = array[i]
  array[i] = array[j]
  array[j] = temp

  return array
end
```

The algorithm can now shift multiple items. It will repeatedly shift the biggest items to the right side of the array until there are no items left. The algorithm is now complete and no more test cases are required.

## <a name="q3"></a>Question 3 - Rails

Refer to the following diagram for an overview of the models and their relationships:

![UML Diagram](https://github.com/Honoo/EngageRocket-assignment/raw/master/UML.PNG "UML Diagram")

The Employee model encompasses both the Company Admin and other employees. It contains the following fields and relationships:
* Personal information like first_name, last_name and email, which can be used to personalize the email.
* Each Employee has a Role; Employee has a role_id field.

The Role model contains the following fields and relationships:
* A name, such as 'Admin'. It denotes the real-life responsiblities of an employee.
* A Role can have many Permissions. A Role having a Permission means that the Employee with the Role can access the corresponding part of the system. 

The Permissions model represents what a Role is able to do or access. It contains the following fields and relationships:
* A controller field, which denotes which controller the Permission is for.
* An action field, which denotes which action of a controller the Permission is for.
* A Permission can belong to many Roles.

The Role and Permission models are necessary to ensure normal employees do not access unauthorized resources such as the dashboard. It can be extended to other parts of the system that require restricted access. To check if users have the required access level, filters can be used in the controllers.

The Survey model represents an individual survey. It contains the following fields and relationships:
* A title, which is used to identify the survey.
* A Survey can have many Questions and SurveyResponses.

The Question model represents a question from a particular Survey. It contains the following fields and relationships:
* A survey_id, which is the Survey it belongs to.
* A question_text field which contains the actual question.

The SurveyResponse model represents an individual response to a particular Survey. It contains the following fields and relationships:
* A survey_id, which is the Survey it is a response to.
* An employee_id, representing the employee whom the response belongs to.
  * A validation can be written on the SurveyResponse model to ensure that each employee only answers a survey once.

The QuestionResponse model is an intermediate model for the many-to-many connection between Questions and SurveyResponses. It contains the following fields and relationships:
* A question_id, which is the Question it corresponds to.
* A survey_response_id, which is the SurveyResponse it corresponds to.
* A score, which is the actual score given by the respondent.

The routes will be as follows:

For employees:
* `get '/survey/new', to: 'survey#new'` will display the survey to employees to fill in.
* `post '/survey', to: 'survey#create'` will create a new survey response in the database.

For the admin:
* `get 'admin/dashboard', to: 'admin/dashboard#index'` will display the dashboard and required information to the admin.
  * The number of people who have filled in the survey can be computed from SurveyResponses.
  * The average score can be computed from QuestionResponses.
* `get 'admin/survey/new', to 'admin/survey#new'` will display the page for creating a survey.
* `post 'admin/survey', to: 'admin/survey#create'` will create a new survey.
* `get 'admin/survey/:id/edit', to: 'admin/survey#edit'` will display the page for editing the survey.
* `put 'admin/survey/:id', to: 'admin/survey#update'` will update the survey.
* `get 'admin/email/new', to 'admin/email#new'` will display the page for creating an email. It will have a text editor for customization.
* `post 'admin/email', to: 'admin/email#create'` will send the email.
  * The receipients can be determined in the email controller by selecting Employees who do not have a SurveyResponse for the corresponding Survey. The actual reciepients will not be shown on the email creation page.
