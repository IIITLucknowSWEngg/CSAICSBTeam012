# Test Cases for Functional Requirements

## 1.1: User Account Creation with a Secure Password

### Scenario: User creates an account with a secure password
**Given**: The user is on the registration page.  
**When**: The user enters a valid email, name, and a secure password.  
**Then**: The user should be successfully registered.  
**And**: The user should be redirected to the login page.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const registrationPage = require('../pages/registrationPage');

describe('User Registration with Secure Password', function() {
  it('should register user with a secure password', function() {
    registrationPage.open();
    registrationPage.fillRegistrationForm('John Doe', 'john@example.com', 'password123!');
    registrationPage.submitForm();
    expect(registrationPage.getSuccessMessage()).to.equal('Registration successful');
    expect(browser.getUrl()).to.include('/login');
  });
});
```

## 1.2: User Authentication (Email/Phone and Password or Third-party Login)

### Scenario 1: User logs in with email and password
**Given**: The user is on the login page.  
**When**: The user enters valid email and password.  
**Then**: The user should be successfully logged in and redirected to the homepage.

### Scenario 2: User logs in using third-party login (SSO)
**Given**: The user is on the login page.  
**When**: The user selects the "Login with Google" button.  
**Then**: The user should be authenticated via Google SSO and redirected to the homepage.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const loginPage = require('../pages/loginPage');

describe('User Authentication', function() {
  it('should allow login with email and password', function() {
    loginPage.open();
    loginPage.login('john@example.com', 'password123');
    expect(browser.getUrl()).to.include('/home');
  });

  it('should allow login using third-party login (Google)', function() {
    loginPage.open();
    loginPage.loginWithGoogle();
    expect(browser.getUrl()).to.include('/home');
  });
});
```


## 1.3: Seller Registration with Business Information

### Scenario: Seller provides necessary business information during registration
**Given**: The user is on the seller registration page.  
**When**: The seller enters business name, address, and bank details.  
**Then**: The seller account should be created successfully with the provided information.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const sellerRegistrationPage = require('../pages/sellerRegistrationPage');

describe('Seller Registration', function() {
  it('should register seller with business information', function() {
    sellerRegistrationPage.open();
    sellerRegistrationPage.fillRegistrationForm('TechStore', '123 Tech St', '9876543210', 'business@example.com', 'password123');
    sellerRegistrationPage.submitForm();
    expect(sellerRegistrationPage.getSuccessMessage()).to.equal('Seller registration successful');
  });
});
```


## 1.4: Password Recovery Mechanism

### Scenario: User initiates password recovery through email
**Given**: The user is on the password recovery page.  
**When**: The user enters their registered email address.  
**Then**: A password recovery email should be sent to the user.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const passwordRecoveryPage = require('../pages/passwordRecoveryPage');

describe('Password Recovery', function() {
  it('should send a recovery email when email is provided', function() {
    passwordRecoveryPage.open();
    passwordRecoveryPage.enterEmail('user@example.com');
    passwordRecoveryPage.submitForm();
    expect(passwordRecoveryPage.getSuccessMessage()).to.equal('Password recovery email sent');
  });
});
```



## 2.1: Product Search by Keyword, Category, or Brand (Buyers)

### Scenario: User searches for a product by keyword
**Given**: The user is on the homepage.  
**When**: The user enters a product keyword (e.g., "laptop") into the search bar.  
**Then**: The system should display a list of products matching the keyword "laptop".

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const homePage = require('../pages/homePage');

describe('Product Search by Keyword', function() {
  it('should display products matching the keyword', function() {
    homePage.open();
    homePage.enterSearchQuery('laptop');
    homePage.submitSearch();
    const searchResults = homePage.getSearchResults();
    expect(searchResults).to.include.members(['Laptop Model A', 'Laptop Model B']);
  });
});
```


## 2.2: Search Filters (Price Range, Rating, Stock Status) (Buyers)

### Scenario: User filters products by price range
**Given**: The user is on the search results page.  
**When**: The user applies a price filter (e.g., "$100 - $500").  
**Then**: The system should display only products within the selected price range.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const searchPage = require('../pages/searchPage');

describe('Product Search by Price Range', function() {
  it('should display products within the selected price range', function() {
    searchPage.open();
    searchPage.applyPriceFilter('1000 - 5000');
    const filteredProducts = searchPage.getFilteredProducts();
    filteredProducts.forEach(product => {
      const price = product.getPrice();
      expect(price).to.be.within(100, 500);
    });
  });
});
```



## 3.1: Product Listing Creation (Sellers)

### Scenario: Seller creates a new product listing
**Given**: The seller is logged in to their account.  
**When**: The seller navigates to the product listing page and enters product details (e.g., name, description, images, price, category).  
**Then**: The system should successfully create the product listing and display it in the seller’s product catalog.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const productPage = require('../pages/productPage');
const sellerAccount = require('../pages/sellerAccount');

describe('Product Listing Creation', function() {
  it('should create a new product listing successfully', function() {
    
    sellerAccount.login('seller@example.com', 'securepassword');
    productPage.open();
    
    
    productPage.fillProductForm('Wireless Mouse', 'A high-quality wireless mouse', 'images/mouse.jpg', 29.99, 'Electronics');
    productPage.submitForm();
    
    
    expect(productPage.getSuccessMessage()).to.equal('Product created successfully');
    expect(productPage.getProductList()).to.include('Wireless Mouse');
  });
});
```



## 3.2: Product Listing Editing and Deletion (Sellers)

### Scenario 1: Seller edits a product listing
**Given**: The seller is logged in and viewing their product listings.  
**When**: The seller selects a product and updates the product details (e.g., price, description, stock level).  
**Then**: The system should successfully update the product listing with the new details.

### Scenario 2: Seller deletes a product listing
**Given**: The seller is logged in and viewing their product listings.  
**When**: The seller selects a product and deletes it.  
**Then**: The system should successfully delete the product listing from the catalog.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const productPage = require('../pages/productPage');
const sellerAccount = require('../pages/sellerAccount');

describe('Product Listing Editing and Deletion', function() {
  it('should edit a product listing successfully', function() {
    
    sellerAccount.login('seller@example.com', 'securepassword');
    productPage.open();
    
    
    productPage.selectProduct('Wireless Mouse');
    productPage.editProductDetails('Wireless Mouse', 'A high-quality wireless mouse with adjustable DPI', 35.99);
    productPage.submitForm();
    
    
    expect(productPage.getSuccessMessage()).to.equal('Product updated successfully');
    expect(productPage.getProductDetails('Wireless Mouse')).to.include('adjustable DPI');
  });

  it('should delete a product listing successfully', function() {
    
    productPage.selectProduct('Wireless Mouse');
    productPage.deleteProduct();
    
    
    expect(productPage.getSuccessMessage()).to.equal('Product deleted successfully');
    expect(productPage.getProductList()).to.not.include('Wireless Mouse');
  });
});
```


## 3.3: Product Inventory Management (Sellers)

### Scenario 1: Seller updates product inventory levels
**Given**: The seller is logged in and viewing their product listing.  
**When**: The seller updates the inventory level for a product.  
**Then**: The system should successfully update the inventory level and display the new stock count.

### Scenario 2: Seller receives low-stock alert
**Given**: The seller is logged in and viewing their product listing.  
**When**: The inventory level for a product goes below a predefined threshold.  
**Then**: The system should send a low-stock alert notification to the seller.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const inventoryPage = require('../pages/inventoryPage');
const sellerAccount = require('../pages/sellerAccount');

describe('Product Inventory Management', function() {
  it('should update product inventory successfully', function() {
    
    sellerAccount.login('seller@example.com', 'securepassword');
    inventoryPage.open();
    
    
    inventoryPage.selectProduct('Wireless Mouse');
    inventoryPage.updateInventoryLevel(50);
    inventoryPage.submitForm();
    
    
    expect(inventoryPage.getSuccessMessage()).to.equal('Inventory updated successfully');
    expect(inventoryPage.getProductInventory('Wireless Mouse')).to.equal(50);
  });

  it('should send low-stock alert to seller', function() {
    
    inventoryPage.selectProduct('Wireless Mouse');
    inventoryPage.updateInventoryLevel(2);  
    
    
    expect(inventoryPage.getLowStockAlert()).to.equal('Low stock alert for Wireless Mouse');
  });
});
```


## 4.1: Product Management (Buyers)

### Scenario 1: Buyer views product details
**Given**: The buyer is browsing products on the platform.  
**When**: The buyer clicks on a product to view its details.  
**Then**: The system should display product details, including the name, price, description, images, reviews, and specifications.

### Scenario 2: Buyer views product reviews and ratings
**Given**: The buyer is viewing the product details page.  
**When**: The buyer scrolls down to the reviews section.  
**Then**: The system should display reviews and ratings for the product.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const productPage = require('../pages/productPage');
const buyerAccount = require('../pages/buyerAccount');

describe('Product Management', function() {
  it('should display product details', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    productPage.open('Wireless Mouse');
    
    
    expect(productPage.getProductName()).to.equal('Wireless Mouse');
    expect(productPage.getProductPrice()).to.equal('$25.99');
    expect(productPage.getProductDescription()).to.include('Wireless mouse with ergonomic design');
    expect(productPage.getProductImages()).to.have.lengthOf.at.least(1);
  });

  it('should display product reviews and ratings', function() {
    
    productPage.scrollToReviews();
    
    
    expect(productPage.getReviewCount()).to.be.greaterThan(0);
    expect(productPage.getAverageRating()).to.be.within(0, 5);
  });
});
```


## 4.2: Product Management (Buyers)

### Scenario 1: Buyer submits a product review
**Given**: The buyer is on the product details page.  
**When**: The buyer submits a review with a rating and a comment.  
**Then**: The system should save the review and display it on the product page.

### Scenario 2: Buyer views product reviews and ratings
**Given**: The buyer is on the product details page.  
**When**: The buyer scrolls down to the reviews section.  
**Then**: The system should display all reviews and the average rating for the product.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const productPage = require('../pages/productPage');
const buyerAccount = require('../pages/buyerAccount');
const reviewPage = require('../pages/reviewPage');

describe('Product Management', function() {
  it('should allow a buyer to submit a review', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    productPage.open('Wireless Mouse');
    
    
    reviewPage.submitReview(5, 'Great product, very comfortable!');
    
    
    expect(reviewPage.getReviewMessage()).to.equal('Review submitted successfully');
    expect(productPage.getReviewCount()).to.be.greaterThan(0);
  });

  it('should display all reviews and average rating', function() {
    
    productPage.scrollToReviews();
    expect(productPage.getReviewCount()).to.be.greaterThan(0);
    expect(productPage.getAverageRating()).to.be.within(0, 5);
  });
});
```


## 4.3: Product Management (Buyers)

### Scenario 1: System displays real-time stock availability
**Given**: The buyer is on the product details page.  
**When**: The buyer views the product's stock status.  
**Then**: The system should show whether the product is in stock or out of stock.

### Scenario 2: System displays out-of-stock notification
**Given**: The buyer is on the product details page.  
**When**: The buyer views a product that is out of stock.  
**Then**: The system should display a message indicating that the product is out of stock.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const productPage = require('../pages/productPage');
const buyerAccount = require('../pages/buyerAccount');

describe('Product Management', function() {
  it('should display the stock availability status', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    productPage.open('Wireless Keyboard');
    
    
    expect(productPage.getStockStatus()).to.equal('In Stock');
  });

  it('should display out-of-stock notification when product is unavailable', function() {
    
    productPage.open('Gaming Laptop');
    
    
    expect(productPage.getStockStatus()).to.equal('Out of Stock');
    expect(productPage.getOutOfStockMessage()).to.equal('This product is currently unavailable');
  });
});
```



## 5.1: Shopping Cart and Checkout (Buyers)

### Scenario 1: System allows buyers to add products to the shopping cart
**Given**: The buyer is on the product details page.  
**When**: The buyer clicks the "Add to Cart" button.  
**Then**: The system should add the product to the shopping cart.

### Scenario 2: System allows buyers to remove products from the shopping cart
**Given**: The buyer has added a product to the shopping cart.  
**When**: The buyer clicks the "Remove" button next to the product in the cart.  
**Then**: The system should remove the product from the cart.

### Scenario 3: System allows buyers to modify product quantities in the shopping cart
**Given**: The buyer has added a product to the shopping cart.  
**When**: The buyer increases or decreases the quantity of the product in the cart.  
**Then**: The system should update the product quantity in the cart.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const cartPage = require('../pages/cartPage');
const productPage = require('../pages/productPage');
const buyerAccount = require('../pages/buyerAccount');

describe('Shopping Cart and Checkout', function() {
  it('should add product to the shopping cart', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    productPage.open('Wireless Headphones');
    productPage.addToCart();
    
    
    expect(cartPage.getCartItemCount()).to.equal(1);
    expect(cartPage.getCartItemName(1)).to.equal('Wireless Headphones');
  });

  it('should remove product from the shopping cart', function() {
    
    cartPage.removeItem(1);
    
    
    expect(cartPage.getCartItemCount()).to.equal(0);
  });

  it('should allow modifying product quantity in the cart', function() {
    
    productPage.open('Wireless Keyboard');
    productPage.addToCart();
    cartPage.changeQuantity(1, 3);
    
    
    expect(cartPage.getCartItemQuantity(1)).to.equal(3);
  });
});
```


## 5.2: Shopping Cart and Checkout (Buyers)

### Scenario 1: System allows buyers to complete a multi-step secure checkout process
**Given**: The buyer has products in the shopping cart.  
**When**: The buyer proceeds to checkout and enters shipping information, payment details, and reviews the order.  
**Then**: The system should allow the buyer to submit the order and display a confirmation message.

### Scenario 2: System validates buyer's payment information during checkout
**Given**: The buyer is at the checkout page.  
**When**: The buyer enters invalid payment details (e.g., expired credit card).  
**Then**: The system should display an error message indicating the issue with the payment details.

### Scenario 3: System securely processes the buyer's payment and confirms the order
**Given**: The buyer has provided valid payment details and shipping information.  
**When**: The buyer submits the order for payment.  
**Then**: The system should securely process the payment and confirm the order with a success message.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const checkoutPage = require('../pages/checkoutPage');
const cartPage = require('../pages/cartPage');
const paymentPage = require('../pages/paymentPage');
const buyerAccount = require('../pages/buyerAccount');

describe('Shopping Cart and Checkout', function() {
  it('should allow buyer to complete secure checkout process', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    cartPage.viewCart();
    checkoutPage.proceedToCheckout();
    checkoutPage.enterShippingInformation('John Doe', '123 Street', 'City', 'Country');
    checkoutPage.enterPaymentDetails('4111111111111111', '12/23', '123');
    checkoutPage.submitOrder();
    
    
    expect(checkoutPage.getConfirmationMessage()).to.equal('Your order has been placed successfully');
  });

  it('should display error for invalid payment details', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    cartPage.viewCart();
    checkoutPage.proceedToCheckout();
    checkoutPage.enterShippingInformation('John Doe', '123 Street', 'City', 'Country');
    checkoutPage.enterPaymentDetails('4111111111111111', '12/20', '123'); 
    
    
    expect(paymentPage.getErrorMessage()).to.equal('Payment method expired. Please enter a valid card.');
  });

  it('should securely process payment and confirm order', function() {
    
    buyerAccount.login('buyer@example.com', 'buyerpassword');
    cartPage.viewCart();
    checkoutPage.proceedToCheckout();
    checkoutPage.enterShippingInformation('John Doe', '123 Street', 'City', 'Country');
    checkoutPage.enterPaymentDetails('4111111111111111', '12/23', '123');
    checkoutPage.submitOrder();
    
    
    expect(checkoutPage.getConfirmationMessage()).to.equal('Your order has been placed successfully');
  });
});
```


## 5.3: Shopping Cart and Checkout (Buyers)

### Scenario: System supports guest checkout without account registration
**Given**: The buyer has products in the shopping cart.  
**When**: The buyer proceeds to checkout without logging in or registering an account.  
**Then**: The system should allow the buyer to provide shipping and payment details, and complete the checkout as a guest.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const cartPage = require('../pages/cartPage');
const guestCheckoutPage = require('../pages/guestCheckoutPage');

describe('Shopping Cart and Checkout (Guest Checkout)', function() {
  it('should allow guest checkout without account registration', function() {
    
    cartPage.viewCart();
    guestCheckoutPage.proceedToCheckout();
    guestCheckoutPage.enterShippingInformation('John Doe', '123 Street', 'City', 'Country');
    guestCheckoutPage.enterPaymentDetails('4111111111111111', '12/23', '123');
    guestCheckoutPage.submitOrder();
    
    
    expect(guestCheckoutPage.getConfirmationMessage()).to.equal('Your order has been placed successfully as a guest');
  });
});
```


## 6.1: Payment and Transaction Processing (Buyers)

### Scenario: System supports multiple payment methods (credit/debit cards, e-wallets, COD)
**Given**: The buyer is at the checkout page with items in the cart.  
**When**: The buyer selects a payment method (credit/debit card, e-wallet, or COD).  
**Then**: The system should allow the buyer to complete the payment using the selected method.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const checkoutPage = require('../pages/checkoutPage');

describe('Payment and Transaction Processing', function() {
  it('should support multiple payment methods', function() {
    
    checkoutPage.viewCart();
    checkoutPage.selectPaymentMethod('Credit Card');
    checkoutPage.enterPaymentDetails('4111111111111111', '12/23', '123');
    checkoutPage.submitOrder();
    
    
    expect(checkoutPage.getConfirmationMessage()).to.equal('Payment successful. Your order is being processed');
    
    
    checkoutPage.selectPaymentMethod('E-wallet');
    checkoutPage.enterPaymentDetails('E-wallet-12345');
    checkoutPage.submitOrder();
    
    
    expect(checkoutPage.getConfirmationMessage()).to.equal('Payment successful. Your order is being processed');
    
    
    checkoutPage.selectPaymentMethod('COD');
    checkoutPage.submitOrder();
    
    
    expect(checkoutPage.getConfirmationMessage()).to.equal('Your order will be shipped and paid upon delivery');
  });
});
```



## 6.2: Payment and Transaction Processing (Buyers)

### Scenario: System securely processes payments using a payment gateway
**Given**: The buyer has selected a payment method and entered the necessary payment details.  
**When**: The buyer submits the payment for processing.  
**Then**: The system should securely process the payment using an integrated payment gateway and confirm the payment status.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const paymentPage = require('../pages/paymentPage');
const paymentGateway = require('../services/paymentGateway');

describe('Payment and Transaction Processing', function() {
  it('should securely process payments using a payment gateway', function() {
    
    paymentPage.viewCart();
    paymentPage.enterPaymentDetails('4111111111111111', '12/23', '123');
    
    
    const paymentResponse = paymentGateway.processPayment('4111111111111111', '12/23', '123');
    
    
    expect(paymentResponse.status).to.equal('Success');
    expect(paymentResponse.transactionId).to.exist;
    expect(paymentPage.getConfirmationMessage()).to.equal('Payment processed successfully');
  });
});
```


## 6.3: Payment and Transaction Processing (Sellers)

### Scenario: Sellers can receive payments and view transaction history
**Given**: The seller has a valid payment account and has completed the sale of products.  
**When**: A payment has been successfully processed for a sale.  
**Then**: The seller should be able to view the payment in their transaction history.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const sellerDashboard = require('../pages/sellerDashboard');
const transactionHistoryService = require('../services/transactionHistoryService');

describe('Payment and Transaction Processing (Sellers)', function() {
  it('should allow sellers to receive payments and view transaction history', function() {
    
    const transaction = transactionHistoryService.recordTransaction('seller123', 100, 'completed');
    
    
    sellerDashboard.viewTransactionHistory('seller123');
    
    
    const transactions = sellerDashboard.getTransactionHistory();
    expect(transactions).to.have.lengthOf.above(0);
    expect(transactions[0].status).to.equal('completed');
    expect(transactions[0].amount).to.equal(100);
    expect(sellerDashboard.getSuccessMessage()).to.equal('Transaction recorded successfully');
  });
});
```


## 7.1: Order Tracking and Fulfillment (Buyers)

### Scenario: Buyers can view their order history and track current order status
**Given**: The buyer has placed at least one order on the system.  
**When**: The buyer accesses their order history page.  
**Then**: The system should display a list of past orders with details including order status, shipping information, and expected delivery date.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const orderHistoryPage = require('../pages/orderHistoryPage');
const orderService = require('../services/orderService');

describe('Order Tracking and Fulfillment (Buyers)', function() {
  it('should allow buyers to view their order history and track current order status', function() {
    
    const order = orderService.placeOrder('buyer123', 'product456', 2);
    
    
    orderHistoryPage.open();
    
    
    const orders = orderHistoryPage.getOrderHistory();
    expect(orders).to.have.lengthOf.above(0);
    expect(orders[0].orderId).to.equal(order.orderId);
    expect(orders[0].status).to.equal('Processing');
    expect(orderHistoryPage.getSuccessMessage()).to.equal('Order history loaded successfully');
  });
});
```



## 7.2: Order Tracking and Fulfillment (Sellers)

### Scenario: Sellers can manage and fulfill orders, including updating shipment status and tracking
**Given**: The seller has received a new order from a buyer.  
**When**: The seller accesses the "Manage Orders" section in the seller dashboard.  
**Then**: The seller should be able to view the details of the new order, including the buyer's shipping information and order items.  
**And**: The seller should be able to update the order status (e.g., "Shipped", "Out for Delivery") and track the shipment.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const sellerDashboardPage = require('../pages/sellerDashboardPage');
const orderService = require('../services/orderService');

describe('Order Tracking and Fulfillment (Sellers)', function() {
  it('should allow sellers to manage and fulfill orders, and update shipment status', function() {
    
    const order = orderService.placeOrder('buyer123', 'product456', 2);
    
    
    sellerDashboardPage.open();
    
    
    const orders = sellerDashboardPage.getOrdersList();
    expect(orders).to.have.lengthOf.above(0);
    expect(orders[0].orderId).to.equal(order.orderId);
    expect(orders[0].status).to.equal('Pending');
    
    
    sellerDashboardPage.updateOrderStatus(order.orderId, 'Shipped');
    
    
    expect(sellerDashboardPage.getOrderStatus(order.orderId)).to.equal('Shipped');
    expect(sellerDashboardPage.getSuccessMessage()).to.equal('Order status updated successfully');
  });
});
```


## 7.3: Order Tracking and Fulfillment (Sellers)

### Scenario: Sellers can handle returns, refunds, and order cancellations
**Given**: The seller has received an order and the buyer has requested a return, refund, or cancellation.  
**When**: The seller accesses the "Manage Orders" section in the seller dashboard.  
**Then**: The seller should be able to view the return, refund, or cancellation request.  
**And**: The seller should be able to process the request (e.g., approve the return, issue a refund, or cancel the order).

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const sellerDashboardPage = require('../pages/sellerDashboardPage');
const orderService = require('../services/orderService');

describe('Order Tracking and Fulfillment (Sellers)', function() {
  it('should allow sellers to handle returns, refunds, and order cancellations', function() {
    
    const order = orderService.placeOrder('buyer123', 'product456', 2);

    
    sellerDashboardPage.open();
    
    
    orderService.requestReturn(order.orderId);

    
    const orders = sellerDashboardPage.getOrdersList();
    const orderWithReturn = orders.find(o => o.orderId === order.orderId);
    expect(orderWithReturn.returnStatus).to.equal('Requested');
    
    
    sellerDashboardPage.processReturn(order.orderId, 'Approved');
    
    
    expect(sellerDashboardPage.getOrderStatus(order.orderId)).to.equal('Return Approved');
    expect(sellerDashboardPage.getSuccessMessage()).to.equal('Return processed successfully');
  });
});
```


## 8.1: Sales Reporting and Analytics (Sellers)

### Scenario: Sellers can access sales reports and analytics
**Given**: The seller is logged into the seller dashboard.  
**When**: The seller navigates to the "Sales Reports" section.  
**Then**: The seller should be able to view sales reports, including total revenue, best-selling products, and monthly sales.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const sellerDashboardPage = require('../pages/sellerDashboardPage');
const salesService = require('../services/salesService');

describe('Sales Reporting and Analytics (Sellers)', function() {
  it('should allow sellers to access sales reports and analytics', function() {
    
    sellerDashboardPage.open();
    sellerDashboardPage.goToSalesReports();

    
    const reports = sellerDashboardPage.getSalesReports();
    expect(reports).to.have.property('totalRevenue');
    expect(reports).to.have.property('bestSellingProducts');
    expect(reports).to.have.property('monthlySales');

    
    expect(reports.totalRevenue).to.be.a('number');
    expect(reports.bestSellingProducts).to.be.an('array').that.is.not.empty;
    expect(reports.monthlySales).to.be.an('array').that.is.not.empty;
  });
});
```



## 9.1: Notifications (Buyers and Sellers)

### Scenario: Notifications sent to buyers for order updates and promotional offers
**Given**: The buyer has placed an order and opted in for promotional notifications.  
**When**: The order status changes (e.g., shipped, delivered) or a promotional offer is available.  
**Then**: The buyer should receive an email or SMS notification regarding the order update or promotional offer.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const orderService = require('../services/orderService');
const notificationService = require('../services/notificationService');

describe('Notifications (Buyers and Sellers)', function() {
  it('should send notification to buyer for order updates and promotional offers', function() {
    
    const buyerId = 'buyer123';
    const orderId = orderService.placeOrder(buyerId, ['product1', 'product2']);

    
    orderService.updateOrderStatus(orderId, 'shipped');

    
    const notifications = notificationService.getNotifications(buyerId);
    const orderNotification = notifications.find(notification => notification.orderId === orderId);
    expect(orderNotification).to.have.property('type').that.equals('orderUpdate');
    expect(orderNotification).to.have.property('message').that.contains('Your order has been shipped');
  });
});
```


## 9.2: Notifications (Buyers and Sellers)

### Scenario: Notifications sent to sellers for new orders, payment status, and inventory alerts
**Given**: A seller has active listings on the platform.  
**When**: A new order is placed, a payment status is updated, or the inventory reaches a low threshold.  
**Then**: The seller should receive an email or SMS notification regarding the new order, payment status update, or inventory alert.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const orderService = require('../services/orderService');
const paymentService = require('../services/paymentService');
const inventoryService = require('../services/inventoryService');
const notificationService = require('../services/notificationService');

describe('Notifications (Buyers and Sellers)', function() {
  it('should send notification to seller for new orders, payment status, and inventory alerts', function() {
    
    const sellerId = 'seller123';

    
    const orderId = orderService.placeOrder('buyer123', ['product1']);
    const orderNotification = notificationService.getNotifications(sellerId).find(notification => notification.orderId === orderId);
    expect(orderNotification).to.have.property('type').that.equals('newOrder');
    expect(orderNotification).to.have.property('message').that.contains('New order placed');

    
    const paymentStatus = paymentService.updatePaymentStatus(orderId, 'completed');
    const paymentNotification = notificationService.getNotifications(sellerId).find(notification => notification.type === 'paymentStatus');
    expect(paymentNotification).to.have.property('message').that.contains('Payment completed');

    
    inventoryService.updateInventory('product1', 2); 
    const inventoryNotification = notificationService.getNotifications(sellerId).find(notification => notification.type === 'inventoryAlert');
    expect(inventoryNotification).to.have.property('message').that.contains('Low stock for product1');
  });
});
```



## 10.1: Customer Support (Buyers and Sellers)

### Scenario: Providing customer support access via email, phone, or live chat
**Given**: A buyer or seller needs assistance with an issue related to their account or a transaction.  
**When**: The buyer or seller attempts to contact customer support through email, phone, or live chat.  
**Then**: The system should allow the buyer or seller to access customer support through their preferred method (email, phone, or live chat).

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const supportService = require('../services/supportService');
const userService = require('../services/userService');

describe('Customer Support (Buyers and Sellers)', function() {
  it('should allow users to contact customer support via email, phone, or live chat', function() {
    const buyerId = 'buyer123';
    const sellerId = 'seller123';

    
    const emailResponse = supportService.contactSupport(buyerId, 'email');
    expect(emailResponse).to.have.property('status').that.equals('success');
    expect(emailResponse).to.have.property('method').that.equals('email');

    
    const phoneResponse = supportService.contactSupport(sellerId, 'phone');
    expect(phoneResponse).to.have.property('status').that.equals('success');
    expect(phoneResponse).to.have.property('method').that.equals('phone');

    
    const liveChatResponse = supportService.contactSupport(buyerId, 'live chat');
    expect(liveChatResponse).to.have.property('status').that.equals('success');
    expect(liveChatResponse).to.have.property('method').that.equals('live chat');
  });
});
```


## 10.2: Customer Support (Buyers and Sellers)

### Scenario: Handling product return requests, disputes, and resolution for both buyers and sellers
**Given**: A buyer or seller needs to initiate a product return request, dispute, or resolution.  
**When**: The buyer or seller submits a request for a return, dispute, or resolution via the customer support system.  
**Then**: The system should process the request and route it to the appropriate support team for resolution.

**Chai.js Code:**
```javascript
const chai = require('chai');
const expect = chai.expect;
const returnService = require('../services/returnService');
const disputeService = require('../services/disputeService');

describe('Customer Support (Buyers and Sellers)', function() {
  it('should handle product return requests, disputes, and resolution', function() {
    const buyerId = 'buyer123';
    const sellerId = 'seller123';

    
    const returnRequest = returnService.initiateReturn(buyerId, 'order123', 'defective product');
    expect(returnRequest).to.have.property('status').that.equals('success');
    expect(returnRequest).to.have.property('requestType').that.equals('return');

    
    const disputeRequest = disputeService.initiateDispute(sellerId, 'order456', 'misleading product description');
    expect(disputeRequest).to.have.property('status').that.equals('success');
    expect(disputeRequest).to.have.property('requestType').that.equals('dispute');

    
    const resolutionStatus = returnService.resolveRequest('order123');
    expect(resolutionStatus).to.have.property('status').that.equals('resolved');
    expect(resolutionStatus).to.have.property('resolved').that.equals(true);
  });
});
```