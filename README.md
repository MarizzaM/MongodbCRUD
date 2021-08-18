# Python_Django_MongoDB_CRUD

    pip install django==2

    pip install djongo

    django-admin startproject DjangoCrudMongoDB

    cd DjangoCrudMongoDB

    python manage.py startapp DjangoCrudApp

DjangoCrudMongoDB -> settings.py 

    DATABASES = {
        'default': {
            'ENGINE': 'djongo',
            'NAME': "django_mongodb_tutorial",
        }
    }
    
DjangoCrudApp -> models.py

    from djongo import models
    # Create your models here.

    class Posts(models.Model):
        _id=models.ObjectIdField()
        post_title=models.CharField(max_length=255)
        post_description=models.TextField()
        comment=models.JSONField()
        tags=models.JSONField()
        user_details=models.JSONField()
        objects=models.DjongoManager()
        
Terminal

    python manage.py makemigrations
    
    python manage.py migrate
    

![image](https://user-images.githubusercontent.com/75089445/129479650-f3e3c0b4-782d-4947-aa60-39893430923b.png)

![image](https://user-images.githubusercontent.com/75089445/129479671-5e56f0c9-ac76-4915-93bc-8f1ed891578b.png)

Terminal

        python manage.py createsuperuser

        admin
        admin@gmail.com
        123
        123
        y
        
![image](https://user-images.githubusercontent.com/75089445/129480040-da48f2ea-9541-47e0-8d8d-36b16042af7a.png)

![image](https://user-images.githubusercontent.com/75089445/129480048-f24ab231-f220-4ff4-87f1-32816ecf95c6.png)


DjangoCrudMongoDB -> urls.py 

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('add_post/', views.add_post),
            path('update_post/<str:id>', views.update_post),
            path('delete_post/<str:id>', views.delete_post),
            path('read_post/<str:id>', views.read_post),
            path('read_post_all', views.read_post_all),
        ]

DjangoCrudApp -> views.py

        from bson import ObjectId
        from django.http import HttpResponse

        from DjangoCrudApp.models import Posts
        from django.views.decorators.csrf import csrf_exempt


        @csrf_exempt
        def add_post(request):
            comment = request.POST.get("comment").split(",")
            tags = request.POST.get("tags").split(",")
            user_details = {"first_name": request.POST.get("first_name"), "last_name": request.POST.get("last_name")}
            post = Posts(post_title=request.POST.get("post_title"), post_description=request.POST.get("post_description"),
                         comment=comment, tags=tags, user_details=user_details)
            post.save()
            return HttpResponse("Inserted")


        @csrf_exempt
        def update_post(request, id):
            post = Posts.objects.get(_id=ObjectId(id))
            post.user_details['first_name'] = request.POST.get('first_name')
            post.save()
            return HttpResponse("Post Updated")


        def delete_post(id):
            post = Posts.objects.get(_id=ObjectId(id))
            post.delete()
            return HttpResponse("Post Deleted")


        def read_post(id):
            post = Posts.objects.get(_id=ObjectId(id))
            stringval = "First Name : " + post.user_details['first_name'] + " Last name : " + post.user_details[
                'last_name'] + " Post Title " + post.post_title + " Comment " + post.comment[0]
            return HttpResponse(stringval)


        def read_post_all():
            posts = Posts.objects.all()
            stringval = ""
            for post in posts:
                stringval += "First Name : " + post.user_details['first_name'] + " Last name : " + post.user_details[
                    'last_name'] + " Post Title " + post.post_title + " Comment " + post.comment[0] + "<br>"

            return HttpResponse(stringval)



DjangoCrudMongoDB -> settings.py 

    INSTALLED_APPS = [
        ...
        'django.contrib.staticfiles',
        'DjangoCrudApp',
    ]

![image](https://user-images.githubusercontent.com/75089445/129480861-9eafe49f-0d8d-40ec-a788-5992b2598444.png)

Postman

Create

![image](https://user-images.githubusercontent.com/75089445/129854934-ebd0ebf1-7811-4115-b78c-d784f7e4603d.png)

Compass

![image](https://user-images.githubusercontent.com/75089445/129855063-0448ca11-2156-4584-a352-7d53acb2848f.png)

Postman

Read

![image](https://user-images.githubusercontent.com/75089445/129855666-8e7fe984-0a0e-4fe5-936c-17431601e385.png)

Update

![image](https://user-images.githubusercontent.com/75089445/129857307-fa0d2dba-3ab8-4595-a1f0-4f17284364b9.png)

Delete

![image](https://user-images.githubusercontent.com/75089445/129858243-457cd50d-c833-4e78-a832-138febe11a69.png)
