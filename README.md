# Last-Task
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Checkout</title>
    <style>
        /* Base styles */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            line-height: 1.6;
            color: #333;
            background-color: #f5f5f5;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .checkout-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
        }

        @media (min-width: 768px) {
            .checkout-grid {
                grid-template-columns: 2fr 1fr;
            }
        }

        /* Card styles */
        .card {
            background: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        /* Form styles */
        .form-group {
            margin-bottom: 1rem;
        }

        .form-label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 500;
        }

        .form-input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
        }

        .form-input:focus {
            outline: none;
            border-color: #4A90E2;
            box-shadow: 0 0 0 2px rgba(74,144,226,0.2);
        }

        .error {
            color: #dc3545;
            font-size: 0.875rem;
            margin-top: 0.25rem;
        }

        /* Product list styles */
        .product-list {
            list-style: none;
        }

        .product-item {
            display: grid;
            grid-template-columns: 80px 1fr auto;
            gap: 1rem;
            padding: 1rem 0;
            border-bottom: 1px solid #eee;
        }

        .product-image {
            width: 80px;
            height: 80px;
            object-fit: cover;
            border-radius: 4px;
        }

        /* Payment methods */
        .payment-methods {
            display: flex;
            gap: 1rem;
            margin-bottom: 1rem;
        }

        .payment-method {
            flex: 1;
            padding: 1rem;
            border: 1px solid #ddd;
            border-radius: 4px;
            cursor: pointer;
            text-align: center;
        }

        .payment-method.selected {
            border-color: #4A90E2;
            background-color: rgba(74,144,226,0.1);
        }

        /* Button styles */
        .btn {
            display: inline-block;
            padding: 0.75rem 1.5rem;
            border: none;
            border-radius: 4px;
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            text-align: center;
            transition: all 0.2s;
        }

        .btn-primary {
            background-color: #4A90E2;
            color: white;
        }

        .btn-primary:hover {
            background-color: #357ABD;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="checkout-grid">
            <div class="checkout-main">
                <div class="card">
                    <h2>Shipping Information</h2>
                    <form id="shipping-form">
                        <div class="form-group">
                            <label class="form-label" for="name">Full Name</label>
                            <input type="text" id="name" class="form-input" required>
                        </div>
                        <div class="form-group">
                            <label class="form-label" for="address">Address</label>
                            <input type="text" id="address" class="form-input" required>
                        </div>
                        <div class="form-group">
                            <label class="form-label" for="phone">Phone Number</label>
                            <input type="tel" id="phone" class="form-input" required>
                        </div>
                        <div class="form-group">
                            <label class="form-label" for="city">City</label>
                            <input type="text" id="city" class="form-input" required>
                        </div>
                        <div class="form-group">
                            <label class="form-label" for="zip">ZIP Code</label>
                            <input type="text" id="zip" class="form-input" required>
                        </div>
                    </form>
                </div>

                <div class="card">
                    <h2>Payment Method</h2>
                    <div class="payment-methods">
                        <div class="payment-method" data-method="card">
                            <img src="/api/placeholder/32/32" alt="Credit Card">
                            <div>Credit Card</div>
                        </div>
                        <div class="payment-method" data-method="paypal">
                            <img src="/api/placeholder/32/32" alt="PayPal">
                            <div>PayPal</div>
                        </div>
                        <div class="payment-method" data-method="cod">
                            <img src="/api/placeholder/32/32" alt="Cash on Delivery">
                            <div>Cash on Delivery</div>
                        </div>
                    </div>
                    <div id="payment-details"></div>
                </div>
            </div>

            <div class="checkout-sidebar">
                <div class="card">
                    <h2>Order Summary</h2>
                    <ul class="product-list" id="order-items">
                        <!-- Products will be dynamically inserted here -->
                    </ul>
                    <div class="total">
                        <h3>Total: <span id="order-total">$0.00</span></h3>
                    </div>
                    <button class="btn btn-primary" style="width: 100%; margin-top: 1rem;">Complete Order</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Sample product data
        const products = [
            { id: 1, name: 'Product 1', price: 29.99, quantity: 1, image: '/api/placeholder/80/80' },
            { id: 2, name: 'Product 2', price: 39.99, quantity: 2, image: '/api/placeholder/80/80' }
        ];

        // Render order summary
        function renderOrderSummary() {
            const orderList = document.getElementById('order-items');
            orderList.innerHTML = products.map(product => `
                <li class="product-item">
                    <img src="${product.image}" alt="${product.name}" class="product-image">
                    <div>
                        <h3>${product.name}</h3>
                        <p>Quantity: ${product.quantity}</p>
                    </div>
                    <div>$${(product.price * product.quantity).toFixed(2)}</div>
                </li>
            `).join('');

            const total = products.reduce((sum, product) => sum + (product.price * product.quantity), 0);
            document.getElementById('order-total').textContent = `$${total.toFixed(2)}`;
        }

        // Form validation
        function validateForm() {
            const form = document.getElementById('shipping-form');
            const inputs = form.querySelectorAll('input');
            
            inputs.forEach(input => {
                input.addEventListener('blur', () => {
                    if (!input.value) {
                        input.classList.add('error');
                        if (!input.nextElementSibling?.classList.contains('error')) {
                            const error = document.createElement('div');
                            error.className = 'error';
                            error.textContent = 'This field is required';
                            input.parentNode.appendChild(error);
                        }
                    } else {
                        input.classList.remove('error');
                        const error = input.nextElementSibling;
                        if (error?.classList.contains('error')) {
                            error.remove();
                        }
                    }
                });
            });
        }

        // Payment method selection
        function initializePaymentMethods() {
            const methods = document.querySelectorAll('.payment-method');
            const detailsContainer = document.getElementById('payment-details');

            methods.forEach(method => {
                method.addEventListener('click', () => {
                    methods.forEach(m => m.classList.remove('selected'));
                    method.classList.add('selected');

                    // Show relevant payment details form
                    const paymentType = method.dataset.method;
                    if (paymentType === 'card') {
                        detailsContainer.innerHTML = `
                            <div class="form-group">
                                <label class="form-label">Card Number</label>
                                <input type="text" class="form-input" placeholder="1234 5678 9012 3456">
                            </div>
                            <div class="form-group">
                                <label class="form-label">Expiry Date</label>
                                <input type="text" class="form-input" placeholder="MM/YY">
                            </div>
                            <div class="form-group">
                                <label class="form-label">CVV</label>
                                <input type="text" class="form-input" placeholder="123">
                            </div>
                        `;
                    } else {
                        detailsContainer.innerHTML = '';
                    }
                });
            });
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            renderOrderSummary();
            validateForm();
            initializePaymentMethods();
        });
    </script>
</body>
</html>
