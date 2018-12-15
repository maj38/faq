# faq

To run the FAQ project:

1. Git clone https://github.com/maj38/faq.git
2. CD into FAQ and run composer install
3. cp .env.example to .env
4. setup database/ with sqlite (https://laravel.com/docs/5.7/eloquent)

Adding Captcha (honeypot) for security (spams)

1. Added Captcha_Key and Captcha_secret in .env file
2. Added <script src='https://www.google.com/recaptcha/api.js'></script> to app.blade.php in line 15
3. Added in register.blade.php line 51
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

6. Added following code in line 27 Captcha.php

public function passes($attribute, $value)
    {
        $recaptcha = new ReCaptcha(env('CAPTCHA_SECRET'));
        $response = $recaptcha->verify($value, $_SERVER['REMOTE_ADDR']);
        return $response->isSuccess();
    }

7. Added following to the public message line 41
return 'Please complete the recaptcha to submit the form';

8. Register controller 
use App\Rules\Captcha;

9. Add following in line 56 RegisterController.php

 'g-captcha-response' => new Captcha(),
 
10. Run in terminal the following 
php artisan serve 

11. Change captch to recaptcha in line 56 RegisterController.php
12.  Change captch to recaptcha in line 57 register.blade.php


Honeypot 

1. added following to web.php line 18

Route::get('myform', function()
{
    return View::make('myapp');
});

