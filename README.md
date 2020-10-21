
## Gateway iOS 

Our payment module allows you to easily integrate payments into your journey inside iOS Mi Vodafone app. By updating a session directly with the Gateway, you avoid the risk of handling sensitive card details on your server. The included sample app demonstrates the basics of installing and configuring the SDK to complete a simple payment.

## Usage

This simple example demonstrates how a `purchase` can be made .

One must "register" their account with `gringotts` ie, put all the
authentication details in the Application config. Usually via
`config/config.exs`

```elixir
# config/config.exs

config :gringotts, Gringotts.Gateways.Monei,
    userId: "your_secret_user_id",
    password: "your_secret_password",
    entityId: "your_secret_channel_id"
```

Copy and paste this code in a module or an `IEx` session, or use this handy
[`.iex.exs`][monei-bindings] for all the bindings.

```elixir
alias Gringotts.Gateways.Monei
alias Gringotts.CreditCard

# a fake sample card that will work now because the Gateway is by default
# in "test" mode.

card = %CreditCard{
  first_name: "Harry",
  last_name: "Potter",
  number: "4200000000000000",
  year: 2099, month: 12,
  verification_code:  "123",
  brand: "VISA"
}

# a sum of $42
amount = Money.new(42, :USD)

case Gringotts.purchase(Monei, amount, card) do
  {:ok,    %{id: id}} ->
    IO.puts("Payment authorized, reference token: '#{id}'")

  {:error, %{status_code: error, raw: raw_response}} ->
    IO.puts("Error: #{error}\nRaw:\n#{raw_response}")
end
```

[hexpm]: https://hex.pm/packages/gringotts
[monei]: http://www.monei.net
[monei-bindings]: https://gist.github.com/oyeb/a2e2ac5986cc90a12a6136f6bf1357e5

## Configuration 

In order to use the payment module, you must initialize the `VFMA10PaymentConfigurationBuilder` object with your journey's needs and build the object to generate the controller of iframe .

```
let paymentBuilder = VFMA10PaymentConfigurationBuilder()


let controller = paymentBuilder.build(inside: tray) 
```
Note:- if you need to build your payment journey inside tray (VFBottomOverlay) build your payment with it in builder . 
