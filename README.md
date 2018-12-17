
-------------HONEYPOT----------------- 

"Honeypot" method of spam prevention is an effective way to protect your site from spam bots.
An input field is created that should be left empty by the real user of the site but will be filled by the spam bot.
This package creates a hidden <DIV> with two fields in it, honeypot field my name and a honeytime field,
an encrypted timestamp and notes the time when the page was served to user. When the form containing these inputs (invisible to the user) 
is submitted to your application, a custom validator that comes with the package checks that the honeypot field is empty 
and also checks the time it took for the user to fill out the form. If the form was filled out too quickly (i.e. less than 5 seconds) 
or if there was a value put in the honeypot field, this submission is most likely from a spam bot.

Steps:

1) Move to Your App Folder using Cd Command
2) Then type this Command "composer require msurguy/honeypot"
3) It will install the honeypot in our app
4) Then how To use it 
   
   
//For Laravel 5 & Above versions//

   
In Your Form Put this line {!! Honeypot::generate('my_name', 'my_time') !!}



5) So above code will generate this kind of markup 

<div id="my_name_wrap" style="display:none;">
    <input name="my_name" type="text" value="" id="my_name">
    <input name="my_time" type="text" value="*************************************************************************">
</div>

6) Now You can Validate Your Honeypot Check Like This 


$rules = array(
    'email'     => "required|email",
    ...
    'my_name'   => 'honeypot',
    'my_time'   => 'required|honeytime:5'
);

$validator = Validator::make(Input::get(), $rules);


7) Disable it by the following code to test it. 
Once disabled, validation will pass.  

  Honeypot::disable();
  
  



-------------CAPTCHA----------------------

A program intended to distinguish human from machine input, 
way of thwarting spam and automated extraction of data from websites.

1) Add Captcha_Key and Captcha_secret in .env file

2) Add <script src='https://www.google.com/recaptcha/api.js'></script> to app.blade.php in line 15

3) Add in register.blade.php line 51
                               <div class="form-group row">
                                     <div class="col-md-6 offset-md-4">
                                         <div class="g-recaptcha" data-sitekey="{{env('CAPTCHA_KEY')}}"></div>
                                     @if($errors->has('g-recaptcha-response'))
                                         <span class="invalid-feedback" style="display:block">
                                             <strong>
                                                 {{$errors->first('g-captcha-response')}}
                                             </strong>
                                         </span>
                                         @endif
                                          </div>
 
 
4. Run in terminal 
composer require google/recaptcha '~1.1'  

5. Run in terminal 
php artisan make:rule Captcha 

6. Add following code in line 27 Captcha.php

public function passes($attribute, $value)
    {
        $recaptcha = new ReCaptcha(env('CAPTCHA_SECRET'));
        $response = $recaptcha->verify($value, $_SERVER['REMOTE_ADDR']);
        return $response->isSuccess();
    }

7. Add following to the public message line 41
return 'Please complete the recaptcha to submit the form';

8. Register controller 
use App\Rules\Captcha;

9. Add following in line 56 RegisterController.php

 'g-captcha-response' => new Captcha(),
 
10. Run in terminal the following 
php artisan serve 

11. Change captch to recaptcha in line 56 RegisterController.php

12.  Change captch to recaptcha in line 57 register.blade.php




