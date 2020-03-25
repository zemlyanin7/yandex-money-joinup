[![яндекс касса](/i/yakassalogo.png "Яндекс Касса")](https://kassa.yandex.ru) / [помощь](https://yandex.ru/support/checkout/) / [api-докуметация](https://kassa.yandex.ru/docs/checkout-api/#api-yandex-kassy), [api-гайды](https://kassa.yandex.ru/docs/guides/#bystryj-start), [YandexCheckout.js](https://kassa.yandex.ru/docs/checkout-js/#yandexcheckout-js), [PHP-SDK](https://github.com/yandex-money/yandex-checkout-sdk-php), [mSDK](https://kassa.yandex.ru/docs/client-sdks/#mobil-nye-sdk), [en](https://checkout.yandex.com/docs/checkout-api/#using-the-api)

URL for notifications
===================

> URL for notifications - this is your script, the link to which you are writing [in shop configurations](#Personal-account) in our personal account (changes take effect instantly). Yandex.Checkout payment system automatically sends notifications to this URL [waiting_for_capture](#waiting_for_capture) or [succeded](#succeded) while your payer is making the payment. In response, our system should receive an HTTP 200 response. Details below.

## Payment Status Notifications Overview

### Payment passed, no callback

**q**: We formed a payment, received `confirmation_url`, we switched to payment and spent it, payment was successful, but there is no callback on the url for notifications.

**a**: Check out the following points:
1. Is the notification URL really accessible via HTTPS? Over HTTP, our notifications are not delivered.
2. In request for [create payment](https://checkout.yandex.com/developers/api#create_payment) was transferred `"capture": "true"`? If yes, then in this case only one notification succeded will come.
3. In response to our notification, we should receive an HTTP code of 200. Any other codes will be regarded as "delivery of the notification is not successful."

> A notification to your notification URL is delivered when the payment changes to status `"waiting_for_capture"` or `"succeded"`.

### waiting_for_capture

Status `"waiting_for_capture"` means that the payer has completed a successful payment and now the payment is awaiting your capture ([payment confirmation](https://checkout.yandex.com/developers/api#capture_payment), including partial) or cancel ([cancel](https://checkout.yandex.com/developers/api#cancel_payment)).

Attention! in Status `"waiting_for_capture"`, if it is payment by credit card, the payment will be 7 days, and for other payment methods 2 hours. In this interval you definitely need to execute capture or cancel. If you do not capture, the order will be canceled (cancel) automatically and the money will be returned to the payer.

**Example** (an example of our callback to your notification URL; such data will come to him from us, by POST request):
```
{"type":"notification","event":"payment.waiting_for_capture","object":{"id":"2185355e-000f-5081-a000-0000000",
"status":"waiting_for_capture","paid":true,"amount":{"value":"10.00","currency":"RUB"},"created_at":"2017-09-27T12:07:58.702Z",
"expires_at":"2017-11-03T12:08:23.080Z","metadata":{},"payment_method":{"type":"bank_card","id":"2185355e-000f-5081-a000-0000000",
"saved":false,"card":{"last4":"1026","expiry_month":"12","expiry_year":"2025","card_type":"Unknown"},"title":"Bank card *1026"},
"recipient":{"account_id":"000005","gateway_id":"0000015"}}}
```

### succeded

Status `"succeded"` means that the payer made a successful payment and the money will be debited in favor of your store.

**Example** (an example of our callback to your notification URL; such data will come to him from us, by POST request)::
```
{"type":"notification","event":"payment.succeeded","object":{"id":"2203aa1d-000f-5000-8000-17102541fd31",
"status":"succeeded","paid":true,"amount":{"value":"1.00","currency":"RUB"},"captured_at":"2018-01-31T10:12:06.249Z",
"created_at":"2018-01-31T10:11:41.499Z","description":"Оплата тестового заказа №100500 для магазина Вереница","metadata":{"zakaz":"100500","param3":"value3","param2":"value2"},
"payment_method":{"type":"bank_card","id":"2203aa1d-000f-5000-8000-17102541fd31","saved":false,
"card":{"last4":"1026","expiry_month":"12","expiry_year":"2025","card_type":"Unknown"},"title":"Bank card *1026"},
"recipient":{"account_id":"500105","gateway_id":"1500105"},"refunded_amount":{"value":"0.00","currency":"RUB"},"test":true}}
```
## Personal account

In your Yandex.Checkout personal account, you can specify the necessary URL for your script for our notifications.

![url for notifications in PA.Checkout](/checkout-api/api.notification.url.lk.png "url for notifications in PA.Checkout")

## Nuances

### PHP

**q**: We did everything as described above, but our PHP script does not receive any data (the request body is empty).

**a**: Most likely you are intercepting (receiving data) a method `$_POST` and therefore get an empty result. Below are the correct working options:

Variant1:  
```
  $source = file_get_contents('php://input');
  $json = json_decode($source, true);
```

Variant2:  
`file_put_contents ("путь_до_лога", print_r(json_decode(file_get_contents("php://input")), true));`.

<!--
### Perl, Content-Length

**q**: Делаем `read( STDIN, $buffer, $ENV{'CONTENT_LENGTH'} );`, но не получается.

**q**: В настоящий момент мы не передаем `Content-Length`. Есть постановка на модификацию протокола, но до этого момента, пожлалуйста, учитывайте, что в Header мы не передаем `Content-Length`.
-->
### IP

**q**: What IPs will notifications come from?

**a**: List of our IPs from which you will receive notifications:
```
185.71.76.0/27
185.71.77.0/27
77.75.153.0/25
77.75.154.128/25
2a02:5180:0:1509::/64
2a02:5180:0:2655::/64
2a02:5180:0:1533::/64
2a02:5180:0:2669::/64
```
<!--
185.71.77.2
185.71.77.3
185.71.77.4
185.71.77.5
185.71.76.2
185.71.76.3
185.71.76.4
-->

### Timing

**q**: If my notification URL was not available (server rebooted, temporary network unavailability, etc.), how many additional notifications will there be and at what interval?

**a**: There will be 7 notifications in total. From the first to the moment of the last notification, 24 hours will pass. Current notification intervals in seconds: `10, 42, 84, 168, 672, 5376, 86016` second.

### Change URL

**q**: If there was `payment-1`, while in the settings was `url-for-notifications-1`. Notification was not delivered. We changed the settings to `url-for-notifications-2`. Where notifications will be delivered by `payment-1`?

**a**: Notifications are delivered to the URL that was valid at the time of payment. This means that if there was `payment-1` and setting `url-for-notifications-1`, then after changing the setting to `url-for-notifications-2` notifications will be delivered to `url-for-notifications-1`.

### Тще fications after capture

**q**: We did not wait for the notification, because through [request order status](https://checkout.yandex.com/developers/api#capture_payment) found out that the status is already `"waiting_for_capture"` and performed **capture**. Why do notifications keep coming?

**a**: Notification delivery continues regardless of status change `"waiting_for_capture"` to other (`canceled` or `succeeded`) until the HTTP code 200 is received in response or until executed [7 delivery attempts](#Timing).

### Temporary replacement of url for notifications for testing

https://webhook.site/

<!--
2all: если у мерч нет вебхука (он же урл для уведомлений), скажем, его еще надо сделать, а тестироваться уже хочется, можно использовать сервис https://webhook.site/

1. Заходим по урлу,
2. Нажимаем NEW URL, заполняем дефолтом (там в подсказках предлагается). Получаем уникальный урл.
3. Прописываем урл в тестовом магазине.
4. Получаем уведомления, открыв урл эти уведомления видны.

Например, система дала урл https://webhook.site/78164af0-367e-41d1-b7dc-623381bdc478
При платеже наш колбек виден
https://webhook.site/#/78164af0-367e-41d1-b7dc-623381bdc478/9ec54a73-0673-4872-8a06-a7844c7c887c/0
-->
