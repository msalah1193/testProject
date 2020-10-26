
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
in order to add message Placeholder to replace in feedback message , you should add `messagePlaceholder` before building the module 

> for example if your message "your payment 50$ is successfully done at 12/5/2020 " 
> your `messagePlaceholder` object should contain ["50$","12/5/2020"] the value that didn't depend on `WCS`, and The arrangement is taken into account .


```swift

 let messagePlaceholder = FeedbackPlaceholder(ok: <# filled object of MessagePlaceholder#>)
 paymentBuilder.add(messagePlaceholder: messagePlaceholder)
 let controller = paymentBuilder.build() // handle the result 

```
in order to disable Refund in timeout and cancel , you should add `disableRefund` before building the module 

> by default refund is enable 

```swift

 paymentBuilder.disableRefund()
 let controller = paymentBuilder.build() // handle the result 

```
### Feedback Actions 
You can override your feedback action for (close - primary - secondary) , using the dedicated delegate methods

> by default actions is dismissing the controller
  
  - success actions

Tells the builder to delegate the success action to a delegate class.

```swift

 paymentBuilder.delegateSuccessActions(to: <# class or controller that implement MVA10PaymentSuccessFeedbackActionDelegate protocol#>)
 let controller = paymentBuilder.build() // handle the result 

```
The success feedback controller overrides click on its own button. Use the dedicated `MVA10PaymentSuccessFeedbackActionDelegate` methods .

```swift
    // your override action for PrimaryAction(the first one)
   func paymentSuccessPrimaryAction(result: VFMVA10PaymentResult, navigator: VFPaymentNavigator) {
       
    }
   
     // your override action for SecondaryAction(the second one)
    func paymentSuccessSecondaryAction(result: VFMVA10PaymentResult, navigator: VFPaymentNavigator) {
        navigator.dismiss(animated: true, completion: nil)
        
    }
    
    // your override action for closeAction(only on the tray)
    func paymentSuccessCloseAction(result: VFMVA10PaymentResult, navigator: VFPaymentNavigator) {
        
    }
```
> `VFMVA10PaymentResult` refer to payment journey result .

> `VFPaymentNavigator` refer to navigator inside the payment journey so you SHOULD USE it to navigate inside the payment module .


  - failure actions

Tells the builder to delegate the failure action to a delegate class.

```swift

 paymentBuilder.delegateFailureActions(to: <# class or controller that implement MVA10PaymentFailureFeedbackActionDelegate protocol#>)
 let controller = paymentBuilder.build() // handle the result 

```
The Failure feedback controller overrides click on its own button. Use the dedicated `MVA10PaymentFailureFeedbackActionDelegate` methods .

```swift
    // your override action for PrimaryAction(the first one)
   func paymentFailurePrimaryAction(result: VFMVA10PaymentResult, type: MVA10PaymentFailureType, navigator: VFPaymentNavigator) {
       
    }
   
     // your override action for SecondaryAction(the second one)
    func paymentFailureSecondaryAction(result: VFMVA10PaymentResult,type: MVA10PaymentFailureType, navigator: VFPaymentNavigator) {
        navigator.dismiss(animated: true, completion: nil)
        
    }
    
    // your override action for closeAction(only on the tray)
    func paymentFailureCloseAction(result: VFMVA10PaymentResult,type: MVA10PaymentFailureType, navigator: VFPaymentNavigator) {
        
    }
```
> `VFMVA10PaymentResult` refer to payment journey result .

> `MVA10PaymentFailureType` refer to the error type that happen .
```swift
    case invalidCard
    case incorrectCardData
    case unsupportedCreditCard
    case systemNotAvailable
    case timeout
    case general
    case serverError(errorCode: String?) // only when load payment url

```
> `VFPaymentNavigator` refer to navigator inside the payment journey so you SHOULD USE it to navigate inside the payment module .
