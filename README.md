
## VFMVA10Payment iOS 

Our payment module allows you to easily integrate payments into your journey inside iOS Mi Vodafone app.

## Usage

This simple example demonstrates how a `purchase` can be made .


## Configuration 

In order to use the payment module, you must initialize the `VFMA10PaymentConfigurationBuilder` object with your journey's needs and build the object to generate the controller of iframe .

```
let paymentBuilder = VFMA10PaymentConfigurationBuilder()


let controller = paymentBuilder.build() 
```
> if you need to build your payment journey inside tray (VFBottomOverlayController) , use ```paymentBuilder.build(inside: tray)``` . 

### Basic Implementation
Using your journey Id , amount and paymentUserInfo you can build the basic module:

```swift
let amount = "<# your amount in double #>"
let concept = "<# your journey's concepto value #>"
let wcsChannel = "<# your journey's wcsChannel to retrieve the WCS content#>"
let documentId = "<#document Id for the one who pay#>"
let accountId = "<#account Id for the one who pay#>"
let subscriptionId = "<#subscription Id for the one who pay#>"
let journeyId = "<# your journey's id #>"
let paymentInfo = PaymentUserInfo(documentId: documentId, accountId: accountId, subscriptionId: subscriptionId)
let paramModel = VFMVA10PaymnetParamsModel(paymentInfo: paymentInfo, amount: amount, journeyId: journeyId)

 let paymentBuilder = VFMA10PaymentConfigurationBuilder()
 paymentBuilder.with(paramsModel: paramModel, concept: concept, wcsChannel: wcsChannel)

 let controller = paymentBuilder.build() // handle the result (by pushing - presenting , ....)

```
> Note:- for anonmous journey . 

> let documentId = "<#should be empty string #>"

> let accountId = "<#should be "ANONMOUS"#>"

> let subscriptionId = "<#subscription Id for the one who paid for#>". 


in order to change your content language , you should change the `language` before building the module 

> Possible languages values include, `es`and `en` and by default is `es`

```swift

 paymentBuilder.changeLanguage(to "<# journey's language #>") 
 let controller = paymentBuilder.build() // handle the result 

```
in order to add credit Card to use it instead of entering your credit card info, you should add `VFCreditCardInfo` before building the module 

> using saved credit card , consider a different journey with a different journey id 

```swift
 paymentBuilder.add(creditCard: <# filled object of VFCreditCardInfo#>)
 let controller = paymentBuilder.build() // handle the result 

```
in order to add message Placeholder to replace in success message , you should add `messagePlaceholder` before building the module 

> for example if your message "your payment 50$ is successfully done at 12/5/2020 " 
> your `messagePlaceholder` object should contain ["50$","12/5/2020"] the value that didn't depend on `WCS`, and The arrangement is taken into account .


```swift

 let messagePlaceholder = FeedbackPlaceholder(ok: <# filled object of MessagePlaceholder#>)
 paymentBuilder.add(messagePlaceholder: messagePlaceholder)
 let controller = paymentBuilder.build() // handle the result 

```
