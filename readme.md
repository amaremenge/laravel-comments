
# laraComments  
  
This package can be used to comment on any model you have in your application.  
  
All comments are stored in a single table with a polymorphic relation for content and a one-to-many relation for the user who posted the comment.  

Example is php realization - is bad way. Good way is using api for get data through JS and build ui with Vue js (or any another library). 
  
### Features  
  
- [x] View comments  
- [x] Create comment  
- [x] Delete comment  
- [x] Edit comment  
- [x] Reply to comment  
- [x] Authorization rules  
- [x] View customization  
- [x] Dispatch events  
- [x] API for basic function: get, update, delete, create
- [x] HTML filter customization (using HTMLPurifier)
  
## Requirements
- php 7.1 + 
- laravel 5.5 +
## Installation    
```bash  
composer require tizis/lara-comments  
```  
### Run migrations  
  
We need to create the table for comments.  
  
```bash  
php artisan migrate  
```  
  
### Add Commenter trait to your User model  
  
Add the `Commenter` trait to your User model so that you can retrieve the comments for a user:  
  
```php  
use Laravelista\Comments\Commenter;  
  
class User extends Authenticatable  
{  
    use ..., Commenter;  
}  
```  
  
### Add Commentable trait to models  
  
Add the `Commentable` trait and the `ICommentable` interface to the model for which you want to enable comments for:  
  
```php  
  
use tizis\laraComments\Contracts\ICommentable;  
use tizis\laraComments\Traits\Commentable;  
  
class Post extends Model implements ICommentable  
{  
    use Commentable;  
      
```  
  
### Publish Config & configure (optional)  
  
In the `config` file you can specify:  
  
- where is your User model located; the default is `\App\User::class`  
- policy prefix, you can create custom policy class and implement ICommentPolicy;  
- allow tags for html filter
  
Publish the config file (optional):  
  
```bash  
php artisan vendor:publish --provider="tizis\laraComments\Providers\ServiceProvider" --tag=config  
```  
  
### Publish views (customization)  
  
The default UI is made for Bootstrap 4, but you can change it however you want.  
  
```bash  
php artisan vendor:publish --provider="tizis\laraComments\Providers\ServiceProvider" --tag=views  
```  
  
## Usage  
  
In the view where you want to display comments, place this code and modify it:  
  
```  
@comments(['model' => $book])  
@endcomments  
```  
  
In the example above we are setting the `commentable_type` to the class of the book. We are also passing the `commentable_id` the `id` of the book so that we know to which book the comments relate to. Behind the scenes, the package detects the currently logged in user if any.  
  
If you open the page containing the view where you have placed the above code, you should see a working comments form.  
  
## Events  
  
This package fires events to let you know when things happen.  
  
- `tizis\laraComments\Events\CommentCreated`  
- `tizis\laraComments\Events\CommentUpdated`  
- `tizis\laraComments\Events\CommentDeleted`