#UddoktaPay

```
scp ./UddoktaPay.zip root@143.110.188.116:/var/www
cd /var/www && unzip UddoktaPay.zip -d /var/www/uddoktapay
cd /etc/nginx/sites-available && sudo nano uddoktapay.conf
```

```
server {

    root /var/www/uddoktapay;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name payment.tryshop.in;

    location / {
     try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }

}
```
 
```
sudo ln -s /etc/nginx/sites-available/uddoktapay.conf /etc/nginx/sites-enabled/
cd /usr/local/src
wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xvf ioncube_loaders_lin_x86-64.tar.gz
cd ioncube/
php -i | grep  extension_dir
cp /usr/local/src/ioncube/ioncube_loader_lin_7.4.so /usr/lib/php/20190902
echo "zend_extension=ioncube_loader_lin_7.4.so" > /etc/php/7.4/mods-available/ioncube.ini
ln -s /etc/php/7.4/mods-available/ioncube.ini /etc/php/7.4/cli/conf.d/01-ioncube.ini
systemctl restart php7.4-fpm
sudo apt-get install php7.4-curl
sudo apt install php7.4-bcmath
sudo certbot --nginx -d payment.tryshop.in
sudo chmod -R 777 /var/www/uddoktapay/app/config/database.php && 
sudo chmod -R 777 /var/www/uddoktapay/app/config/config.php && 
sudo chmod -R 777 /var/www/uddoktapay//app/logs && 
sudo chmod -R 777 /var/www/uddoktapay/app/core && 
sudo chmod -R 777 /var/www/uddoktapay/install/install.uddoktapay

https://serverok.in/how-to-install-ioncube-on-ubuntu-20-04
```

#Setup Database and Complete Installing...

#Server Code


routes/api.php
```
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\UddoktapayController;

Route::post( 'webhook', [UddoktapayController::class, 'webhook'] )->name( 'uddoktapay.webhook' );
Route::post( 'pay', [UddoktapayController::class, 'pay'] )->name( 'uddoktapay.pay' );
```
routes/api.php
```
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\UddoktapayController;

Route::post( 'webhook', [UddoktapayController::class, 'webhook'] )->name( 'uddoktapay.webhook' );
Route::post( 'pay', [UddoktapayController::class, 'pay'] )->name( 'uddoktapay.pay' );
```

app/http/controllers/UddoktapayController.php

```
<?php

namespace App\Http\Controllers;

use App\Library\UddoktaPay;
use App\Upay;
use App\User;
use Illuminate\Http\Request;
use Log;
class UddoktapayController extends Controller {

    /**
     * Show the payment view
     *
     * @return void
     */
    public function show() {
        return view( 'uddoktapay.payment-form' );
    }

    /**
     * Initializes the payment
     *
     * @param Request $request
     * @return void
     */
    public function pay( Request $request ) {
        $validatedData = $request->validate( [
            'amount'    => ['required', 'integer'],
        ] );
            
        $user = User::find($request->user_id);

        $order = Upay::create( [
            'user_id'   => $user->id,
            'full_name' => $user->name,
            'email'     => $user->email,
            'amount'    => $validatedData['amount'],
        ] );

        $requestData = [
            'full_name'    => $user->name,
            'email'        => $user->email,
            'amount'       => $validatedData['amount'],
            'metadata'     => $order->id,
            'redirect_url' => route( 'uddoktapay.success' ),
            'cancel_url'   => route( 'uddoktapay.cancel' ),
            'webhook_url'  => env( "UDDOKTAPAY_WEBHOOK_DOMAIN" ),
        ];

        $paymentUrl = UddoktaPay::init_payment( $requestData );
        $o=Upay::find($order->id);
        $o->paymenturl=$paymentUrl;
        $o->save();
        return $paymentUrl;
    }

    /**
     * Reponse from sever
     *
     * @param Request $request
     * @return void
     */
    public function webhook( Request $request ) {

        $headerApi =  $request->header('RT-UDDOKTAPAY-API-KEY');

        if ( $headerApi != env( "UDDOKTAPAY_API_KEY" ) ) {
            return response( "Unauthorized Action", 403 );
        }

        $validatedData = $request->validate([
            'invoice_id'     => 'required',
            'metadata'       => 'required',
            'payment_method' => 'required',
            'sender_number'  => 'required',
            'transaction_id' => 'required',
            'status'         => 'required',
        ]);

        Upay::findOrFail($validatedData['metadata'])->update( [
            'status'         => $validatedData['status'],
            'payment_method' => $validatedData['payment_method'],
            'sender_number'  => $validatedData['sender_number'],
            'transaction_id' => $validatedData['transaction_id'],
            'invoice_id'     => $validatedData['invoice_id'],
        ]);

        $pay=Upay::find($validatedData['metadata']);

        if($pay->status=='COMPLETED'){
            $user = User::find($pay->user_id);
            $user->wallet = $user->wallet+$pay->amount;
            $user->save();
        }

        return response( 'Database Updated' );
    }

    /**
     * Success URL
     *
     * @return void
     */
    public function success() {
        return redirect('https://canvabd.com/upays');
    }


    public function upaytnx($id) {

        return Upay::where('user_id',$id)->get();
    }
    /**
     * Cancel URL
     *
     * @return void
     */
    public function cancel() {
        return redirect('https://canvabd.com/paymentfailed');
    }

    public function cancleallupay()
    {
        $transaction = Upay::where('status',NULL)->get();
        foreach ($transaction as $key => $var) {
            $value=Upay::find($var->id);
            $value->delete();
        }
        return Redirect::back();
    }


}

```

app/Library/UddoktaPay.php

```
<?php

namespace App\Library;

// use Illuminate\Support\Facades\Http;

class UddoktaPay {
    
    /**
     * Send payment request
     *
     * @param array $requestData
     * @return void
     */
    public static function init_payment($requestData) {

        $client = new \GuzzleHttp\Client();
        $url= env( "UDDOKTAPAY_PAYMENT_DOMAIN" ) . "/api/checkout";

        // $response = Http::withHeaders( [
        //     'Content-Type'          => 'application/json',
        //     'RT-UDDOKTAPAY-API-KEY' => env( "UDDOKTAPAY_API_KEY" ),
        // ] )
        // ->asJson()
        // ->withBody( json_encode( $requestData ), "JSON" )
        // ->post( env( "UDDOKTAPAY_PAYMENT_DOMAIN" ) . "/api/checkout" );

        
        $response = $client->request('POST', $url, [ 
            'headers' => [
                'Content-Type'          => 'application/json',
                'RT-UDDOKTAPAY-API-KEY' => env( "UDDOKTAPAY_API_KEY" ),
            ],
            'body' => json_encode( $requestData )
            
        ]);

        $content  = $response->getBody()->getContents();
        $d=json_decode($content);

        if ( $d->status ) {
            return $d->payment_url;
        } else {
            dd( $response->getBody()->getContents() );
        }
    }
}


```

```
UDDOKTAPAY_API_KEY=E626-7BC5-E1FE-BDCB
UDDOKTAPAY_PAYMENT_DOMAIN='https://payment.tryshop.in'
UDDOKTAPAY_WEBHOOK_DOMAIN='https://admin.tryshop.in/api/webhook'
```



.env

```
UDDOKTAPAY_API_KEY=E626-7BC5-E1FE-BDCB
UDDOKTAPAY_PAYMENT_DOMAIN='https://payment.tryshop.in'
UDDOKTAPAY_WEBHOOK_DOMAIN='https://admin.tryshop.in/api/webhook'
```
