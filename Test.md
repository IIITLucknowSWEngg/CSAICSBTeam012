
## Registration page

# Given:
The user is on the registration page.

# When:
The user enters the valid information about himself/herself (name, email, password).

# Then:
The user should be successfully registered.
The user should be redirected to the login page.
If this doesn't happen then there must be some error

# code:

// app.js
const express = require('express');
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

// MongoDB Connection (replace with your URI)
mongoose.connect('mongodb://localhost:27017/registrationDB', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB Connected'))
  .catch(err => console.log('MongoDB Error:', err));

// Define User Schema
const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true },
  password: String,
});

// User Model
const User = mongoose.model('User', userSchema);

// Register Route
app.post('/register', async (req, res) => {
  const { name, email, password } = req.body;

  // Simple validation
  if (!name || !email || !password) return res.status(400).json({ message: 'All fields are required' });

  try {
    // Check if user exists
    const existingUser = await User.findOne({ email });
    if (existingUser) return res.status(400).json({ message: 'Email already in use' });

    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);

    // Create user
    const newUser = new User({ name, email, password: hashedPassword });
    await newUser.save();

    // Success
    res.status(201).json({ message: 'User registered successfully' });
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: 'Server error' });
  }
});

// Start server
app.listen(5000, () => console.log('Server running on port 5000'));

User Login

# Given:
The user is on the login page.

# When:
The user enters valid credentials (email, password).

# Then:
The user should be successfully logged in.
The user should be redirected to the dashboard.

# code:

const chai = require('chai');
const expect = chai.expect;
const loginPage = require('../pages/loginPage'); // Simulating a page object

describe('User Login', function() {
  it('should login user successfully', function() {
    loginPage.open();  // Simulate opening the login page
    loginPage.enterCredentials('john@example.com', 'password123'); // Enter login credentials
    loginPage.submitLogin();  // Simulate submitting the form
    expect(loginPage.getWelcomeMessage()).to.include('Welcome, John Doe');  // Check the welcome message
    expect(browser.getUrl()).to.include('/dashboard');  // Check the redirected URL
  });
});

# adding items in cart

first user will click on add to cart button

after that his/her item will be added in the cart

code:

const chai = require('chai');
const expect = chai.expect;
const cartPage = require('../pages/cartPage'); // Simulating a cart page object

describe('Add to Cart', function() {
  it('should add product to the cart successfully', function() {
    cartPage.open();  
    cartPage.addItemToCart('product123', 1);  
    const cartItemCount = cartPage.getCartItemCount();  
    expect(cartItemCount).to.equal(1);
    expect(cartPage.getCartItemDetails('product123')).to.include('Product Name');  
  });

  it('should show error when trying to add more than available stock', function() {
    cartPage.open();  
    cartPage.addItemToCart('product124', 100);  
    expect(cartPage.getErrorMessage()).to.include('Not enough stock');
    expect(cartPage.getCartItemCount()).to.equal(0);
  });
});


# Payment 

# Scenario: User completes a payment for cart items

# Given:
The user has a payment userinterface.

# When:
The user selects a payment method and enters payment details.

# Then:
The payment should be processed successfully.  
The user should receive a payment confirmation.

# code:

const chai = require('chai');
const expect = chai.expect;
const paymentPage = require('../pages/paymentPage');

describe('Payment Processing', function() {
  it('should process payment successfully', function() {
    paymentPage.open();
    paymentPage.enterPaymentDetails('1234 5678 9012 3456', '12/25', '123');
    paymentPage.submitPayment();
    expect(paymentPage.getPaymentConfirmation()).to.equal('Payment successful');
    expect(browser.getUrl()).to.include('/payment-confirmation') ;
  });
});
  

# User information update

# Given:
The user is logged in.

# When:
The user navigates to the profile page.  
The user updates their profile information.

# Then:
The profile should be updated successfully.

# code:

const chai = require('chai');
const expect = chai.expect;
const profilePage = require('../pages/profilePage');

describe('User Profile Management', function() {
  it('should update user profile successfully', function() {
    profilePage.open();
    profilePage.updateProfile('John Updated', 'johnupdated@example.com');
    expect(profilePage.getSuccessMessage()).to.equal('Profile updated successfully');
  });
});

