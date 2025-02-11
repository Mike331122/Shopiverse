<template>
    <div class="grid h-16 place-items-center">

        <div class="p-4 border" id="checkout-page">
                <!-- Display a payment form -->
                <form @submit.prevent="handleSubmit" id="payment-form">
                    <div id="payment-element">
                        <!--Stripe.js injects the Payment Element-->
                    </div>
                        <button id="submit">
                            <div class="spinner hidden" id="spinner"></div>
                            <span id="button-text">Pay now</span>
                        </button>
                    <div id="payment-message" class="hidden"></div>
                </form>
        </div>

    </div>
</template>


<script setup>
    const config = useRuntimeConfig()
    const {stripePK} = config.public
    let stripe
    let elements
    let clientSecret
    const cart = useCart()
    const calcTotalCart = () => {
        let total = 0
        cart.value.forEach(product => {
            total += parseFloat(product.price)
        })

        return total * 100
    }

    onMounted(async () => {
        stripe = await Stripe(stripePK)
        const initialize = async () => {
            const {data} = await useFetch('/api/stripe/paymentintent', {
                method: 'post',
                body: {
                    amount: calcTotalCart()
                }
            })

            clientSecret = data.value
            const apperance = {
                them: 'stripe'
            }

            const paymentElementOption = {
                layouts: 'tabs'
            }

            elements = stripe.elements({
                apperance,
                clientSecret,
            })

            const paymentElement =  elements.create('payment', paymentElementOption)

            if (document.querySelector('#payment-element')) {
                paymentElement.mount('#payment-element');
            } else {
                console.error("Error: #payment-element does not exist.");
            }
        }

        setTimeout(() => {
          initialize();
          checkStatus();
        }, 0); // Mikro-opóźnienie, aby upewnić się, że DOM jest gotowy
    })

    const handleSubmit = async () => {
        setLoading(true);

        const { error } = await stripe.confirmPayment({
            elements,
            confirmParams: {
            // Make sure to change this to your payment completion page
            return_url: "https://shopiverse-fake-store.netlify.app/payment-success",
            },
        });

        // This point will only be reached if there is an immediate error when
        // confirming the payment. Otherwise, your customer will be redirected to
        // your `return_url`. For some payment methods like iDEAL, your customer will
        // be redirected to an intermediate site first to authorize the payment, then
        // redirected to the `return_url`.
        if (error.type === "card_error" || error.type === "validation_error") {
            showMessage(error.message);
        } else {
            showMessage("An unexpected error occurred.");
        }

        setLoading(false);
    }

    async function checkStatus() {
  const clientSecret = new URLSearchParams(window.location.search).get(
    "payment_intent_client_secret"
  );

  if (!clientSecret) {
    return;
  }

  const { paymentIntent } = await stripe.retrievePaymentIntent(clientSecret);

  switch (paymentIntent.status) {
    case "succeeded":
      showMessage("Payment succeeded!");
      break;
    case "processing":
      showMessage("Your payment is processing.");
      break;
    case "requires_payment_method":
      showMessage("Your payment was not successful, please try again.");
      break;
    default:
      showMessage("Something went wrong.");
      break;
  }
}

// ------- UI helpers -------

function showMessage(messageText) {
  const messageContainer = document.querySelector("#payment-message");

  messageContainer.classList.remove("hidden");
  messageContainer.textContent = messageText;

  setTimeout(function () {
    messageContainer.classList.add("hidden");
    messageContainer.textContent = "";
  }, 4000);
}

// Show a spinner on payment submission
function setLoading(isLoading) {
  const submitButton = document.querySelector("#submit");
  const spinner = document.querySelector("#spinner");
  const buttonText = document.querySelector("#button-text");

  if (!submitButton || !spinner || !buttonText) {
    console.error("One of the required elements is missing!");
    return; // Jeśli elementy nie istnieją, zakończ funkcję
  } 

  if (isLoading) {
    submitButton.disabled = true;
    spinner.classList.remove("hidden");
    buttonText.classList.add("hidden");
  } else {
    submitButton.disabled = false;
    spinner.classList.add("hidden");
    buttonText.classList.remove("hidden");
  }
}

</script>

<style scoped>
/* Variables */
* {
  box-sizing: border-box;
}

#checkout-page {
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 16px;
  -webkit-font-smoothing: antialiased;
  display: flex;
  justify-content: center;
  align-content: center;
  height: 100vh;
  width: 100vw;
}

form {
  width: 30vw;
  min-width: 500px;
  align-self: center;
  box-shadow: 0px 0px 0px 0.5px rgba(50, 50, 93, 0.1),
    0px 2px 5px 0px rgba(50, 50, 93, 0.1), 0px 1px 1.5px 0px rgba(0, 0, 0, 0.07);
  border-radius: 7px;
  padding: 40px;
}

.hidden {
  display: none;
}

#payment-message {
  color: rgb(105, 115, 134);
  font-size: 16px;
  line-height: 20px;
  padding-top: 12px;
  text-align: center;
}

#payment-element {
  margin-bottom: 24px;
}

/* Buttons and links */
button {
  background: #5469d4;
  font-family: Arial, sans-serif;
  color: #ffffff;
  border-radius: 4px;
  border: 0;
  padding: 12px 16px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  display: block;
  transition: all 0.2s ease;
  box-shadow: 0px 4px 5.5px 0px rgba(0, 0, 0, 0.07);
  width: 100%;
}
button:hover {
  filter: contrast(115%);
}
button:disabled {
  opacity: 0.5;
  cursor: default;
}

/* spinner/processing state, errors */
.spinner,
.spinner:before,
.spinner:after {
  border-radius: 50%;
}
.spinner {
  color: #ffffff;
  font-size: 22px;
  text-indent: -99999px;
  margin: 0px auto;
  position: relative;
  width: 20px;
  height: 20px;
  box-shadow: inset 0 0 0 2px;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
}
.spinner:before,
.spinner:after {
  position: absolute;
  content: "";
}
.spinner:before {
  width: 10.4px;
  height: 20.4px;
  background: #5469d4;
  border-radius: 20.4px 0 0 20.4px;
  top: -0.2px;
  left: -0.2px;
  -webkit-transform-origin: 10.4px 10.2px;
  transform-origin: 10.4px 10.2px;
  -webkit-animation: loading 2s infinite ease 1.5s;
  animation: loading 2s infinite ease 1.5s;
}
.spinner:after {
  width: 10.4px;
  height: 10.2px;
  background: #5469d4;
  border-radius: 0 10.2px 10.2px 0;
  top: -0.1px;
  left: 10.2px;
  -webkit-transform-origin: 0px 10.2px;
  transform-origin: 0px 10.2px;
  -webkit-animation: loading 2s infinite ease;
  animation: loading 2s infinite ease;
}

@-webkit-keyframes loading {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes loading {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

@media only screen and (max-width: 600px) {
  form {
    width: 80vw;
    min-width: initial;
  }
}
</style>