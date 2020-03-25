[![яндекс касса](/i/yakassalogo.png "Яндекс Касса")](https://kassa.yandex.ru) / [помощь](https://yandex.ru/support/checkout/) / [api-докуметация](https://kassa.yandex.ru/docs/checkout-api/#api-yandex-kassy), [api-гайды](https://kassa.yandex.ru/docs/guides/#bystryj-start), [YandexCheckout.js](https://kassa.yandex.ru/docs/checkout-js/#yandexcheckout-js), [en](https://checkout.yandex.com/docs/checkout-api/#using-the-api)

Test environment API.Yandex.Checkout
===================================
> Easy to install and easy to use environment in order to learn our API to developers who are integrating or interested.

### N.B.

** Atention! ** Starting from 04.05.2018 we change password (`api_key`). If you use old password, just update. Old password ~test_OGpIHQMdVeLp1giWoPn033vKRxNUAGcAdZizIymbOfg~.

```
### For independent tests without Insomnia
	"shopid": "54401",
	"api_key": "test_Fh8hUAVVBGUGbjmlzba6TB0iyUbos_lueTHE-axOwM0"
```
<!--
### BUG FIX 21.02.2018

Unfortunately, during the operation of the test store there was one technical overlay. Previously, in requests it was not required to transfer the product number (and it should be so in the usual shopid), but we tested multi-product mode and now in the payments request send:

```
	"recipient": {
    		"gateway_id": "289250"
  	}
```
Otherwise, you will receive an error of the form:
```
{
	"type": "error",
	"id": "fb202ed8-e975-46b1-991f-b5ec948d1fee",
	"code": "invalid_request",
	"description": "Failed to resolve gateway id, please provide recipient field in request",
	"parameter": "recipient"
}
```
This is temporary. We eill correct it.
-->

### Overview

![test environment example for testing API.Yandex.Checkout in REST client Insomnia](/checkout-api/sample/rest/insomnia/api.yandex.checkout.insomnia-sample.png "test environment example for testing API.Yandex.Checkout in REST client Insomnia")

 * The test environment provides examples for payment by credit card 
 (there are no real means, only test ones).
 * Use *CC for testong*: `5555555555554444`, exp date: `12 20`, cvv: `000`. Or [anothet CC for testing](https://checkout.yandex.com/developers/using-api/testing#test-bank-card).
 * You will interact with our work system in real-time:
   * fulfilling the request, you will get a visual representation of what request is sent, what response is returned, etc.;
   * You can change the request if necessary.
 * At each step there is help on the "Docs" tab and the necessary links.
 * Notifications of changes in order status are received on [URL for notifications](/checkout-api/031-01%20url%20for%20notification.md),but you won’t see them, because it is general, closed, so the status of the order in this public store must be requested through [status request](https://checkout.yandex.com/developers/api#capture_payment).
 * **No registration required**, everything works out of the box.

---

## Installation Instructions
- [ ] Download the program distribution [Insomnia](https://insomnia.rest/) and install it (~65 Mb; popular, free REST-client, working on Windows, OSX and Ubuntu);
- [ ] **[Download the query collection API.Yandex.Checkout](/checkout-api/sample/rest/insomnia/API.Yandex.Kassa_test-env-for-insomnia.v1-002.ru.json.zip)** (~10 Кб), unzip and [import](#Import-Collections) in Insomnia;
- [ ] Use default settings or [configure](#Setting) you shopid and secret key.

#### Import Collections

Import a query collection file API.Checkout in Insomnia.

> ##### Step 1
> ![Insomnia import step1](/checkout-api/sample/rest/insomnia/insomnia-import-step1.png "Insomnia import step1")

> ##### Step 2
> ![Insomnia import step2](/checkout-api/sample/rest/insomnia/insomnia-import-step2.png "Insomnia import step2")

> ##### Step 3
> ![Insomnia import step3](/checkout-api/sample/rest/insomnia/insomnia-import-step3.png "Insomnia import step3")

#### Setting

In the settings, write down your test shopid and secret key (if they are not, leave those that are registered by default). The test environment is ready.

> ![Insomnia settings step1](/checkout-api/sample/rest/insomnia/settings-step1.png "Insomnia settings step1")
> ![Insomnia settings step2](/checkout-api/sample/rest/insomnia/settings-step2.png "Insomnia settings step2")

<!--
#### Links
* [Insomnia](https://insomnia.rest/) - convenient, free REST client for all operating systems.
* Файл с коллекцией запросов API.Кассы
* Документация API.Кассы
* Гайды API.Кассы
:mortar_board: Тестовые окружение для работы с нашим API, это подготовленная нашими специалистами легкий у установке комлекс

-->
