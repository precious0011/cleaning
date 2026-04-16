🧪 Task 2: Test Log

⸻

✅ TEST LOG TABLE

Task 2: Test log	Description of test	Test data to be used (if required)	Expected outcome	Actual outcome	Comments and intended actions
Test 1	Register a new user	Email: test@gmail.com, Password: 1234	User should be added to database and dashboard opens	User registered successfully and dashboard opened	Working correctly, no changes needed
Test 2	Login with correct details	Email: test@gmail.com, Password: 1234	User should log in and access dashboard	Login successful	System works as expected
Test 3	Login with incorrect details	Email: test@gmail.com, Password: wrong123	System should show error message	Error message displayed	Validation works correctly
Test 4	Add product to cart	Click “Add” on Milk	Item should be added to cart	Item added successfully	Works correctly
Test 5	View cart	Click “View Cart”	All selected items should display with total	Cart displayed with correct total	No issues
Test 6	Checkout with items	Cart contains Milk, Bread	Order should be saved in database	Order saved successfully	Works correctly
Test 7	Checkout with empty cart	No items in cart	System should show error	Error message shown	Validation works correctly
Test 8	Search product (Version 4)	Search: “Milk”	Only Milk should display	Correct product displayed	Search works correctly
Test 9	Search invalid product	Search: “Pizza”	No products should display	No results shown	Works as expected
Test 10	Stock reduction after checkout	Buy Milk (stock 10 → 9)	Stock should decrease by 1	Stock updated correctly	Feature working correctly
Test 11	Attempt to buy out-of-stock item	Stock = 0	System should prevent purchase	Error message shown	Works correctly
Test 12	Producer updates stock	Product: Milk, Stock: 20	Stock should update in database	Stock updated	Input validation added
Test 13	Enter invalid stock value	Stock: “abc”	System should show error	Error shown	Validation works correctly
Test 14	View orders	Click “Orders”	Previous orders should display	Orders displayed correctly	No issues
Test 15	System restart (data persistence)	Close and reopen app	Data should remain stored	Data still present	Database working correctly

⸻

🧠 How to describe your testing (add this under the table)

The system was tested using a range of normal, boundary, and erroneous test cases to ensure reliability and functionality. Each feature was tested individually, including login, registration, cart operations, checkout, and database storage. The results show that the system performs as expected, with validation preventing incorrect inputs and ensuring data integrity.
