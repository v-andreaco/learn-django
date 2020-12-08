## Exercise: Creating a model

Now that we have activated our database it is time to start creating our models. By creating a model we are able to update the database by adding any essential fields and defining the behavior of our data. In this exercise we will continue to use our "Hello, world!" app that was created in the last module and add two models: **Question** and **Answer**.

1. The first step in our process is to add the models. This can be achieved by going to the **hello_world/models.py** file and adding two Python classes to contain our models as shown below.

    ~~~
    class Question(models.Model):
        question_text = models.CharField(max_length=200)


    class Answer(models.Model):
        question = models.ForeignKey(Question, on_delete=models.CASCADE)
        answer_text = models.CharField(max_length=200)
    ~~~

    By adding these models you are generating a field in the database and defining how that field should behave. For instance, in **question_text** the field will accept characters and have a character limit of 200. 

    Also take notice of the term **ForeignKey** that was added in the **Answer** class. This keyword tells Django there is a relationship between an **Answer** and a **Question**. By defining this relationship we are telling Django that every answer is related to a single **Question**.

## The \_\_str__ method

Now that we have created the **Question** and **Answer** models for our app there is one important addition that needs to be addressed when defining classes.
For this example we start by creating another class named **Question**.

~~~
class Question:
    def __init__(self, question_text):
        self.question_text = question_text
~~~  

After creating the class, we now create an instance by entering the question that we would like to ask.

~~~
my_question = Question('Is anyone out there?')
~~~

Now print the information to the console to see what appears.

~~~
print(my_question)
~~~

<img src="..\Module2\Module2_Images\Module2_NoStr.PNG" alt="SQLite Database Folder" style="width:550px; height:auto" />

When looking at the output of the **my_question** variable it doesn't give us any details. While it does give the class name, it only returns the id or memory address of the object. To solve this issue we need to add the \_\_str__ method to our class.

Now let's take the same code, but add the \_\_str__ method as below.

~~~
class Question:
    def __init__(self, question_text):
        self.question_text = question_text

    def __str__(self):
        return f'I asked the question: {self.question_text}'
~~~

Since we have now defined the \_\_str__ method let's again create an instance by entering the below line of code.

~~~
my_question = Question('Is anyone out there?')
~~~

Now print again to the console.

~~~
print(my_question)
~~~

<img src="..\Module2\Module2_Images\Module2_WithStr.PNG" alt="SQLite Database Folder" style="width:350px; height:auto" />

As you can see this now returns the question that was asked, and we have even added more detail to make it more helpful. With that said, lets now make our models more informative by adding a \_\_str__ method.

~~~
class Question(models.Model):
    question_text = models.CharField(max_length=200)

    def __str__(self):
        return self.question_text

class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)

    def __str__(self):
        return self.choice_text
~~~

With this addition to our **Question** and **Answer** classes it will now print out the question text along with the answer.

## Exercise: Activating the model

Now that we have added the model code to our file it is time for activation.

First, we need to find the configuration class name within the **hello_world** app. To find this class name go to the **hello_world/apps**.**py** file to find the below code and see that the class name is **HelloWorldConfig**.

~~~
class HelloWorldConfig(AppConfig):
    name = 'hello_world'
~~~

Now that you have the class name, return to the inner **myfirstproject** folder and **settings**.**py** file to add the app config line to the list of **INSTALLED_APPS** as below.

~~~
INSTALLED_APPS = [
    'hello_world.apps.HelloWorldConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
~~~

By adding this line to the list of **INSTALLED_APPS** it tells Django that this app needs to be included when running the project.

Next, we need to tell Django that new models are added and we would like for the changes to be stored as a migration. In order to do this run the below code in the command line.

    python manage.py makemigrations hello_world

After running the command then you should see something similar to below stating it has stored both models as a migration.

<img src="..\Module2\Module2_Images\Module2_Migrations.PNG" alt="Django Model Migration" style="margin-left: 30px;width:250px; height:auto" />

The final step to actually make the changes to our database is to run the migrate command as below.

    python manage.py migrate

Once this command has completed then you should be able to see the new additions in the schema of the database. 

<img src="..\Module2\Module2_Images\Module2_VSC_SQLiteDBAddModels.PNG" alt="Django Model Migration" style="margin-left: 30px;width:200px; height:auto" />

[!NOTE] If you don't remember how to display the schema refer to the previous unit **Displaying The Schema**.